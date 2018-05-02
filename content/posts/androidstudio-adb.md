---
title: Android Studioを入れたMacBook Airでbashからadbコマンドを使えるようにする
date: 2014-09-14 18:39:30
categories:
- Andriod
tags:
- AndroidStudio
- adb
---

なんやかんやあれやこれやでadbコマンド使わないといけなくなったので、その手順をメモしておく。
母艦のWindowsマシンには、Android SDKを手動セットアップしてたのでそれ使えば良いだけの話なんだけど、たまたま手元にあるのはMacBook Airだったので、adbコマンド使えるようにしとこう、というわけです。

前提として、Android Studioはインストール済み。

## Android SDKを別途入れなくても、Android StudioにはSDKが含まれてる
ので、それのPATHを通してやれば良いわけです。

## Android Studioに含まれるSDKの場所
環境によって異なるかもしれないので注意が必要。
僕のMacBook AirにインストールしたAndroid Studioの構成では以下のパス以下にSDKが含まれていた。

```
/Applications/Android\ Studio.app/sdk/
```

目的のadb本体は、`sdk`ディレクトリ以下の、`platform-tools`にある。


## adbコマンドを使用できるように、PATHを通す

ターミナルに使うのが何かで設定が異なる。
僕のMacBook Airは初期設定のままのbashを使ってるので、以下のようにする。

### bashの場合

```
~/.bash_profile
```

に以下の一行を追加する。

```
export PATH=$PATH:/Applications/Android\ Studio.app/sdk/platform-tools
```

エディタはお好きにどうぞ。

### 普段あんま使わないけど、zshの場合は……

```
~/.zshrc
```

に以下同文。

## adbが動作するか確認する。

設定を読み込みし直さないといけないので、ターミナル再起動。

```bash
$ adb help
```
とかでヘルプが出て来ればオッケーなんじゃないですかね。

僕からは以上です。
