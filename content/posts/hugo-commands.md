---
title: "Hugoアップデートといつも使うコマンド"
date: 2018-07-25T04:12:08+09:00
tags: [ "hugo" ]
draft: false
---

# いつも忘れるので書いとく
この投稿は、完全にメモです。

## Hugoバージョンアップ

```shell
$ brew update
$ brew upgrade hugo
```

## 新規投稿作成

```shell
$ hugo new posts/new-post.md
```

## ローカルサーバ立ち上げ

```shell
$ hugo server -D
```

## デプロイ

```shell
$ ./deploy.sh
```

`deploy.sh`の中身は↓

```sh
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo -t Memorandum # if using a theme, replace with `hugo -t <YOURTHEME>`

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```
