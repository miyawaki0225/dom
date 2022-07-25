https://zenn.dev/ankouh/books/javascript-dom/viewer/b412c1
JavaScriptによるDOM操作入門

1. 要素の取得
2. テキストの操作
3. 属性の操作
4. クラスの操作
5. 関連要素の取得
6. 要素の生成
7. 要素の削除

よく使う3つの取得関数
- document.getElementById()
- document.querySelector()
- document.querySelectorAll()
- まとめ  

DOM操作における主要な操作対象は以下の3つに分類することができます。
- 要素
- テキスト
- 属性
要素はタグのことです。テキストはタグで囲まれたテキストのことを指し、属性はタグに設定された属性を意味します。

例えば以下のようなHTMLがあったとします。

<a href="xxx">Link</a>

この場合、aが要素、Linkがテキスト、hrefが属性を表します。xxxはhref属性の値です。

DOM操作とはこれらに対して何らかの操作を行うことです。操作とは主に、取得（読み取り）、更新、作成、削除となります。p要素のテキストを変更する、a要素の属性を削除する、新たにテーブル要素を作成する。これらの処理を実現する方法を学ぶのが本書の目的です。

最初の1章では、要素の取得を考えます。なぜ要素の取得が最初なのかというと、これがDOM操作の基本だからです。HTMLにおいて、テキストや属性というのはそれ自体では成り立ちません。p要素のテキスト、a要素の属性というように、必ず何らかの要素と関連しています。要素を取得しなければテキストにも属性にもアクセスできません。つまり、要素の取得はDOM操作において必要な土台なのです。

では、要素の取得をやってみましょう。

よく使う3つの取得関数
JavaScriptには要素の取得をするための関数があります。よく使うものを紹介します。

document.getElementById()
document.querySelector()
document.querySelectorAll()

他にも色々ありますが、この3つを押さえておけばまず問題はありません。順番に見ていきましょう。

document.getElementById()
getElementByIdは引数で指定したIDを持つ要素を取得します。

let element = document.getElementById(id);

引数idには取得した要素のidを文字列で指定します。戻り値の型はHTMLElementとなります。ここに要素に関する様々な情報が入っているということを押さえておけばいいでしょう。このオブジェクトを通してテキストや属性にアクセスすることができます。

例えば、以下のHTMLがあるとします。(html,bodyタグなどは省略)

<h1 id="title">タイトル</h1>
<p id="info">DOM操作を覚えよう</p>

getElementByIdを使って、要素を取得してみましょう。

//id=titleの要素を取得
let title = document.getElementById('title');

//id=infoの要素を取得
let info = document.getElementById('info');

idがtitle,infoの要素を取得しています。戻り値は、title,infoという名前の変数に代入しています。これらの変数を通して、テキストや属性の取得や変更を行っていきます（次章以降でやります）。

さて、getElementByIdはIDを指定して要素を取得する非常にシンプルな関数なのですが、逆に言うと機能が限定的です。取得対象をIDでしか指定できません。これは、IDが付いていない要素は取得できないことを意味します。HTML上の取得したい要素にIDを追加するという対応もあり得ますが、要素を取得するためだけにIDを付けるのは面倒な上、HTMLが読みにくくなってしまいます。

document.querySelector()
IDだけでなく他の条件を指定して要素を取得することができるのが、querySelectorです。

let element = document.querySelector(selector);

引数selectorにはCSSのセレクタを指定します。IDはもちろん、クラス名やタグ名、属性値などを指定できるので、様々な状況に対応することができます。ただし、指定したセレクタに該当する要素が複数存在する場合に注意が必要です。querySelectorでは、該当要素が複数存在する場合、最初の要素を返すという仕様になっています。最初というのはHTMLの中でより上部に記述されている要素が優先されるということです。

具体例を見てみましょう。

<ul id="menu">
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>

//id=menuの要素を取得
let menu = document.querySelector('#menu');

//id=menuの中のli要素を取得
let li = document.querySelector('#menu li');

idがmenuの要素と、menuの下にあるli要素を取得しています。idがmenuの要素は1つしか存在しないので問題ありません。ところが、「idがmenuの中にあるli要素」は3つ存在しています。この場合は最初の要素（一番上にある要素）であるitem1のliを取得します。

