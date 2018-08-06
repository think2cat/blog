---
title: XUL学习笔记(3)
tags:
  - XUL
id: 878
categories:
  - Web
abbrlink: 7069078b
date: 2006-03-29 23:56:20
---

该节讲的是扩展开发的，代码很多，这里跳过，简单说一下就好了，哈

### 三、目录和文件结构

#### 3.1 目录结构

一个标准的扩展程序，解压后有几个目录
* **Chrome**：必备目录，底下有一个JAR文件，保存着扩展外观和逻辑功能代码。解压缩此 JAR 文件之后，一般会生成以下 3 个目录。
  * **content**：用于存储负责描述扩展界面的 XUL 文件和完成实际逻辑功能的 JS 文件；
  * **locale**：录下还会有针对不同语言的子目录，这些子目录会被起成如"en-US"，"zh-CN"这种用来区分"语言-国家/地区" 的名称。通过这种国际上标准的语言区分方式，Mozilla 会根据其自身的语言，选择一个最合适的语言目录让 content 中的文件进行引用；
  * **skin**：用于存储负责美化界面外观的样式表文件和图片文件，这些文件中的样式和图片会被 content 目录中的文件所引用，如"classic"，"modern"等。不过，一般情况下，我们只创建针对 classic 的"皮肤"。如果扩展没有使用单独的样式表文件和图片，那么它也是可以被省略的；
* **Components**：存放自定义XPCOM组件，在没自定义XPCOM组件情况下，改目录不存在
* **defaults**：存放默认数据，其下还有子目录

另外，有3个必备的特殊文件
* **install.rdf**：它是一个 RDF/XML 格式的文件，用于描述当前扩展的注册信息和附加信息等。扩展在安装时，负责安装扩展的程序会自动分析此文件的信息，然后将这些信息注册到 Mozilla 系统下。此文件必须被命名为 install.rdf，并置于扩展压缩包的顶级目录下；
* **install.js**：负责安装扩展的脚本，此文件可选。一般情况下，install.rdf 完全可以胜任扩展的安装注册工作。但是，如果有些扩展要在安装时做一些额外的准备工作，则要通过一个称为 XPInstall 的机制来完成，那些负责额外工作的代码则要被固定地写到此文件中；
* **chrome.manifest**：负责将扩展的各种包注册到 Mozilla 的 chrome 系统中。Gecko 1.8 内核新引入的机制，用来代替原有的 contents.rdf 文件；
<!--more-->
#### 3.2 contents.rdf 文件

content，locale，skin 这三个目录都被称为包（package），包是一组文件集合，包注册到Mozilla系统后，就可以通过chrome地址协议进行访问，包可以包含任何类型的文件，包通常是jar文件，但也可以是目录。

contents.rdf文件就是用来分别描述这些包，包括每种包的结构和功能，当安装扩展时，系统会分析这些contents.rdf文件，根据内容注册到Mozilla系统中。

基于Gecko 1.8内核的Mozilla程序采用一种新的方式来进行包的注册，所以contents.rdf 其实已经被废弃掉了。新的注册是通过chrome目录下chrome.manifest完成同样功能，它是一个纯文本文件，例如
```
content contact file:content/
locale contact en-US file:locale/en-US/
skin contact contact skin/
```

但为了兼容Gecko 1.8以前的版本，还是建议在扩展中使用 contents.rdf 文件格式。

#### 3.2.1 适用于 content 包的 contents.rdf 文件

