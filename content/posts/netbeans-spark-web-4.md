---
title: NetBeansでSparkを使ってサクッとWebアプリケーションを作る ④JSONを返すWeb-APIの作成
date: 2016-06-23 01:42:53
categories:
- Spark
tags:
- Java8
- Spark
- Tutorial
---

## 今回やることのソースコード

GitHubに置いておきます。

[Nagatani/netbeans-spark-webapp-sample2](https://github.com/Nagatani/netbeans-spark-webapp-sample2)

## JSONを返すWeb APIの作成

最終的には、MySQLからデータを取得してJSON形式で返すAPIと、JavaScriptでJSONデータを取得して画面に表示するプログラムが完成する。

やることはそれほど難しいことでなく、単純にJSON変換ライブラリであるGoogleのgsonを使用してJavaのインスタンスをJSON形式に変換し、それをHTTPのレスポンスで返すだけである。

- [google/gson: A Java serialization/deserialization library that can convert Java Objects into JSON and back.](https://github.com/google/gson)

gsonは、Mavenリポジトリに登録されているので、`pom.xml`の`dependencies`に以下のdependencyを追加する。

```xml
        <!-- 以下はJSON形式への変換ライブラリとして使用します。 -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.6.2</version>
        </dependency>
```

### JSONに変換して返すクラスの作成

今回は、仮にユーザーの一覧を返す仕組みを作成していく。

ユーザーデータには、ユーザーID(id)、ユーザー名(name)、メールアドレス(email)を持たせる。

ユーザーデータのクラスは以下（便宜上、パッケージをmodelsにしている。）

```java
package me.nagatani.dev.spark.webapp.sample2.models;

public class User {

    private int id;
    private String name;
    private String email;

    public User() {
    }

    public User(int id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

## SparkでJSONを返す

作成したUserクラスのインスタンスをListに格納して、JSONに変換して返してみる。

今回からSparkのメインクラスは、`App.java`にしている。

```java
package me.nagatani.dev.spark.webapp.sample2;

import com.google.gson.Gson;
import java.util.ArrayList;
import java.util.List;
import me.nagatani.dev.spark.webapp.sample2.models.User;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import static spark.Spark.*;

/**
 *
 * @author Nagatani
 */
public class App {

    public static void main(String[] args) {

        // Gsonを使うための準備
        Gson gson = new Gson();

        // ログ出力機能の初期化
        Logger logger = LoggerFactory.getLogger(App.class);

        // /src/main/resources/public を公開ディレクトリとする
        staticFileLocation("/public");

        // http://localhost:4567/users
        get("/users", (req, res) -> {

            // Userのリストを作成して返す
            List<User> users = new ArrayList<>();
            users.add(new User(1, "name1", "name1@sample.com"));
            users.add(new User(1, "name2", "name2@sample.com"));
            users.add(new User(1, "name3", "name3@sample.com"));
            return users;

        }, gson::toJson);
        // ↑Gson.toJsonメソッドをポインタで渡す

        // NetBeans上で実行する際にサーバーの停止に必要
        // http://localhost:4567/stop
        get("/stop", (req, res) -> {
            stop();
            return null;
        });
    }
}
```

以下の部分が、JSONを返すのに必要な部分である。

```java
// Gsonを使うための準備
Gson gson = new Gson();

// ...

// http://localhost:4567/users
get("/users", (req, res) -> {

    // Userのリストを作成して返す
    List<User> users = new ArrayList<>();
    users.add(new User(1, "name1", "name1@sample.com"));
    users.add(new User(1, "name2", "name2@sample.com"));
    users.add(new User(1, "name3", "name3@sample.com"));
    return users;

}, gson::toJson);
// ↑Gson.toJsonメソッドをポインタで渡す
```

***ごめんこれまだ書いてる途中だったわ……***  
更新予定なしなので、このまま公開しときます。
