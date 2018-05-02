---
title: MacのFinederで不可視となっているディレクトリやファイルを表示する方法
date: 2014-10-07 14:40:51
categories:
- Mac
tags:
- Mac
- terminal
---

# ターミナル開いて`ls -a`使え！
嘘です。ごめん。

# `ls -a`使えば見れるけど、Finderでも見たい。

そんなあなたに、以下のコマンドを捧げる。
ターミナル開いて、2行入れるだけ。

```bash
$ defaults write com.apple.finder AppleShowAllFiles true
$ killall Finder
```

※`killall Finder`でFinderが再起動されるので一応注意すること。

# 戻し方も教えとく
1行目のコマンドの最後を`true`から`false`に変えるだけ。

```bash
$ defaults write com.apple.finder AppleShowAllFiles false
$ killall Finder
```

# Finderでシステム関連の大事なファイルやディレクトリも表示される
なので、特に理由がないなら、不可視なものは不可視の方が何かと安全ですね。
用事が済んだら、元に戻しておくのがオススメ。


p.s.
JavaFXの記事を書くといったな、あれはまだ下書きだ！
