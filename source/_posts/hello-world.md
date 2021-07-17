---
title: The relationship between Hexo and Blog
date: 2016-09-22 20:35:20
---
记录是个良好的习惯，及时地将工作和学习中的知识点记录下来，不仅能加深记忆还能帮助理解，俗话说得好，好记性不如烂笔头，不爱笔记三天学习两天忘记。现实中人们记录的方式有很多，最常用的莫过于博客了，当然现在提供博客服务的网站有很多，但大多千篇一律，不够个性化，为了不受限于各大平台，搭建个人定制化的博客很有必要，这里给大家介绍一种当下流行的，比较有逼格的个性化博客定制方案Hexo+Github Pages，当然不在乎形式的童鞋请略过。


## Quick Start With A Personal Blog
<!--more-->
### Precondition
#### <font color="#0000dd"> 什么是 Hexo？</font>
- Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

#### <font color="#0000dd">安装前提</font>
+ 安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
+ • Node.js
+ • Git
* 备注下， Node.js 是一个让 JavaScript 运行在服务端的开发平台，它让 JavaScript 成为与PHP、Python、Perl、Ruby 等服务端语言平起平坐的脚本语言, 它也是一个基于 Chrome V8 引擎的 JavaScript 运行环境。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。
* 因为hexo是一款基于Node.js的静态博客框架，所以在使用hexo搭建博客站点时，必须要先安装Node.js进行解析，而Git提供了分布式版本管理的实现，包括生成，启动服务，部署等操作，基于node.js的hexo博客是需要类似于Git的工具来实现网站的发布和管理的，当然DOS-Mode下也是可以的。


完成上面步骤后，本地选择一个文件夹作为blog根目录，然后启动git cmd, Dos-Mode 或 git bash（多数用git bash）执行如下hexo命令，创建本地博客框架
```
$ cd D:\myblog_repository # 假定你的blog仓库为 myblog_repository
$ hexo init # 初始化blog site
$ nmp install XXXXpackages # 根据上一步的提示安装相应的依赖包
```

上面的安装步骤下来，如果没有报错，你的blog环境配置就完成了。接下来只需运行几个命令就可以生成默认静态页面了
```
Hexo clean /hexo c
Hexo generate /hexo g
Hexo deploy /hexo d
Hexo server /hexo s
```

在本地浏览器下输入localhost：4000,回车后看看能不能看到hello world页面，如果能看到，恭喜你！博客本地架构已生效，可以开始专注写博客了。
 ![hexo hello world](/images/hexodefault.jpg "hexo default")


### Basic Hexo Syntax
#### Create a new page/post
``` bash
常用几种写法 -
$ hexo new "My New Post"
$ hexo new page "tags"
$ hexo new post "categories"
```
More info: [Writing](https://hexo.io/docs/writing.html)


#### Run server
``` bash
$ hexo server
```
More info: [Server](https://hexo.io/docs/server.html)


#### Generate static files
``` bash
$ hexo generate
```
More info: [Generating](https://hexo.io/docs/generating.html)


#### Deploy to remote sites
``` bash
$ hexo deploy
```
More info: [Deployment](https://hexo.io/docs/deployment.html)


#### <font color="#0000dd">小结一下：</font>
  > Hexo就是一个Nodejs的产品，用它可以方便快捷的生成静态页面，而github的pages又可以为静态页面提供展示和域名空间服务，并且能很好地支持Markdown文法，这就为个性化定制blog提供了绝好的机会。blog的精神在于内容，高效的写作博文还需要掌握一些基本技能，如Yaml文法，Markdown基本语法以及页面格式和编排的Html，CSS文法等等，这些我将在后续的博文中陆续介绍。
