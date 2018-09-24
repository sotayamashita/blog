+++
draft = false
type  = "post"
title = "Node.js リリースフローについて"
date  = "2017-04-27T22:15:44+09:00"
tags  = ["translation", "nodejs"]
categories = ["development"]
+++
Rod Vagg 氏によって書かれた "Essential Steps: Long Term Support for Node.js" の翻訳記事です。ここでは Node.js のリリースフローについて詳しく書かれています。
<!--more-->

> Translation Note 📝<br><br>
> [Rod Vagg](Essential Steps: Long Term Support for Node.js) 氏が “[Essential Steps: Long Term Support for Node.js](Essential Steps: Long Term Support for Node.js)” というタイトルで Node.js の今後のリリーススケジュールについて説明していました。Node.js を現在本番環境で使っている人や今後使ってみたいと考えてる人ならそのリリーススケジュールを理解しておくことは重要だと思うので、[著者の Rod Vagg 氏に許可を頂いて翻訳させて頂きました。](https://twitter.com/rvagg/status/825644128071397376)何か間違いや分かりにくいところ、より良くするための助言などありましたら気軽に [@sota0805](https://twitter.com/sota0805) までコメントお願いします。翻訳の際にいつも相談にのってくださる **Node.js 日本ユーザグループ** の方々に感謝しています。😁 <br><br>
> （以下、2015年06月24日に Rod Vagg 氏によって書かれた “Essential Steps: Long Term Support for Node.js” の翻訳です。）

---

まずはじめに 2015 年始めに Node.js LTS ワーキンググループを立ち上げられたことを誇りに思っています。何回かの試行錯誤のあと Node.js の過去および将来のリリースを考慮した長期サポート（LTS）戦略を正式化しました。このような戦略を採用することは、エンタープライズの顧客や他の専門機関で広く採用されているオープンソースプロジェクトにとって不可欠なステップです。

Node.js の LTS 計画を確立するのは 、明確に定義された拡張性のあるライフサイクルを持つ予測可能なスケジュールで、新しいバージョンを提供することによって既存の安定したリリースサイクル上に構築するためです。これはオープンソースソース「すばやく、頻繁にリリース」というの伝統を考えると奇妙なことに思うかもしれませんが、エンタープライズアプリケーションの開発および運用チームにとって必要不可欠なものです。また私達のような Node.js の専門的なサポートを提供する企業にも影響を与えます。（詳細は [N | Support annoucement](https://nodesource.com/blog/nodesource-announces-nsupport) を御覧ください。）

継続可能な LTS 戦略を作成するには、いくつかの重要なことを考える必要があります。LTS はどのコードをベースにしているのか？いつ、どれくらいの頻度で新しい LTS ブランチを作るのか？そして、それはどれくらいの期間サポートされるのか？どのようなサポートなのか？どのような変更が LTS ブランチのサポート期間に発生するのか？リリースが誰によって管理されるのか？

Node.js LTS ワーキンググループはすでに先程の質問のほとんどに対する答えをもっています、そして LTS のリリースはコミュニティーやユーザーからのフィードバックを受けて戦略を改善し続けています。

変化が激しいいくつかの詳細についてはもう少しまってください。しかし、現在のプランは以下のとおりです：

## 何をリリースするのか？

最初の LTS リリースは、新しく統合された Node.js コードベースです。このコードは、io.js には現在含まれてはいませんが、Node.js v0.12 に含まれている追加機能を加えた最新の io.js がコードベースになっています。これらのうち最大のものは Intl オブジェクトの適切なサポートです。

統合作業の結果作られる Node.js の新しいバージョンは、その時点でもっとも大きい io.js のメジャーバージョンを上回るものになります。現在の計画では 8 月に新しいリリースが予定されており、io.js で v3 が出る可能性が高いため、次のバージョン（正式に Node.js と呼ばれるもの）はおそらく Node.js v4.0.0 になります。

統合の作業と、時には ios.js と Node.js v0.12 との間に大きな違いがないという事実のために、既存のアプリケーションを実行するために必要な修正はほとんどありません。最大の違いは、 v8 がJavaScript を最適化する方法の変化によるパフォーマンス・プロファイルの変更と、Node および V8 のネイティブ・アドオン・レイヤの両方で、パフォーマンス・プロファイルの変化です。アドオンの互換性に関する話は、アップグレードの際に問題なる可能性がありますが、別の投稿のために残しておきます。

## いつリリースするのか？

統合作業が終わり新しい Node.js が主流になると現在の計画では 10 月を最初の LTS リリースとし、毎年 10 月を LTS リリースにする予定です

バージョニングについてはセマンティック・バージョンに従っているのでどちらのバージョンとも意図的な破壊的変更がない限りはメジャーバージョンではなくマイナーバージョンのみ増加します。メジャーバージョンは定期的な安定版のリリースの際に増加します。しかし最初の LTS リリースより前にメジャーバージョンが増えることはありません。

10月になると LTS ワーキンググループが安定版リリースブランチを引き継いで現在のメジャーバージョン番号でのリリースは正式に LTS リリースになります。

たとえば、8月以降に Node.js v4 から十分な作業がリリースされた場合、LTS ワーキンググループが引き継ぐ最初の LTS リリースは v4.1.4 になります。最初の LTS リリースの間、常にそのメジャーバージョン番号 4 です。パッチバージョン番号が増えていく可能性が最も高くなります：v4.1.5、v4.1.6など（下記参照）。

面白い事実： LTS のリリースにはコードネームが付けられており差別化をさらに助けます。ニュースを待ってください。

## どれくらいの頻度でリリースするのか？

LTS のメジャーバージョンは12ヶ月に1度リリースされ、その後何度かアップデートリリースが行われます。ほとんどはパッチバージョンのアップデートですが、バグ修正のためにどうしても必要な場合はマイナーバージョンがアップデートされることもあります。

新しい主要な LTS バージョンのスケジュールは12ヶ月のサイクルに固定されますが、これらの各リリースにおける増分リリースのスケジュールは、バグ修正、セキュリティ修正、その他の重要ではない小さな変更などによって決まります。安定性に重点が置いています。ここでいう安定性とは既知のバグの数を最小限に抑え、セキュリティ上の問題が発生したときにそれを維持することも含まれます。

## どれくらいの期間サポートされるのか？

新しい LTS は 12 ヶ月ごとにリリースされますが、これらのメジャーバージョンはそれぞれ 18 ヶ月サポートされます。さらに 12 ヶ月間このブランチはメンテナンスとなります。ここでは深刻なバグやセキュリティ上の問題にのみが解決されます。LTS とメンテナンスの違いは、修正の重要度にあります。

これは、明らかに、任意の時点で、複数のアクティブな LTS およびメンテナンスラインが存在することを意味します。こうすることで綿密な管理が必要な複雑な環境にスムーズな移行パスと十分な埋め込み時間を可能にします。

Node.js v0.10 と v0.12 は LTS 計画の特殊なケースです。 v0.10 は10月にメンテナンスに移行し、12ヶ月間続きます。v0.12 は 10月から6ヶ月の初期LTS 期間とそれに続く12ヶ月間のメンテナンス期間があります。 io.js リリースには正式な LTS やメンテナンスのサポートはありません。

![](https://cdn-images-1.medium.com/max/800/1*nbwuU0fQJrlTnuTidLho5Q.png)

もちろん LTS ワーキンググループの傑出した課題の1つは、Chromiumチームによって長い間忘れられていた V8 のバージョンをサポートすることです。これは簡単な作業ではなく、安定性とセキュリティを確保するために細心の注意を払う方法を見つける必要がありますが、我々は挑戦しています！

## 誰がメンテナンスしているのか？

Node.js LTS ワーキンググループには、IBM、NodeSource、Joyent、StrongLoop からの代表が含まれています。このグループは、時間の経過と共に進化し実際のサービスで長期的につかっていく Node.js のユーザにサービスを提供することに共通の信念をもっていいます。

## 重要なステップ

NodeSource での私たちの使命はエンタープライズ向けの Node.js とNode.js のためのエンタープライズ環境を準備することです。

明確な移行パスと健全なサポートメカニズムを備えた予測可能なリリースサイクルを作成することがこのミッションの鍵です。だからこそ、Node.js の LTS プロセスをサポートすることは NodeSoure, 特に N|Support チームにとっての優先事項なのです。

大規模な本番環境での運用や急成長中のアプリケションプラットフォーム、世界最大のパッケージエコシステムなどの Node.js の主要なマイルストーンを考えると予測可能なリリース及びサポートをする仕組みが不可欠です。

[LTS プラン](https://github.com/nodejs/LTS/#lts-plan.)をお読みください。現在の LTS 戦略がどのように組織に影響を与えるかについてのフィードバックするには、LTSリポジトリに issue を作成するか、直接私に連絡してください：[rvagg@nodesource.com](https://github.com/nodejs/LTS/#lts-plan.)。