もし2番目以降の要素を取得したい場合、その要素だけに該当するようなセレクタを指定する必要があります。例えば以下のような指定ができます。

//id=menuの中の2番目のli要素を取得
let li2 = document.querySelector('#menu li:nth-child(2)');

CSSセレクタは色々な条件指定ができるので、慣れれば自在に要素を取得することができます。IDしか指定できないgetElementByIdよりも適用範囲が広いのです。

document.querySelectorAll()
querySelectorでは条件に該当する最初の要素を取得しますが、該当する全ての要素を取得したいときもあります。そういう場合にquerySelectorAllを使います。

let nodeList = document.querySelectorAll(selector);

引数にはquerySelectorと同様にCSSセレクタを使います。戻り値はNodeListオブジェクトでHTMLElementを複数集めたモノと思ってください。条件に該当する全ての要素の情報が入っています。

先程の例と同じHTMLで見てみましょう。

<ul id="menu">
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>

// id=menuの中のすべてのli要素を取得
let liList = document.querySelectorAll('#menu li');

これを実行すると、liListはli要素が3つ入ったNodeListオブジェクトを指します。
このオブジェクトから各要素へアクセスするには配列のように[]を使います。

let li1 = liList[0];
let li2 = liList[1];
let li3 = liList[2];

各要素の型はHTMLElementになります。

まとめ
要素を取得する関数は他にもありますが、今回学んだ３つを覚えておけば困ることはあまりないでしょう。

document.getElementById
document.querySelector
document.querySelectorAll
要素を取得するのはDOM操作の最初の一歩です。次章からは、取得した要素に関連した情報（テキストや属性）にアクセスしていきます。

このチャプターの目次
テキストの取得
テキストの変更
テキストの追記
テキストの削除
まとめ
本章では取得した要素内のテキストの操作をしていきます。具体的には、テキストの取得・変更（追記）・削除を扱います。

テキストの取得
要素内のテキストは要素オブジェクトのtextContentプロパティでアクセスできます。

let text = element.textContent;

elementは要素のオブジェクトです。これは前章で学んだquerySelectorなどの関数で取得します。そして、textContentプロパティを使ってテキストの取得ができます。

具体例で確認します。

<p>こんにちは</p>

pタグ内のテキスト（こんにちは）を取得することを考えます。要素オブジェクトを取得して、textContentプロパティを参照します。

//p要素を取得
let p = document.querySelector('p');

//テキストをアラート表示
alert(p.textContent);

これでアラートに「こんにちは」が表示されます。

!
補足として、以下のように要素内にテキストとインライン要素が入っているときのtextContentプロパティの挙動を確認しておきましょう。

<p>あいうえお<span>アイウエオ</span></p>

ここでp要素のtextContentプロパティを参照すると、何が取得できるのでしょうか。「あいうえお」なのか、「アイウエオ」なのか。もしくは...。

結論を言うと、「あいうえおアイウエオ」が取得されます。要素内のテキストを取得すると、中のインライン要素内のテキストも含めた文字列を取得する仕様になっています。

テキストの変更
element.textContent = 'Hello';

テキストの変更はtextContentに値を代入するだけです。textContentに値を代入するので、既存のテキストは上書きされてしまうことに注意しましょう。

具体例で確認すると、

<p>こんにちは</p>

//p要素を取得
let p = document.querySelector('p');

//テキストを変更
p.textContent = 'Hello';

これでHTMLにはこんにちはと書いてあっても、JavaScriptで上書きされているのでHelloが表示されます。

テキストの追記
テキスト変更のバリエーションとして、追記を考えます。textContentプロパティへの代入は既存のテキストを上書きしてしまいますが、既存テキストに対して追記したい場合もあるでしょう。追記は変更の応用で実現できます。

element.textContent += '追加テキスト';

+=演算子を使うことで、既存のテキストの末尾に新たなテキストを追記することができます。

<p>Hello</p>

//p要素を取得
let p = document.querySelector('p');

//テキストを追記
p.textContent += ' World';

テキストの削除
次はテキストを削除することを考えます。実はこれも変更の応用で実現することができます。テキストを削除することをテキストを空文字に変更すると考えます。つまり、textContentに空文字を代入することでテキストを削除することができるのです。

element.textContent = '';

実際にやってみましょう。

