---
title: "Twitterを過去に遡って検索してデータを取得するやつ作った"
date: 2018-05-30T23:30:36+09:00
tags: [ "nodejs", "nightmarejs", "twitter" ]
draft: false
---

APIの仕様変更にうんざりしている皆様こんにちは。

↓まぁこんなことがあったので、日頃の情報収集のツールから検索の部分だけ取り出して加筆修正したのを公開しました。

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">Twitter APIでの検索が過去7日だっけかの制限があるから過去データ検索できないっつってる人いたんだけど、Twitter Webでの検索は日付の制限ないの教えてあげれば良かったかなって</p>&mdash; ながたに🐈 (@Nagatani) <a href="https://twitter.com/Nagatani/status/1001803626459811842?ref_src=twsrc%5Etfw">May 30, 2018</a></blockquote>

<blockquote class="twitter-tweet" data-lang="en"><p lang="ja" dir="ltr">SeleniumでもNightmare.jsでも良いんだけど、検索のパラメータに検索キーワードと日付設定してスクロールどんどんさせて最後まで読み込んだら document.querySelectorAll(&quot;.tweet[data-tweet-id]&quot;) で良いんじゃないかと</p>&mdash; ながたに🐈 (@Nagatani) <a href="https://twitter.com/Nagatani/status/1001804373398192129?ref_src=twsrc%5Etfw">May 30, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


## リポジトリ

Twitterの利用規約的にグレーゾーンなような気もするので、怒られたら速攻で消します。

- [Nagatani/TweetSearchFromWeb](https://github.com/Nagatani/TweetSearchFromWeb)

## なにやってるのか
Twitter Webの検索フォーム( https://twitter.com/search-home )はユーザー登録も不要で検索機能が使えるんですよ。

APIだと過去7日とかの制限があるんですが、これは制限がありません。(たぶん)

そこで、詳細検索( https://twitter.com/search-advanced )のパラメータをいくつか設定してみると、URIのパラメータにそれっぽいものが出てきます。こいつを使わない手はないな。ということでE2Eテストとかで使われていたりするnightmare.jsを使って自動化し、検索、データ取得、JSONで保存をしてます。

## あとは
ライセンスWTPFLなのであとは好きにすればいいと思います。

不具合あっても知らん。動かないとかも受け付けない。メールもいらんし、ISSUEも立てないでほしい。

ただ、***Twitterの利用規約的にまずい場合はすぐに教えてほしい。***

私からは以上です。
