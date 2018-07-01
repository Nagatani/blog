---
title: "TomlデータをJSONに変換する"
date: 2018-06-30T23:00:00+09:00
tags: [ "nodejs" ]
draft: false
---

作りました。

- [Nagatani/toml2json: TomlファイルをJSONファイルに変換します。](https://github.com/Nagatani/toml2json)

なんで作ったかと言うと、JSONデータでの設定ファイルの場合、コメントが残せないという愚痴を聞いて、tomlで設定ファイル書いてデプロイスクリプトのにJSONデータに変換すれば良くないか？と普通に疑問に思ったからですね。

で、何してるかというと、npmの[toml](https://www.npmjs.com/package/toml) 使うと、tomlファイルをJavaScriptのオブジェクトにパースできるので、それを`JSON.stringify`すれば、はい、JSONファイルの出来上がり。という簡単なやつです。

詳しくはGitHub見てね。

あと全然関係ないけど、WebStormのライセンス買いました。

- [WebStorm: The Smartest JavaScript IDE by JetBrains](https://www.jetbrains.com/webstorm/)

Node.jsだけじゃなく、React,Vueのコーディングサポートがあるので便利。

私からは以上です。
