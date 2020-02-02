---
title: 欢迎进入HEXO的世界系列2 —— 为HEXO穿上美丽的衣裳
date: 2020-02-02 15:23:44
tags: hexo
reward: true
---
<!-- toc -->

### 漂亮衣服哪里去找

&emsp;&emsp;首先去网上查看一些关于HEXO的主题的的相关信息，毕竟我们的主要还是想走简约风，让自己的blog看起来更加的美观和有个性。戳[这里](<https://hexo.io/themes/>)直达官方主题商店，可以根据大家的兴趣以及喜好为HEXO穿上好看的衣裳！
<!-- more -->

&emsp;&emsp;不过我是在[壁虎](<https://www.zhihu.com/question/24422335>)上找到的一个名叫[yilia](<https://github.com/litten/hexo-theme-yilia>)的主题，看起来不失大气与简约，同时兼顾颜值，确认过眼神，是我对上眼神的主题。


### 怎样为HEXO穿上衣服？

&emsp;&emsp;这里我就按照我自己使用的这个[yilia](<https://github.com/litten/hexo-theme-yilia>)主题为例，一件一件为自己的blog穿上华美的衣裳！

&emsp;&emsp;第一步怎么搞？——当然是按照[github](<https://github.com/litten/hexo-theme-yilia>)上他们给出的官方操作啦！边看边学，边学边做，一不小心就理解了!

> 1. 在自己的blog根目录中执行以下的命令，从远程仓库中拉取主题代码至本地的themes中
>
> ```shell
> git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
> ```
>
> 2. 修改HEXO根目录下的`_config.yml`中的themes字段
>
>    ```yaml
>    theme: yilia
>    ```
>
> 3. 进入到主题文件中，同步远程仓库
>
>    ```shell
>    cd themes/yilia
>    git pull
>    ```

&emsp;&emsp;再按照作者所说的配置(详细见GitHub中的readme文件中的描述)。

&emsp;&emsp;然后再次启动你的blog，就会发现blog已经穿上新的衣服了！——


![1580432924749](1580432924749.png)



### 进行定制化界面 

&emsp;&emsp;其实上述界面已经是我配置完成之后的一些界面了，在配置的过程中有一些比较糟心的事儿，记录下来以便后期再次进行重现的时候能快速查找。

#### 设置界面中的一些基本信息

&emsp;&emsp;设置自己的左侧边栏中的一些信息，主要是两个`_config.yml`文件的修改，一个是HEXO的安装根目录下的该文件，还有一个是themes/对应主题下的该文件。

+ 网页标签显示文字、描述文字、用户名 这些是在HEXO根目录下的`_config.yml`文件中修改

```yaml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Mica 的心灵树洞
subtitle: 'hello world!'
description: '上帝是佩奇，女娲是阿狸'
keywords:
author: Mica Dai
language: en
timezone: ''

...
```

&emsp;&emsp;具体的对应关系，可以结合本文件和上面的成果图进行对比了解

+ 头像、目录、导航栏、智能菜单(包含具体内容)、所有文章、友链、关于我 等，都是在themes对应的`_config.yml`文件中配置的

```yaml
# Header
menu:
  主页: /
  随笔: /essay
  相册: /galley

# SubNav
subnav:
  github: "https://github.com/50mengzhu"
  #weibo: "#"
  #rss: "#"
  zhihu: "https://www.zhihu.com/people/50meng-zhu-27"
  mail: "mailto:1670041876@qq.com"
  #qq: "#"
  #weixin: "#"
  #jianshu: "#"
  #douban: "#"
  #segmentfault: "#"
  #bilibili: "#"
  #acfun: "#"
  #mail: "mailto:litten225@qq.com"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# 是否需要修改 root 路径
# 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，
# 请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
root: /

# Content

# 文章太长，截断按钮文字
excerpt_link: more
# 文章卡片右下角常驻链接，不需要请设置为false
show_all_link: '展开全文'
# 数学公式
mathjax: true
# 是否在新窗口打开链接
open_in_new: true

# 打赏
# 打赏type设定：0-关闭打赏； 1-文章对应的md文件里有reward:true属性，才有打赏； 2-所有文章均有打赏
reward_type: 1
# 打赏wording
reward_wording: '谢谢你请我吃糖果'
# 支付宝二维码图片地址，跟你设置头像的方式一样。比如：/assets/img/alipay.jpg
alipay: /img/alipay.png
# 微信二维码图片地址
weixin: /img/wechat.png

# 目录
# 目录设定：0-不显示目录； 1-文章对应的md文件里有toc:true属性，才有目录； 2-所有文章均显示目录
toc: 1
# 根据自己的习惯来设置，如果你的目录标题习惯有标号，置为true即可隐藏hexo重复的序号；否则置为false
toc_hide_index: true
# 目录为空时的提示
toc_empty_wording: '目录，不存在的…'

# 是否有快速回到顶部的按钮
top: true

# Miscellaneous
baidu_analytics: ''
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: /img/timg.jfif

#是否开启分享
share_jia: true

#评论：1、多说；2、网易云跟帖；3、畅言；4、Disqus；5、Gitment
#不需要使用某项，直接设置值为false，或注释掉
#具体请参考wiki：https://github.com/litten/hexo-theme-yilia/wiki/

#1、多说
duoshuo: false

#2、网易云跟帖
wangyiyun: false

#3、畅言
changyan_appid: false
changyan_conf: false

#4、Disqus 在hexo根目录的config里也有disqus_shortname字段，优先使用yilia的
disqus: false

#5、Gitment
gitment_owner: false      #你的 GitHub ID
gitment_repo: ''          #存储评论的 repo
gitment_oauth:
  client_id: ''           #client ID
  client_secret: ''       #client secret

# 样式定制 - 一般不需要修改，除非有很强的定制欲望…
style:
  # 头像上面的背景颜色
  header: '#4d4d4d'
  # 右滑板块背景
  slider: 'linear-gradient(200deg,#a0cfe4,#e8c37e)'

# slider的设置
slider:
  # 是否默认展开tags板块
  showTags: true

# 智能菜单
# 如不需要，将该对应项置为false
# 比如
#smart_menu:
#  friends: false
smart_menu:
  innerArchive: '所有文章'
  friends: '友链'
  aboutme: '关于我'

friends:
  github.io: https://50mengzhu.github.io/
  # 友情链接2: http://localhost:4000/
  # 友情链接3: http://localhost:4000/
  # 友情链接4: http://localhost:4000/
  # 友情链接5: http://localhost:4000/
  # 友情链接6: http://localhost:4000/

aboutme: describe me something you need to know
```



##### 问题一：图片引用

> 这里主要说一下，怎样防止图片，使用相对路径引用图片——
>
> &emsp;&emsp;通常我们使用图片的方式有两种，
>
> ① 其中之一就是使用网上的图片，使用网页链接直接得到图片（但是这样会有不好的地方，主要是图片的控制全不在自己的手中，一旦别人的图片删除或者移动，会直接导致我们拿不到图片。PS，当然也可以自己去部署一个属于自己的图片/文件服务器专门用于存放这些）
>
> ② 还有就是使用本地的图片，当然这就是将图片直接和blog系统放在一起，相对来说对网站的内存消耗比较大
>
> &emsp;&emsp;毕竟我们都是一个小小的破站点，因此直接使用这种方式完成图片部署。

首先查看一下目录结构——

![1580434763272](1580434763272.png)



使用图片的操作分为以下几步：

+ 将图片放在主题文件夹下的source/img下面
+ 然后在主题目录下的`_config.yml`中通过这样的方式对图片进行引用

```yaml
weixin: /img/wechat.png
```



##### 问题二：怎样开启评论

&emsp;&emsp;我这边使用的是Gitment这个插件——因为是GitHub上给出的插件，因此首先需要去GitHub网站上绑定一个认证[Application](<https://github.com/settings/developers>)——位置在这里——

![1580528639819](1580528639819.png)

&emsp;&emsp;创建完成自己的认证Application，放置自己的网址和描述就好，最终在创建的Application之后得到我们想要的东西——

![1580528762925](1580528762925.png)

&emsp;&emsp;然后再在yilia的主题配置文件`_config.yml`中进行配置——

```yaml
#5、Gitment
gitment_owner: '50mengzhu'      #你的 GitHub ID
gitment_repo: 'blog-comment'          #存储评论的 repo
gitment_oauth:
  client_id: 'xxxx'           #client ID
  client_secret: 'xxxx'       #client secret
```

&emsp;&emsp;然后一套发布操作连击——

```shell
npx hexo cl
npx hexo g
npx hexo d
```



&emsp;&emsp;然后发现出现了这样一个问题——

![1580529097453](1580529097453.png)



&emsp;&emsp;以上的一顿操作之后，查询各种[网站](<https://github.com/imsun/gitment/issues/170>)、以及[网站]([https://www.wenjunjiang.win/2017/07/02/gitment%E8%AF%84%E8%AE%BA%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%85%A5hexo/](https://www.wenjunjiang.win/2017/07/02/gitment评论模块接入hexo/))之后成功放弃了，其实我是尝试过的，主要的原因实际上是就是Gitment作者自己写的一个认证服务器的https证书失效了，导致认证不成功！感兴趣的同学可以参考[链接](<https://segmentfault.com/a/1190000018177680?utm_source=tag-newest>)自行解决。

&emsp;&emsp;在下由于是个小白，不是很懂nodejs的使用方法，因此我就没有继续深究下去，相反发现了另外一种妙不可言的评论系统——[Valine](<https://valine.js.org/>)查看官网了解如何在不同的theme下部署hexo的评论组件。以我使用的主题为例[yilia](<https://github.com/litten/hexo-theme-yilia/pull/646>)，以上有使用方法的教程。完成之后直接可以进行部署。效果图如下——

![1580545302311](1580545302311.png)



##### 问题三：markdown不支持TOC这个标签

&emsp;&emsp;执行这条命令，这个是安装一个`hexo-toc`的插件

```shell
npm install hexo-toc --save
```

&emsp;&emsp;然后在markdown中写作的时候使用标签`< !-- toc -- >`就可以支持目录了

##### 问题四：markdown中的图片怎样加载

&emsp;&emsp;为HEXO安装一个资源访问插件——

```shell
npm install https://github.com/CodeFalling/hexo-asset-image --save
```

&emsp;&emsp;开启HEXO的配置文件中的`_config.yml` 文件中的允许访问资源——

```yaml
# 是否启动资源文件夹，开启后通过 hexo new :title.md 生成新文章会建立一个同名的文件夹
post_asset_folder: true
# 生成文章链接的格式，这是默认的格式；修改的规则也比较简单，标签前面要加英文冒号；（注意图片资源生成的格式必须是这个格式，否则会出现图片加载失败的情况，可见下方第6条生成的图片资源的引入格式）
permalink: :year/:month/:day/:title/
```

&emsp;&emsp;然后就可以在markdown中正常使用图片功能了。

##### 问题五：开启赞赏功能

&emsp;&emsp;这个其实yilia的作者已经帮我们完成了，为我们提供了三种选项——

> 打赏type设定：0-关闭打赏； 1-文章对应的md文件里有reward:true属性，才有打赏； 2-所有文章均有打赏
> reward_type: 1

&emsp;&emsp;依照这样的方式，那么这里重点说一下，当自定义打赏的时候应该将`reward: true`写到哪里？实际上就是将这个写在文章头部的yaml语法中就ok！

&emsp;&emsp;示例——

```markdown
---
title:  标题：欢迎进入HEXO的世界
reward: true
---
```

##### 问题六：怎样添加tag

&emsp;&emsp;直接在markdown的yaml语法中添加以下语句即可——

```yaml
---
title:  标题：欢迎进入HEXO的世界
reward: true
tags: xxx 
---
```

##### 问题七：怎样让markdown支持流程图等

&emsp;&emsp;安装一个流程图插件——

```shell
npm install --save hexo-filter-flowchart
npm install --save hexo-filter-sequence
```

&emsp;&emsp;然后就可以按照正常的流程图和正常的时序图来进行绘制了!

##### 问题八：怎样创建其他导航栏的文章

##### 问题九：怎样创建新的导航栏

&emsp;&emsp;这个问题HEXO为我们解决了！按照以下的方式直接一条命令搞定创建导航栏的任务

```shell
npx hexo new page "xxx"
```

&emsp;&emsp;创建之后使用的page名将会成为url的一部分，然后再在主题的`_config.yml`文件中对目录进行如下的配置——

```yaml
menu:
  主页: /
  随笔: /essay
  相册: /galley
  测试: /test
  小思绪: /小思绪
```

##### 问题十：怎样设置改tab标签中的icon

&emsp;&emsp;这个首先需要准备一个`favicon.ico`的图标，然后将这个资源放置在资源文件夹下，最后在theme文件夹下的配置文件`_config.xml`中创建一个如下的icon——

```yaml
# Miscellaneous
baidu_analytics: '' # 使用百度统计完成网站的点集统计
google_analytics: ''
favicon: /img/favicon.ico
```



##### 问题十一：怎样对自己网站的点击情况进行统计

&emsp;&emsp;首先进入到[百度统计](<https://tongji.baidu.com/web/welcome/login>)的官网，注册。。获取一段百度给出的代码——

&emsp;&emsp;然后在问题十中的yaml代码中的`baidu_analytics`字段中设置为yes，并创建一个`themes/yilia/layout/_partial/baidu_analytics.ejs`

```js
<% if (theme.baidu_analytics) { %>
        百度提供的代码
<% } %>
```

&emsp;&emsp;最后的效果就是可以去百度统计的网站中查看自己网站的点击数量。

##### 问题十二：怎样统计文章的阅读量

&emsp;&emsp;有两种方式可以进行数量统计，一种是采用[不蒜子](<http://busuanzi.ibruce.info/>)的方式完成直接完成数据的统计，更加详细的配置可以参考[这里](<http://ibruce.info/2015/04/04/busuanzi/>)。在yilia可以使用的是以下的方式——

&emsp;&emsp;找到yilia配置文件`_config.yml`，在其中添加这样一行——

```yaml
#不蒜子 博客浏览次数统计
busuanzi: 
  enable: true
```

&emsp;&emsp;其次，再在themes/yilia/layout/_partial/footer.ejs中添加以下的代码——

```html
<% if (theme.busuanzi && theme.busuanzi.enable){ %>
	<!-- 不蒜子统计 -->
	<span >
		本站总访问量<span id="busuanzi_value_site_pv"></span>次
	</span>
	<span class="post-meta-divider">|</span>
	<span >
		本站访客数<span id="busuanzi_value_site_uv"></span>人
	</span>
	<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
    </script>
<% } %>
```



&emsp;&emsp;还有一种是采用LeadCloud的方式进行数据统计——[详情](<https://www.bbsmax.com/A/amd0Elg1zg/>)请查看：主要是按照以下的步骤——

+ 首先去网站上注册一下，创建一个名字叫做Counter的Class用于存放阅读的数据。

![1580623814861](1580623814861.png)

&emsp;&emsp;&emsp;然后获取到对应的应用ID，appKey等信息记录。

+ 在主题对应的配置文件`_config.yml`中添加一下的字段——

```yaml
# leancloud 浏览次数统计 
leancloud_visitors:
  enable: true
  app_id: # appid
  app_key: # app key
  
#添加一下js插件的 CDN地址
js_cdn:
  jquery: https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js
  av : //cdn1.lncld.net/static/js/2.5.0/av-min.js
```

+ 找到主题文件夹下对应的`title.ejs`文件，添加以下内容，用于在标题旁边显示阅读数量

```html
<% if (theme.leancloud_visitors.enable){ %>
  <a href="javascript:;" class="archive-article-date">
      <i class="icon-smile icon"></i> 阅读数:<span id="<%- url_for(post.path) %>" class="pageViews">0</span>次
  </a>
<% } %>
```

+ 在主题对应的`after_footer.ejs`文件中添加以下的内容

```html
<script src="<%- theme.js_cdn.jquery %>"></script>
<% if (theme.leancloud_visitors.enable){ %>
//这是自定一个js文件,放在 layout\_parial\post 目录 leancloud.ejs中
<%- partial('post/leancloud') %>
<% } %>
```

+ 最后需要创建一个`layout\_parial\post\leanCloud.ejs`文件，文件内容为——

```html
<script src="<%- theme.js_cdn.av %>"></script>
<script type="text/javascript">
if(<%- theme.leancloud_visitors.enable %>){
    var leancloud_app_id  = '<%- theme.leancloud_visitors.app_id %>';
    var leancloud_app_key = '<%- theme.leancloud_visitors.app_key %>';
    AV.init({
        appId: leancloud_app_id,
        appKey: leancloud_app_key
    });
    var pageViewsLength = $(".pageViews").length;
    var isIndex = $(".pageViews").length > 1 ?true:false;
    function showTime() {
        var Counter = AV.Object.extend("Counter");
        if(isIndex){
            $(".pageViews").each(function(){
                showPageViewsNum($(this),Counter);
                addPageViewsNum($(this));
            });
        }else{
            showPageViewsNum($(".pageViews"),Counter);
            addPageViewsNum($(".pageViews"));
        }
    }
    //显示阅读量
    function showPageViewsNum(ele,Counter){
        var query = new AV.Query("Counter");
        var url = ele.attr('id').trim();
            query.equalTo("words", url);
            query.count().then(function (number) {
                $(document.getElementById(url)).text(number?  number : '0');
            }, function (error) {
                 $(document.getElementById(url)).text('0');
            });
    }
    //追加阅读量
    function addPageViewsNum(ele){
        var url = ele.attr('id').trim();
        var Counter = AV.Object.extend("Counter");
        var query = new Counter;
        query.save({
            words: url
        }).then(function (object) {});
    }
    if(pageViewsLength){ //此处判断等于 1 在执行 可去除循环
        showTime();
    }
}
</script>
```

P.S. 别忘记了在leadCloud配置的时候，需要在`安全中心 -> web安全域名`中添加自己的网址，就可以愉快的玩耍了！

##### 问题十三：修复失效的微信分享码

找到`themes\yilia\layout\_partial\post\share.ejs`文件，将——

```html
<div class="page-modal wx-share js-wx-box">
    <a class="close js-modal-close" href="javascript:;"><i class="icon icon-close"></i></a>
    <p>扫一扫，分享到微信</p>
    <div class="wx-qrcode">
      <img src="<%- 'qrcode' in locals ? qrcode(sUrl) : '//pan.baidu.com/share/qrcode?url=' + sUrl  %>" alt="微信分享二维码">
    </div>
</div>
```

中的`//pan.baidu.com/share/qrcode?url=`修改为——`//api.qrserver.com/v1/create-qr-code/?size=150x150&data=`

##### 问题十四：左侧显示总文章数

找到文件`themes\yilia\layout\_partial\left-col.ejs`，在

```html
<nav class="header-menu">
    <ul>
    <% for (var i in theme.menu){ %>
        <li><a href="<%- url_for(theme.menu[i]) %>"><%= i %></a></li>
    <%}%>
    </ul>
</nav>
```

后面追加以下内容——

```html
<nav>
    总文章数 <%=site.posts.length%>
</nav>
```

##### 问题十五：文章字数统计

首先需要通过npm安装文字统计插件`hexo-wordcount`——

```shell
npm i --save hexo-wordcount
```

在文件`themes\yilia\layout\_partial\left-col.ejs`中添加以下的内容：

```html
<nav>
    总字数 <span class="post-count"><%= totalcount(site, '0,0.0a') %></span>
</nav>
```



然后编辑以下的文件——`themes\yilia\layout\_partial\article.ejs`，可以在header下准备写下这样的代码——

```html
<div align="center" class="post-count">
	字数：<%= wordcount(post.content) %>字 | 预计阅读时长：<%= min2read(post.content) %>分钟
</div>
```





