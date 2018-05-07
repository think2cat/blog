---
title: 《XUL指南》第九章：RDF和模版
tags:
  - xml
  - XUL
  - XUL指南
id: 904
categories:
  - Web
date: 2006-12-03 23:53:08
---

**翻译时间：06年4月13日，未翻译完，不过可能不会再译了**

**本文件为Word格式，有兴趣接着翻译的请跟我联系**

## 九、RDF和模版

### 9.2 模板

该节我们将认识如何使用数据扩展内容

### 扩展内容

XUL提供使用RDF格式的数据创建内容的方法，可以是RDF文件也可以是内部数据源。Mozilla提供众 多数据源，例如书签、历史和邮件信息，在下节有详细介绍。

通常使用数据来对树状分类和菜单进行扩展，而你也可以选择其它元素只要有需要。不过这里我们会使用其它元素进行讲解，因为树和菜单的代码多得多。

要允许创建基于RDF数据的元素，你需要提供一个用于复制元素的简单模板，也就是说你只要提供第一个元素，剩下的元素将从第一个构造生产。

模板由template元素组成，把template元素放置你想构造的元素里面。例如你想在tree容器使用模板，你必须把template放在tree里面。

用一个例子比较好说明，这里我们做一个为每个顶层书签加个按钮的例子。Mozilla提供书签数据源，所以我们可以从那获取数据。这个例子只读取书签文件夹和顶层书签来创建按钮，为了分清子级书签，我们需要使用tree或menu元素来显示。

基于安全着想，访问内部时Mozilla只允许使用chrome地址形式访问RDF数据源，而不允许使用其它方式访问。

