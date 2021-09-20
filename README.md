# MarkDown的语法

## 1. 快捷键

&#8195;功能|&#8195;快捷键
:-:|:-:
&#8195;加粗|&#8195;Ctrl + B
&#8195;斜体|&#8195;Ctrl + I
&#8195;引用|&#8195;Ctrl + Q
&#8195;插入链接|&#8195;Ctrl + L
&#8195;插入代码|&#8195;Ctrl + K
&#8195;插入图片|&#8195;Ctrl + G
&#8195;提升标题|&#8195;Ctrl + H
&#8195;有序列表|&#8195;Ctrl + O
&#8195;无序列表|&#8195;Ctrl + U
&#8195;横线|&#8195;Ctrl + R
&#8195;撤销|&#8195;Ctrl + Z
&#8195;重做|&#8195;Ctrl + Y

## 2. 基本语法

### 2.1 字体设置斜体、粗体、删除线  

```  
    *这里是文字*  
    _这里是文字_  
    **这里是文字**  
    ***这里是文字***  
    ~~这里是文字~~  
```    

&#8195;![](https://img-blog.csdn.net/20180802154402427?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 2.2 分级标题

写法1：

```
    # 一级标题  
    ## 二级标题  
    ### 三级标题  
    #### 四级标题  
    ##### 五级标题  
    ###### 六级标题  
```  

这个写法和  `**文字**`  效果是一样的   

&#8195;![](https://img-blog.csdn.net/2018080215454373?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 2.3 链接

#### （1）插入本地图片链接  

语法规则，有两种写法： 

&#8195;![](https://img-blog.csdn.net/20180802155057285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

注意：这个图片描述可以不写。  
示例图如下：  

&#8195;![](https://img-blog.csdn.net/20180802155239626?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （2）插入互联网上图片

语法规则： 

&#8195;![](https://img-blog.csdn.net/20180802155336302?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

注意：这个图片描述可以不写。  
示例如下：  

&#8195;![](https://img-blog.csdn.net/20180802155413115?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （3）自动连接

Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用<>包起来，Markdown 就会自动把它转成链接。也可以直接写，也是可以显示成链接形式的   
例如：
 
&#8195;![](https://img-blog.csdn.net/20180802155459346?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

### 2.4 分割线

你可以在一行中用三个以上的星号(*)、减号(-)、底线(_)来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。  

&#8195;![](https://img-blog.csdn.net/20180802155556110?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 2.5 代码块

对于程序员来说这个功能是必不可少的，插入程序代码的方式有两种，一种是利用缩进(tab), 另一种是利用英文`  ``  `符号（一般在ESC键下方，和~同一个键）包裹代码。    

#### （1）代码块：

缩进 4 个空格或是 1 个制表符。效果如下：  

&#8195;![](https://img-blog.csdn.net/20180802155701855?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)    

#### （2）行内式：

