---
title: "Github Deploy Error"
slug: github-deploy-error
date: 2022-02-22T22:16:16+09:00
categories: ["Hugo"]
with_date: true
tags: ["Github","Hugo"]
---

## エラー内容
GithubでHugoをデプロイしようとすると以下のエラーが起こりました。
```
(一部抜粋)
ERROR 2022/02/22 13:07:50 render of "page" failed: execute of template failed: template: page/search.html:11:19: executing "left-sidebar" at <partial "sidebar/left.html" .>: error calling partial: "/home/runner/work/blog/blog/themes/hugo-theme-stack/layouts/partials/sidebar/left.html:43:58": execute of template failed: template: partials/sidebar/left.html:43:58: executing "partials/sidebar/left.html" at <.Params.newTab>: can't evaluate field Params in type *navigation.MenuEntry
Total in 164 ms
Error: Process completed with exit code 255.
```

## 解決策
HugoのExtendedをビルド時にダウンロードするようにします。以下のようにして、Extendedを有効にしてください。
```yaml
name: github pages

on:
  push:
    branches:
      - main 

jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  
          fetch-depth: 0   

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.92.2'
          extended: true

      - name: Build
        run: hugo --minify


      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          external_repository: kurusunagisa/blog
          publish_branch: main
```

## なぎさの一言
{{<chat face="wink" text="エラーはしっかりとググって解決することが大事です！">}}