示例格式如下：
```xml
<?xml version="1.0"?>
<RDF:RDF xmlns:RDF="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
         xmlns:chrome="http://www.mozilla.org/rdf/chrome#">

  <!-- list all the packages being supplied by this directory -->
  <RDF:Seq about="urn:mozilla:package:root">
    <RDF:li resource="urn:mozilla:package:sampleext"/>
  </RDF:Seq>

  <!– package information –>
  <RDF:Description RDF:about="urn:mozilla:package:sampleext" chrome:name="sampleext">
  <!–  additionally for Mozilla Suite and old Firefox/Thunderbird versions include:
        chrome:extension="true"
        chrome:displayName="Sample Extension"
        chrome:author="Your Name Here"
        chrome:authorURL="http://sampleextension.mozdev.org/"
        chrome:description="A sample extension with advanced features"
        chrome:settingsURL="chrome://sampleext/content/settings.xul" –>
  </RDF:Description>

  <!– overlay information –>
  <RDF:Seq about="urn:mozilla:overlays">
    <RDF:li resource="chrome://browser/content/browser.xul"/>
  </RDF:Seq>

  <RDF:Seq about="chrome://browser/content/browser.xul">
    <RDF:li>chrome://sampleext/content/overlay.xul</RDF:li>
  </RDF:Seq>
</RDF:RDF>
```
下面对以上的一些格式做解释说明，先看一下它的文件头部。
```xml
<?xml version="1.0"?>
<RDF:RDF xmlns:RDF="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
         xmlns:chrome="http://www.mozilla.org/rdf/chrome#">
```
上面这一段格式相对固定，它主要是用来引入 RDF 和 chrome 命名空间。接下来
```xml
<!-- list all the packages being supplied by this directory -->
<RDF:Seq about="urn:mozilla:package:root">
  <RDF:li resource="urn:mozilla:package:sampleext"/>
</RDF:Seq>
```
这里的 package 和 sampleext 都需要注意。首先，package 用来说明它描述的是一个 content 类型的包。对于 locale 和 skin 类型的包，应把它替换成 locale 和 skin，虽然这看上去与包目录的命名有些不一致。sampleext 是用来唯一标识扩展的名称，Mozilla 系统靠它来识别是哪个扩展的注册信息。后面还有好几处出现了同样的内容，你都需要注意。对于 locale 和 skin 则不能写能 locale:sampleext 或 skin:sampleext 这种"类推"出来的格式，它们的格式后面做说明。

文件的中间部分

```xml
<RDF:Description RDF:about="urn:mozilla:package:sampleext" chrome:name="sampleext">
<!–  additionally for Mozilla Suite and old Firefox/Thunderbird versions include:
      chrome:extension="true"
      chrome:displayName="Sample Extension"
      chrome:author="Your Name Here"
      chrome:authorURL="http://sampleextension.mozdev.org/"
      chrome:description="A sample extension with advanced features"
      chrome:settingsURL="chrome://sampleext/content/settings.xul" –>
</RDF:Description>
```

`<!– –>` 标识中的内容已经说明，它是适用于 Mozilla Suite 和老版本 Firefox/Thunderbird 的附加信息，所以对于内核较老的 Mozilla 必须被写成：
```xml
<RDF:Description RDF:about="urn:mozilla:package:sampleext" chrome:name="sampleext">
      chrome:extension="true"
      chrome:displayName="Sample Extension"
      chrome:author="Your Name Here"
      chrome:authorURL="http://sampleextension.mozdev.org/"
      chrome:description="A sample extension with advanced features"
      chrome:settingsURL="chrome://sampleext/content/settings.xul">
</RDF:Description>
```

对于新版本的 Mozilla，`<!– –>` 中的内容可以忽略。这段内容主要用来描述扩展的一些附加信息，如作者和扩展的显示名称等。其实在 install.rdf 文件中，存在同样的一段描述信息。因而，这里再做描述显得有些罗嗦，作者也不建议你在 contents.rdf 中加入那些附加的信息。

文件的结尾部分
```xml
<RDF:Seq about="urn:mozilla:overlays">
  <RDF:li resource="chrome://browser/content/browser.xul"/>
</RDF:Seq>

<RDF:Seq about="chrome://browser/content/browser.xul">
  <RDF:li>chrome://sampleext/content/overlay.xul</RDF:li>
</RDF:Seq>
```
这是 content 类型的 contents.rdf 文件所描述的关键部分。Mozilla 下的扩展之所以能够和浏览器整合在一起，像一个程序一样工作，是因为它采用了一种被称为"覆盖（Overlays）"的技术。你的扩展可以在 Mozilla 的某个已有界面上，再组合上另外的 XUL 界面元素，而不会与之产生任何的冲突和不协调。这种技术与其说是"覆盖"，不如说是"合并"更合适。因为，原有的界面元素会根据加上去的界面元素自动调整自己的位置，以适应变化。同时,"覆盖"技术还为扩展程序的运行提供了"入口点"，这也正是我们编写的扩展能够运行的原因。

在上面的内容中，第一个 `RDF:Seq` 标记指明要对 Mozilla 的界面进行覆盖；而要对哪些界面进行覆盖，则通过它的子标记 RDF:li 标记进行描述，如：
```
<RDF:li resource="chrome://browser/content/browser.xul"/>
```
它意思是要覆盖负责描述浏览器界面的 browser.xul 文件，这个文件就是通过 chrome 地址协议进行指定的，后面你还将看到在许多情况下，我们都需要通过 chrome 地址协议来访问注册到 Mozilla 系统下的包资源。如果你还想覆盖其它的界面文件，你只需像上面一样指定多个 RDF:li 标识。

