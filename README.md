AMP
===

Googleが中心となって開発・実装されているAMP（Accelerated Mobile Pages）プロジェクトは2016年の2月に正式公開されました。既にウェブページやアプリでは利用できるようになっており、その表示速度に驚いた人も多いのではないでしょうか？


### 成り立ちの背景（0.5P）

後に詳しく述べますが、AMPは高速化を目的としています。しかしその背景は少し違います。通常のウェブページはその構成の自由さから、いくらでも遅く作れてしまうことを強く問題視しているようです。つまりAMPに従って作成し、AMPに従って利用すればどうやっても*遅くならない*というデザインです。


### 制作する人・利用する人（0.5P）

当初はGoogle謹製のサービスやアプリ、そして当初から開発に協力していたTwitterのサービスでのみ利用されるだけでしたが、[Bingがアプリで利用を開始する][1]などそのメリットに惹かれる第三者も増えつつあります。国内でもはてなブックマーク・アプリを始め、利用しているアプリは散見されます。AMPを利用し始める彼らは、いったいAMPのどこに惹かれ、そしてなぜ開発リソースを割いてまで通常のウェブページよりも優先するのでしょうか。


[1]: http://blogs.bing.com/search/September-2016/bing-app-joins-the-amp-open-source-effort


<!-- #toc -->