<p>こんにちは</p>

//p要素を取得
let p = document.querySelector('p');

//テキストを削除（空文字に変更）
p.textContent = '';

まとめ
この章では要素内のテキストの操作を学びました。textContentプロパティで要素内のテキストを取得、変更ができます。


このチャプターの目次
属性の取得
属性の変更（追加）
属性の削除
まとめ
本章では属性の操作を学びます。

属性の取得
属性を取得するには、getAttributeメソッドを使います。

let value = element.getAttribute(attr);

引数attrには取得したい属性名を指定します。戻り値は属性の値（文字列）になります。

さっそく試してみましょう。

<a href="b.html">リンク</a>

ここでリンク先のURL（href属性の値）を取得してみます。

//a要素を取得
let a = document.querySelector('a');

//a要素のhref属性の値を取得
let url = a.getAttribute('href');

//アラート表示
alert(url);

要素オブジェクトを取得してgetAttributeメソッドを呼び出すだけです。今回はhref属性を取得したいので、引数に'href'を指定します。他の属性を取得する場合も同様です。

<a id="menu" href="b.html" title="メニュー">メニュー</a>

//a要素を取得
let a = document.querySelector('a');

//title属性を取得
let title = a.getAttribute('title');

//id属性を取得
let id = a.getAttribute('id');

属性の変更（追加）
今度は属性の変更、あるいは追加を見ていきましょう。属性が付与されていない場合は追加となります。使うのはsetAttributeメソッドです。

element.setAttribute(attr, value);

引数attrには属性名をvalueには設定する値を指定します。

具体例で確認しましょう。

<a href="b.html">リンク</a>

//a要素を取得
let a = document.querySelector('a');

//a要素のhref属性をc.htmlに変更
a.setAttribute('href', 'c.html');

//a要素のtarget属性を値_blankで追加
a.setAttribute('target', '_blank');

a要素のリンク先をc.htmlに変更し、さらに新規タブで開くようにtargetに_blankを設定しています。

属性の削除
属性を削除するにはremoveAttributeメソッドを使います。

element.removeAttribute(attr);

引数attrは削除する属性名を指定します。

実際に試してみましょう。

<a href="b.html">リンク</a>

//a要素を取得
let a = document.querySelector('a');

//a要素のhref属性を削除
a.removeAttribute('href');

まとめ
本章では、属性の操作について学びました。関数は以下の3つです。

getAttribute
setAttribute
removeAttribute


このチャプターの目次
通常の属性操作の問題点
classList
クラスの存在確認
クラスのトグル
まとめ
本章ではクラスの操作について学びます。クラスは属性なので、前の章で学んだ属性の操作はそのままクラスにも適用することができます。しかし、通常の属性操作をクラスに対して使うと不便なことが多いのです。なぜかというと、クラスは複数の値を設定することがあるからです。複数の値を操作をする上で、通常の属性操作は機能不足なのです。まずは、通常の属性操作でクラスを操作することの問題点を示し、その後にクラス専用の操作方法を学びます。

通常の属性操作の問題点
クラスは属性であることには変わりないので、前の章で学んだことをそのまま使うことができます。復習も兼ねて見ていきましょう。

<h1 class="title">タイトル</h1>

//h1の要素を取得
let h = document.querySelector('h1');

//クラスを取得
let c = h.getAttribute('class');

//クラスを設定
h.setAttribute('class', 'sub_title');

//クラスを削除
h.removeAttribute('class');

このように、クラスの取得、変更、削除ができます。まったく問題ありません。問題となるのは複数のクラスを扱う時です。

<a class="btn">ボタン</a>

a要素にクラスbtnが設定されています。さて、a要素にクラスactiveを追加することを考えます。「追加」なのでbtnは残したままにする必要があります。しかし、setAttributeでは既存の属性値は消えてしまいます。そのため、既存のクラスはそのまま残しておくには、以下のようなコードになります。

//h1の要素を取得
let a = document.querySelector('a');

//既存のクラスを取得
let c = a.getAttribute('class');

//activeを追加
a.setAttribute('class', c + ' ' + 'active');

既存のクラスを残しておくために、まずはgetAttributeで既存の値を取得し、さらにsetAttributeで現在の値と新しい値をスペースを付けて連結しなければなりません。