如果在一个行内需要引用代码，只要用反引号`引起来就好（一般在ESC键下方，和~同一个键）  

&#8195;![](https://img-blog.csdn.net/20180802155907712?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

#### （3）多行代码块与语法高亮：

在需要高亮的代码块的前一行及后一行使用三个单反引号````包裹，就可以了。  
示例如下：  

&#8195;![](https://img-blog.csdn.net/20180802160015673?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （4）代码块里面包含html代码   

在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，剩下的 Markdown 都会帮你处理。  

注意：简书代码块里不支持html。  
示例如下：  

&#8195;![](https://img-blog.csdn.net/20180802160155483?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 2.6 引用

在被引用的文本前加上>符号，以及一个空格就可以了，如果只输入了一个>符号会产生一个空白的引用。 

#### （1）基本使用  

使用如下图所示： 

&#8195;![](https://img-blog.csdn.net/20180802160324418?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （2）引用的嵌套使用  

使用如图所示：  

&#8195;![](https://img-blog.csdn.net/20180802160343802?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （3）引用其它要素  

引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等。  
使用如图所示：  

&#8195;![](https://img-blog.csdn.net/20180802160421785?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

### 2.7 列表  

#### （1）无序列表

使用 *，+，- 表示无序列表。  

注意：符号后面一定要有一个空格，起到缩进的作用。  

&#8195;![](https://img-blog.csdn.net/2018080216053733?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （2）有序列表

使用数字和一个英文句点表示有序列表。 

注意：英文句点后面一定要有一个空格，起到缩进的作用。

&#8195;![](https://img-blog.csdn.net/2018080216061165?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （3）无序列表和有序列表同时使用

&#8195;![](https://img-blog.csdn.net/20180802160735370?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （4）列表和其它要素混合使用

列表不光可以单独使用，也可以使用其他的 Markdown 语法，包括标题、引用、代码区块等。

注意事项：  
（1）加粗效果不能直接用于列表标题里面，但是可以嵌套在列表里面混合使用。  
（2）列表中包含代码块（前面加2个tab或者8个空格，并且需要空一行，否则不显示）。

使用示例如下图：  

&#8195;![](https://img-blog.csdn.net/20180802160851570?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （5）注意事项

在使用列表时，只要是数字后面加上英文的点，就会无意间产生列表，比如2017.12.30 这时候想表达的是日期，有些软件把它被误认为是列表。解决方式：在每个点前面加上\就可以了。如下图所示：  

&#8195;![](https://img-blog.csdn.net/20180802161039348?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 2.8 表格

表格的基本写法很简单，就跟表格的形状很相似：  

&#8195;![](https://img-blog.csdn.net/20180802161209660?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

表格对齐方式：我们可以指定表格单元格的对齐方式，冒号在左边表示左对齐，右边表示有对齐，两边都有表示居中。  
如下图所示：  

&#8195;![](https://img-blog.csdn.net/20180802161235917?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

## 3. 常用技巧

### 3.1 换行

方法1: 连续两个以上空格+回车  
方法2：使用html语言换行标签：

### 3.2 缩进字符

不断行的空白格   或  半角的空格   或  全角的空格   或    

&#8195;![](https://img-blog.csdn.net/20180802162415963?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 3.3 特殊符号

#### （1）对于 Markdown 中的语法符号，前面加反斜线\即可显示符号本身。

示例如下：  

&#8195;![](https://img-blog.csdn.net/20180802162507298?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### （2）其他特殊字符，示例如下：

&#8195;![](https://img-blog.csdn.net/20180802162542616?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

想知道字符对应的Unicode码，可以看这个网站：https://unicode-table.com/cn/  

附上几个工具对特殊字符的支持的对比图:  

&#8195;![](https://img-blog.csdn.net/20180802162726962?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 3.4 字体、字号与颜色

Markdown是一种可以使用普通文本编辑器编写的标记语言，通过类似HTML的标记语法，它可以使普通文本内容具有一定的格式。但是它本身是不支持修改字体、字号与颜色等功能的！  
CSDN-markdown编辑器是其衍生版本，扩展了Markdown的功能（如表格、脚注、内嵌HTML等等）！对，就是内嵌HTML，接下来要讲的功能就需要使用内嵌HTML的方法来实现。  

字体，字号和颜色编辑如下代码   

&#8195;![](https://img-blog.csdn.net/20180802162832307?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

具体颜色分类及标记请看下表： 

&#8195;![](https://img-blog.csdn.net/20180802162907453?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 3.5 链接的高级操作

链接的高级操作（这个需要掌握一下，很有用） 

#### 1.行内式

这个在上文第二条基本语法的 链接这个小节已经过，这里就不继续讲解了。 

#### 2.参考式链接

在文档要插入图片的地方写![图片或网址链接][标记]，在文档的最后写上[标记]:图片地址 “标题”。（最后这个”标题”可以不填写）   
示例如下：  

&#8195;![](https://img-blog.csdn.net/2018080216303854?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)   

#### 3.内容目录

在段落中填写 [TOC] 以显示全文内容的目录结构。  

#### 4.锚点

锚点其实就是页内超链接。比如我这里写下一个锚点，点击回到目录，就能跳转到目录。 在目录中点击这一节，就能跳过来。  

注意：在简书中使用锚点时，点击会打开一个新的当前页面，虽然锚点用的不是很舒服，但是可以用注脚实现这个功能。

语法说明：  
在你准备跳转到的指定标题后插入锚点{#标记}，然后在文档的其它地方写上连接到锚点的链接。

使用如下图所示：

&#8195;![](https://img-blog.csdn.net/20180802163343495?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

#### 5.注脚

语法说明：  
在需要添加注脚的文字后加上脚注名字[^注脚名字],称为加注。 然后在文本的任意位置(一般在最后)添加脚注，脚注前必须有对应的脚注名字。  

示例如下：

&#8195;![](https://img-blog.csdn.net/20180802163447739?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

注：

```
脚注自动被搬运到最后面，请到文章末尾查看，并且脚注后方的链接可以直接跳转回到加注的地方。
由于简书不支持锚点，所以可以用注脚实现页面内部的跳转。
```

### 3.6 背景色

Markdown本身不支持背景色设置，需要采用内置html的方式实现：借助 table, tr, td 等表格标签的 bgcolor 属性来实现背景色的功能。举例如下： 

&#8195;`<table><tr><td bgcolor=orange>背景色是：orange</td></tr></table>`

&#8195;![](https://img-blog.csdn.net/20180802164159599?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 3.7 emoji表情符号

emoji表情使用:EMOJICODE:的格式，详细列表可见
https://www.webpagefx.com/tools/emoji-cheat-sheet/  

当然现在很多markdown工具或者网站都不支持。  
下面列出几个平台的对比： 

工具或网站|是否支持emoji表情符号
:-:|:-:
简书|否
MarkDownPad|否（不知道付费版是否支持）
有道云笔记|否
zybuluo.com|否
github|是  

&#8195;![](https://img-blog.csdn.net/20180802164508437?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

## 4. 高端用法

### 4.1 Latex数学公式

使用LaTex数学公式： 

1.行内公式：使用两个”$”符号引用公式：` $公式$`   
2.行间公式：使用两对“$$”符号引用公式：` $$公式$$`   

输入`$\sqrt{x^{2}}$`  
显示结果是$\sqrt{x^{2}}$  

具体可以参考 markdown编辑器使用LaTex数学公式  
(https://link.jianshu.com/?t=http%3A%2F%2Fblog.csdn.net%2Ftestcs_dn%2Farticle%2Fdetails%2F44229085)  

latex数学符号详见：常用数学符号的 LaTeX 表示方法  
(https://www.mohu.org/info/symbols/symbols.htm)

### 4.2 流程图

这里简单介绍一下流程图的语法，仅作为了解，如下图所示： 

&#8195;![](https://img-blog.csdn.net/20180802165820199?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 4.3 制作一份待办事宜—-Todo 列表

&#8195;![](https://img-blog.csdn.net/20180802165859799?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

### 4.4 绘制 序列图

&#8195;![](https://img-blog.csdn.net/2018080216592352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

### 4.5 绘制 甘特图

&#8195;![](https://img-blog.csdn.net/20180802165941601?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

## 5. Markdown工具

1.markdownpad软件，就是利用markdown语言写笔记的。官网下载地址：http://markdownpad.com/  

软件安装之后的示意图如下图所示： 

&#8195;![](https://img-blog.csdn.net/20180802170021581?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

2.有道云笔记支持markdownpad语法。官方网址：http://note.youdao.com/ 它有在线网页版以及PC端可以下载。当然有道云笔记也支持html语法。  
网页版使用markdown示例图如下：

&#8195;![](https://img-blog.csdn.net/20180802170102176?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQwNjE2MzA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

3.在线编辑markdown https://www.zybuluo.com/mdeditor  
本文参考文章：
http://blog.csdn.net/u010177286/article/details/50358720  
https://www.zybuluo.com/mdeditor  
http://blog.leanote.com/post/freewalk/Markdown-%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C#title-18