预览该例子时你需要创建一个chrome包及载入文件，然后你可以在浏览器地址栏输入chrome地址。

    &lt;vbox datasources="rdf:bookmarks" ref="NC:BookmarksRoot" flex="1"&gt;
    &lt;template&gt;
    &lt;button uri="rdf:*" label="rdf:http://home.netscape.com/NC-rdf#Name"/&gt;
    &lt;/template&gt;
    &lt;/vbox&gt;`</pre>

    这里创建了一个包含几行垂直排列按钮的容器，每一个按钮都是顶层书签，你可以看到template包含了一个简单的button，这个按钮用来创建其它按钮，如图显示。当这个窗口打开的时候，试试在浏览器添加一个书签，你可以注意到窗口按钮会同步更新。

    这个模板被放到一个垂直布置的容器（box）里面，容器有2个用于模板的特殊属性，用来指定数据来源。第一个是datasources，声明用来创建元素的RDF数据源。在例子里使用了rdf:bookmarks，你大概可以猜到这是用来指定书签数据源，因为这里由Mozilla提供数据源。如果你是使用自己创建的数据源，你必须为datasources属性指定RDF文件的URL地址，如下所示：

    ![](/images/2006/12/03_12764.jpg "xul")

    <pre>`&lt;box datasources="chrome://zoo/content/animals.rdf"
    ref="http://www.some-fictitious-zoo.com/all-animals"&gt;`</pre>
    你甚至可以同时指定多个数据源，只需在属性里面用空格分割开来，这样可以同时显示多个数据源。

    ref属性指出从哪里获取数据源，在前面书签例子中，NC:BookmarksRoot用来指出书签根目录，其它属性依赖于你所使用的数据源。如果使用自己创建的RDF文件作为数据源，会经常用到about属性配合在RDF中的Bag、Seq和Alt元素。

    前面例子的box增加了那2个属性之后，就允许你使用模板生产元素了，不过模板里面的元素仍需要声明，你可能注意到例子里的按钮使用了uri属性，以及使用了不同以往的label属性。

    模板里面使用”rdf:”开头的属性值指明数据来源，就像前面例子的label属性，值是来自数据源。它使用来自数据源和附加特性用命名空间namespace生成。如果你无法理解这些，请重读前一节最后一部分，讲解了如何读取RDF中的资源。这里我们只用到书签名称，但其它众多数据源都是可调用的。

    因为这里需要用书签名称来设置按钮，所以按钮label用URI指定其值，我们也可以在其它元素的其它属性使用URI进行指定，这样值将从数据源获取.。

    下面例子演示了如何用从数据源来设置其它属性，当然，这里的数据源提供了相应的数据支持。如果缺少其中某个值，将返回一个空字符串。

    <pre>`&lt;button class="rdf:http://www.example.com/rdf#class"

    uri="rdf:*"

    label="rdf:http://www.example.com/rdf#name"/&gt;

    crop="rdf:http://www.example.com/rdf#crop"/&gt;`</pre>

    如上所示，你可以从数据源动态生产许多元素和属性。

    uri属性用来指定数据从哪里开始，以前内容只能被创建一次，而现在可以利用数据源多次生成。接下来我们创建一个tree的模板来帮助理解。

    在box容器添加一些模板特征，然后在模板里添加一些元素，我们可以从外部数据生产各种有趣的列表。我们也可以在模板里添加更多元素然后为其指定RDF属性。如下所示。

    <pre>`&lt;vbox datasources="rdf:bookmarks" ref="NC:BookmarksRoot" flex="1"&gt;

    &lt;template&gt;

    &lt;vbox uri="rdf:*"&gt;

    &lt;button label="rdf:http://home.netscape.com/NC-rdf#Name"/&gt;

    &lt;label value="rdf:http://home.netscape.com/NC-rdf#URL"/&gt;

    &lt;/vbox&gt;

    &lt;/template&gt;

    &lt;/vbox&gt;`</pre>

    例子创建一个垂直排列的数据来自书签的按钮和标签，按钮显示书签名称、标签显示网址。

    这种创建元素的方法跟直接写到XUL文件的方法是不同的，id属性从模板添加到每个生成的元素，你可以用该属性来识别各个元素。

    你也可以用空格作为分割符，指定多个数据源应用到同一属性上，如下所示。[More about resource syntax](http://www.xulplanet.com/tutorials/xultu/templateex.html).

    <pre>`&lt;vbox datasources="rdf:bookmarks" ref="NC:BookmarksRoot"

    flex="1"&gt;

    &lt;template&gt;

    &lt;label uri="rdf:*" value="rdf:http://home.netscape.com/NC-rdf#Name rdf:http://home.netscape.com/NC-rdf#URL"/&gt;

    &lt;/template&gt;

    &lt;/vbox&gt;`</pre>

    ### 如何创建模板

    当一个元素指定了datasources属性，说明它将由模板创建。注意构造内容的是datasources属性而不是template标签。当属性被声明之后，元素将创建一个构造对象，该对象负责从模板构造内容。在JavaScript里你可以使用builder属性访问该构造对象，不过一般只有在需要更新不会自动更新的内容的时候才需要用到它。

    构造对象分为两种，一种适用于大部分情况的内容构造，另一种只能用于树（tree）。

    构造对象将对模板元素
    里的内容进行一对一复制生成，举个例子，在前面例子里如果用户有10个书签，那将会在vbox里创建10个label元素.如果使用DOM函数分析树（tree），你会发现每个元素都有各自的特性，这些元素都会显示出来，但template本身不显示，即使它也存在文档里。另外，每一个标签（label）的id属性都会从RDF资源获取。

    内容构造对象总是从声明uri="rdf:*"开始，如果uri属性放置在元素树的底层，那上层的元素只会创建一次。在下面例子，hbox只创建一次，而label将被创建多次。

    <pre>`&lt;template&gt;

    &lt;hbox&gt;

    &lt;label uri="rdf:*" value="rdf:http://home.netscape.com/NC-rdf#Name"/&gt;

    &lt;/hbox&gt;

    &lt;/template&gt;

如果template外面有包含datesources属性的元素，那内容也会生成，这个方法适用于创建静态文本和动态文本混合的元素。

## 十、皮肤

### 10.1 修饰Tree

::-moz-tree-cell:：一个单元格，可以定义边框和背景
::-moz-tree-cell-text：单元格里面的文字，可以设置字体和颜色
::-moz-tree-twisty：展开和折叠子目录时边缘的图标
::-moz-tree-image：单元格的图标，可以使用list-style-image属性进行设置
::-moz-tree-row：行，可以设置一整行的背景颜色
::-moz-tree-indentation：展开下级菜单时左边的空白缺口
::-moz-tree-column:：列
::-moz-tree-line：连接子父级的线，可用visibility: hidden;隐藏
::-moz-tree-separator：分割条
::-moz-tree-progressmeter：条目栏，你可以用type属性在列里创建progressmeter
::-moz-tree-drop-feedback：拖动反应
::-moz-tree-checkbox：设置使用检验框的列的图像