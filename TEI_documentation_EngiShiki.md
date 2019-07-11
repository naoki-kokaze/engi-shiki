# 歴史資料TEIマークアップのための日本語マニュアル：延喜式を事例に

**ver 1.0**

Last updated on 11th July 2019

# Naoki Kokaze, Kiyonori Nagasaki, Yuta Hashimoto, Ayano Kokaze, and Makoto Goto

## Table of Contents

0. [はじめに](#ch0)
1. [メタデータ記述](#ch1)
2. [全体構造記述](#ch2)
3. [目的に即した記述](#ch3)
4. [多言語対応](#ch4)
5. [スキーマカスタマイズ](#ch5)
6. [表示やアプリケーションの例](#ch6)
7. [参考文献](#ch7)

---
0. # <a id="ch0">はじめに</a>
歴史資料のテクストを機械可読形式の構造化データとして記述する（以下、マークアップ）場合、1987年から欧米を中心に環境構築が進んでいるText Encoding Initiativeに準拠することが一般的になりつつあります（[永崎，2019](#nagasaki2019)）。TEIは年を重ねるにつれてバージョンアップが進み、2019年7月現在では第5版を数えており、およそ550ものマークアップ用のタグが定義されています。TEIによるマークアップは、テクストそれ自体の理解と、マークアップ言語XMLの理解の双方が必要となるため、人文学的知識と情報学的知識に長けた個人が担うか、分野横断的なプロジェクトグループが組織的に行うことが有効であると思われます。

このようなグループによるマークアッププロジェクトを進める際に重要になるのが、マークアップのための方針です。その際、マークアップに触れるのが初めての人に対しても、マークアップ作業を円滑に進めてもらえるように、マークアップの方針とマークアップ方法を記したマニュアルを作成するのが慣習となっています（[Terras, 2009](#terras2009)）。ただし、欧米圏のテクストを対象としたマークアップマニュアルは多いものの（例えば、[*TEI: Teach Yourself TEI*](#https://tei-c.org/support/learn/teach-yourself-tei/) のGuides to Local Practiceを参照）、東アジア文化圏のテクストを対象としたものは依然として少ないのが現状です。

そこで本稿は、古代日本の律令行政における施行細則を記した延喜式を事例に、日本語歴史資料のためのマークアップマニュアルを提供することを目指します。このようなマニュアル作成の意義について、[後藤，2018](#goto2018) が詳述していますので、あわせてご参照ください。

---

1. # <a id="ch1">メタデータ記述</a>

## 1-1. **史資料の書誌情報をどのように記述するか**

## 1-2. **画像とテクストの対応付け**

次のセクションで本文のマークアップ方法について検討する前に、画像とテクストを関連させる方法について簡単に記しておきたいと思います。史料画像とテクストの対応関係を記述したい場合の方法として、ここではOxygen XML Editorに搭載されているImage Map Editorの機能の使い方を紹介します。

- 使用するタグ

    - <span style="color:#4788ff">**\<facsimile\>**</span>			teiHeaderの終了タグの直後に配置する
    - <span style="color:#4788ff">**\<surface\>**</span>		画像一枚ごとの情報をまとめるためのタグ
    - <span style="color:#4788ff">**\<graphic\>**</span>	URLや相対パスで画像を指定するタグ（<span style="color:orange">**@url**</span> 属性を用いる）
    - <span style="color:#4788ff">**\<zone\>**</span>		Image Map Editorにより<span style="color:orange">**@xml:id**</span> 属性付きで自動生成されるタグ

- Image Map Editorについて

    Oxygen XML Editorには3つのモード、すなわち「テキスト」「グリッド」「作者」があります。そのうち「作者」モードは、マークアップしたテクストを簡単に変換して、見やすい形で表示してくれる機能を持っているのですが、画像との対応付けをマークアップするImage Map Editorもこの「作者」モードに含まれています。

    上記の「使用するタグ」で説明したように<span style="color:#4788ff">**\<graphic\>**</span>タグで画像のURIを指定すると、「作者」モード中のImage Map Editorで、画像のどの部分を切り取って座標情報をデータ化するかを指定することができます。画像の切り取りにあたっては、長方形での切り取り・多角形ポリゴンの切り取りを選択することができます。
    
    なお、この操作に関しては、[TEI Zoner](#http://teicat.huma-num.fr/zoner.php)というウェブツールでも同じことができます。詳しくは、#dariahTeachの[*DH teaching material*](#https://teach.dariah.eu/)のうち、[Digital Scholarly Editions: Annotating Images](#https://teach.dariah.eu/mod/page/view.php?id=452)のレッスン動画をご覧ください。
    
    下図は、国立国会図書館デジタル・コレクションでIIIF形式で提供されている延喜式画像の一行ごとの座標情報を切り出したものです。キャプチャ右側のID欄に見られるように、<span style="color:orange">**@xml:id**</span> が機械的に生成されています。

![image map editor](image_map_editor.jpg "image map editor")

2. # <a id="ch2">全体構造記述</a>

## 2-1. **テクストの階層構造**
[Wittern, 2010, p. 552](#wittern2010) が述べるように、TEIにおいてテクストは順序立てられた階層構造（OHCO: Ordered Hiearchy of Contents Objects）を持つと理解されています。厳密に言えば、この階層構造には2つの意味が含意されており、ひとつは物理的な階層、もうひとつは意味的な階層です。物理的な階層というのは、例えば「ページ」と「行」との間の関係のように、大きい区分がより小さい区分を包摂するような関係のことです。

これに対しTEIは、意味的な階層構造をより重要視しています。つまり、部、章、段落といった意味ごとのまとまりがどのように記述されているのかを優先的にマークアップすること（※これをDescriptive Markupと呼びます）を優先しているのです（[Renear, 2004](#renear2004)）。なお、このように階層構造に優劣をつけるのは、TEIが階層構造を持つXML言語（かつてはSGML言語）に準拠しており、複数の階層関係を両立してマークアップできないことに起因しています（[Ide, et al., 2018](#ide2018)）。

これに伴い、TEIでは物理的な区切りに関しては、意味的な階層構造の記述に支障をきたさないように、空要素（マイルストーン・タグ）を用意しています。これは、タグの中身にテクストを持たず、物理的な区切りを指し示す目印としてだけ使われるタグがいくつか存在するのです（cf. TEI-C, eds. '**3.10.3 Milestone Elements**', *TEI P5*. Version 3.4.0. Last updated on 23rd July 2018, http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/CO.html#CORS5 ）。

- <span style="color:#4788ff">**\<milestone\>**</span> : 当該セクションが構造要素により表現できない場合に、テクストの各セクション間にある境界点を示す。
- <span style="color:#4788ff">**\<lb\>**</span> : line beginning. ある版における印刷上の新しい行の始まりを示す。
- <span style="color:#4788ff">**\<cb\>**</span> : column beginning. テクストの段と段の境界を示す。
- <span style="color:#4788ff">**\<pb\>**</span> : page beginning. テクストのページ境界を示す。
- <span style="color:#4788ff">**\<gb\>**</span> : gathering beginning. 紙の一帖などの区切りを示す。

いずれの空要素も、標準的な参照システム、つまり@n属性や@xml:id属性、@facs属性などを用いて、史料画像と電子化テクストの対応関係を表現することが多くあります。以下に、\<pb\>エレメントの使用例を示します（ http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/examples-pb.html ）。

```xml
<body>
 <pb n="1" facs="page1.png"/>
<!-- page1.png contains an image of the page;
                        the text it contains is encoded here -->
 <p>
<!-- ... -->
 </p>
 <pb n="2" facs="page2.png"/>
<!-- similarly, for page 2 -->
 <p>
<!-- ... -->
 </p>
</body>
```
上記の例では、ページ数を<span style="color:orange">**@n**</span> 属性（numberの略）の値で表現し、対応する史料画像のファイル名を<span style="color:orange">**@facs**</span> 属性の値として記述しています。なお、より細かく画像の中の特定の箇所を指し示したい時には、Oxygen XML EditorのImage Map Editorという機能を用いることにより実現できます（ https://www.oxygenxml.com/doc/versions/20.0/ug-editorEclipse/topics/image-map-editor.html ）。


## 2-2. **延喜式における階層構造のマークアップ**

さて、延喜式における意味的な階層構造について具体的に考えてみましょう。延喜式は、大分類として全部で50の式があり、その中に複数の条文が中分類として記述されています。そして式と条はそれぞれ表題を持っており、表題のあとに本文（として機能する条文）が続くようになっています。

例として、延喜式第五巻斎宮式の第一条の条文を見てみましょう。

    斎宮
    凡天皇即位者、定伊勢大神宮斎王、仍簡内親王未嫁者卜之、〈若無内親王者、依世次、簡定女王卜之、〉訖即遣勅使於彼家、告示事由、神祇祐已上一人、率僚下随勅使共向、卜部解除、神部以木綿著賢木、立殿四面及内外門、〈賢木・木綿所司儲之、解除料散米酒肴等本家儲之、〉 其後択日時、百官為大祓、〈同尋常二季儀、〉

この条文を、意味のまとまりに注目してマークアップした例を以下に示します。

```xml
<div ana="斎宮" n="5" subtype="条" type="式">
    <head ana="5斎宮式" n="上_258">斎宮</head>
    <div ana="斎宮" n="5.1" type="条">
        <head ana="定斎王" n="上_258"/>
        <p>凡天皇即位者、定伊勢大神宮斎王、仍簡内親王未嫁者卜之、
        〈<note place="inline" type="割書">若無内親王者、依世次、簡定女王卜之、</note>〉訖即遣勅使於彼家、告示事由、神祇祐已上一人、率僚下随勅使共向、卜部解除、神部以木綿著賢木、立殿四面及内外門、〈<note place="inline" type="割書"> 賢木・木綿所司儲之、解除料散米酒肴等本家儲之、</note>〉 其後択日時、百官為大祓、〈<note place="inline" type="割書">同尋常二季儀、</note>〉
        </p>
    </div>
```

式に関するメタデータを格納する<span style="color:#4788ff">**\<div\>**</span>タグには、次のような属性を用いて記述しました。

- <span style="color:orange">**@ana**</span>:		属性値として式の名称を記述し、索引として機能させる（下図参照）
- <span style="color:orange">**@n**</span>:		属性値として式の番号を記述する。
- <span style="color:orange">**@type**</span>:		属性値として”式”と記述し、式を表すdivタグであることを明示
- <span style="color:orange">**@subtype**</span>:	属性値として”条”と記述し、子要素として条を表す<span style="color:#4788ff">**\<div\>**</span>タグを持つことを明示

![oxygen outline](oxygen_outline.jpg "oxygen outline")

上記の属性を用いて式を表したdivタグの子要素として、次の2つのタグをマークアップしています。

- <span style="color:#4788ff">**\<head\>**</span>		式の始まりを明記する本文中の表題をマークアップするためのタグ
- <span style="color:#4788ff">**\<div\>**</span>		条を表すために用いた新たな<span style="color:#4788ff">**\<div\>**</span>タグ

さらに、この<span style="color:#4788ff">**\<head\>**</span>タグは子要素を1つ持っています。

- <span style="color:#4788ff">**\<title\>**</span>		電子版のテクストに記載されていたメタデータを格納する空タグ

条を表す<span style="color:#4788ff">**\<div\>**</span>タグは、次のような属性を用いて記述しました。

- <span style="color:orange">**@ana**</span>: 属性値として、その条が属する式の名称を記述する。
- <span style="color:orange">**@n**</span>: 属性値として、式と条の番号をピリオドで区切ってセットで記述する。
- <span style="color:orange">**@type**</span>: 属性値として”条”と記述し、条を表す<span style="color:#4788ff">**\<div\>**</span>タグであることを明示

最下層の条を表すdivタグの子要素として基本的なものは、以下の2つです。

- <span style="color:#4788ff">**\<p\>**</span>:	条文の内容すべてを格納するタグ（特に段落を区切っているわけではない）。子要素に<span style="color:#4788ff">**\<title\>**</span>タグを持ち、条の名称を<span style="color:orange">**@ana**</span>の属性値として格納する
- <span style="color:#4788ff">**\<note\>**</span>: さまざまな形式の註記情報をマークアップすることができるタグ。ここでは、<span style="color:orange">**@place**</span>属性と<span style="color:orange">**@type**</span>属性を用いて、本文中に挿入されたタグであることを示そうとしている。

なお、割註のマークアップのより詳しい検討については、[王・永崎，2018](#wang2018) をご参照ください。

---

3. # <a id="ch3">目的に即した記述</a>

## 3-1. **基本的な情報の記述**

本節では、歴史資料のテクストに出現する基本的な情報、つまり日付や人名・地名といった固有名詞をどのようにマークアップするかを検討します。なお、本節における説明は、[小風，2019a: ch. 2–4](#kokaze2019a) の一部を直接引用しています。

### 3-1-1. **日付の記述**

TEIでは、日付を記述するためのタグとして、<span style="color:#4788ff">**\<date\>**</span>を用意しています（http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/ref-date.html ）。

```
ここにサンプルテキストが入ります
```

上記のサンプルテクストでは年代の範囲と和暦年が記述されていることがわかりますので、例えば <span style="color:orange">**@when, @from, @to**</span> 属性を用いて下記のようにマークアップすることになります。

※なお、これらの属性に入れる値としては、W3C (World Wide Web Consortium) の *XML Scheme Part 2: Datatypes Second Edition* に従うことが推奨されています。標準的な形式でデータを記述することは、コンピュータによる処理を容易にするため、TEIにおける時間の記述においても、W3Cなどの定めるデータフォーマットが望ましいとされています ([Wittern et al., 2009](#wittern2009), p. 288)。

```xml
<date from="1840" to="1842">一八四〇―四二</date>
<date when="1848">一八四八年</date>
```

これが、延喜式に見られるように和暦で記述されていた場合は、次のように <span style="color:orange">**@when-custom, @from-custom, @to-custom**</span> 属性を、<span style="color:orange">**@datingMethond, @calendar**</span> 属性と併用すると良いでしょう。

```xml
<date datingMethod="#japanese" calendar="#japanese" from-custom="天保:10" to-custom="天保:12">天保十年―天保十二年</date>
<date datingMethod="#qing_dynasty" calendar="#qing_dynasty" when-custom="道光:28">道光二十八年</date>
```

上記の <span style="color:orange">**@when-custom**</span> 属性などの値の区切り値として、半角コロンが用いられていますが、TEI P5ガイドライン上では特に指定はなく、プロジェクト内で一貫した記述をすることだけが推奨されています（http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/ref-att.datable.custom.html#tei_att.when-custom ）。


### 3-1-2. **人名・組織名・官職名の記述**

人名・組織名・官職名のマークアップ例は、それぞれ<span style="color:#4788ff">**\<persName\>**</span>, <span style="color:#4788ff">**\<orgName\>**</span>, <span style="color:#4788ff">**\<roleName\>**</span> を使って、次のようになるでしょう。

```xml
<persName ref="http://viaf.org/viaf/257519240">土御門泰重</persName>
<orgName></orgName>
<roleName></roleName>
```

TEIマークアップ中で何かを参照しようとする際に用いられる属性は、上記の <span style="color:orange">**@ref**</span> 属性のほかにも、<span style="color:orange">**@key**</span> 属性が挙げられます。TEI P5ガイドラインの att.canonical 属性クラスの解説によれば、<span style="color:orange">**@ref**</span> 属性での参照はより明示的で、外部で定義されたURIなどを指定する際に用いる一方、<span style="color:orange">**@key**</span> 属性は何らかの決まりにしたがった形で外部参照の手段を提供する際に用いるとされています (http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/ref-att.canonical.html)。

つまり、<span style="color:orange">**@key**</span> 属性の方が、プロジェクト毎のルールに沿った柔軟な参照方法を記述できるわけですが、VIAF（ヴァーチャル国際典拠ファイル）など外部のデータベースなどで定義された学術情報などにリンク付けしたい場合には、<span style="color:orange">**@ref**</span> 属性を用いて外部のURIを直接記述する方が適切であるということになります。

### 3-1-3. **地名の記述**

一般に地名の記述には、「横浜市」などのように都市や行政区画を表す語が見られる一方、「アジア」などのように、明確な行政的領域というよりは概念上の地政学的領域を示した語も見られます。TEIガイドラインでは、このような様々な種類の「地名」をマークアップするにあたって、<span style="color:#4788ff">**\<placeName\>**</span>タグのほかにも、様々なタグセットを用意しています（http://www.tei-c.org/release/doc/tei-p5-doc/ja/html/ND.html#NDPLAC ）。しかしながら、国名や地域名などのように時代や見解によって解釈が分かれる地名を詳しくマークアップすることは、時として論争的かつ政治的な側面を持ち得ることは念頭に置いても良いでしょう。

以下のように、延喜式における地名のマークアップについては、<span style="color:#4788ff">**\<placeName\>**</span>タグを用いました。

```xml
<placeName>山城国</placeName>
```

なお、歴史的地名や地理的領域に関しては、様々な地名辞典が存在します。日本の地名に関して言えば、平凡社『<a href="https://japanknowledge.com/contents/rekishi/index.html">日本歴史地名体系</a>』や、人間文化研究機構およびH-GIS研究会により公開された「<a href="https://www.nihu.jp/ja/publication/source_map">歴史地名データ</a>」などを挙げることができます。

### 3-1-4. **イベントの記述**

意外なことに、歴史的事象、より広義にはイベントについては、人名や地名と違い、本文のマークアップで使用することのできる <span style="color:#4788ff">**\<eventName\>**</span> というタグが用意されていません。代わりに、<span style="color:#4788ff">**\<name\>**</span>タグの中の <span style="color:orange">**@type**</span> 属性の値として event と記述することで代替すれば良いと考えられているためです ([Ore & Eide, 2009, pp. 167-169](#ore2009))。より明示的なデータ記述ができるようにTEIガイドラインが改訂されてきていることを考えれば、このようなタグが導入される可能性も考えられます。

<span style="color:#4788ff">**\<name\>**</span> タグとは別に、<span style="color:#4788ff">**\<rs\>**</span> (referring strings) タグを用いることも考えられます。これらのタグは、TEI P4ガイドラインにおいて、固有表現のマークアップのために用いられていました ([Eide, 2014](#eide2014), chap. 2: P4 to P5)。本ガイドラインでは、<span style="color:#4788ff">**\<name\>**</span> を用いておきたいと思います。

```xml
<name type="event">四時祭</name>
```

### 3-1-5. **度量衡の単位および換算式の記述**

[kokaze, et al., 2019b](#kokaze2019b) では、歴史上に存在したあらゆる度量衡の単位およびその換算式をマークアップするためのデータセットを提案し、2019年7月に予定されたTEI P5ガイドラインのアップデートに盛り込まれる予定です。新しいタグセットがガイドラインに盛り込まれる一連の議論と過程の例にご関心がある方は、TEI技術委員会の[GitHub Issue＃1707, 'How to encode measurement'](#https://github.com/TEIC/TEI/issues/1707) をご覧ください。

```xml
<encodingDesc>
  <unitDecl>
    <unitDef xml:id=“斤”>
      <label>斤</label>
      <conversion fromUnit=“#兩” formula=“#兩*16”/>
    </unitDef>
    <unitDef xml:id=“兩”>
      <label>兩</label>
      <conversion toUnit=“#斤” formula=“#兩*16”/>
    </unitDef>
  </unitDecl>
</encodingDesc>
<!-- 途中省略 -->
<measureGrp>
  <measure commodity=“海鼠腸”>海鼠腸</measure>
  <measure quantity=“62” unit=“#斤”>六十二斤</measure>
　<measure quantity=“8” unit=“#兩”>八兩</measure>
</measureGrp>
```

---

4. # <a id="ch4">多言語対応</a>

## 4-1. **in progress**



5. # <a id="ch5">スキーマカスタマイズ</a>

## 5-1. **ルビのマークアップ**

以下の記述は、2019年4月17日付のdigitalnagasakiのブログ「[TEI P5ガイドラインの最新版（v3.5.0)でルビを。](#http://digitalnagasaki.hatenablog.com/entry/2019/04/17/032130)」の一部を直接引用しています。

    TEI P5ガイドラインには、まだ「ルビ」がありません。既存のタグを流用することでなんとかしのいでいるところではありますが、「ルビ」という融通無碍なルールにぴたっとハマるエレメントは、やはり既存のものではちょっと難しいところがあります。ルビは、発音を示すものであることもあれば、意味を示すこともあり、もう一つの本文として扱うこともできます。それぞれの文章中での位置づけを既存のエレメントに対応づけるという方向性も考えられなくはないですが、それをしてしまうと「ルビ」の多義性を損なってしまうこともあると思います。作業効率から考えても「ルビ」は「ルビ」として扱った方が日本の文化的コンテクストには適切なのではないかと思うのです。

    そのようなことで、「ルビ」タグを使えるようにTEI P5ガイドラインをカスタマイズしたいのですが、もちろん、TEI協会ではカスタマイズのためのツール Roma を提供しています。このツールの使い方は、簡単なものですが、[こちらの記事](#https://www.dhii.jp/nagasaki/blog/node/12)に日本語のものが用意してあります。で、ごにょごにょとカスタマイズした結果、以下のように記述できるようになりました。

```xml
<?xml-model href="http://www.dhii.jp/nagasaki/tei_all_ja_ruby_350.rnc" type="application/relax-ng-compact-syntax"?>
<TEI xmlns="http://www.tei-c.org/ns/1.0" xmlns:eaj="http://www.example.org/ns/eaj">
<!-- 途中省略 -->
<eaj:ruby>
  <eaj:rb>鎮花</eaj:rb>
  <eaj:rt>ハナシヅメ</eaj:rt>
</eaj:ruby>
```
実際のブログでは、カスタマイズしたスキーマの割り当て方法についても記されていますが、上記のマークアップ例では、そのまま貼り付けても機能するように記述しています。

---

6. # <a id="ch6">表示やアプリケーションの例</a>

## 6-1. **in progress**


7. # <a id="ch7">参考文献（アルファベット順）</a>



- <a id="#ide2018">Ide, N. et al. (2018) </a>TEI：それはどこからきたのか。 そして、なぜ、今もなおここにあるのか？. デジタル・ヒューマニティーズ. 13–28. [online]. Available from: https://www.jstage.jst.go.jp/article/jadh/1/0/1_2/_article/-char/ja (Accessed 9 March 2019).

- <a id="kokaze2019a">小風尚樹 (2019a) </a>PythonによるセマンティックTEIマークアップのためのガイドライン. 東京大学学術機関リポジトリ. [online]. Available from: https://repository.dl.itc.u-tokyo.ac.jp/index.php?active_action=repository_view_main_item_detail&page_id=28&block_id=31&item_id=51399&item_no=1 (Accessed 5 July 2019).

- <a id="nagasaki2019">永崎研宣 (2019) </a>‘歴史データのさまざまな応用：Text Encoding Initiativeの現在’, in 後藤真 & 橋本雄太編. 歴史情報学の教科書：歴史のデータが世界をひらく. 文学通信. pp. 131–154. [online]. Available from: http://repository.bungaku-report.com/htdocs/?active_action=repository_view_main_item_detail&page_id=3&block_id=8&item_id=30&item_no=1 (Accessed 1 April 2019).

- <a id="ore2009">Ore, C.-E. & Eide, Ø. (2009) </a>TEI and Cultural Heritage Ontologies: Exchange of Information? *Literary and Linguistic Computing*. 24 (2), 161–172. [online]. Available from: https://academic.oup.com/dsh/article/24/2/161/978118 (Accessed 15 November 2018).

- <a id="#renear2004">Renear, A. (2004) </a>‘Text Encoding’, in Susan Schreibman et al. (eds.) *A Companion to digital humanities*. Chichester, West Sussex: John Wiley & Sons, Ltd. p. [online]. Available from: http://www.digitalhumanities.org/companion/view?docId=blackwell/9781405103213/9781405103213.xml&chunk.id=ss1-3-5&toc.depth=1&toc.id=ss1-3-5&brand=default (Accessed 29 September 2018).

- <a id="wang2018">王一凡 & 永崎研宣 (2018) </a>東アジア文献へのTEIの適用をめぐって. 研究報告人文科学とコンピュータ（CH）. 2018-CH-118 (4), 1–4. [online]. Available from: https://ipsj.ixsq.nii.ac.jp/ej/index.php?active_action=repository_view_main_item_detail&page_id=13&block_id=8&item_id=190812&item_no=1 (Accessed 8 July 2019).

- <a id="wittern2010">Wittern, C. (2010) </a>デジタル漢籍の新しいテキストモデル. 東方学報. (85), 543–554. [online]. Available from: https://repository.kulib.kyoto-u.ac.jp/dspace/handle/2433/131774 (Accessed 8 July 2019).

