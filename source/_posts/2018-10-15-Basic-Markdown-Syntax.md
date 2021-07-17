---
title: Basic Markdown Syntax
date: 2016-10-15 13:34:38
tags: [Markdown]
categories: [Markdown]
---

##### 什么是Markdown
markdown是一个纯文本的标识语言，可以用它来生成一定格式的文本文件，Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。 Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然Markdown 语法的目标是就是要成为一种适用于网络的书写语言。

##### 常用的Markdown语法介绍
**<font color="red"/>标题**
1. 支持两种标题语法，类Setext和类atx -
这是类Setext的表示形式（= 和 -）
东方闪电
==
东方闪电
--  
<!--more-->

2. 这是atx的表示形式（#一共有6级）
# 东方闪电
## 东方闪电
### 东方闪电
#### 东方闪电
##### 东方闪电
###### 东方闪电

**<font color="red">区块（Blockquote）引用</font>**
Markdown 标记区块引用是使用类似 email 中用 > 的引用方式。如果你还熟悉在 email 信件中的引言部分，你就知道怎么在 Markdown 文件中建立一个区块引用，那会看起来像是你自己先断好行，然后在每行的最前面加上 >
```
>这部分内容为区块引用
```
>这部分内容为区块引用

**<font color="red">列表</font>**
Markdown 支持有序列表和无序列表。无序列表使用星号、加号或是减号作为列表标记；有序列表则使用数字接着一个英文句点表示。
无序列表 -
```
*  Red
*  Green
*  Blue
等同于
+  Red
+  Green
+  Blue
也等同于
-  Red
-  Green
-  Blue
```
显示效果为 -
*   Red
*   Green
*   Blue

有序列表 -
```
1. red
2. Green
3. Blue
```
显示效果为 -
1. red
2. Green
3. Blue

**<font color="red">分隔线</font>**
你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线
```
****
* * *
-----
______
```
如下效果 -
****
* * *
_______

**<font color="red">文本样式</font>**
```
** 加粗 **
*斜体*
~~删除线~~
` 底纹 `
```
效果如下 -

**加粗**

*斜体*

~~删除线~~

`底纹`

**<font color="red">图片与链接</font>**
如果只是简单的插入图片和连接，那么非常简单。两者仅仅是一个 「 ! 」 的区别。
```
图片
![图片描述](链接的地址)
快捷键：control + shift + I
** 链接 **
[文本内容](链接的地址)
快捷键：control + shift + L
```
效果如下 -  

![这是图片链接](/images/1126559.png)  
[文本链接](https://www.lello.top)

**<font color="red">代码框</font>**
位于\`\`\`和\`\`\`之间的包裹部分为代码框的内容表述
如 -
```
这里属于代码框表述的部分
```

**<font color="red">表格</font>**
这部分有点复杂，关键是分隔符的使用，下面三种情况都能表示类似的效果，请参考 -
![markdowntable.png](\images\markdowntable.png)

**<font color="red">注释</font>**
  注释用来标注用户自定义信息，和html语法类似，这部分内容只给用户编辑看的，发布后没有实际显示效果
 ```
 <!--我是注释，标注只给我自己看的内容-->
 ```

**<font color="red">脚注</font>**
  脚注总是成对出现的，「 [^1] 」作为标记，可以点击跳至末尾注解。「 [^1]: 」填写注解，不论写在什么位置，都会出现在文章的末尾。  

  点击这里看看，我有注释哦！[^1]
  [^1]: 这是我的注释 (这个我貌似没有实现，暂且放过。。。)


**<font color="red">缩进</font>**
  在输入法的「全角」模式下，输入两个空格键即可　
　　在全角模式下，首行缩进

其他markdonw语法使用频率不是很高，可参考Markdown[官网文档](http://markdownpad.com/)，个人认为，掌握上面这些对于文本写作足矣！
