---
title: HTML代码优化
tags:
  - html
  - seo
  - web
  - 优化
id: 899
categories:
  - Web
date: 2006-10-23 01:32:04
---

![](/images/2006/10/23_2006-10-1023851983_12739.gif)

<span style="font-size: 12px;">刚刚翻硬盘发现的，这是7月初写给美工组的文档，主要针对网页制作、切图人员

其中6.5节可能因为文件覆盖而不全![](/images/2003/12/31_12731.gif)

</span>

<font size="2">HTML代码优化</font>

作者：梦游的猫

最后修改时间 2006年7月7日 16:40:37

**目录**

一. 为什么要优化？

二. 优化原则

三. 优化技巧

四. SEO

五. 关于网页标准

1.  何谓标准？

2.  使用标准价值在哪？

3.  团队合作流程

4.  如何实现

六. 实例分析

1.  删除空标签

2.  合并标签

3.  合理安排表格布局

4.  表格列表优化

5.  利用&lt;div&gt;简化代码

七. 修订记录

八. 附录

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A. 参考文档

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; B. 相关资源

* * *

<font size="3"><font color="red">**一. 为什么要优化？**</font></font>

1.  <font color="red">**节省带宽**</font>

        服务器带宽是有限制的，为了支持更多在线人数，可以租用更多带宽，也可以增加服务器，但如果能通过优化代码减小网页体积，例如减少50%，那原本支持1000人同时在线的带宽现在就可以支持2000人了，这样就节省了昂贵的带宽费用

2.  <font color="red">**利于程序员编写代码**</font>

        静态页面转为动态页面时，经常要分析结构，把重复代码改为循环，因此简洁的代码有利用提高效率

3.  <font color="red">**减少页面下载及解析时间**</font>

        虽然现在大部分都是宽带，带宽非常充足，但一般访客都不只打开一个页面，而是同时访问多个网站，加上聊天、下载等工具占用一定带宽，这样带宽就显得拥挤了，减少页面体积可以缩短下载时间，同时也能减少客户端浏览器的解析时间

<font size="3"><font color="red">**二. 优化原则**</font></font>

1.  不影响原外观

2.  不要把整个页面放到一个table里面

3.  不要嵌套太多表格，因为表格需要代码全部下载才能显示出来

4.  不要用太大、太多Flash动画

5.  不要用太多JavaScript特效，用户是来看内容不是看特效

6.  尽量把大图切成小图，可以再次利用的图片就无需再另切图了

<font size="3"><font color="red">**三. 优化技巧**</font></font>

1.  使用静态页面可以有效减少服务器资源占用

2.  重复使用的样式写到外部CSS文件，一般是整站一个共用CSS文件，各独立栏目共用一个CSS文件，某页面特有的可以写在本页面内

3.  需要从数据库读取但又不经常更换的内容，写到单独JS文件，例如各个栏目，内容是从数据库获取的，但又不会频繁更换

4.  需要链接外部的Flash、图片等，可以用iframe标签，避免外部网络延迟而拖慢整体页面显示

<font size="3"><font color="red">**四. SEO**</font></font>

SEO全称是Search Engine Optimization，即针对搜索的优化。网站在推出之后，最大的难点莫过于推广，而通过搜索引擎带来的流量是不可忽视的，如何通过优化代码使搜索引擎最大限度的收录，也就成为重要课题。

1.  明确的标题，即&lt;title&gt;标签

        例如搜索&ldquo;电脑&rdquo;，搜索结果有2个页面，一个标题包含&ldquo;电脑&rdquo;，另一个只包含在正文&lt;body&gt;标签中，第一个页面会排第一位。

        根据观察，各搜索引擎都会对标题进行截断，具体为baidu20字、google30字、Yahoo20字、MSN Search25字（均为中文）

        所以标题不能过长，要跟正文内容符合，例如一个《碟中碟》电影页面，标题可以为&ldquo;碟中碟 汤姆克鲁斯 酷娱乐 91Q.com&rdquo;

