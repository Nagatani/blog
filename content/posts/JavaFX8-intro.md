---
title: JavaFX8 FXMLでのGUIアプリケーション開発 - 事始め
date: 2014-09-17 15:11:01
categories:
- JavaFX8
tags:
- Java
- JavaFX8
- JavaFXSceneBuilder2
- FXML
---
今回から、JavaFX8(JavaFX on Java8)についての記事を幾つか書こうと思う。
前持って言って置かなければならないんだけど、私はJavaFXでの開発実績がない。
この記事もある程度勉強進めた段階で書いてるけど、自分がわかるように書くだけなので参考にならなくても知らんし、間違った内容もあるかもしれんし、途中で飽きたら放置するのでそのつもりでお願いします。

## Java8から標準機能となったJavaFX
Swingと取って代わって、JavaFXがJavaのGUI開発の標準的な位置づけになりつつある（なった）と思って差し支えないかと思う。
何言ってんだSwingが標準だろうが！ってお怒りの方、落ち着いてください。そういう意見もあります。(あるのかどうかも知らんけど)
<!-- more -->
## とりあえず Java 8 を NetBeans 8 で開発できるようにしよう
Eclipse？
そんなものは知らん。

IntelliJ IDEA？
ライセンス欲しい。

Java8とNetBeans8がセットになったJava SE 8が以下のサイト(Oracle)よりダウンロードできる。
JavaでGUIアプリケーションを今から始める。という方は、迷わずNetBeans付きのものをダウンロードすると良いだろう。

