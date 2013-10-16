Title: Opera for Devices: Out of Memory システム
----
Date: 2012-06-08 07:31:12
----
Lang: ja
----
Author: 
----
License: Creative Commons Attribution-Noncommercial-Share Alike 3.0 Unported
----
License_url: http://creativecommons.org/licenses/by-nc-sa/3.0/
----
Text:

<p><a href="http://www.opera.com/business/devices/">Opera for Devices</a> は Opera のヒープ領域の使用に関して、厳密な制限を設定するための強力なメカニズムを提供しています。多くのデバイスにとってメモリは貴重な資源であり、予期できない事象が発生する状況のもとで、異なるプログラム間によるリソースの競合が発生することがあります。Out of Memory (OOM) システムは、Opera の機能性を保証しながらも、メモリの使用量に制限をかけることができます。</p>

<p>このトピックはコンテンツ開発者よりも Opera を自社のデバイスに実装されるお客様向けにフォーカスしていますが、メモリ制約の厳しいデバイス上で、メモリの割り当てがどのように行われるかを理解することは、自身のアプリケーションの最適化の方法や、Opera がメモリ不足に陥ったときにどのような挙動をするかを理解する上で重要です。</p>

<h2>内容:</h2>

<ul>
    <li><a href="#understanding">Out of Memory（メモリ不足）の状況について理解する</a></li>
    <li><a href="#system">Opera OOM システム</a></li>
    <li><a href="#problems">それぞれのメカニズムはどの問題を解決するものですか？</a></li>
    <li><a href="#employing">OOM システムを利用する</a></li>
</ul>

<h2 id="understanding">Out of Memory（メモリ不足）の状況について理解する</h2>

<p>Linux は一般的に <span class="code">malloc()</span> が信頼できないなど、システムのメモリ不足に対して安全な仕組みを提供してるとは言えません。Linux 環境は一般的に <span class="code">malloc()</span> が NULL をまったく、またはほとんど返さないよう設定されています。それで、多くの Linux アプリケーションやライブラリは NULL の返り値を予期していませんし、そのための正しい処理が行われません。この仕組みは、メモリ不足がほとんどは発生しないデスクトップシステム上でのみ正しく動作します。</p>

<p>まれに Linux システムがメモリ不足に陥った場合、カーネル内の高機能なアルゴリズムによりメモリを使用しすぎているプロセスを選択し kill (停止) します — これを <i>OOM-killer</i> と呼びます。ここでの問題点は、メモリ不足の状況が、アプリケーションが回復できない状態になるまでアプリケーション側に通知されず、アプリケーションがシステム側に停止されてしまうという点です。エンドユーザー側からすると、これは単純にクラッシュように見えます。限られたメモリしか持たないデバイス上では、これは望ましくない動作であり、避けるべき振る舞いです。</p>

<h2 id="system">Opera OOM システム</h2>

<p>限られたメモリ上でこの問題に対応するために、Opera ではメモリの少ない状況でもブラウザが正しく動作する仕組みを実装しています。それは Opera の使用するメモリヒープを効率的に制限する２つのメカニズムによって提供されます。</p>

<h3>ヒープサイズ制限</h3>

<p>一つ目のメカニズムは Opera 内のメモリ割り当てを監視し、プロセスのヒープサイズが設定された制限値を超えた場合に、メモリの割り当てを強制終了させるものです。</p>

<h3>アロケーション（割り当て）サイズ制限</h3>

<p>二つ目のメカニズムは、内部で Opera が割り当てたメモリの合計量を記録し、制限値を超えた場合にどんな種類のメモリ割り当ても行わないように強制するものです。</p>

<h3>Internal OOM の動作</h3>

<p>Internal OOM は、上記二つのメカニズムによりメモリ割り当ての強制終了が発生した際に実行されます。Internal OOM は、必要であればページの読み込みを停止するなどして、出来る限りメモリを解放します。これにより、Opera がヒープが制限値を超えて増加することを防ぎ、Opera がメモリ割り当て量を設定された制限値内に保つことができるようになります。</p>

<p>Opera はさらに OOM 通知を実装レイヤーに送信するので、Opera がメモリ付属に陥っていることを警告することができます。この状況がどのように扱われるかは Opera が実装された製品ごとに異なります。たとえば使用していないアプリケーションを閉じるよう勧めるダイアログが表示されます。</p>

<h2 id="problems">それぞれのメカニズムはどの問題を解決するものですか？</h2>

<p>OOM システムにあるこれら二つの異なるメカニズムは全く異なる方法で、異なる問題を解決します。それで、OOM システムの最適な設定を決めるにはデバイスごとのユースケースを考慮する必要があります。一般的には両方のメカニズムを使用することが望ましいと言えますが、振る舞いを正しく調整する上で、それらのメカニズムがどのような影響を与えるかを理解することは重要です。</p>

