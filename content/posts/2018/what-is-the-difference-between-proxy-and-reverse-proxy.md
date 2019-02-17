---
draft: true
slug:  "what-is-the-difference-btw-forward-proxy-and-reverse-proxy"
date:  2018-08-31T18:37:04+09:00
title: "フォワードプロキシとリバースプロキシの違い"
categories:
- development
tags:
- difference-btw
- network
isCJKLanguage: true
type: "post"
---

## プロキシとは

まず "proxy" の英語の意味は、他の誰かのために何かをするということです。

まずはじめに "proxy" の英語の意味について説明すると "proxy" とは他の誰かのために何かをするということです。それぞれの違いは


それぞれの違いは他の誰かのために"何"をするかということです。

## プロキシサーバとは

プロキシサーバはフォーワードプロキシサーバともいいます。通常ネットワークの出入り口に置かれユーザー（"client" コンピュータ）の代わりにリクエストをしてレスポンスを受けます。プロキシサーバを使う場合は以下のような登場人物が以下のように流れでつながります：

```
X --> Y --> Z
```

- `X`：ネットワーク内のユーザーもしくは "client" コンピュータ
- `Y`：プロキシサーバ
- `Z`：リクエストをする先のサーバ

### プロキシサーバを使用する理由

- `X` が `Z` に対して直接リクエストを送ることができない
  1. なぜならば、 `X` の管理者が `Z` へのリクエストを禁止しているから
      - 例：
          - URL をフィルタリングして特定のウェブサイトへのリクエストを禁止
  1. なぜならば、 `Z` の管理者が `X` からのリクエストを禁止しているから
      - 例：
          - `Z` が `X` から攻撃をうけて



## 参照

- [Difference between proxy server and reverse proxy server](https://stackoverflow.com/questions/224664/difference-between-proxy-server-and-reverse-proxy-server)
- [プロキシサーバとリバースプロキシサーバの違い](https://itsakura.com/network-proxy)
