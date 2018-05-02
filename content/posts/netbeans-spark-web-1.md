---
title: NetBeansでSparkを使ってサクッとWebアプリケーションを作る①Hello,World!!
date: 2016-06-01T17:03:14+09:00
categories:
- Spark
tags:
- Java8
- Spark
- Tutorial
---

※この記事は、以下のブログに投稿したものと同じです。

[NetBeansでSparkを使ってサクッとWebアプリケーションを作る①Hello,World!! - 水に溶けるドキュメント](http://nagatani.hatenablog.jp/entry/2016/06/01/NetBeans%E3%81%A7Spark%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%82%B5%E3%82%AF%E3%83%83%E3%81%A8Web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E4%BD%9C%E3%82%8B)

書いてから数日後、そういえばこの技術関連のブログ放置してたわ…ってなったので、こっちに移動しました……。

# 本題

今回から、何回かに分けてJava8でかつSparkを使ってWebアプリケーションをサクッと作る方法について書く。  
(これもオープンソースへの貢献ということにしたい。)

今回は、その準備編となるHello, World!!から行う。  
なんでNetBeansを使うのか、という点は説明が面倒なのでしない。  
内容は主に以下の内容に沿って行っているため、他にも似たようなことを書いている記事もあることだろうと思うが敢えて書く。

- [Spark Framework - Documentation](http://sparkjava.com/documentation.html)


## Spark Frameworkについて

- [Spark Framework - A tiny Java web framework](http://sparkjava.com/)

![](/image/netbeans-spark-web-1/spark.png)

Apache Sparkではないのでググラビリティ低いから注意が必要。  
Spark Frameworkは、JavaでサクッとちょっとしたWebアプリケーションを作るのに便利なマイクロフレームワーク。

## 事前準備

- NetBeans
    + [NetBeans IDE ダウンロード](https://netbeans.org/downloads/?pagelang=ja)

インストールしておく。

## Mavenプロジェクトの作成

Gradle使いたい気持ちもあるけど、SparkのサイトでもMavenで書いてあるからここはMavenで行こうと思う。

#### NetBeansで新規プロジェクトを作成
「Maven」の「Javaアプリケーション」で良い。

![](/image/netbeans-spark-web-1/01.png)

ここで、違うのにするといろいろおかしくなる。

続いて、プロジェクトの詳細を設定する。  
プロジェクト名とかは好きなの設定すればいいと思う。  
自分のドメインを持っている場合は、com.sampleとか変えたほうがいいんじゃないかな。

![](/image/netbeans-spark-web-1/02.png)

## pom.xmlを編集してSparkをプロジェクトに追加

Mavenなのでpom.xmlを編集して依存関係を追加する。
pom.xmlはプロジェクトウィンドウの以下のところにある。

![](/image/netbeans-spark-web-1/03.png)

pom.xmlは以下のようになっている。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.sample</groupId>
    <artifactId>spark-project</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
</project>
```

ここから、[Sparkのドキュメント](http://sparkjava.com/documentation.html)にあるように以下の依存関係を追加する訳だけど、Sparkのドキュメントでは`dependency`のノードしか書かれていない。

```xml
<dependency>
    <groupId>com.sparkjava</groupId>
    <artifactId>spark-core</artifactId>
    <version>2.5</version>
</dependency>
```

NetBeansでプロジェクト作った状態だと、これを入れる`dependencies`のノード自体がないので以下のように`dependencies`ノードから追加する必要がある。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    modelVersionとかなんかいろいろある


    <dependencies>
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-core</artifactId>
            <version>2.5</version>
        </dependency>
    </dependencies>
</project>
```

## 一度ビルドする

ショートカットの`F11`キーか、メニューバーの「実行」→「プロジェクトをビルド」で良い。

![](/image/netbeans-spark-web-1/04.png)

これでpom.xmlのdependenciesにあるライブラリのjarを依存関係込みで、手っ取り早くダウンロードしてきてくれる。

## クラスを追加する

プロジェクト作成時に作成されているpackage内に、新規クラスを追加する。

![](/image/netbeans-spark-web-1/05.png)

クラス名はSparkのサイトに合わせて`HelloWorld`にしておく。

![](/image/netbeans-spark-web-1/06.png)

#### importで`spark.Spark.*`を追加する

```java
import static spark.Spark.*;
```

`static`インポートする点に注意。

#### mainメソッドを追加する

追加した`HelloWorld`クラスに、以下のmainメソッドを入れる。

```java
    public static void main(String[] args) {
        // 待受のポート番号を変えることもできる（デフォルトは4567）
        // port(4567);

        // http://localhost:4567/hello
        get("/hello", (req, res) -> "Hello World");

        // http://localhost:4567/stop
        get("/stop", (req, res) -> {
            stop();
            return null;
        });
    }
```

Sparkのドキュメントと違う点は、Sparkが動作するJettyのWebサーバーを停止するstopの受付を増やす必要がある点。  (理由は後述)


以上で、SparkのHelloWorldは作成完了。  
あとは起動して動作確認しよう。

## 起動

`F6`キーかメニューバーの「実行」から「プロジェクトの実行」で起動できる。  
メインクラスの指定は、HelloWorldを選択する。

![](/image/netbeans-spark-web-1/07.png)


実行後、出力ウィンドウでエラーが出てないことを確認する。  

![](/image/netbeans-spark-web-1/08.png)

エラーが出てると、何かしらどこかで間違えてるのでもう一度よく確認する。  
エラーが出てなければ、今度はブラウザを立ち上げて動作確認しよう。

## ブラウザからアクセス

`http://localhost:4567/hello`にアクセスしてみると、以下のような結果が得られる。

![](/image/netbeans-spark-web-1/09.png)


これでひとまずSparkの世界に挨拶を済ますことができた。

動作の確認が終了したら、NetBeansで実行を停止することに加えて、以下のURLにアクセスし、Jettyのプロセスも終了させる。

`http://localhost:4567/stop`

NetBeansでは実行を止めるだけではJettyのプロセスが停止してくれないため、必ず`/stop`へアクセスする必要がある。  
これをやらないと、プロジェクトを再実行したときにポートが使用中で実行ができない。  
ただし、`/stop`は実際にWebアプリケーションとして公開する場合に残しておいてはいけないものであることを忘れないように。
外部からプロセス停止させられたら溜まったもんじゃない。


## ①はここまで

次回は近日中に、テンプレートエンジンを使ってHTMLを出力する方法を紹介する予定。