[Java SE Downloads - Netbeans + JDK Bundle](http://www.oracle.com/technetwork/java/javase/downloads/jdk-netbeans-jsp-142931.html)

NetBeansはGUIアプリケーション開発に非常に向いているIDEだと思う。
Oracleが配布してるし、なんとなく安心感を得られます。
……よく落ちるけど。

ダウンロードからインストールまでは、それほど躓くところはないと思う。
PATHも多分インストーラーが通してくれるはず。

余談だけど、WindowsでNetBeans立ち上げると、フォント見た瞬間に拒絶反応出る。
最近はJavaでの開発はもっぱらMac OSでやってる。
WindowsでNetBeans使う場合は、まず最初にエディタのフォントを見やすいのに変えることをオススメしたい。

## FXMLのアプリケーションには必需品と言ってもいい JavaFX Scene Builder 2.0

これ入れておくと捗る場合がほとんどだと思うので、入れておく。
じゃないとGUIの調整に、FXMLと呼ばれるJavaFX用のXMLを直接直す必要性が出てくる。
慣れてる人なら入れなくてもいいかもしれないが、便利なツールなので入れておくに越したことはないと思う。

[JavaFX Scene Builder 2.0 Download](http://www.oracle.com/technetwork/java/javase/downloads/sb2download-2177776.html)

また、JavaFX8には、Swing互換っぽい動きをするJavaFX in Swingなるものと、JavaFX FXMLなるものがある。
Swingに慣れている人は前者を使えば良い。それなら Scene Builderはいらない。
だけど、どうせなら FXML を覚えた方が後々役に立つだろうと思う。

## とりあえず JavaFX8 で世界に挨拶をしとこう。

みんな最初は世界に挨拶することから始まります。
手順は以下のとおり。

1. NetBeans8を起動
2. メニューバーの[ファイル]→\[新規プロジェクト\]  (Macなら[shift] + [⌘] + [n])
3. カテゴリから「JavaFX」を選択
4. プロジェクトから「JavaFX FXMLアプリケーション」を選択
5. 「次へ」
6. プロジェクトの保存先等を設定する
7. 「終了」
8. プロジェクトを実行

## もう少し詳しく

### 1. NetBeans8を起動
### 2. メニューバーの[ファイル]→\[新規プロジェクト\]  (Macなら[shift] + [⌘] + [n])
![これ](/image/sc-javafx8-intro-01.png)
### 3. カテゴリから「JavaFX」を選択
![「次へ」を押すのはまだ早い！](/image/sc-javafx8-intro-02.png)
### 4. プロジェクトから「JavaFX FXMLアプリケーション」を選択
### 5. 「次へ」
![ちゃんとJavaFX FXMLアプリケーションを選ぼう。](/image/sc-javafx8-intro-03.png)
### 6. プロジェクトの保存先等を設定する
![保存先とかは分かりやすいところにまとめておいたほうが良いと思う。](/image/sc-javafx8-intro-04.png)
### 7. 「終了」
![プロジェクトが生成できた。](/image/sc-javafx8-intro-05.png)
### 8. プロジェクトを実行
![こんなボタンがあるはず。探して押そう。](/image/sc-javafx8-intro-06.png)

## 実行すると
以下の様な小さい画面が出てくる。
![](/image/sc-javafx8-intro-07.png)

さも押してほしそうなボタン、「Click Me!」をクリック
![](/image/sc-javafx8-intro-09.png)

これで通過儀礼である世界への挨拶が完了した。

## 生成されたソースコードを見てみよう。
![プロジェクトのソースから各ファイルを見れる](/image/sc-javafx8-intro-05.png)
まずは、`FXMLDocument.fxml`をダブルクリックすると、JavaFX Scene Builder 2.0が立ち上がる。(もちろんインストールが完了していれば)
![](/image/sc-javafx8-intro-10.png)

この画面でGUIを直感的に操作できるし、コントロールの追加もドラッグアンドドロップでできる。
全部英語なのは我慢すべし。

NetBeansに戻ると、以下の様にXMLが書かれた`FXMLDocument.fxml`が開かれていると思う。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<?import java.lang.*?>
<?import java.util.*?>
<?import javafx.scene.*?>
<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<AnchorPane id="AnchorPane" prefHeight="200" prefWidth="320" xmlns:fx="http://javafx.com/fxml/1" fx:controller="javafxapplication1.FXMLDocumentController">
    <children>
        <Button layoutX="126" layoutY="90" text="Click Me!" onAction="#handleButtonAction" fx:id="button" />
        <Label layoutX="126" layoutY="120" minHeight="16" minWidth="69" fx:id="label" />
    </children>
</AnchorPane>
```

これが、FXMLである。
画面全部XMLで表現できるし、ここでプログラミングすることもできる。

次に、`FXMLDocumentController.java`を見てみよう。
ソースコードは以下のようになっている。

```java
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

package javafxapplication1;

import java.net.URL;
import java.util.ResourceBundle;
import javafx.event.ActionEvent;
import javafx.fxml.FXML;
import javafx.fxml.Initializable;
import javafx.scene.control.Label;

/**
 *
 * @author Nagatani
 */
public class FXMLDocumentController implements Initializable {

    @FXML
    private Label label;

    @FXML
    private void handleButtonAction(ActionEvent event) {
        System.out.println("You clicked me!");
        label.setText("Hello World!");
    }

    @Override
    public void initialize(URL url, ResourceBundle rb) {
        // TODO
    }    

}
```

FXMLと関連付けるコードの上に、`@FXML`というアノテーションが付けられている。
ボタンクリックのイベントは、`FXMLDocument.fxml`の11行目にあるButtonコントローラの`onAction`属性に含まれる`#handleButtonAction`から、`FXMLDocumentController.java`の26行目以降の`handleButtonAction`メソッドが呼ばれるようになっていることがソースから分かる。

`initialize`メソッドには、`// TODO`コメントが入っているので、画面要素の初期化を行う場合はここに書けということだろう。


最後に、`JavaFXApplication1.java`を見よう。

```java
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

package javafxapplication1;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;

/**
 *
 * @author Nagatani
 */
public class JavaFXApplication1 extends Application {

    @Override
    public void start(Stage stage) throws Exception {
        Parent root = FXMLLoader.load(getClass().getResource("FXMLDocument.fxml"));

        Scene scene = new Scene(root);

        stage.setScene(scene);
        stage.show();
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        launch(args);
    }

}
```

これは単純に画面を呼び出すだけのコードが書かれている。
FXMLの読み込みには、`FXMLLoader`が使われていることが分かる。

ここまで一気に作成してくれるのはありがたいけど、ある程度詳しくなると、空のJavaFXアプリケーションからFXML追加してやった方が良さそうかもしれないので複雑な心境である。

# まとめ
FXMLは、コードとUIデザインを分離できるので、良い。
MicrosoftのWPFと似てるような(そうでもないような)感じなので、意外とすんなり入ってきた。

今回は簡単にプログラマの通過儀礼である世界への挨拶を済ませたので、次回以降はもう少し踏み込んだ内容を書き進めたい。

※次回の記事の内容は未定です。