<p>このトピックではいくつかの問題点と、それが OOM システムにどのような影響を与えるかを説明します。</p>

<h3>メモリ不足</h3>

<p>Internal OOM がブラウザのレンダリングや、ページのブラウジングに必要なメモリを確保できず、Opera が事実上のメモリ不足に陥ることがあります。この問題は Opera のヒープが制限量まで達しており、Opera が依然として十分のメモリを確保できていない時に発生します。</p>

<h3>この問題の原因は？</h3>

<p>Opera のプロセスには Plug-in やユーザーインターフェースなど OOM システムに影響されない部分が存在します。それらは Opera のヒープ領域を共有していますが、Opera はそれらをコントロールすることができません。ある状況では、それらのプロセスがヒープをメモリ領域いっぱいまで割り当ててしまい、Opera に許容された領域以上に達してしてしまうことがあります。それらに多くのメモリが割り当てられるということは、その分 Opera が利用できる部分が少なくなります。このことが Opera のメモリ不足を引き起こし、ヒープを増やすことが一切許されない状況を生み出します。</p>

<p>この問題の発生を減らすには、Opera の OOM 通知をリスニングして、メモリを割り当てる必要のない Plug-in やユーザーインターフェース用のメモリを開放することが必要です。</p>

<p>メモリ不足になる別の原因は、ヒープ領域が深刻なフラグメンテーション（断片化）を起こす場合です。この状況では、ヒープの拡大をせずに大きなメモリ領域を提供することができません。特に初期のヒープ制限に十分な余裕がない場合、このような状況が発生します。この特別な状況では、ヒーブがひどく断片化し、Opera の機能が正しく実行できない可能性があります。</p>

<p>もしメモリ不足の問題が顕著に現れる場合、ヒープ制限量を増やす、またはメモリ割当量の制限をより厳しくすることが解決策となるでしょう。</p>

<h3>ヒープサイズ制限とアロケーションサイズ制限</h3>

<p>Opera のヒープサイズ制限のひとつの重要な側面は、それが単にヒープを<i>増加させる</i>メモリ割り当てのみを止めるという点です。もしプロセス内の他の部分によりヒーブが増加した場合、Opera はヒープ内の利用可能なメモリを使用することが許可されます。このことにより特定の状況ではヒープが大きく増加し、Opera が大量のメモリを使用してしまうことになります。これには良い面と悪い面があるかもしれません。一方では、ヒープ内で利用可能なメモリを有効に使わなければ、そのメモリ領域は使われないままになる可能性があります。他方、メモリが一時的に割り当てられているだけの場合もあり、Linux がヒープを縮小しようとする時、それを阻害してしまう可能性もあります。これは非常に悪い循環となります。</p>

<p>この現象を止めるためには、アローケーションサイズ制限を使用し、メモリを割り当てるスペースが十分あると思える状況でも、Opera のヒープが大きくなりすぎることを防ぐ必要があります。</p>

<h3>ヒープの肥大化</h3>

<p>アローケーションサイズ制限を使用する際の一つの問題は、ヒーブサイズが非常に大きくなるという点です。アローケーションサイズ制限はフラグメンテーション効果を考慮に入れていません。それでもしヒープがフラグメンテーション（分断化）をおこしている場合でも、Opera は注意することなくメモリを割り当て続けてしまい、ヒープが増大します。</p>

<p>さらにプロセス内の他の部分が一時的に通常よりも多くのメモリを割り当てる場合には、それが Opera の機能に関わる部分であっても、Opera がヒープを増大させないようにすることが望まれるでしょう。</p>

<p>この問題の影響を軽減するためには、ヒープ制限をより厳密にする必要があります。</p>

<h3>結論</h3>

<p>すべてのプラットフォームにおいてあてはまるわけではありませんが、OOM システムの2つのメカニズムをぜひとも利用したいと思われることでしょう。ユーザーインターフェースや Plug-in がヒープを取得できるようにするため、アローケーションサイズ制限はヒープサイズ制限より小さくなるべきです。お使いのプラットフォーム上でのそれらの最適な数値は、慎重なテストによってのみ得ることができます。</p>

<h2 id="employing">OOM システムを利用する</h2>

<p>Opera の起動時に、Linux シェルにて環境変数 <b>OPERA_HEAP_LIMIT</b> と <b>OPERA_ALLOC_LIMIT</b> を設定することで Opera の OOM 制限をコントロールできます。これらの値は Opera が起動するとすぐに適用されます。使用例:</p>

<dl>
<dt>export OPERA_HEAP_LIMIT=8388608</dt>
<dd>Opera のヒープサイズを 8MB に設定します。</dd>
<dt>export OPERA_ALLOC_LIMIT=6291456</dt>
<dd>Opera が使用するメモリの合計を 6MB に制限します。</dd>
</dl>