他の例を考えてみると、例えばクラスbtnとactiveが設定されているときに、activeだけを削除したいときもremoveAttributeでは難しいことがわかります。removeAttributeはすべての値が消えてしまうので、消した後に必要なものを再度設定し直すといったことをしなければなりません。

このようにクラスは複数設定することがあるために、通常の属性操作メソッドでは機能不足なのです。

classList
クラス専用の操作をするためのclassListプロパティがあります。このプロパティを使えば、クラスの追加、削除が簡単にできます。

クラスの追加はaddメソッドを使います。

element.classList.add(class);

引数classに追加するクラス名を指定します。このメソッドは追加なので、既存の値はまったく影響を受けません。上書きを気にせずに使うことができます。

クラスの削除はremoveメソッドです。

element.classList.remove(class);

引数classに削除するクラス名を指定します。これも指定したクラスのみが削除されるので他のクラスは影響を受けません。

具体例を見てみましょう。

<a class="btn">ボタン</a>

//aの要素を取得
let a = document.querySelector('a');

//クラスactiveを追加
a.classList.add('active');

//クラスactiveを削除
a.classList.remove('active');

classListによるクラスの追加は既存のクラスは消えないし、クラスの削除も指定したものだけが消えてくれるので便利です。

クラスの存在確認
classListには追加、削除の他にも便利なメソッドがあります。その1つが、あるクラスがその要素に存在するかを確認するメソッド、containsです。

element.classList.contains(class);

引数classには存在確認するクラス名を指定します。戻り値はboolean型になります。存在すればtrue、しなければfalseが返ります。

具体例を見てみましょう。

//クラスactiveが存在するか？
if(a.classList.contains('active')){
    alert('activeは存在します');
} else {
    alert('activeは存在しません');
}

クラスのトグル
classListの便利機能にもう一つ、クラスのトグルがあります。トグルというのはスイッチのON/OFFのように2つの状態が切り替わる仕組みのことをいいます。クラスのトグルは、クラスの存在/非存在を切り替えます。toggleメソッドを使います。

element.classList.toggle(class);

引数classにはトグルするクラス名を指定します。引数に指定したクラスが存在すれば削除、存在しなければ追加します。

具体例を見てみましょう。

<a class="btn active">ボタン</a>

//クラスのトグル
//activeが存在するので、削除
a.classList.toggle('active');

//もう一回実行すると、今度は存在しないので、追加
a.classList.toggle('active');

これはボタンクリック時などのイベント処理でよく使います。便利な機能なので是非覚えてください。

まとめ
今回はクラス専用の操作について学びました。

classList.add
classList.remove
classList.contains
classList.toggle

このチャプターの目次
親要素
子要素
隣接要素
まとめ
本章では、ある要素に関連する要素を取得する方法を学びます。ある要素に関連する要素とは以下のような要素のことです。

親要素
子要素
隣接要素
親要素
まずは親要素から始めましょう。親要素を取得するにはparentNodeプロパティを使います。

let parent = element.parentNode;

elementが取得済みの要素オブジェクトです。その親要素を取得しています。

具体例で見てみましょう。

<ul>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>

//2番めのliを取得
let li2 = document.querySelector('li:nth-child(2)');

//親要素(ul)の取得
let parent = li2.parentNode;

子要素
次は子要素を取得します。子要素を取得するにはchildrenプロパティを使います。

let children = element.children;

childrenプロパティはHTMLCollection型で、子要素の複数の情報を持っています。厳密には配列とは違いますが、配列のようなものと考えてください。[]演算子で各要素を、lengthプロパティで子要素の数を取得することができるなど、配列と同じような機能もあります。

具体例を見てみましょう。

<div id="box">
  <h1>タイトル</h1>
  <p>本文です。</p>
</div>

//id=boxの要素を取得
let box = document.querySelector('#box');

//boxの子要素のリスト（h1とp）を取得
let children = box.children;

//各要素のテキストをアラート
for(let i=0; i<children.length; i++){
    alert(children[i].textContent);
}

この例のようにfor文を使って、すべての子要素に対して処理をすることができます。

隣接要素
今度は隣接要素、つまり同階層の隣り合った要素を取得してみましょう。前隣の要素はpreviousElementSibling、後隣の要素はnextElementSiblingを使います。