* [AMPの目的](#amp%E3%81%AE%E7%9B%AE%E7%9A%84)
* [AMP HTMLとHTMLとの違い](#amp-html%E3%81%A8html%E3%81%A8%E3%81%AE%E9%81%95%E3%81%84)
* [パフォーマンスとセキュリティーにおけるメリット](#%E3%83%91%E3%83%95%E3%82%A9%E3%83%BC%E3%83%9E%E3%83%B3%E3%82%B9%E3%81%A8%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%83%BC%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88)
* [Googleへの依存とプライバシーにおけるデメリット](#google%E3%81%B8%E3%81%AE%E4%BE%9D%E5%AD%98%E3%81%A8%E3%83%97%E3%83%A9%E3%82%A4%E3%83%90%E3%82%B7%E3%83%BC%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E3%83%87%E3%83%A1%E3%83%AA%E3%83%83%E3%83%88)
* [さいごに](#%E3%81%95%E3%81%84%E3%81%94%E3%81%AB)

<!-- /toc -->


AMPの目的
---------

プロジェクトのウェブサイトでは「Instant. Everywhere.」というキャッチコピーが使われています。AMPの目的はこれに尽き、つまりウェブページが「どこでも。即座に。」表示されるようにすることです。


### 高速化の必要性（0.5P）

ウェブページが高速であることは多くの意味を持ちます。特にモバイル環境では常に回線品質とマシンパワーの問題に晒されているため、できうる限り高速化するニーズは大きいでしょう。HTMLのパフォーマンスを上げるためには様々な手法が考案されてきました。それらは`script`要素の`async`属性のように実装だけでなく仕様に取り込まれたため、10年前と比較するとそれなりに環境は整ってきたと言えるでしょう。しかし実装や仕様の進歩速度にもやはり限界はあります。また増え続ける高速化手法を適切に取捨選択することが難しいことも言うまでもないでしょう。


### 「速い」と「即座」の違い（0.5P）

速い（fast）ことと即座（instant）であることは微妙な違いがあります。たとえウェブページの読み込みが速くても、その表示が即座ではないと、ユーザーの体感としては「遅い」ものとなるでしょう。AMPでは即座に表示されるために様々な工夫を行っています。


AMP HTMLとHTMLとの違い
----------------------

AMPは以下の3つからなるプロジェクトです。

1. HTMLからパフォーマンスを下げる要因を排除した*AMP HTML*
2. AMP HTMLを処理しレンダリングを高速化する*AMP JS*
3. AMP HTMLを高速に配信する*AMP Cache*

AMP JSとAMP Cacheはオープンソースで開発されたものを一方的に利用するだけで、私たちが主に扱うのはAMP HTMLです。


### 要素の制限 (1.5P)

AMP HTMLはHTMLのサブセットというと近いでしょう。その違いは利用できる要素にあります。既に述べたように`script`要素は原則としてその利用を禁止されています。また`img`や`video`要素といった次節で述べる[カスタム要素][2]で置き換えられています。これら制限はパフォーマンスとセキュリティーの面から課されています。

- パフォーマンスを考慮した`style`要素の制限
- セキュリティーを考慮した`base`要素の制限


### カスタム要素 (1.5P)

AMP HTMLでは`amp-`で始まる名前を持つカスタム要素が多く使われます。カスタム要素の多くはプレースホルダー的な意味合いを持ち、関連付けられたJavaScriptファイルを読み込み、実行することでコンポーネントとして機能するようになります。

- `amp-img`要素
- `amp-twitter`要素


[2]: https://html.spec.whatwg.org/multipage/scripting.html#custom-elements


パフォーマンスとセキュリティーにおけるメリット
----------------------------------------------

AMPのメリットは主にパフォーマンスとセキュリティーです。通常のブラウザーだけでなく、アプリ内ブラウザーでウェブページが開かれることが多くなった昨今、両者を追求することはウェブサイト制作において優先すべき課題のひとつでしょう。


## AMP Cacheの強み（1P）

パフォーマンスには既に述べたAMP HTMLでの制限が大きく関わっていますが、それだけではありません。AMP Cacheという強力なキャッシュ・サーバーを経由することも大きな意味を持ちます。現在AMP Cacheは、間違いなく世界最高クラスの資金力と運営能力を持つGoogleが運営しています。私たちはそのAMP Cache経由でAMPを配信し、また閲覧することになるので、絶大な効果があるものと期待できます。


## 安全の保障（1P）

AMP HTMLに課されている制限により、配信者は自前で書いたJavaScriptを含めることができません。そしてそのAMP HTMLは、AMP Cacheにキャッシュされる際、その正当性をチェックされます。そのためAMP Cache経由であるならAMP HTMLの制限を無視した怪しい文書であることはほぼなく、セキュリティー上の問題はほぼ起こらないと考えられます。またサードパーティー製のJavaScriptもAMPプロジェクトのCDN経由でしか配信されないため、それらのセキュリティー・リスクは非常に低いものと考えられます。


Googleへの依存とプライバシーにおけるデメリット
----------------------------------------------

AMPは、インターネットそのものを抽象化し、置き換えかねない性質を持ちます。それが可能だとは思えませんが、それでもそういった性質を持つものが一企業の完全な支配下にあることには留意するべきでしょう。


### AMP Cacheの中立性（1P）

ここまで話してきたように、AMPはオープンソース・プロジェクトではありますが、Googleとは切り離して考えることはできません。AMP Cacheを経由せずにAMP HTMLやAMP JSを取得することは可能ですし、それなりの効果を見込めますが、現実的ではないでしょう。特にセキュリティー上の観点からはオススメできません。仮にあらゆるウェブページがそのAMPバージョンを持ったとすると、それを利用することはGoogleが完全に支配した別のインターネットを利用することに他なりません。


### トラッキングへの懸念（1P）

すべてがGoogle経由であることは、プライバシー上の懸念もあります。何らかのアプリで内部的にAMPを利用してコンテンツを表示する場合、かなりひそやかな形でトラッキングは行われてしまうことでしょう。そういったトラッキングがGoogleアカウントやそのアプリのユーザー・アカウントと結びつけられることは現実としてはなさそうですが、どのような形で利用されるかは不透明ですし、アプリの利用規約くらいにしかそのことは明示されないことでしょう。


さいごに
--------

AMPは現実と理想のギャップを埋める手段として注目に値するものです。その仕様もHTMLを始めとしたウェブ標準仕様から逸脱したものではなく、汎用性や学習コストの点からも評価できます。


### 普通のウェブページではダメなのか（0.5P）

しかし、普通にHTMLとCSS、そしてJavaScriptを書くと、ブラウザーその他が頑張ってくれるという未来は少しづつ近づいています。高速で安定したウェブサーバーという問題も、安価に運用可能なCDNサービスの選択肢が増えたことにより、なんとかなることでしょう。ですからAMPには勝てないまでも、そん色のないウェブページを制作することは既に可能なはずです。


### 普通のHTMLでは絶対に勝てないところはあるのか（0.5P）

ただウェブページのパフォーマンスを大きく落とす広告においては、AMPの持つ広告コンポーネントの威力は見過ごせません。ことウェブ経由での収益において、広告は鍵になります。そういった広告が高速かつ安全に表示できることを保証できるAMPはとても魅力的です。
