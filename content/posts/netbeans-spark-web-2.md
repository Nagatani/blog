---
title: NetBeansでSparkを使ってサクッとWebアプリケーションを作る ②テンプレートエンジンでWebページの出力
date: 2016-06-08 13:36:36
categories:
- Spark
tags:
- Java8
- Spark
- Tutorial
---

今回は、Javaのテンプレートエンジンである[FreeMarker](http://freemarker.org/)を使い、Sparkで作成したWebアプリケーションに、Webページの出力機能を追加する。

***※ 追記：FreeMarkerの綴り間違えてたりしてることに公開後気づいたりした。でもキャプチャとか直すの大変なのでこのままにしておく。***  
（やっぱり深夜にこういう記事書いちゃダメだね）

## SparkでのWebページ出力

SparkのWebアプリケーションでは、受け付けたリクエストに従ってデータをレスポンスすることができる。  
頑張れば、テキストデータを組み上げてHTMLとCSSとJavaScriptを吐き出せる。

例えばこんな感じ

```java
public class HelloWorld {
    public static void main(String[] args) {

        // http://localhost:4567/hello
        get("/hello", (req, res) ->
                "<!DOCTYPE html>\n" +
                "<html lang=\"ja\">\n" +
                "<head>\n" +
                "  <meta charset=\"UTF-8\">\n" +
                "  <meta name=\"viewport\" content=\"width=device-width,initial-scale=1\">\n" +
                "  <title>Spark</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "  <section class=\"main\">\n" +
                "    <h1 class=\"message\">Hello, World!!</h1>\n" +
                "  </section>\n" +
                "</body>\n" +
                "</html>\n" +
                "\n");
    }
}
```

シンプルなWebページであればこれで良いかも知れないが、あまりに非効率すぎて規模が大きくなることを想像するだけでもつらくなる。  
これを解決する方法として、テンプレートエンジンを使用する方法が挙げられる。

テンプレートエンジンについての説明は面倒なのでWikipedia([テンプレートエンジン - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3))でも見ておけば良いだろうと思う。

## Sparkで使用できるテンプレートエンジン

[Spark Frameworkのドキュメント](http://sparkjava.com/documentation.html)を見ると、記事執筆時点で以下のテンプレートエンジンが使用できる。

1. [Velocity - Velocity User Guide](http://www.jajakarta.org/velocity/velocity-1.4/docs-ja/user-guide.html)
2. [FreeMarker Java Template Engine](http://freemarker.org/)
3. [mustache](https://mustache.github.io/)
4. [Handlebars.js: Minimal Templating on Steroids](http://handlebarsjs.com/)
5. [Jade - Template Engine](http://jade-lang.com/)
6. [Thymeleaf](http://www.thymeleaf.org/)
7. [jetbrick-template :: JAVA 模板引擎](http://subchen.github.io/jetbrick-template/)
8. [Pebble](http://www.mitchellbosecke.com/pebble/home)
9. [tiagobento/watertemplate-engine: A lighweight, fast Java 8 template engine](https://github.com/tiagobento/watertemplate-engine)

何を使ってもらっても問題ないと思う。好みに合わせえて選べば良い。が、僕は2番めのFreeMarkerを選択する。  
理由は、[Heroku](https://www.heroku.com)のJavaプロジェクトサンプルが、SparkかつFreeMarkerのテンプレートエンジンで動作してたから、というだけ。  
（Sparkを知ったきっかけも実はこれ）


## テンプレートエンジンの依存関係をPOM.xmlに追記

前回作成したプロジェクトに[Spark Frameworkのドキュメント](http://sparkjava.com/documentation.html)から対応するテンプレートエンジンの`dependency`要素を追記する。

今回は、FreeMarkerを使用するので追記するdependencyは以下となる。

```xml
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-template-freemarker</artifactId>
            <version>2.3</version>
        </dependency>
```

pom.xmlは最終的に、以下のようになる。

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
    <dependencies>
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-core</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-template-freemarker</artifactId>
            <version>2.3</version>
        </dependency>
    </dependencies>
</project>
```

追記が完了したら、プロジェクトをビルドしておくと良い。  
依存関係をダウンロード、再構築してくれるはず。

## テンプレートファイル保存場所の準備

まずは、テンプレートファイルを保存する場所を用意する必要がある。  
テンプレートファイルの保存先は、各テンプレートエンジンが持っている`Configuration`クラスを使うと変更できるが、一旦デフォルトの設定で行う。  
変更方法は後述。

FreeMarkerでは、プロジェクトのソースコードのルートディレクトリが`src`ディレクトリの場合、`main/resources/spark/template/freemarker`のディレクトリを用意する。

![NetBeansのファイルウィンドウを表示して、プロジェクトのsrcディレクトリを選択し、「新規」→「フォルダ」](/image/netbeans-spark-web-2/01.png)

![main/resources/spark/template/freemarkerを作成](/image/netbeans-spark-web-2/02.png)

## テンプレートファイルの新規作成

作成した保存場所に、適当な名前（今回は、[spark-template-engines/spark-template-freemarker GitHub](https://github.com/perwendel/spark-template-engines/tree/master/spark-template-freemarker)に合わせて「hello.ftl」とする）でテンプレート用のファイルを新規作成する。

![](/image/netbeans-spark-web-2/03.png)

![main/resources/spark/template/freemarker以下にhello.ftlファイルを新規作成](/image/netbeans-spark-web-2/04.png)


FreeMarkerのテンプレートファイルの内容は、基本的にHTMLを使用し、プログラムから制御したい部分に関して`${ テンプレート変数名 }`のような記述にする。

今回は、以下の内容を使う。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Spark with Freemarker</title>
</head>
<body>
  <section class="main">
    <h1 class="message">${message}</h1>
  </section>
</body>
</html>
```

## mainメソッドにテンプレートエンジンを使用してWebページを出力する処理を追加

まずは、テンプレートエンジンとその他のimportを追加する。

```java
import java.util.HashMap;
import java.util.Map;
import spark.ModelAndView;
import spark.template.freemarker.FreeMarkerEngine;
```

ここでエラー(警告メッセージではなく赤のアンダーライン)が出てる場合は、Mavenのpom.xmlを変更後ビルドしていない可能性があるので確認する。

続いてmainメソッド内に、以下の処理を追加する。

```java
        // http://localhost:4567/
        get("/", (req, res) -> {
            // テンプレートに渡すデータを作成する
            Map<String, Object> attributes = new HashMap<>();
            // キーとしてテンプレート変数名、値をその後に書く
            attributes.put("message", "Hello, FreeMaker!!");
            // Viewとして、作成した"hello.ftl"を渡す
            return new ModelAndView(attributes, "hello.ftl");
        }, new FreeMarkerEngine());
```

追加した結果、mainメソッドを記述したクラスの全体は以下のようになっているはず。（コメントは除外した）

```java
package com.sample.spark.project;

import java.util.HashMap;
import java.util.Map;
import spark.ModelAndView;
import static spark.Spark.*;
import spark.template.freemarker.FreeMarkerEngine;

public class HelloWorld {

    public static void main(String[] args) {
        // 待受のポート番号を変えることもできる（デフォルトは4567）
        // port(4567);

        // http://localhost:4567/hello
        get("/hello", (req, res) -> "Hello World");

        // http://localhost:4567/
        get("/", (req, res) -> {
            Map<String, Object> attributes = new HashMap<>();
            attributes.put("message", "Hello, FreeMaker!!");
            return new ModelAndView(attributes, "hello.ftl");
        }, new FreeMarkerEngine());

        // http://localhost:4567/stop
        get("/stop", (req, res) -> {
            stop();
            return null;
        });
    }
}
```


## 実行と動作確認
mainメソッドに問題がなさそうなら、[F6]でプロジェクトを実行する。

Webブラウザで`http://localhost:4567/`にアクセスし、以下のように「Hello, FreeMaker!!」のメッセージが出ているか確認する。

![ブラウザで http://localhost:4567/ にアクセス](/image/netbeans-spark-web-2/05.png)

ブラウザでWebページのソースを表示させれば、テンプレートファイルのテンプレート変数が指定された箇所が適切に変更されていることが確認できる。

テンプレートエンジンの動作確認は以上で完了した。

プロジェクトの実行停止（下図）とWebブラウザで`http://localhost:4567/stop`にアクセスしてWebサーバーのサービスを停止する。

![プロジェクトの実行停止](/image/netbeans-spark-web-2/stop.png)


# もっと柔軟にWebページを作成する
これでWebページのHTMLをテンプレート経由で出力させることができた。

ただ、これだけでは通常のWebアプリケーションを作るには足りないものが多すぎる。  
例えばCSSやJavaScriptなどの静的ファイルや、画像などのコンテンツのファイルはどこに配置すれば良いのか。という点がある。この問題をまず解決しよう。

## 公開ディレクトリを設定する

公開ディレクトリを設定することでテンプレートエンジンから出力されたHTMLから、各種ファイルをその公開ディレクトリから読み取ることができるようになる。  
この設定は、テンプレートエンジンとは関係がなく、Spark経由でWebサーバー側の設定を変更する方法である。

まずは、`src/main/resources/`のディレクトリに、`public`ディレクトリを置く。  
（publicじゃなくても良いが、publicがわかりやすいと思うためpublicとする）

![](/image/netbeans-spark-web-2/06.png)

![publicディレクトリを作成](/image/netbeans-spark-web-2/07.png)

publicディレクトリを作成したら、ついでにCSS、JavaScript、画像ファイルを置くディレクトリも作成しておく。

![今回だと、CSSのディレクトリは使うので必ず用意しておく。](/image/netbeans-spark-web-2/08.png)

続いて、mainメソッドにて、Spark経由で公開ディレクトリの設定を行う。

mainメソッドに以下のソースコードを追記する。

```java
// /src/main/resources/public を公開ディレクトリとする
staticFileLocation("/public");
```

**※2016-06-13 追記**
`staticFileLocation("/public");`を入れる箇所は、getメソッドを呼ぶ前にする必要があるので貼り付け位置に注意。


## CSSファイルを設定しテンプレートから出力されたHTMLから参照させる

### CSSファイルの作成

公開ディレクトリとして設定した中（`public/css`）に、`style.css`を追加する。

![](/image/netbeans-spark-web-2/09.png)

![](/image/netbeans-spark-web-2/10.png)

style.cssに以下のスタイルを記述しておく。

```css
@charset "UTF-8";
.message {
    color: #3F51B5;
}
```

### テンプレートファイルにCSSへのリンクを設定する
テンプレートファイルの`hello.ftl`に記述したHTMLの`<head>`要素内に以下の1行を追加する。

```html
<link rel="stylesheet" href="/css/style.css">
```

`hello.ftl`全文は以下のようになる

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Spark with Freemarker</title>
  <link rel="stylesheet" href="/css/style.css">
</head>
<body>
  <section class="main">
    <h1 class="message">${message}</h1>
  </section>
</body>
</html>
```

### 実行と確認
同様に実行、確認すると以下のようにH1タグの文字の色が変わっていることを確認できる。

![](/image/netbeans-spark-web-2/11.png)

### 公開ディレクトリのファイル変更に関して

公開ディレクトリにあるファイルの変更や、テンプレートファイルの変更は、コンパイルが必要なJavaのプログラムとは切り離されて管理されている。  
そのため、Javaのソースコードに変更を加えない限り、NetBeans上のプロセスは実行させたままで問題ない。ファイルを変更、保存したら、ブラウザでページの更新をするとファイルの変更が適用される。

ただし、Javaのソースコードに対して、何らかの変更を加えたら必ずNetBeansの実行プロセスの終了と、`/stop`にアクセスしてWebサーバーのサービスを停止し再実行が必要であることを忘れないようにしておこう。

# テンプレートファイルの保存先の変更と拡張子の変更

FreeMarkerのテンプレートファイルは、現状では`src/main/resources/spark/template/freemarker`と言ったようにちょっと不便な場所に保存されている。  
これはデフォルトの設定がそうなっているだけで、変更が可能である。

## テンプレートファイルの保存先の変更

mainメソッドに以下のimportと、`/`へのアクセス時の処理の変更を行う。

```java
import freemarker.template.Configuration;
```

```java
        // テンプレートファイルの保存先をresources/views以下に設定するための設定情報
        Configuration conf = new Configuration();
        conf.setClassForTemplateLoading(HelloWorld.class, "/views");

        // http://localhost:4567
        get("/", (req, res) -> {
            Map<String, Object> attributes = new HashMap<>();
            attributes.put("message", "Hello, FreeMaker!!");
            // テンプレートファイルの拡張子をftlからhtmlに変更
            return new ModelAndView(attributes, "hello.html");
        // confを設定情報としてFreeMarkerEngineに渡す
        }, new FreeMarkerEngine(conf));
```

ここで、ついでにテンプレートファイルの拡張子をhtmlに変更しておく。  
これは好みの問題かもしれないが、ftlファイルはデフォルトのNetBeansではHTMLファイルとして認識してくれないため、シンタックスハイライトが使えず、作業がしづらい。  
テンプレートファイルの拡張子がftlでなくても、FreeMarkerのテンプレートエンジンは問題なく解析してくれるので、NetBeansでも編集がし易いようにhtmlとしておくことをおすすめしたい。  
（ちなみに、僕はHTMLやCSSの編集にNetBeansはほぼ使わず、Atomなどのテキストエディタに拡張機能積んで作業することが多い）

## 新たにviewsディレクトリとhello.htmlを追加

![](/image/netbeans-spark-web-2/12.png)

![ディレクトリ名はviewsとする](/image/netbeans-spark-web-2/13.png)

![](/image/netbeans-spark-web-2/14.png)

この時、HTMLファイルがコンテキストメニューの選択肢に出てこない場合は「その他」から選べるはず。

![hello.htmlを追加する](/image/netbeans-spark-web-2/15.png)

## hello.htmlの修正

以下のHTMLに差し替えよう。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Spark with Freemarker</title>
  <link rel="stylesheet" href="/css/style.css">
</head>
<body>
  <header>
    <h1 class="page-title">Spark with Freemarker</h1>
  </header>
  <section class="main">
    <h1 class="message">${message}</h1>
  </section>
  <footer>
    <p class="copyright">&copy; ○○○○.</p>
  </footer>
</body>
</html>
```

## style.cssの修正

以下のCSSに差し替える。

余計なアニメーションとか追加されているが、そこは気にしないでおこう。  
それと、css3のflexboxをベンダープレフィックスなしで使用しているため、SafariやIEでは表示が崩れる可能性がある。Chromeを使おう。

```css
@charset "UTF-8";

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-size: 16px;
  font-family: Avenir , "Open Sans" , "Helvetica Neue" , Helvetica , Arial , Verdana , Roboto , "游ゴシック" , "Yu Gothic" , "游ゴシック体" , "YuGothic" , "ヒラギノ角ゴ Pro W3" , "Hiragino Kaku Gothic Pro" , "Meiryo UI" , "メイリオ" , Meiryo , "ＭＳ Ｐゴシック" , "MS PGothic" , sans-serif;
  width: 100%;
  height: 100%;
}
header, footer {
  background: #000;
  color: #fff;
}
header {
  height: 120px;
}
footer {
  height: 60px;
  text-align: center;
}
.main {
  height: calc(100vh - 120px - 60px);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.page-title {
  padding: 30px;
  font-size: 3em;
}
.copyright {
  padding: 1em;
}
.message {
  font-size: 5em;
  position: relative;
  overflow: hidden;
}
.message:before {
  animation: shining-slider 5s ease 1.6s infinite;
  content: "";
  position: absolute;
  top: 0;
  left: -50px;
  width: 100%;
  height: 100%;
  transform: rotate3d(0,0,1,-45deg) translate3d(0,-100%,0);
}
@keyframes shining-slider {
  0% {
    transform: rotate3d(0,0,1,-45deg) translate3d(0,-100%,0);
    background: rgba(255,255,255,0.4);
  }
  10% {
    transform: rotate3d(0,0,1,-15deg) translate3d(0,150%,0);
    background: rgba(255,255,255,0.4);
  }
  100% {
    transform: rotate3d(0,0,1,-15deg) translate3d(0,150%,0);
    background: rgba(255,255,255,0);
  }
}
```

## いままでのテンプレートファイルをフォルダごと削除する

使わないものは消しておこう

![](/image/netbeans-spark-web-2/16.png)


## 実行と確認

もちろん、テンプレートファイルの保存先の変更で、Javaファイルへの変更を行ったので、プロジェクトは再実行しよう。

以下のようにCSSでのスタイルが適用されたWebページが表示できるはず。

![](/image/netbeans-spark-web-2/result.gif)


## mainメソッドを変更してメッセージが変わるところなどを確認しておこう
mainメソッドでテンプレート変数に設定する値を設定しているため、その部分を変えて再実行する。  
文言が変わったかなどを確認しておくことをおすすめしたい。

また、FreeMarkerでは、テンプレートファイルからテンプレートファイルをインポートする機能などが備わっているため、Webシステムの開発はかなり楽に行えるだろう。  
説明が長くなるので割愛するが、便利な機能がたくさんあるので調べてみるとよい。

# ソースコード一式

GitHubにおいておきます。

[Nagatani/netbeans-spark-webapp-sample1](https://github.com/Nagatani/netbeans-spark-webapp-sample1)

# ②はここまで

次回は、WebアプリケーションのAPI周りについて書く予定。  
今回までに作成したプロジェクトはもう使わないと思う。
