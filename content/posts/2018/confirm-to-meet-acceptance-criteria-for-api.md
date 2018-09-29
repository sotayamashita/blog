+++
draft = false
type  = "post"
slug  = "confirm-to-meet-acceptance-criteria-for-api"
title = "API の要件を満たせているか確認する方法"
date  = "2018-09-12T15:54:06+09:00"
tags  = ["development-management"]
categories = ["development"]
+++
Jira で定義した API の要件を満たせているかを確認するための方法を調べたときのメモです。
<!--more-->

普段はタスクを Jira に管理してその他の保存期間が長いものはすべて Confluence で管理しています。こういったツールの上に乗せることを前提で考えると OpenAPI を使って API を定義してそれを Confluence のプラグインである [Open API Documentation for Confluence](https://marketplace.atlassian.com/apps/1215176/open-api-documentation-for-confluence) を使用することにしました。その場合の作業の流れは以下のようになっています。（2018/09/29日時点）

### 作業の流れ
1. Jira で要件を定義
1. 要件を満たすための下書きをする
1. [Daily.co](https://www.daily.co/) で同じ画面を共有しながらすり合わせをする
1. それぞれが実装
1. お互いの実装がすんだら疎通確認
1. 完了 :100:

## まとめ
インターフェースを最初に決めてそれを一つの場所で管理してそれぞれが分業していくという風に開発をすることで要件を満たす必要最低限に機能をすばやく実装できるいるという実感があります。はじめからインターフェースを決めるというのは当たり前のようなことかもしれませんが、できるだですり合わせを丁寧に行うことで開発がスムーズになって疎通まで特に問題なく通すことができました。今は API にかかわらず DB の設計などに関してはすべて PlantUML を使って細かいレベルまですり合わせてから実装するようにしています。引き続きこの作業をやりつつよりより丁寧に早くプロダクトを出せるように改善していきそのメモをどんどんブログで書いていこうと思います。