第二个 `RDF:Seq` 对那些被覆盖的文件做更进一步的描述，它描述了指定的目标文件会被当前包下的哪几个文件所覆盖。如：
```xml
<RDF:Seq about="chrome://browser/content/browser.xul">
  <RDF:li>chrome://sampleext/content/overlay.xul</RDF:li>
</RDF:Seq>
```
意思是用 overlay.xul 覆盖 browser.xul 文件，并且 overlay.xul 也是以 chrome 方式进行定位的，或者说是以已经注册后的地址进行定位的。如果还有更多的界面文件要覆盖到 browser.xul 上，照上面格式书写即可。

#### 3.2.2 适用于 locale 包的 contents.rdf 文件

示例格式如下：
```xml
<?xml version="1.0"?>
<RDF:RDF xmlns:chrome="http://www.mozilla.org/rdf/chrome#"
         xmlns:RDF="http://www.w3.org/1999/02/22-rdf-syntax-ns#">

  <RDF:Seq about="urn:mozilla:locale:root">
     <RDF:li resource="urn:mozilla:locale:en-US"/>
  </RDF:Seq>

  <RDF:Description about="urn:mozilla:locale:en-US"
          chrome:author="Me"
          chrome:displayName="English(US)"
          chrome:name="en-US">
    <chrome:packages>
      <RDF:Seq about="urn:mozilla:locale:en-US:packages">
        <RDF:li resource="urn:mozilla:locale:en-US:sampleext"/>
      </RDF:Seq>
    </chrome:packages>
  </RDF:Description>
</RDF:RDF>
```
我们可以看到，locale 包的 contents.rdf 文件的与 content 包的 contents.rdf 文件没有什么太大的区别，只有文件前面的 `RDF:li`，其 resource 中写的是 locale:en-US，注意区别前面 content 中的 package:sampleext。后面的 RDF:Description 格式相对固定，只是你要注意将 RDF:li 中的 sampleext 替换成与上面一致的扩展名称。另外，由于这一类型的 contents.rdf 是用来描述语言包的，所以，你必须要处理好相应的语言描述信息。

#### 3.2.3 适用于 skin 包的 contents.rdf 文件

示例格式如下：
```xml
<?xml version="1.0"?>
<RDF:RDF xmlns:RDF="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
         xmlns:chrome="http://www.mozilla.org/rdf/chrome#">

  <RDF:Seq about="urn:mozilla:skin:root">
    <RDF:li resource="urn:mozilla:skin:classic/1.0" />
  </RDF:Seq>

  <RDF:Description about="urn:mozilla:skin:classic/1.0">
    <chrome:packages>
      <RDF:Seq about="urn:mozilla:skin:classic/1.0:packages">
        <RDF:li resource="urn:mozilla:skin:classic/1.0:sampleext" />
      </RDF:Seq>
    </chrome:packages>
  </RDF:Description>
</RDF:RDF>
```
我们可以看到，它与 locale 包的 contents.rdf 文件差不多，格式也很相近，只是 `RDF:Description` 标记中的属性少了一些。因为，这两种资源的描述几乎是采用一致的格式进行注册的，你只要注意 skin:classic/1.0 即可。

以上只是给出一些标准的格式，随着你对扩展开发的深入，你会发现一些特殊的格式及应用。另外，由于这个文件是 XML 的，所以作者提醒你保存成不带文件头标记（BOM）的 UTF-8 格式，并且建议你在创建完此文件之后，用浏览器再验证一下文件的格式和编码问题。

#### 3.3 install.rdf 文件

