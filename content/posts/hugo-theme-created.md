---
title: "Hugoのテーマ作りました。"
date: 2018-05-06T12:54:26+09:00
categories: [ "hugo" ]
tags: [ "hugo", "theme", "sample" ]
draft: false
---

## リポジトリ

- [Nagatani/hugo-memorandum: Memorandum - hugo theme](https://github.com/Nagatani/hugo-memorandum)

## サンプル
このサイトがそれです。

hugo themeのサイトには登録されていません。  
今後、もっとブラッシュアップできたら登録準備するかもしれません。

## 使い方

### リポジトリからコピー

Hugoサイトのディレクトリに以下のコマンドでコピーを作成してください。

```
git clone https://github.com/Nagatani/hugo-memorandum.git themes/Memorandum
```
または、サブモジュールにしても良いです。

```
git submodule add https://github.com/Nagatani/hugo-memorandum.git themes/Memorandum
```

### 設定変更 (`config.toml`)

必要に応じて以下のサンプルから抜粋してください。

```toml
baseURL = "https://your-site.local/"
languageCode = "ja-jp"
title = "サイトタイトル" # アルファベットがおすすめ
theme = "Memorandum"     # 必須
hasCJKLanguage = true    # 日本語なら必須

copyright = "&copy; 2018 YourName" # あったほうが良い
disqusShortname = ""               # DISQUSでユーザー登録、Adminから新規サイトを作ってそのショートネームを設定
googleAnalytics = ""               # google Analyticsの追跡タグ 追跡が要らなかったら行ごと削除
summarylength = 200                # トップページの記事一覧の要約文字数(英語の場合単語数)

[taxonomies]
tag = "tags"
category = "categories"

[params]
description = "メモにメモ以上の価値を求めるのはいけない"  # 必須 サイトの副題
keywords = []   # SEO用キーワード
author = "YourName" # 必須
profile = "自己紹介を書いて" # 必須
displayauthor = false # 記事自体に書いた人を表示するかどうか
DateForm = "2006-01-02" # 日付の書式( https://gohugo.io/functions/dateformat/ )

[params.highlight]
theme = "monokai-sublime" # highlight.jsのテーマ名 必須

[[params.social]]
url = "https://github.com/"   # Githubアカウント
fa_icon_class = "fab fa-github"

[[params.social]]
url = "https://twitter.com/"  # Twitterアカウント
fa_icon_class = "fab fa-twitter"

[[params.social]]
url = "/index.xml"            # RSS
fa_icon_class = "fas fa-rss"
# fa_icon_class は、font awesome 5 free のアイコンクラスを指定します。
# https://fontawesome.com/icons?d=gallery&s=brands&m=free
```

## プロフィール画像の配置

正方形の写真を以下のパスに配置してください。

- `[Hugoインストールディレクトリ]/static/image/profile.jpg`

もし画像形式がjpgじゃない場合などは、`themes/Memorandum/layouts/partials/profile.html`を編集すると良いです。

## カスタムCSS
テーマの中に`custom.css`が入っています。
上書きする場合には、以下のパスに`custom.css`を配置してください。

- `[Hugoインストールディレクトリ]/static/css/custom.css`



# 表示サンプル

Markdownのサンプルは、最後に。

# H1
↑これはH1
## H2
見出しは微妙な文字サイズ変更と余白調整が付いています。
### H3
#### H4
##### H5
###### H6

## 文字関連

*Italic 斜体* では蛍光ペンでマーキングが付きます。  
**strong 強調** 太字。  
***Italic & strong*** 斜体で強調の合わせ技。

## リスト

残念ながら、子リスト、孫リストには対応しておりません。

- 箇条書きリスト1
    + 箇条書き子リスト1
        * 箇条書き孫リスト1
        * 箇条書き孫リスト2
    + 箇条書き子リスト2
- 箇条書きリスト2
- 箇条書きリスト3

番号付きリストは、子孫リスト対応してます。

1. 番号付きリスト1
    1. 番号付き子リスト1
        * 番号付き子リストの中の箇条書き孫リスト1
        * 番号付き子リストの中の箇条書き孫リスト2
    2. 番号付き子リスト2
2. 番号付きリスト2
3. 番号付きリスト3

## 引用(blockquote)

> ことしもまたごいっしょに九億四千万キロメートルの宇宙旅行をいたしましょう。
> これは地球が太陽のまわりを一周する距離です。
> 速度は秒速二十九・七キロメートル。マッハ九十三。安全です。
> 他の乗客たちがごたごたをおこさないよう祈りましょう。  
>  
> 出典： 星新一「思考の麻痺」『きまぐれ博物誌』

## hr線引き

----

## 注釈[^1].
[^1]: 注釈はここに書きます。

Footnoteってやつですね。ページ下部、コメント欄前にまとめられます。


## サンプルMarkdown
```markdown
# H1
↑これはH1
## H2
見出しは微妙な文字サイズ変更と余白調整が付いています。
### H3
#### H4
##### H5
###### H6

## 文字関連

*Italic 斜体* では蛍光ペンでマーキングが付きます。  
**strong 強調** 太字。  
***Italic & strong*** 斜体で強調の合わせ技。

## リスト

残念ながら、子リスト、孫リストには対応しておりません。

- 箇条書きリスト1
    + 箇条書き子リスト1
        * 箇条書き孫リスト1
        * 箇条書き孫リスト2
    + 箇条書き子リスト2
- 箇条書きリスト2
- 箇条書きリスト3

番号付きリストは、子孫リスト対応してます。

1. 番号付きリスト1
    1. 番号付き子リスト1
        * 番号付き子リストの中の箇条書き孫リスト1
        * 番号付き子リストの中の箇条書き孫リスト2
    2. 番号付き子リスト2
2. 番号付きリスト2
3. 番号付きリスト3

## 引用(blockquote)

> ことしもまたごいっしょに九億四千万キロメートルの宇宙旅行をいたしましょう。
> これは地球が太陽のまわりを一周する距離です。
> 速度は秒速二十九・七キロメートル。マッハ九十三。安全です。
> 他の乗客たちがごたごたをおこさないよう祈りましょう。  
>  
> 出典： 星新一「思考の麻痺」『きまぐれ博物誌』

## hr線引き

----

## 注釈[^1].
[^1]: 注釈はここに書きます。

Footnoteってやつですね。ページ下部、コメント欄前にまとめられます。
```
