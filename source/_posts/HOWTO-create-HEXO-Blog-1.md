---
title: 欢迎进入HEXO的世界系列1 —— 安装搭建HEXO
date: 2020-02-02 15:19:08
tags: hexo
reward: true
---
<!-- toc -->

### 首先，HEXO是什么？

&emsp;&emsp;其实HEXO就是一个轻量级的前端blog框架，形式简约而不简单。在[官网](<https://hexo.io/>)中将其描述为

> A fast, simple & powerful blog framework

实际上，这就是他的定位。
<!-- more -->

&emsp;&emsp;其次，这个框架好用在哪里呢？一方面相信大家在谈起blog的时候首先就会在脑中出现的就是大名鼎鼎的[WordPress](<https://wordpress.com/>)！！这里没有任何黑WP的意思，只是这个框架有点过于庞大和“重量级”导致很多人望而生畏，~~像我这种比较懒惰的人，面对庞大的系统来说简直就是不想看好吧，因此索性和WP的缘分还没开始就已经结束了~~。相比起来，HEXO简直就是香到爆好不啦！小巧玲珑，配上完美丰富的主题，再加上对程序员必备语言[markdown](<https://www.markdownguide.org/>)的完美支持，简直就是天上掉下来的小仙女！！



### 其次，HEXO需要什么样的环境支持？

&emsp;&emsp;参照HEXO的官方github的[指南](<https://hexo.io/docs/>)，需要的前提条件有二——

+ [git](<https://git-scm.com/downloads>) —— 常在某hub上混迹，这个技能是必须点亮的
+ [nodejs]() —— 这个就不用说了，由于HEXO是个前端框架，因此需要node来调教



### 一切准备就绪，就等安装HEXO

&emsp;&emsp;其实安装HEXO相当简单，只需要短短一行命令就可以完成安装——

+ 普通用户使用npm进行安装

```shell
npm install -g hexo-cli
```

+ 高级用户使用npm进行安装

```shell
npm install hexo
```

&emsp;&emsp;我这次因为是对这个框架不算很熟悉，算是新手玩家，于是我只充当一个弟弟角色，使用普通用户的方式安装的HEXO，目前还不知道高级用户可以进行什么样的调教(<font color="red">此处挖一个坑</font>)



### 怎样使用HEXO？

+ 选定一个合适的目录作为HEXO的根目录，然后在这个根目录下首先进行初始化操作，只需要一条命令

```shell
npx hexo init
```

![1580396108311](1580396108311.png)

&emsp;&emsp;事实上从以上的打印出来的信息可以看到，这个初始化的过程实际上是：

```flow
st=>start: github上拉取HEXO的仓库
e=>end: 结束
op=>operation: 使用node运行仓库中的js文件
st->op->e
```

&emsp;&emsp;因此我们在前期的准备中需要安装好git和nodejs这两大神器！



&emsp;&emsp;至此，我们离最后的运行只剩最后一条命令——

```shell
npx hexo server
```

&emsp;&emsp;呈现出的界面——

![1580397564268](1580397564268.png)

&emsp;&emsp;老实说，这样的界面已经比较精神抖索了，而且还是比较简约的，但是咱们在颜值的路上需要越走越远，因此下一节我将详细说说怎样为我们自己的HEXO穿上美丽的花衣裳，简约而不简单！ 