2.  META标记

        <table width="100%" cellspacing="2" cellpadding="5" bgcolor="#f7f7f7" code="" class="TBBG9">        <tbody>            <tr>                <td bgcolor="#eeeeee" class="TBBG1">&lt;meta name=&quot;description&quot; content=&quot;网页简述&quot;&gt;

                    &lt;meta name=&quot;keywords&quot; content=&quot;关键字&quot;&gt;</td>            </tr>        </tbody>    </table>
3.  链接文本

        文章、新闻经常标题过长，为了页面美观而不得不截短，加入title属性，一来有利于访客阅读完整标题，也有利于搜索引擎的收录

        <table width="100%" cellspacing="2" cellpadding="5" bgcolor="#f7f7f7" code="" class="TBBG9">        <tbody>            <tr>                <td bgcolor="#eeeeee" class="TBBG1">&lt;a href=&quot;test.htm&quot; title=&quot;浅谈Flash ActionScript代码优化&quot;&gt;浅谈Flash ActionScr&hellip;&lt;/a&gt;</td>            </tr>        </tbody>    </table>
4.  图像

        同上，相应属性为alt

5.  另外其它如&lt;h1&gt;&lt;input&gt;都可以加入关键字

        SEO还包括其它方面的内容，以上只是在代码方面的优化

<font size="3"><font color="red">**五. 关于网页标准**</font></font>

1.  <font size="4"><font color="blue">何谓标准？</font></font>

        网页标准是进2年流行的话题，简单的说，就是抛弃HTML+Table改用XHTML+CSS来写网页，做到结构和样式分离。XHTML标准分过渡型和严格型，其中过渡型可以使用table，而严格型只能用div来进行布局。

2.  <font size="4"><font color="blue">使用标准价值在哪？</font></font>

        1.  **易于维护**

                HTML代码发展到现在，已经变得十分臃肿，这对网页设计师来说是非常不利的，特别在重构大型网站的时候，经常显得力不从心

        2.  **加速开发**

                通过结构与表现层分离，可以优化团队工作流程，提高工作效率

        3.  **拓展访问渠**

                不需做大改动即可在PDA等掌上设备浏览

        4.  **节约带宽成本

                **一般运用网页标准进行网站重构之后，代码可以减少一半以上，每页面如果能减少几十K，乘以访问量，一个月下来，就可以节约昂贵的带宽费用了

        5.  **提高用户体验**

                干净简介的代码，总比臃肿复杂代码显示得快，用户不必再忍受缓慢的访问速度
3.  <font size="4"><font color="blue">团队合作流程</font></font>

        ![](/blog/upload/2006/10/23/002724.gif)

4.  <font size="4"><font color="blue">如何实现</font></font>

        1.  XHTML标签和属性全部要小写，比如&lt;BODY&gt;&l
t;HEAD&gt;&lt;BR /&gt;

        2.  标签注意要关闭，常见的比如&lt;br /&gt;&lt;input /&gt;等，还有&lt;img src=&quot;&quot; alt=&quot;&quot; /&gt;，别把alt属性忘记了

        3.  不用&lt;font&gt;标签，可以用&lt;span&gt;标签加CSS定义

        4.  XHTML不具备HTML标签所有属性和值，例如居中align='absmiddle'，在XHTML里面是没有absmiddle值的，只有top/bottom/right/left/center，某些属性例如onload也被废除了

        5.  公用函数&lt;script&gt;&lt;/script&gt;放在&lt;head&gt;&lt;/head&gt;之间，共有的写在单独文件再在网页导入

        6.  尽量用div布局而不用table，但table还是可以用的

<font size="3"><font color="red">**六. 实例分析**</font></font>

1.  **删除空标签**

        &lt;td height=&quot;23&quot;&gt;&lt;div align=&quot;left&quot; class=&quot;lineh15 style4&quot;&gt; &lt;a href=&quot;#&quot;&gt;剧情看点： &lt;/a&gt;&lt;%=exStr1%&gt;&lt;/div&gt;&lt;div align=&quot;left&quot;&gt;&lt;/div&gt;&lt;/td&gt;

        &lt;div align=&quot;left&quot;&gt;&lt;/div&gt; 是可以去掉的

