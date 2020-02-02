---
title: 欢迎进入HEXO的世界系列3 —— blog发布
date: 2020-02-02 15:23:50
tags: hexo
reward: true
---
<!-- toc -->

&emsp;&emsp;其实大家在搜索的时候发现网上有很多关于HEXO发布的相关文章，但是大多都是直接发布到github提供的一个blog页面，这种清醒在网上搜索很快就能得到结果。我这里想说的是将自己的blog怎样发布到自己的服务器中，比如阿里云。

<!-- more -->

### 前提：首先你得有一个服务器

&emsp;&emsp;[阿里云](<https://www.aliyun.com/activity#/>)的服务器大家可以去购买，配置方法网上一搜一大片，这里不冗余描述。如果自己受不了一直使用ip访问自己的界面，这里建议去购买[域名](<https://wanwang.aliyun.com/>)，不过需要注意的是网站备份什么的注意事项参考[此处](<https://www.itwuhan.com/news/1567.html>)。到最后在阿里云的后台管理界面可以直接将域名和ip进行绑定。

&emsp;&emsp;还有需要注意的是域名的备案需要一定的时间！

### 具体操作

#### 远程的操作

+ 授权端口

&emsp;&emsp; 阿里云默认不授权80端口访问，我们需要关闭防火墙或者放行防火墙的80端口。

```shell
# centos
firewall-cmd --add-port=80/tcp
firewall-cmd --add-port=80/tcp --perment
# ubuntu
ufw allow 80/tcp
```

+ 安装nginx执行代理服务

```shell
## centos
yum -y install nginx
## ubuntu
apt-get install nginx
```

&emsp;&emsp;安装完成之后，nginx的服务是默认启动的，因此可以直接在本地的浏览器中输入服务器的公共ip地址看是否能够访问。如果出现nginx的欢迎界面说明服务已经启动。



&emsp;&emsp;接下来就需要使用到nginx强大的代理功能了，为了便于管理，我们直接在`/etc/nginx/`目录下创建一个`vhost`的文件夹，在其中创建一个关于blog网站的配置文件——`blog.conf`，向其中添加以下的内容——

```conf
server{
	listen    	80;
	root 		/home/www/website; # 博客目录存放的地址
	server_name ip或者域名;
	location 	/{
	}
}
```

&emsp;&emsp;这样就完成一个网页的配置文件了。然后将这个配置文件include到nginx的主配置文件当中去——`/etc/nginx/nginx.conf`——

```conf
include /etc/nginx/vhost/*.conf
```



&emsp;&emsp;因为我们将blog的目录放在/home/www/website这个目录下，因此需要去创建网站文件的目录，所以首先创建以上的目录——需要注意的是<font color="red">如果是以root身份创建该文件夹，那么需要修改该文件夹的权限让其他人有写的权限，否则当以非root身份进行blog的部署的时候会导致如法写入这个文件而造成403的问题</font>

+ 安装git将服务器作为代码提交的仓库

&emsp;&emsp;使用apt或者yum进行软件git的安装，安装完成之后创建一个名为git的用户，这个用户将被用来存放我们的远程仓库，为了以后方便的提交代码——这里可以将本地主机的公钥上传到远程主机的git用户中——

```shell
ssh-copy-id -i 公钥路径 username@remoteIP
```

&emsp;&emsp;执行以上命令输入用户登录远程IP的密码就可以将本地公钥上传到远程机器上。或者可以将本地生成的公钥放到远程服务器中`{user home}/.ssh/authorized_keys`中按照对应的格式添加一行即可。



&emsp;&emsp;设置完密钥之后，我们还需要创建一个git仓库，用于接收来自本地的提交——

```shell
git init --bare blog.git
```

&emsp;&emsp;然后在blog.git/hooks下创建一个post-receive文件，用于自动化部署，指定工作区、git目录等

```shell
git --work-tree=/home/www/website --git-dir=/home/git/blog.git checkout -f
```

&emsp;&emsp;因为这个文件最后是需要执行的，因此需要添加可执行权限——

```shell
chmod +x post-receive
```

+ 安装nodejs运行系统

&emsp;&emsp;直接使用yum或者apt安装即可。



#### 本地的操作

+ 使用npm安装提交时的插件——

| 插件名称          | 作用                                       |
| ----------------- | ------------------------------------------ |
| hexo-deployer-git | 用于提交代码                               |
| hexo-server       | 用户本地查看HEXO的服务器效果，本地启动HEXO |

+ 配置提交远程仓库目录

&emsp;&emsp;在HEXO的安装目录下的配置文件`_config.yml`中的deploy字段中填写如下：——

```yaml
deploy:
	type: git
	repo: git@服务器公网IP:/home/git/blog.git
	branch: master
	message: 
```

+ 部署三部曲

```shell
npx hexo cl
npx hexo g
npx hexo d
npx hexo s
```



愉快的享受吧！



<https://blog.csdn.net/NoCortY/article/details/99631249>