let previous = element.previousElementSibling;
let next = element.nextElementSibling;

<ul>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>

//ul下の２番めのliを取得
let li2 = document.querySelector('ul li:nth-child(2)');

//li2の前の要素を取得
let li1 = li2.previousElementSibling;
alert(li1.textContent);

//li2の後の要素を取得
let li3 = li2.nextElementSibling;
alert(li3.textContent);

まとめ
この章では関連要素の取得について学びました。関連要素へのアクセスを覚えれば、DOM内を自由に駆け巡ることができます。親から子を取得し、子の隣接要素を取得してさらにその子要素を・・・・とDOM構造内を自由に操作できるのです。

このチャプターの目次
要素の生成
最後の子要素として追加
要素の直前に追加
innerHTML
まとめ
ここまではHTMLに記述されたDOM構造内の要素のテキストや属性に対して操作をしてきましたが、ここからは、DOM構造自体に変化を与える操作をしていきます。この章では新しい要素を生成してDOM構造に加える方法を学びます。

要素の生成
さっそく要素を生成してみましょう。createElementという関数を使った方法を紹介します。

let element = document.createElement(tag_name);

引数tag_nameには生成したい要素名（タグ名）を指定します。戻り値は生成された要素オブジェクト（HTMLElement）になります。

実際に要素を生成してみましょう。

//div要素を生成
let div = document.createElement('div');

//テキストを設定
div.textContent = 'hello';

この例ではdiv要素を生成し、テキストを設定しています。しかし、このコードを実行してみればわかりますが、この処理だけでは生成したdiv要素は画面上には表示されません。要素を生成しただけで、DOM構造に追加していないからです。要素はDOM構造に加えて初めて描画されます。

最後の子要素として追加
要素をDOM構造に加えるためには、要素を「どこに追加するのか」という情報が必要です。どこに追加するかによって使う関数が違います。まずは、ある要素の子要素に追加するappendChildから見てみましょう。

element.appendChild(target);

引数targetには追加する要素を指定します。targetはelementの最後の子要素として追加されます。

具体例を見てみましょう。

<ul>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
  <li>item4</li>
</ul>

//li要素を生成
let li = document.createElement('li');

//テキストを設定
li.textContent = 'item5';

//ulを取得
let ul = document.querySelector('ul');

//ulの最後の子要素として追加
ul.appendChild(li);

appendChildを実行すると、item5がDOMに追加され、画面に描画されます。

要素の直前に追加
appendChildでは最後の子要素として追加されてしまいますが、他の場所に追加したいときもあるでしょう。そこで、今度は他の要素の直前に追加する方法を見ていきましょう。insertBeforeを使います。

以下の例を見てください。

<ul>
  <li>item1</li>
  <li>item3</li>
  <li>item4</li>
</ul>

item3の前にitem2を追加したいとします。

//まずは要素を生成
let li = document.createElement('li');

//テキストを設定
li.textContent = 'item2';

//親要素を取得
let ul = document.querySelector('ul');
//追加したい位置の直後の要素を取得
let li3 = ul.children[1];
//ul下のli3の直前にliを追加
ul.insertBefore(li, li3);

!
注意点としてinsertBeforeはappendChildと同様に親のオブジェクトから呼び出すということです。だから、以下のような書き方は間違いです。

//これは駄目
//親(ul)のメソッドとして呼ばないといけない
li3.insertBefore(li, li3);

!
今回の例は以下のようにも書くことができます。

//まずは要素を生成
let li = document.createElement('li');

//テキストを設定
li.textContent = 'item2';

//追加したい位置の直後の要素を取得
let li3 = document.querySelector('ul>li:nth-child(2)');
//li3の直前にliを追加
li3.parentNode.insertBefore(li, li3);

最初の例では親要素（ul）を取得してからchildrenプロパティで挿入場所直後の子要素（li3）を取得しました。しかし、この例では親要素は取得せずにli3をquerySelectorで直接取得しています。そして、insertBefore呼び出し時に、parentNodeを使って親要素(ul)のメソッド呼び出しにしているのです。こうすることによって、querySelectorで親要素を取得する必要がなくなります。

innerHTML
DOM要素を作るにはcreateElement以外にも方法があります。innerHTMLというプロパティを使うと要素の生成とDOMへの追加が一度にできます。

