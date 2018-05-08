---
title: "CSSで何となくランダムっぽいスタイルを作る"
date: 2018-05-08T08:28:45+09:00
categories: [ "css" ]
tags: [ "css", "tips" ]
draft: false
---

## pure CSSにはrandom()がない

SCSSとかにはある。

何らかの理由で、`random()`関数がある便利なツールが使えない場合にCSSのみでそれっぽく見せる方法を書き残しておく。

### サンプルコード
#### HTML

```html
<div class="random">
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
  <div></div>
</div>
```

#### CSS

雑に必要な部分を取り出す。

```css
.random>div {
  /* ランダム要素の共通スタイル */
  width: 100px;
  height: 100px;
}
/* ↓揺らぎの設定 */
.random>div                { background: #ff7c5c; transform: rotate(2.75deg); }
.random>div:nth-child(2n)  { background: #5cff7c; transform: rotate(-0.5deg); }
.random>div:nth-child(3n)  { background: #7c5cff; transform: rotate(-2.5deg); }
.random>div:nth-child(5n)  { background: #ffce5c; transform: rotate(1.2deg); }
.random>div:nth-child(7n)  { background: #ff5c8e; transform: rotate(5deg); }
```

### やってること
`:nth-child()`擬似クラスを使って、n倍の要素に対してそれぞれ個別のスタイルを書いておく。

これだけで何となくランダムっぽいスタイルが適用できる。

サンプルでは、それっぽくなるように素数を使っているが特に深い意味はない。

疑似ランダムにしたい要素数が多くなる場合は、揺らぎのプロパティを行単位で増やすと、更にそれっぽくなるはず。

### CodePenで動作確認

`.random`要素の中に、100個の`div`要素が入っています。

<p data-height="500" data-theme-id="0" data-slug-hash="JvOZxd" data-default-tab="css,result" data-user="Nagatani" data-embed-version="2" data-pen-title="JvOZxd" class="codepen">See the Pen <a href="https://codepen.io/Nagatani/pen/JvOZxd/">JvOZxd</a> by Nagatani (<a href="https://codepen.io/Nagatani">@Nagatani</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

# まとめ
`:nth-child(それっぽい数n)`で暫定的にCSSのみでランダムっぽい表現をすることができる。

普通に`random()`が使えるやつ使うと良いですよ。
