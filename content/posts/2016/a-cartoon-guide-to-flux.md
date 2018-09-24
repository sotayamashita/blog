+++
draft = false
type  = "post"
title = "図で説明する Flux"
date  = "2016-01-09T18:27:48+09:00"
tags  = ["translation", "javascript"]
categories = ["development"]
+++
Lin Clark] 氏によって書かれた “A cartoon guide to Flux” の翻訳記事です。ここでは Flux についてわかりやすく図で説明されています。
<!--more-->

> Translation Note 📝<br><br>
> [Lin Clark](https://medium.com/@linclark) 氏が “[A cartoon guide to Flux](https://code-cartoons.com/a-cartoon-guide-to-flux-6157355ab207)” というタイトルでとてもわかりやすく Flux についての記事を投稿していました。この文章のなかでも書かれていますが Flux はとてもホットな話題です。しかし、まだまだ十分に理解されていない技術なので、[著者の Lin Clark 氏に許可を頂いて翻訳させていただきました](https://twitter.com/linclark/status/661157282068197376)。何か間違いや分かりにくいところ、より良くするための助言などありましたら気軽に [@sota0805](https://twitter.com/sota0805) までコメントお願いします。 <br><br>
> （以下、2015年9月29日に Lin Clark 氏によって書かれた “A cartoon guide to Flux” の翻訳です。）

Flux は現在のウェブ開発で最もよく理解されていない話題の１つです。このガイドでは誰もが理解できる方法で説明しようとしています。

## 問題

はじめに Flux が解決する基本的な問題について説明する必要があります。Flux はアプリケーション内のデータを処理するためのパターンです。Flux と React は Facebook で開発されました。多くの人が一緒に使っていますが、それぞれを単独で使うことができます。Facebook が直面していた特定の問題に対処するために開発されました。

<img src="https://cdn-images-1.medium.com/max/800/1*EfeNEshl8-uwZSuUw275Ag.png" class="float-left">

よく知られている問題の例は通知のバグです。Facebook にログインすると、メッセージアイコンの上に通知が表示されているでしょう。メッセージアイコンをクリックすると新しいメッセージは表示されなくなります。そして数分後にサイトでなにか操作をした後、また通知が表示されます。再びメッセージアイコンをクリックしてもまだ新しいメッセージはありません。ユーザーはこの作業を繰り返すことになります。

<img src="https://cdn-images-1.medium.com/max/800/1*4xc1FzIHWiyAvb1iAQKSqQ.png" class="float-left">

これは、サイトでのユーザーだけが繰り返す作業ではなかったのではなく Facebook チームが繰り返していることでもありました。バグを修正してしばらくの間は問題ないですが、またこのバグが発生します。問題を解決してもまた問題が出てきます。

だから Facebook はこの堂々巡りから抜け出すための方法を探していました。一時しのぎの修正を求めていたわけではありません。問題が再浮上しないようにする保証できるようにシステムを予想可能にしたかったのです。

## 根本的な問題

彼らが特定した根本的な問題は、アプリケーション内のデータの流れ方でした。

<i>注意: 彼らのプレゼンのなかで紹介されている簡略化されたものから集めたものです。実際のアーキテクチャとは違うものだと確信しています。</i>

<img src="https://cdn-images-1.medium.com/max/800/1*OcTeAqv8AU_z-O2HuucmeA.png" class="float-left">

データを持ち、データを描画するために View 層にデータを渡す Model があります。

ユーザーの操作は View を介して起こっているので、View は時々ユーザーの入力に基づいて Model を更新する必要がありました。

加えて、ときにこれらのユーザーの操作は他の変更の連鎖を誘発します。I envision this as an edge-of-your-seat game of Pong — ボールが落ちよう（または画面外に落ちるようと）としている場所を知ることは難しいです。

<img src="https://cdn-images-1.medium.com/max/800/1*7myoHOaUyFEmPC-dj61CKw.png" class="float-left">

これらの変更は非同期に発生します。１つの変更は、複数の他の変更を引き起こす可能性があります。ピンポン球が入った袋すべてをあなたがプレイしている最中に投げ込み、いろいろな場所に飛んでいろいろな方向に飛んでいくことを想像してみてください。

すべてのことを考え合わせると、データの流れをデバックするのを難しくしています。

## 解決策: 一方向のデータフロー

だから Facebook はデータの流れが１方向の別の種類のアーキテクチャを試すことを決めました。新しいデータを挿入する必要がある場合、流れはもう一度はじめからを始まります。彼らはこのアーキテクチャを Flux を呼ばれています。

<img src="https://cdn-images-1.medium.com/max/1600/1*lZM0yU9ExEMd7DggVxXkxA.png">

実際にこれはとてもクールです・・・しかしおそらく上記の図からはそれを伝えることができません。

Flux を理解すれば、この図はとてもクールです。問題は、もしあなたがまだ Flux のドキュメントを読んでいないのあれば、先ほどの図は Flux を理解するのに役立たないと思います…まさにこれが図がするべきことです。どのようにそれぞれを使うのかを理解しようとする前にシステムの全体像を理解してもらう必要があります。

私のより Flux の理解を助けたくれたのは上記の図のようなものではないが、異なる登場人物の観点でシステムの考えを一緒に考えると。なので私の頭のなかにある主要登場人物を紹介します。

## 登場人物

彼らが互いにどのように関わるのかを説明する前に、登場人物の簡単な説明をします。

### action creator
最初の登場人物は action creator でこれは、すべての変更と操作や入力とそれに対する反応や出力が通過する必要がある action の作ることを担当します。アプリケーションの状態を変更または異なるものを描画するときはいつでも、action を発火させます。

<img src="https://cdn-images-1.medium.com/max/800/1*DATbW4s4Ls6UPieavtdB7w.png" class="float-left">

action creator を電報のオペレータと考えています。action creator は基本的にあなたが送りたいメッセージのことを知っています。そして action creator は他のシステムが理解できるように整形します。

action creator は type と payload で action 作成します。type はシステムなかで action として定義したものの（通常は定数のリスト）ひとつになります。例えば、action はMESSAGE_CREATE または MESSAGE_READ にようになります。

システムが実行できる action のすべてを知っているという素晴らしい副作用をあります。プロジェクトに新しく参加した開発者は action creator ファイルを開き、システムが提供する状態を変更できるすべての API 全体を見ることができます。

action メッセージを作成した後は、 action creator は action を dispatcher に渡します。

### dispatcher

dispather は基本的には、コールバックの登録するところです。これは電話交換台での電話交換手のようなものです。store に送る必要がる action のリストを保持します。action が action creator からくると、dispatcher はあちこちの store に渡します。

<img src="https://cdn-images-1.medium.com/max/800/1*R5XHVGZfkPkmL7BcN5jMqg.png" class="float-left">

先ほど話したようにマルチボールゲーム影響を与える。そしてstores との間の依存性を構築するする必要がありますので他のものが更新したする前に、waitFor() メソッドで管理する dispatcher をもちます。

Flux の dispatcher は他のアーキテクチャのものとは異なります。action の type が何であろうと限らず action はすべての登録した store に送ることができます。ここで言っていることは、store は いくつかの action を監視するものではありません。すべての action と filter で何を木にして何を気にしないのか。

### Store

次は store についてです。store はアプリケーション内のすべての状態を保持し、すべての状態を変更するロジックは store 内部に残ります。

<img src="https://cdn-images-1.medium.com/max/800/1*17MbK8jz94ynP-NFfL0rfw.png" class="float-left">

store を監督者と考えています。すべての変更は別々に行われます。そして、状態を変更を直接リクエストはできません。store には setter メソッドはありません。状態の変更を要求するには、適切な手順に従わなければなりません。つまり action creator/dispatcher を経由して action を提出します。

前述したように、もし store が dispatcher と登録された場合、すべての action は store に送られます。 store の中では状態をスウィッチするものがってそれが action の type を見て store がこの action のことを気にするかどうかを決めます。もし store が action を気にすると、なんの変更が必要かどうかを判断して必要な action と 状態の更新を判断します

store は状態の変更を行った後は、変更イベントを発火します。状態が変更されたことを controller view に通知します。

### Controller view と view

view の役割は状態を持ち、それをユーザーからの入力を受け取ってユーザーに描画することです。

<img src="https://cdn-images-1.medium.com/max/800/1*MY5xNk_JeKvGsGdywYD4EA.png" class="float-left">

view はプレゼンターです。アプリケーションのことを気にする必要はなく、ただ渡されたデータをのこととどのように人々が分かるようにデータをどのように整えるのかを知っていれば大丈夫です。

Controller view は view と store の間の中間管理職のような存在です。store は状態が変更したことを controller view に伝えます。 controller view は新しい状態を集めて行進された状態を view に対して渡します。

## それぞれがどのように一緒に働くのか？
それではどのようにすべての登場人物が一緒に働くのか見てみましょう。

### セットアップ
はじめにセットアップがあります、つまり一度だけ発生するアプリケーションの初期化です。

1. store は action がくるたびに dispatcher に知らせてもらいます。

<img src="https://cdn-images-1.medium.com/max/1600/1*GHrusKRFhQ0Y6rrwjqI6QQ.png">

2. 次に controller views は store に最新の状態を尋ねます。

3. store は controller view に状態を渡したとき view controller は表示するために状態をその子 view に渡します。

<img src="https://cdn-images-1.medium.com/max/1600/1*66hoDpUhczSXrgH2cUUasw.png">

4. controller view もまた store に状態が変化した際に通知をしてもらうように頼みます。

<img src="https://cdn-images-1.medium.com/max/1600/1*tV7VN0RjpFfwaf1PK3chIA.png">

### データの流れ

セットアップが済んだら、アプリケーションがユーザーからの操作を受けけつける準備ができています。それではユーザーが変化を起こすことで、action を発火させてみましょう。

ユーザーの操作からデータの流れをスタートします。

<img src="https://cdn-images-1.medium.com/max/1600/1*SusQ7Aip2fSWg6raQtPSnA.png">

1. view は action creator に action の準備を頼みます。

<img src="https://cdn-images-1.medium.com/max/1600/1*dkm9qsWuD9DtXzH-u-DjJQ.png">

2. action creator は action を整えて dispatcher に送ります

<img src="https://cdn-images-1.medium.com/max/1600/1*fJwvtpq0XQhB4mUZwh7YOQ.png">

3. dispatcher は action を store に順番に送ります。それぞれの store はすべてのアクションの通知をもらいます。次に store がその actions を気にするかとうか決定し、それに応じて状態を変更します。

<img src="https://cdn-images-1.medium.com/max/1600/1*RLrImTDeArSMoA4kZsajLQ.png">

4. 状態を変更したら、 store はそれを監視している view controller に知らせます。

5. これらの view controller はその後、store に更新された状態を渡すように依頼します

<img src="https://cdn-images-1.medium.com/max/1600/1*5SXO2eftdQveFqImGDVB9A.png">

6. store が 状態を controller view に渡した後、view controller は新しい状態に基づいて再描画するために、その子 view に伝えます。

<img src="https://cdn-images-1.medium.com/max/1600/1*fWBaUg9-_1-V5M2YQBWhWg.png">

これが私が考える Flux です。参考になれば幸いです！

## 次の記事

[A cartoon intro to Redux — Code Cartoons](https://code-cartoons.com/a-cartoon-intro-to-redux-3afb775501a6)

## リソース

- [Flux ドキュメント](https://facebook.github.io/flux/docs/overview.html)
- [Fluxxor ドキュメント](http://fluxxor.com/what-is-flux.html)
- [Flux の利用例](https://medium.com/swlh/the-case-for-flux-379b7d1982c6)

<i>[Kent C. Dodds](https://twitter.com/kentcdodds)、[Matt Zabriskie](https://twitter.com/mzabriskie) と [Christopher Chedeau](https://twitter.com/Vjeux) のフィードバックと [BrooklynJS](https://twitter.com/brooklyn_js) のすべての人たちのコメントに感謝しています。</i>
