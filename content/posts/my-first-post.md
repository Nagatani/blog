---
title: "HexoからHugoへ切り替え"
date: 2018-05-02T14:30:39+09:00
draft: false
---

# ました。

公式サイトの手順に従ってなんやかんやしました。

コンテンツの移動もMarkdownで書いてあるものを一旦移動し、画像などは`static`ディレクトリ以下へ移動した。

## Markdownの変更箇所

記事のカテゴリ、タグの表記は、Hexoで書いていた以下の表記法でそのまま行けた。

```
categories:
- Spark
tags:
- Java8
- Spark
- Tutorial
```

新しい記事を書く時は、Hugoの表記に直す予定だけど、今までのは面倒なのでそのまま。

問題はパーマリンク。  
これはもう仕方ないと諦めた。

今までの記事に該当するHTMLを以下のHTMLで置き換えを行った。

```
<!DOCTYPE html>
<html>
  <head>
    <link rel="canonical" href="https://nagatani.github.io/posts/新しいURL/"/>
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <meta http-equiv="refresh" content="0;url=https://nagatani.github.io/posts/新しいURL/"/>
  </head>
</html>
```

301リダイレクトの設定ってどうするんだろうかと考えたけど、この手法が一番手っ取り早く終わりそうなので、そうしたんだけどこれが一番面倒だった……


# サイトテーマ

hucore 使ってる。

[mgjohansen/hucore: Minimal blog theme for hugo based on Hemingway2](https://github.com/mgjohansen/hucore)

Themeを自製しようと思ったけど、時間の都合で一旦は既存のものを使うことにする。
