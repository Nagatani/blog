---
title: NetBeansでSparkを使ってサクッとWebアプリケーションを作る ③サーバーログの出力
date: 2016-06-13 23:34:08
categories:
- Spark
tags:
- Java8
- Spark
- Tutorial
---

前回までで、一旦Webアプリケーションの作成の基本の基本ぐらいなところまでできている。

WebアプリケーションのAPIの作成について触れていきたいところだけど、その前に、NetBeansで実行時にログの出力を有効にしておこう。

ログの出力は、サーバーとクライアントで一対多の通信を行うWebアプリケーションでは必須の機能と言えるので、今回だけでなく次回以降のプロジェクトも同様にログ出力を有効にしておこう。

## ログの出力を有効にするSLF4Jを依存関係に入れる

Sparkのドキュメントにも書かれているが、[SLF4J](http://www.slf4j.org/)のdependencyを`pom.xml`に追加しておくだけで、実行時のログ出力が有効になる。

pom.xmlに以下のdependencyを追加する。

```xml
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.21</version>
        </dependency>
```

これでログの出力が有効になる。便利。

前回作成したプロジェクトを実行してみると、NetBeansの出力ウィンドウには以下のようにWebサーバーのログが出力されるようになる。

```
------------------------------------------------------------------------
Building spark-project 1.0-SNAPSHOT
------------------------------------------------------------------------

--- exec-maven-plugin:1.2.1:exec (default-cli) @ spark-project ---
[main] INFO spark.staticfiles.StaticFilesConfiguration - StaticResourceHandler configured with folder = /public
[Thread-0] INFO org.eclipse.jetty.util.log - Logging initialized @674ms
[Thread-0] INFO spark.embeddedserver.jetty.EmbeddedJettyServer - == Spark has ignited ...
[Thread-0] INFO spark.embeddedserver.jetty.EmbeddedJettyServer - >> Listening on 0.0.0.0:4567
[Thread-0] INFO org.eclipse.jetty.server.Server - jetty-9.3.6.v20151106
[Thread-0] INFO org.eclipse.jetty.server.ServerConnector - Started ServerConnector@241fede1{HTTP/1.1,[http/1.1]}{0.0.0.0:4567}
[Thread-0] INFO org.eclipse.jetty.server.Server - Started @1048ms
```

第1回のHelloWorldのプロジェクトの出力ウィンドウは、以下の様になっていた。

```
------------------------------------------------------------------------
Building spark-project 1.0-SNAPSHOT
------------------------------------------------------------------------

--- exec-maven-plugin:1.2.1:exec (default-cli) @ spark-project ---
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

そもそもSparkでは、SLF4Jを依存関係に入れることが求められていたようだ……。

### ログ出力の方法

前回までに作成したプロジェクトに追記する形で良いだろう。

SLF4Jを使用してログを出力するには、まず、以下のimportを追加する。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
```

続いて、mainメソッド内を以下のようにすることで、ログが出力される。  
追加した処理は、`Logger`のインスタンス化と、`get("/showip", ...)`の処理なので、その部分だけ貼り付けるのでも良いだろう。

```java
public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);

    // IPアドレスの表示 http://localhost:4567/showip
    get("/showip", (req, res) -> {

        //ログ出力
        logger.info("ログ出力：" + req.ip());

        return "your ip :" + req.ip();
    });

    // http://localhost:4567/stop
    get("/stop", (req, res) -> {
        stop();
        return null;
    });
}
```

`http://localhost:4567/showip`にアクセスすると、Webブラウザでは以下のように表示され、

```
your ip :0:0:0:0:0:0:0:1
```

NetBeansの出力ウィンドウでは以下の1行が追加される。
```
[qtp1938979459-16] INFO com.sample.spark.project.HelloWorld - ログ出力：0:0:0:0:0:0:0:1
```

mainメソッドの8行目にある`logger.info()`でログ出力を行っているため、このメソッドに渡す引数に応じてログが出力される。  
(僕の環境ではIPv6が有効になっているため、localhostへのアクセスは、IPv6の::1でアクセスされる。そのためIPアドレスの表示が上記のようにIPv6表記となる。)
デバッグ時だけでなく、実際に運用する上でも役に立つので、必要に応じて適切なログを出力しておくと良いだろう。


# ③はここまで

今回は、API作成の前にちょっと小休止的な内容としてログ出力機能の有効化を行った。  
次回は今度こそAPI作成に入っていく。
