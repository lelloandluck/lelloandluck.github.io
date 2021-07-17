---
title: how to associate your blog site with a domain
date: 2016-10-10 12:01:34
tags:
 - blog
 - domain
categories:
 - blog
 - domain
---

网站建起来了，怎么样通过百度，google搜索到你的站点呢？几乎每个建站人员都有类似问题，下面我从自身的Hexo Site实际经历来梳理下这个问题的解决方案。

1. 购买域名
从万网（阿里云）中购买域名，这个根据个人需要，可以选择不同后缀的域名，当然价格也是不一样的。另外能提供域名注册的机构有很多，阿里云是国内比较知名的站点，这里不仅购买方便，后续的维护和站点的稳定性也是有保障的。
[网址如下](https://www.aliyun.com/?utm_content=se_1000301881)

2. 解析域名
购买域名后并不能直接拿来使用，需要对其进行解析，所谓解析就是将你的域名指向你的网站服务器，这样当别人访问域名时将通过解析引导访问指向你的服务器。

 域名解析在阿里云里很容易设置，网上有很多设置步骤，我的做法是直接将CNAME直接做两次引导，如下面设置
![域名解析](/images/domainAnalysis.PNG)

3. 实名认证
这个没得说，阿里云平台的必须步骤，没有经过实名认证的域名是不能正常使用的，平台将会把域名处于Serverhold状态，无法使用
实名认证后可以ping yourdomainName 来检验下是否该域名生效

4. 将域名和Hexo Site关联起来
完成上述域名申请购买和认证后，接下来就对Hexo site做一些设置了，我的步骤如下

 4.1 新建CANME文件（在hexo根目录下的source文件夹下）
  <font color="red">$ touch CNAME </font> (或者直接鼠标操作，注意不要任何后缀)

 4.2 在CNAME里写上如下代码并保存
  www.yourdomainName.top

 4.3 进入Github在hexo仓库下设置Source page domain 为
  www.yourdomainName.top

 4.4 执行并部署网站
  hexo clean
  hexo g -d
  or
  hexo clean && hexo g && hexo d