install.rdf 文件位于扩展的顶级目录下，用于描述当前扩展的作者信息，升级地址，设置入口，版本兼容信息等。在安装扩展时，Mozilla 会分析其中的信息。它的标准格式如下：
```xml
<?xml version="1.0"?>
<RDF xmlns="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
     xmlns:em="http://www.mozilla.org/2004/em-rdf#">

    <Description about="urn:mozilla:install-manifest">
        <em:id>{XXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}</em:id>
        <em:name>Sample Extension</em:name>
        <em:version>1.0</em:version>
        <em:type>2</em:type>
        <!-- optional items -->
        <em:creator>Your Name Here</em:creator>
        <em:description>A sample extension with advanced features</em:description>
        <em:contributor>A person who helped you</em:contributor>
        <em:contributor>Another one</em:contributor>
        <em:homepageURL>http://sampleextension.mozdev.org/</em:homepageURL>
        <em:updateURL>http://sampleextension.mozdev.org/update.rdf</em:updateURL>
        <em:optionsURL>chrome://sampleext/content/settings.xul</em:optionsURL>
        <em:aboutURL>chrome://sampleext/content/about.xul</em:aboutURL>
        <em:iconURL>chrome://sampleext/skin/mainicon.png</em:iconURL>

        <!-- Firefox -->
        <em:targetApplication>
            <Description>
                <em:id>{ec8030f7-c20a-464f-9b0e-13a3a9e97384}</em:id>
                <em:minVersion>0.9</em:minVersion>
                <em:maxVersion>1.5.0.1</em:maxVersion>
            </Description>
        </em:targetApplication>

         <!-- This is not needed for Firefox 1.1 and later. Only include this
              if you want to make your extension compatible with older versions -->
        <em:file>
            <Description about="urn:mozilla:extension:file:sampleext.jar">
                <em:package>content/</em:package>
                <!-- optional items -->
                <em:skin>skin/classic/</em:skin>
                <em:locale>locale/en-US/</em:locale>
                <em:locale>locale/zh-CN/</em:locale>
            </Description>
        </em:file>
    </Description>
</RDF>
```
我们先看一下此文件的头部分：
```xml
<?xml version="1.0"?>
<RDF xmlns="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
     xmlns:em="http://www.mozilla.org/2004/em-rdf#">
```
同 contents.rdf 文件的头部分一样，它也是负责引入命名空间。而位于头部分下面的 Description 标记，则是此文件的实质性描述。下面对 Description 下的子标功能给予说明。

#### 3.3.1 必需的标记

**em:id**

它用来唯一的标识某个扩展，此标记内容的要求在 Firefox 1.5 版本的前后还发生了变化。在 1.5 以前的版本中，em:id 标记的内容被要求必须以 GUID 格式进行表示；而在 1.5 及后续版本中，你还可以使用一种格式为 extension@domain 的 id。如下示例了两种不同的格式：
```
<em:id>myextension@mysite.com</em:id>
<em:id>{daf44bf7-a45e-4450-979c-91cf07434c3d}</em:id>
```
对于 GUID 格式的 id，我们可以通过一些工具来生成。在 Windows 下，我们可以用微软的 guidgen.exe 工具来生成。如果你在类 Unix 的系统下做开发，你可以用系统自带的 uuidgen 工具来生成。

**em:name**

此标记的内容会被显示在扩展管理器中。它被允许由多个单词组成，且中间可以包含空格。

**em:version**

此标记的内容是一个版本字符串，用来表示扩展的当前版本，它会被显示在扩展管理器中。示例如下：
```xml
<em:version>2.0</em:version>
<em:version>1.0.2</em:version>
<em:version>0.4.1.2005090112</em:version>
```
同时，Mozilla 还对版本字符串的格式做出了规定。如果这对这方面感兴趣，请你查阅 Mozilla 文档。

**em:type**