2.  **合并标签**

        &lt;td height=&quot;23&quot;&gt;&lt;div align=&quot;left&quot;&gt;&lt;strong&gt;&lt;%=sShowName%&gt;&lt;/strong&gt;&lt;/div&gt;&lt;/td&gt;

        div太多，不仅影响浏览器解析，还可能因为未关闭标签造成错乱，也给编辑带来困难，例如用DW打开，如下图

        ![](/blog/upload/2006/10/23/002931.png)

        表格和div密密麻麻的N多线

        &lt;div align=&quot;left&quot;&gt; 的作用是左对其，其实在表格设置水平左对其也一样，修改后为

        &lt;td height=&quot;23&quot; align=&quot;left&quot;&gt;&lt;strong&gt;&lt;%=sShowName%&gt;&lt;/strong&gt;&lt;/td&gt;

3.  **合理安排表格布局**

        ![](/blog/upload/2006/10/23/003023.jpg)

        上图红色表示表格，绿色表示div层，可以看出里面嵌套了4层表格，同时也用了div来使文字靠左，其实&lt;td&gt;标签也有水平位置的align属性，而且表格默认就是左对齐，所以div标签完全可以取消，整理一下，结果如下图

        ![](/blog/upload/2006/10/23/003052.jpg)

4.  **表格列表优化**

        ![](/blog/upload/2006/10/23/003207.png)

        如上图，红色表示表格，绿色是DIV，可以看出套了很多表格，整理一下所要的效果，无非是外面圆角边框加里面数据，其中数据又分上下两部分，上面是图片加文字，而下面是文字。

        下图是优化后，可以看出只有2个表格嵌套，结构很简洁。

        ![](/blog/upload/2006/10/23/003418.png)

        <font color="blue">优化过程：</font>

        1.  将【本类排行】表格，放到外面表格中，作为一行而不是单独表格2.  排行第一的电影，原先用了几个表格嵌套，现改用div将图片浮动左边，文字会自动插入到右边空白处3.  下面的2－10排行不再用表格分割，改用&lt;li&gt;标签，再用css进行修饰
        <font color="blue">代码片段：</font>

        **CSS部分**

        <table width="100%" cellspacing="2" cellpadding="5" bgcolor="#f7f7f7" code="" class="TBBG9">        <tbody>            <tr>                <td bgcolor="#eeeeee" class="TBBG1">#test{

                    &nbsp;color:#000000;

                    &nbsp;list-style:none;

                    &nbsp;line-height:200%;

                    &nbsp;margin:5px&nbsp;0&nbsp;0&nbsp;10px;

                    }

                    #test&nbsp;li{

                    &nbsp;border-bottom:1px&nbsp;dotted&nbsp;#FFFFFF;

                    }</td>            </tr>        </tbody>    </table>
        **排行列表**

        <table width="100%" cellspacing="2" cellpadding="5" bgcolor="#f7f7f7" code="" class="TBBG9">        <tbody>            <tr>                <td bgcolor="#eeeeee" class="TBBG1">&lt;li&gt;2.新精武门&lt;/li&gt;

                    &lt;li&gt;3.赌圣2之街头赌圣&lt;/li&gt;

                    &lt;li&gt;4.流氓状元&lt;/li&gt;

                    &lt;li&gt;5.韩2005爆笑性喜..&lt;/li&gt;

                    &lt;li&gt;6.我的野蛮本色&lt;/li&gt;

                    &lt;li&gt;7.捣蛋三人组-整蛊王&lt;/li&gt;

                    &lt;li&gt;8.笑林小子2之乌龙院&lt;/li&gt;

                    &lt;li&gt;9.妓女竞选总统记&lt;/li&gt;

                    &lt;li&gt;10.一见钟性&lt;/li&gt;</td>            </tr>        </tbody>    </table>
        优化后代码字数为原来32%

