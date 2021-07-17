---
title: How to migrate your blog to another PC
date: 2016-10-03 18:07:45
tags: [Git, Github]
categories: [Git, Github]
---
有时候我们常常需要在另外的设备上更新博客，怎么办呢？这就需要将你部署在Github端（Github Pages）的博客原始文件更新到新的机器上，然后配置好相应的环境，这样就可以像在原始机器上工作一样正常些博客了，这么说，估计你还是云里雾里，不慌，下面就是我的详细步骤，照此来做就行了。

设计思路是在存放Blog的Github仓库里创建一个分支（比如Hexo）这样该仓库就有两个分支了（master和hexo），master分支存放的是hexo生成的静态blog页面，hexo就可以存放hexo的原始文件了，这样将来更换机器时，只需将hexo分支clone到新的终端机器即可。

##### <font color="red">设计思路：</font>
> <font color=#8ab size=2.5> 创建new branch <font color="yellow">--></font> 将hexo原始文件存放在new branch <font color="yellow">--></font> 在新的机器中clone该new branch <font color="yellow">--></font> 配置新机器的hexo环境 <font color="yellow">--></font> 新机器中更新blogs <font color="yellow">--></font> git push更新到new branch <font color="yellow">--></font> hexo生成静态页面并部署到master branch （hexo clean hexo g hexo d）

<!--More-->
##### <font color="red">实施步骤：</font>
1. 在新的机器上安装Nodejs，Git
2. 在新的机器上创建hexo根目录文件夹
3. 在新的blog根目录下安装Hexo，如果之前有安装，并不在该目录，请忽略该步
```
$ npm install -g hexo (-g是代表全局global安装)
或者执行命令
$ sudo npm install -g hexo (有时可能因为权限不够，需要在前面加上 sudo)
```
4. Clone你的blog仓库从Github端到新的blog目录
```
git clone https://github.com/yourname/hexo-test.github.io.git (将该命令中的仓库换成你自己的hexo blog仓库就行)
git clone https://github.com/yourname/hexo-test.github.io.git(将该命令中的仓库换成你自己的hexo blog仓库就行) <font color="red">newblogFolderPath</font>
```

5. 如果在Github创建仓库时能创建新的分支，那最好了，如果没有（大多数人应该没有考虑到这点）那只好重新创建一个分支如hexo了（该命令要在新机器blog根目录执行）
```
$ git branch 'hexo' (创建新的git分支hexo)
$ git checkout 'hexo' (切换分支到hexo)
或者执行如下命令
$ git checkout -b 'hexo' (创建并切换到hexo分支)
```

6. 将新blog根目录下所有的文件删除（就是上一步从github仓库clone的文件），把原机器中hexo根目录下的所有文件copy过来（当然有些可以不用copy如果你知道哪些文件不需要的话，建议全部copy）

7. 把copy过来的文件提到暂存区
```
$ git add --all
```

8. 提交暂存区文件到分支
```
$ git commit -m "commit orign hexo file" (引号中的内容就是description自定义，但要有意义)
```

9. 推送分支到github
```
$ git push --set-upstream origin hexo (hexo是新分支的名称)
```

- 到这一步我们就基本上搞定了，以后再跟新了博客就直接 git push就可以了，hexo的操作跟以前一样不变。

- 经过上面的步骤后，我们就把原机器中hexo的原始文件，搬到了github hexo仓库的hexo分支上了。这么做的好处就是以后无论在哪一台机器上更新blog，直接clone这个hexo分支到本地，npm install安装依赖之后就可以用了。 最后写好博客，直接执行如下命令即可将更新的blog部署到master分支上。
```
$ hexo -clean -g -d
或者分别执行
hexo clean
hexo g
hexo d
```

##### <font color="red">小结：</font>
> 几个常用的命令 -
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

> 几个常用的Hexo依赖 -
本地测试的时候需要用hexo server
npm i hexo-server
将文章部署到github上的模块
npm install hexo-deployer-git --save
安装RSS插件
npm install hexo-generator-feed --save
添加Sitemap,加速网页收录速度
npm install hexo-generator-sitemap --save

> 再次回到原始机器该怎么做呢
需要clone hexo分支，然后再更新blog，和上面新机器一样操作，或者直接将source文件夹更新就可以了（因为影响部署静态文件的，应该是source中的内容）

<table><tr><td bgcolor=#54FF9F>另外，如果本地创建的Hexo架构部署失败后（比如单个文件超过100MB的，在hexo d就会报错，这个坑我到现在还没找到solution），可以快速补救，现将补救步骤描述如下 -
1. 在本地在穿件一个文件夹作为blog根目录, 并将source, theme 以及_config.yml 文件copy过来（其实就是复制原来的hexo架构） 
2. 在该目录下打开Git Bash or Commond Line
2.1 在git命令框中安装Hexo (执行命令： npm install -g hexo)
* 这一步基本不需要做，因为这里是补救之前的hexo + Github框架，这些之前都有安装过，所以不必重复安装
3. 执行命令： hexo ini
4. 执行命令： hexo g
5. 执行命令： hexo d
* 如果出错，请依次执行这几个命令： npm install hexo-deployer-git --save； sudo npm install hexo-server； npm install hexo-server --save
</td></tr></table>