element.innerHTML = html_string;

html_stringはHTMLのタグ記法の文字列が使えます。つまりHTMLを書く感覚で使えるのです。当然、属性もHTMLの感覚で書けます。

実際にやってみましょう。

<div id="box"></div>

このdiv要素の中に3つの項目を持ったリストを作りたいとします。以下のような構造を作ると考えてください。

<div id="box">
  <ul id="nav">
    <li>item1</li>
    <li class="active">item2</li>
    <li>item3</li>
  </ul>
</div>

これをcreateElementやappendChildを駆使して作っていくと少々面倒です。しかし、innerHTMLを使うと以下のように、簡単に書くことができます。

//追加したい親要素を取得
let div = document.querySelector('#box');

//子以下のDOM構造を直接生成
div.innerHTML = '<ul id="nav">' +
    '<li>item1</li>' +
    '<li class="active">item2</li>' +
    '<li>item3</li>' +
    '</ul>';

これだけです。HTMLのタグ記法で直接代入しているのがわかると思います。さらに、createElementとappendChildのように、要素生成とDOM構造への追加が分離していないこともわかり易いポイントとなっています。

innerHTMLに値を文字列を代入すると既存のDOM要素はすべて消えることに注意しましょう。

例えば、以下のHTMLがあるとします。

<div id="box">
  <h1>タイトル</h1>
</div>

ここでh1要素は残しつつ、p要素を追加したいとします。このように、既に他の要素が存在する状態で末尾に追加したい場合は、innerHTMLに再代入するのではなく追記するようにします。

//+=で追記する
div.innerHTML += '<p>テキスト</p>’；

ところで、最新のJavaScript環境ではTemplate stringがあります。対応ブラウザであれば、innerHTMLによるDOM生成をよりシンプルに書くことができます。

先程の3つの項目を持ったリストを追加する例をTemplate stringを使って書き直すと以下のようになります。

//追加したい親要素を取得
let div = document.querySelector('#box');

//子以下のDOM構造を直接生成
div.innerHTML = `
  <ul id="nav">
    <li>item1</li>
    <li class="active">item2</li>
    <li>item3</li>
  </ul>
`;

文字列をバッククォートで囲むと改行をそのまま出力することができるので、1行ずつ文字列の連結をする必要がありません。HTMLを書く感覚でDOM構造を生成できます。

まとめ
本章では、要素を生成する方法について学びました。createElementで要素を生成し、appendChildやinsertBeforeでDOMに追加する方法、innerHTMLで直接タグ記法で要素の生成と追加をする方法。どちらも覚えておきましょう。


このチャプターの目次
要素の削除
子要素をまとめて削除
まとめ
本章では、要素の削除について学びます。

要素の削除
削除するにはremoveChildを使います。

element.removeChild(target);

引数targetは削除対象の要素オブジェクトで、elementの子要素である必要があります。

具体例を見てみます。

<ul>
  <li>item1</li>
  <li>item2</li>
  <li>item3</li>
</ul>

item2のli要素を削除するとします。

//親のulを取得
let ul =  document.querySelector('ul');

//削除したい２番めの要素を取得
let li2 = ul.children[1];

//li2を削除
ul.removeChild(li2);

removeChildは子要素を削除するので、親要素オブジェクトのメソッドとして呼ぶ必要があります。

ただし、以下のようにすれば親要素を取得せずに削除することができます。

//削除対象のli2を取得
let li2 =  document.querySelector('ul li:nth-child(2)');
//自分自身を削除
li2.parentNode.removeChild(li2);

parentNodeを使って子要素から親要素を参照しています。これだと実質的には子要素が自分自身を削除している感覚で使うことができます。

子要素をまとめて削除
複数の子要素を持つ場合に、すべての子要素を削除したいときがあります。そのとき、すべての子要素に対してremoveChildを実行するのは面倒です。このような場合はinnerHTMLを使って子要素を空にする方法が便利です。

element.innerHTML = '';

innerHTMLに空文字を代入すれば、子要素をすべて削除したこと同じ結果が得られます。ちなみに、textContentでも同じことができます。

element.textContent = '';

まとめ
要素の削除はremoveChildを使う方法とinnerHTML（textContent）を使う方法を学びました。