从 Firefox 1.5 版本以后，此标记被引入，它主要用来表示当前安装包的安装类型 (注意不要和包 [package] 弄混）。其实，在 Firefox 的 Theme 包结构里，同样要用 install.rdf 文件来描述其安装信息。而安装程序在处理安装时，要根据安装包的类型做不同的处理，此标记正是为区别安装包的类型设计的。
它的内容是一个整型值，并且只允许以下几个值出现：

  2	Extensions
  4	Themes
  8	Locale
  16	Plugin

对于扩展类型的安装包，type 标记的内容显然要被指定为 2。

**em:targetApplication**

此标记及其子标记用来指明所适用的 Mozilla 平台。由于你开发的扩展可能适用多个 Mozilla 平台，所以你需要确切地指定它所适用的平台。通过它的 `em:id` 子标记，可以指明所适用的平台；通过 `em:minVersion`，`em:maxVersion` 子标记可以进一步所适用平台的版本范围。需要注意的是，如果你编写的扩展在安装时弹出了禁止安装的窗口，多半是由于 `em:targetApplication` 书写有问题造成的，特别是 `em:maxVersion` 设置过低的问题。

如果你编写的扩展适用于多个平台，你可以通过书写多个 em:targetApplication 标记来进行限定。下面将一些常用的 Mozilla 平台与 id 之前的对应关系列出：


Firefox|{ec8030f7-c20a-464f-9b0e-13a3a9e97384}
:--|:--
Thunderbird|{3550f703-e582-4d05-9a08-453d09bdfdc6}
Nvu|{136c295a-4a5a-41cf-bf24-5cee526720d5}
Mozilla Suite|{86c18b42-e466-45a9-ae7a-9b95ba6f5640}
SeaMonkey|{92650c4d-4b8e-4d2a-b7eb-24ecf4f6b63a}
Sunbird|{718e30fb-e89b-41dd-9da7-e25a45638b28}
Netscape Browser|{3db10fab-e461-4c80-8b97-957ad5f8ea47}
Flock Browser|{a463f10c-3994-11da-9945-000d60ca027b}

#### 3.3.2 可选的标记

**em:creator**

此标记的内容用来指明扩展作者的名字。如果你没有提供下面的 `em:aboutURL` 标记，它将被显示在 Firefox 预定义的 about 窗口中。

**em:description**

此标记的内容用来描述扩展的功能或特性，它会被显示在扩展管理器中。但是，建议你在书写此内容时尽量简明扼要。

**em:contributor**

此标记的内容用来指明扩展贡献者的名字。如果你在扩展开发过程中，还得到了其他的人帮助，或者用到了其他人的源代码，请在此处写明贡献者的信息。并且，此标记允许存在多个，而 `em:creator` 标记只允许存在一个。如果你没有提供下面的 `em:aboutURL` 标记，它将被显示在 Firefox 预定义的 about 窗口中。

**em:homepageURL**

此标记的内容用来指明扩展或作者的主页地址。

**em:updateURL**

基于 Mozilla 扩展的最大好处就是可以自动升级，此标记的内容正是用来指明版本检测时的 URL 地址。Mozilla 给扩展提供这样一种升级机制，它通过强制方式或定期检测方式请求升级描述文件，从而获取到有关扩展自身的升级信息，进而对扩展进行升级操作。

为了配合扩展的升级检测操作，Mozilla 还提供了一些环境变量。虽然一般情况下，我们书写的升级检测地址都是固定的，但 Mozilla 提供了一些特殊的环境变量。通过这些环境变量，Mozilla 可以对不同的扩展生成不同的升级检测地址。下面只是一个示例：
```
<em:updateURL>http://www.foo.com/update.cgi?id=%ITEM_ID%&amp;version=%ITEM_VERSION%</em:updateURL>
```
有关这些环境变量的详细用法，请到 Mozilla 网址查阅。

**em:optionsURL**

此标记的内容用来指明当前扩展的选项设置入口，并且要以 chrome:// 地址方式进行指定。由于扩展的一些选项值要由用户来设置，所以扩展的作者有必要为扩展提供一个设置窗口。在扩展管理器中，通过"右键/设置"菜单项，我们可以对不同的扩展打开不同的设置窗口，这就是此标记完成的功能。但前提是，扩展作者必须要为所开发的扩展编写设置窗口，否则此标记不是必须的。

**em:aboutURL**

此标记的内容用来指明自定义"about（关于）"窗口的地址。同 `em:optionsURL` 标记一样，你必须编写并以 chrome:// 地址方式来提供此信息。如果你没有自定义 about 窗口，Firefox 会调用预定义的 about 窗口，并把以上的 em:name，em:version，em:creator，em:description，em:contributor 等信息显示在里面。

**em:iconURL**

此标记的内容用来指明扩展在扩展管理器中显示的图标，它同样要以 chrome:// 地址方式提供。虽然是图标，但它却经常用 PNG 格式的文件来保存。如果你没有指定自己的图标，Firefox 将采用预定义的图案来表示你的扩展。

**em:targetPlatform**

从 Firefox 1.5 版本以后，此标记被引入，它主要用来限制所运行的操作系统（OS）类型。由于某些扩展有二进制兼容的限制，所以就要限制所安装到的 OS 平台。关于此标记的详细用法，请查阅 Mozilla 网站的资料。

#### 3.3.3 已经废弃的标记

**em:file**

从 Firefox 1.5 版本以后，此标记就被废弃了，这是由于引入了 chrome.manifest 文件。有关此标记的解释，作者将其放到了"扩展安装的实现原理"一节。