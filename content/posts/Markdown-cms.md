---
title: Markdownで書けるCMS
date: 2014-09-12 04:00:46
categories:
- CMS
tags:
- Markdown
- CMS
---

## 前置き
CMSについての話です。
いつもは大抵Wordpress使ってるんだけど、なんだかんだ言って更新がめんどいんですよ。
ただ私が面倒くさがりってこともあるんですが……。

講師業で使う資料作成にMarkdownをよく使うようになって、
「Markdownマジ楽。」
からの
「もうMarkdownで記事書けば良くね？」
ってなった次第です。

で、いろいろあって今このブログでHexo使ってます。
だけどこのブログは、ほとんど自分のメモ書きみたいな感じで使っていきたい。

そのいろいろあった時にMarkdownで記事書けるCMSというかブログシステム探した結果報告と言うか見つけたものを雑に紹介しておこうと思ってこれ書きました。
<!-- more -->
# Markdownで記事が書けるCMS
前置きが長くなりましたが、良さそうかなーって思ったものをとりあえず一覧で。

- [Pico](http://picocms.org/)
- [NoShi](http://noshi.cundd.net/)
- [Dropplets](http://dropplets.com/)
- [Singularity](https://github.com/csu/singularity-cms)
- [Hexo](http://hexo.io/)

以降個別に紹介していきます。

## Pico
[Pico - A stupidly simple, blazing fast, flat file CMS.](http://picocms.org/)
![](/image/sc-pico.png)
### 詳細
|項目名|詳細|
|:-|:-|
|動作環境|PHP 5.3以上 Apache:mod_rewrite|
|ライセンス|MIT|

私が管理しているサイトでも一部で使用している。
Markdownファイルをアップロードすると、初回アクセス時にHTMLページが生成される。
ドキュメントが豊富で、プラグインも豊富。
テンプレートエンジンにTwigが使われてて分かりやすい。（個人的な感想）
プラグインの開発も比較的簡単な印象。（これも個人的な感想）

Picoから派生したCoqooなんてのもある。
[Markdown CMS Coqoo ―虚空― | Markdown CMS Coqoo ―虚空―](http://coqoo.net/)
日本人作者なので妙な安心感がある。

ただ、PHPである程度わかって、Picoプラグインの開発ができるのなら、こういった派生のものを使用するより、Picoそのもの使った方が良いってぐらいPicoは簡単かと。
レンタルサーバーで運用する場合は、PHPのバージョンが5.3以上になってることを確認しよう！（自戒を込めて）

## NoShi
[NoShi - The no shi! CMS](http://noshi.cundd.net/)
![](/image/sc-noshi.png)
### 詳細
|項目名|詳細|
|:-|:-|
|動作環境|PHP 5.4以上|
|ライセンス|MIT|

和風っぽいけど日本製ではないのです。
Apacheじゃなくても運用できるので良さ気な印象を受けたが、まだ新しいので、ドキュメントも少なく若干敷居が高く感じた。
サイトのデザインが良さそうな雰囲気だったので紹介しておきます。

## Dropplets
[Dropplets - Welcome to an Easier Way to Blog](http://dropplets.com/)
![](/image/sc-dropplets.png)
### 詳細
|項目名|詳細|
|:-|:-|
|動作環境|PHP (必須バージョン不明)|
|ライセンス|MIT|

使いやすそうなんだけど、ドキュメントが少ないなーって思って使うのを諦めました。
使うの諦めたので、PHPの必須バージョンとか詳細がわからない……。

## Singularity
[Singularity - GitHub:csu/singularity-cms](https://github.com/csu/singularity-cms)
リンク先がGitHubなのでスクリーンショットはなし。
### 詳細
|項目名|詳細|
|:-|:-|
|動作環境|PHP (必須バージョン不明)|
|ライセンス|MIT|

[Strapdown.js](http://strapdownjs.com/)を使って表示のタイミングで都度MarkdownからHTMLに変換するタイプのCMS。
PHPのソース40行しかなく、あとはhtaccessのみで構成されてて非常にシンプルです。
都度Markdownから変換してるので、キャッシュとかそういった観点から中規模から大規模なサイトには使用しづらいかなーって思い、使用を断念しました。
このCMSを知ったおかげでStrapdown.jsも知ることができたので良い。

## Hexo
[Hexo](http://hexo.io/)
![](/image/sc-hexo.png)
### 詳細
|項目名|詳細|
|:-|:-|
|動作環境|Node.js|
|ライセンス|MIT|

このブログがそう。
何が良いって、GitHub Pagesで運用が可能ってところです。
Gitの使い方とかを学ぶついでに良いってところもこれを使用しているポイントですね。
日本語で書かれたブログ記事も豊富なので、インストールにつまずくことは少ないような気がします。

※読み方がわからん。ヘクソでいいの？？

# まとめ
だいぶ雑に紹介した感ある。

Markdown便利だからMarkdownでブログ書くのマジオススメ。
あと、紹介したCMSはどれもデータベースを使用しないファイルベースのものばかりなので、MySQLとかそういう呪縛に囚われない。

個人的に一押しなのがPicoですかね。
カスタマイズしやすいのが一番の理由。

「雑に紹介した」というのを免罪符にしたい気持ちでいっぱいです。
