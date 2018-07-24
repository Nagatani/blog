---
title: "M5StackとDHT12で作る部屋の温度湿度をIFTTTのWebhooksへ投げるやつ"
date: 2018-07-25T02:43:48+09:00
tags: [ "M5Stack", "Arduino", "ESP32", "C++" ]
draft: false
---

タイトルの通りですが、作りました。

制作動機は、我が家のリビングがめっちゃ暑いのと、我が家のネコたちが暑さでぐったりしているのがかわいそうというのがあります。で、先月ぐらいに買って、そのうちなにか作ろうとしていた M5Stack Basic と 温湿度センサのDHT12がセットになったプロトキットがあったので、これで作るか、となった次第です。

やりたかったことは、リビングが指定された温度以上になった場合に、Slackへ通知するシステムを作りたかった感じです。
最終的には、1時間おきに温度湿度をIFTTT経由でSlackの所定のチャンネルへ投稿する仕組みになりました。ちょっとだけ実現された内容が異なる理由は後述します。

M5Stack Basic と プロトキット(温湿度センサ付き) はいつもお世話になってるスイッチサイエンスさんで買いました。買いましょう。

- [M5Stack Basic - スイッチサイエンス](https://www.switch-science.com/catalog/3647/)
- [M5Stack用プロトキット（温湿度センサ付き） - スイッチサイエンス](https://www.switch-science.com/catalog/3651/)

出来上がりは以下の写真のようになりました。

![](/image/M5StackDHT12Temp2IFTT.jpg)

似たようなものを作ろうとしている方、ソースコード一式をGitHubにPushしておきましたので参考程度にしてください。

- [Nagatani/M5Stack_DHT12_TemperatureToIFTTT: M5Stackとプロトタイピングキットについてくる温度湿度センサDHT12を使って、部屋の温度をIFTTTのWebhooksに投げるやつ](https://github.com/Nagatani/M5Stack_DHT12_TemperatureToIFTTT)

IFTTTのWebhooksトリガーは以下のような設定にしてあります。

![](/image/M5StackDHT12Temp2IFTTT_Webhooks.png)

これだけでわかるはず……。

以下、技術的な話です。

## M5Stackの話
M5Stackの何が良いかというと、単体で以下のものがついてくるところです。

- Wi-Fi
- Bluetooth
- TFTカラーディスプレイ
- microSDスロット
- スピーカー
- LiPoバッテリー(ボトム部分)
- モジュール積み上げ可能

至れり尽くせりという状況です。この中でもESP32を使っているためWi-Fi標準装備、かつディスプレイあり、さらにモジュール積み上げで拡張が容易というのがお気に入りポイントですね。

# ちょっと詰まったところ
M5Stackを初めて触ってみて、ちょっと詰まったところを挙げていきます。
## JSTの時刻をNTPから取得し本体の時刻設定を行う
インターネットでいろいろ探して、最終的に以下の方法で落ち着きました。(一部抜粋)

```cpp
Serial.printf("Connecting to %s ", WIFI_SSID);
WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
while (WiFi.status() != WL_CONNECTED) {
  delay(500);
  Serial.print(".");
}
Serial.println(" conected!");

configTzTime("JST-9", "ntp.nict.jp", "time.google.com", "ntp.jst.mfeed.ad.jp");
tm t = getDateTime();

WiFi.disconnect(true);
```

最初は`configTime`関数使っていて、引数のOffsetが秒での設定だと知らず正常な時刻が取得できず悩んでましたが、`configTzTime`関数でJSTを指定できるのは便利だと思いました。
ソースコードは、`ClockController.cpp`でやってます。

## ESP32のマルチタスク処理
特に何も考えずArduinoの開発環境でセットアップしたので、そのままArduino IDEで作っていたんですが、「温度湿度取得から後続の処理」と「時計の表示処理」のタイミングをずらしたい欲求に駆られます。

時刻取得はできていたので、時刻でのトリガーを作るかーという感じでいろいろ調べていたら、なんとESP32ではマルチタスクができるということを知りました。

コードは以下のようになりました。(一部省略)

```cpp
void clockTask(void * pvParameters) {
  for (;;) {
    ・・・
    delay(30000);
  }
}

void updateTempAndHumi(void * pvParameters) {
  for (;;) {
    ・・・

    delay(60000);
  }
}

void setup() {
  M5.begin();

  ・・・

  xTaskCreatePinnedToCore(clockTask, "clockTask", 4096, NULL, 1, NULL, 0);
  xTaskCreatePinnedToCore(updateTempAndHumi, "updateTempAndHumi", 4096, NULL, 2, NULL, 0);
}

void loop() {
  M5.update();
}
```

`xTaskCreatePinnedToCore`を使うことで、マルチタスクが可能になりました。
それぞれ、`delay`を使って処理間隔を調整していて、時刻表示は大事ではないけどある程度は正確な時刻を表示したいので30秒間隔、温度湿度はだいたい1分置きでいっかなーと言う感じで設定しています。

# システム的に困ったところ
我が家は、リビングでGoogle Home、自室でAmazon Echoを使ったスマートホーム化が進んでおり、赤外線送信装置(eRemote mini)を使ってテレビや証明をコントロールできているんですが、エアコンだけがうまく行っていませんでした。

ちなみに、構成は、GoogleHome(またはAmazon Echo) → IFTTT → Slack → Hubot & Redis(RaspberryPiWで動作) → eRemote mini という感じで一見複雑な構成となっています。
複雑ですが、Slackに何かしら投稿ができれば家電を操作できるという点がメリットだったりします。

今回は温度湿度管理ということで、「温度がある一定以上になったら」というトリガーでエアコンを操作したいという欲求があったわけです。

で、エアコン操作の何がダメだったかと言うと、我が家のリビングと自室で使っているエアコンが三菱重工業のBEAVERエアコンなんですが、どうにもエアコンのリモコンによるスイッチON、冷暖房切り替え、温度設定などの操作が単純に赤外線を送信で終わり、というものではなく、赤外線を送信→本体からのレスポンス受信→再送信みたいなことをやっているっぽい(詳細は未確認)んです。これにとても困っていて、今現在使用しているhubotとRedisでの赤外線信号の学習方法では、単純に送信しかできておらず、エアコンの電源は入るが冷暖房切り替えおよび温度設定が不可能という事態になっています。

そう言えば学生たちが夏休み中はほぼ家にいるし、まぁそのうちなんか解決策みつかるやろーという気持ちで楽観視しているんですが、これはいつかどこかで対応したいと思っています。

温度上昇でのトリガーでデータ送信ではなく、1時間ごとの温度湿度データの送信に切り替えた理由はこれです。
どうせ自動でエアコン操作できないなら、温度上昇トリガーじゃなくても良くないか？ってなってしまいました。
Slackのチャンネルに投稿される温度湿度をたまに見て、あまりにも暑そうだったらエアコン入れに行くかーという中途半端なものが出来上がった次第でございます。

夏が過ぎたら、M5Stackは別の用途で使用しようと考えてます。

私からは以上です。
