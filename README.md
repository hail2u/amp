AMP
===

Googleが中心となって開発・実装されているAMP（Accelerated Mobile Pages）プロジェクトは2016年の2月に正式公開されました。当初はGoogle謹製のサービスやアプリ、そして当初から開発に協力していたTwitterのサービスでのみ利用されるだけでしたが、[Bingがアプリで利用を開始する][1]などそのメリットに惹かれる第三者も増えつつあります。国内でもはてなブックマーク・アプリを始め、利用しているアプリは散見されます。AMPを利用し始める彼らは、いったいAMPのどこに惹かれ、そしてなぜ開発リソースを割いてまで通常のウェブページよりも優先するのでしょうか。


[1]: http://blogs.bing.com/search/September-2016/bing-app-joins-the-amp-open-source-effort


AMPの目的
---------

AMPの目的は、プロジェクトのウェブサイトで「Instant. Everywhere.」というキャッチコピーが使われているように、ウェブページが「どこでも。即座に。」表示されるようにすることに尽きます。その即座に表示される仕組みは以下の3つの要素に集約されます。

1. HTMLからパフォーマンスを下げる要因を排除した*AMP HTML*
2. AMP HTMLを処理しレンダリングを高速化する*AMP JS*
3. AMP HTMLを高速に配信する*AMP Cache*

コンテンツとして提供されるAMP HTML、プラットフォームとして（今はGoogleから）提供されるAMP Cache、両者をつなげて高速化を現実のものにするAMP JSという構成というわけです。これらがすべてかみ合うと、同じコンテンツを静的なHTMLファイルで配信するよりも高速にできる……ということになっています。

配信側も利用側もAMP JSとAMP Cacheへ手を触れることはまずありません。また利用側はブラウザーに準じた仕組みさえあればそれで良いようにデザインされているため、開発者は配信側が扱うAMP HTMLを中心に学ぶことになるでしょう。


AMP HTMLとHTMLとの違い
----------------------

HTMLのパフォーマンスを上げるためには様々な手法が考案されてきました。それらは`script`要素の`async`属性のように実装だけでなく仕様に取り込まれたため、10年前と比較するとそれなりに環境は整ってきたと言えるでしょう。しかし実装や仕様の進歩速度にもやはり限界はあります。また増え続ける高速化手法を適切に取捨選択することが難しいことも言うまでもないでしょう。

AMP HTMLでは厳しいルールに従ってウェブ文書を制作し、それをAMP JSで処理させることによって、*遅いウェブページを作れないように*なっています。

AMP HTMLはHTMLのサブセットというと近いでしょう。利用できる要素に様々な制限がかけられていることと、[カスタム要素][2]が多く使われることが特徴です。制限はパフォーマンスのためにかけられ、カスタム要素はその制限により失われた機能を再実装するために使われます。


[2]: https://html.spec.whatwg.org/multipage/scripting.html#custom-elements