5.  **利用&lt;div&gt;简化代码&nbsp; (该节不全)**

        ![](/blog/upload/2006/10/23/003449.jpg)

        如上图，五部影片所在的表格，先是用&lt;td&gt;分割，再用&lt;div&gt;定义样式（主要是定义字体大小），其中影片截图，为了显示圆边框，又套了一个表格，表格采用九宫格布置。单个电影代码如下：

        <table width="100%" cellspacing="2" cellpadding="5" bgcolor="#f7f7f7" code="" class="TBBG9">        <tbody>            <tr>                <td bgcolor="#eeeeee" class="TBBG1">&lt;td&nbsp;height=&quot;52&quot;&nbsp;align=&quot;center&quot;&nbsp;class=&quot;lineh15&quot;&gt;&lt;div&nbsp;align=&quot;left&quot;&nbsp;class=&quot;size14&quot;&gt;

                    &nbsp;&nbsp;&lt;table&nbsp;border=&quot;0&quot;&nbsp;cellspacing=&quot;0&quot;&nbsp;cellpadding=&quot;0&quot;&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/ttl.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;background=&quot;http://www.21ido.com/bbs/images/tt.jpg&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tt.jpg&quot;&nbsp;width=&quot;1&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/ttr.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;background=&quot;http://www.21ido.com/bbs/images/tl.jpg&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tl.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;1&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;width=&quot;65&quot;&nbsp;height=&quot;75&quot;&gt;&lt;a&nbsp;href=&quot;mo
vie2.jsp?sTypeId=164&amp;sPgmId=176&quot;&gt;&lt;img&nbsp;src=&quot;http://cqmovie.bqol.com.cn:8092/logo/凤凰大视野：天空铁路01.jpg&quot;&nbsp;width=&quot;92&quot;&nbsp;height=&quot;124&quot;&nbsp;border=&quot;0&quot;&gt;&lt;/a&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;background=&quot;http://www.21ido.com/bbs/images/tr.jpg&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tr.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;1&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tbl.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;background=&quot;http://www.21ido.com/bbs/images/tb.jpg&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tb.jpg&quot;&nbsp;width=&quot;1&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;img&nbsp;src=&quot;http://www.21ido.com/bbs/images/tbr.jpg&quot;&nbsp;width=&quot;6&quot;&nbsp;height=&quot;6&quot;&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;tr&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;td&nbsp;class=&quot;lineh15&quot;&nbsp;colspan=&quot;3&quot;&gt;&lt;div&nbsp;align=&quot;center&quot;&gt;&lt;span&nbsp;class=&quot;title-16px120b-B00000&quot;&gt;&lt;a&nbsp;href=&quot;movie2.jsp?sTypeId=164&amp;sPgmId=176&quot;&gt;北纬17度线&lt;/a&gt;&lt;/span&gt;&lt;/div&gt;&lt;/td&gt;

                    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/tr&gt;

                    &nbsp;&nbsp;&lt;/table&gt;

                    &lt;/div&gt;&lt;/td&gt;</td>            </tr>        </tbody>    </table>

<font size="3"><font color="red">**七. 修订记录**</font></font>

*   2006年7月3日 星期三 创建文档，整理《实例分析》1．、2．、3． 实例*   2006年7月4日 星期四 《实例分析》增加4．实例，撰写《优化原则》、《优化技巧》等章节*   2006年7月7日 星期五 撰写《SEO》、《关于网页标准》章节

<font size="3"><font color="red">**八. 附录**</font></font>

A. 参考文档

 《优化网页代码提高网页访问速度》作者：Cheney

 《网页Title的优化的原则》

B. 相关资源

 《Robots.txt指南》 [http://www.17google.com.cn/article.asp?articleid=404](http://www.17google.com.cn/article.asp?articleid=404)

&nbsp; 网页设计师 [http://www.w3cn.org](http://www.w3cn.org/)