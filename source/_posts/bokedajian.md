---
title: 个人博客的搭建 Hexo
categories: Hexo教程
comments: true
---
第一次搭建一个属于自己的博客，有点小激动，网上也有很多教程说的都比较详细，我也是根据教程一步步整下来的，幸运的是弄得很顺利，为了加深印象，我自己重新整理下网上的资料.

<!--more-->
## Node.js

Node.js是一个Javascript运行环境(runtime)

### 安装

#### window

Windows平台请到[node官网](https://nodejs.org/)下载安装包进行安装。

#### Linux

Linux平台可通过[包安装](https://nodejs.org/en/download/package-manager)或[nvm](https://github.com/creationix/nvm)进行安装，这里建议采用nvm方式。
首先，安装nvm环境，进入终端，在用户家目录下执行
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash`
```

然后，将以下代码添加至到profile(~/.bash_profile， ~/.zshrc，  ~/.profile， ~/.bashrc其中之一)文件中
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```
重启终端，输入以下指令安装node最新的LTS版本
```
nvm install --lts=boron
```

#### Mac

通过brew安装，[brew](http://brew.sh/)类似于Ubuntu的apt或CentOS的yum，是Mac平台的包管理工具。brew依赖Ruby环境，Mac系统自带Ruby，如果还没brew环境，请直接在终端输入以下指令安装brew
然后输入
```
brew install node
```

Node.js 是通过npm进行包管理的，但国内使用起来很慢，最好使用淘宝的镜像。

## Hexo

安装好Node.js环境后，就可以通过node的npm包管理工具安装Hexo，执行以下指令
```
npm install -g hexo
```

成功安装Hexo后，在自己的工作目录下，创建一个blog目录，在此目录下管理自己的博客源码，进入blog目录，执行以下指令完成Hexo项目环境

```
hexo init
```

hexo初始化会在blog目录生成一些东西，如下图
![image](https://github.com/chestysh/chestysh.github.io/blob/master/image/iamge.png?raw=true)
安装依赖包
```
cnpm install
```
生成静态页面
```
hexo g
```
启动本地服务
```
hexo server
```
生成静态页面与启动本地服务可以结合成以下指令
```
hexo s -g
```
正确启动如下图
![image1](https://github.com/chestysh/chestysh.github.io/blob/master/image/image1.png?raw=true)
打开浏览器，输入 http://localhost:4000/ ，看到以下页面说明hexo环境安装成功。
![image2](https://github.com/chestysh/chestysh.github.io/blob/master/image/F8D99DA7-4F79-4B5D-843C-DF2476805DEC.png?raw=true)

在根目录下/blog,使用命令hexo就可以创建文章等操作，可以使用一下命令查看各种操作
```
hexo help
```
## 部署环境

我们的静态页面是部署在[GitHub](https://github.com/),所以首先需要申请一个账号，已经有账号可以略过这一步。

### 创建仓库

点击[创建仓库](https://github.com/new),注意仓库名称格式，一定要以`yourname`.github.io,标红的yourname一定要与你账户的username相同,我的仓库名称就必须是chestysh.github.io。这个仓库名称就是以后访问你博客站点的默认域名。换言之，就是一个用户只能创建一个Github Pages环境。
![imgae3](https://github.com/chestysh/chestysh.github.io/blob/master/image/7BE8BCB7-E999-400A-8637-5B4C5B59DEF0.png?raw=true)
最后点击Create repository按钮，完成仓库创建.

### 部署公钥

我使用的是Mac电脑作为例子，首先需要在自己的电脑上生成一对公钥和私钥,打开终端
```
cd ~/.ssh
```
如果没有密钥则不会有此文件夹，有则备份删除

生成秘钥
```
$ ssh-keygen -t rsa
```
按3个回车，密码为空即可。最后得到了两个文件：id_rsa和id_rsa.pub

添加密钥到ssh
```
ssh-add 文件名
```
需要之前输入密码。
在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥。

设置Git的user name和email：
```
$ git config --global user.name [用户名]
$ git config --global user.email [登录账号]
```

测试,在终端中输入一下代码
```
ssh git@github.com
```

```
The authenticity of host ‘github.com (207.97.227.239)’ can’t be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added ‘github.com,207.97.227.239′ (RSA) to the list of known hosts.
ERROR: Hi tekkub! You’ve successfully authenticated, but GitHub does not provide shell access
Connection to github.com closed.
```
如果公钥泄漏，请及时移除对应的公钥，重新生成

## 发布站点

下面，我们将本地的静态页面部署到网站上
首先，打开blog目录下的_config.yml，找到deploy段，配置成这样
```
deploy:
  type: git
  repository: git@github.com:yourname/yourname.github.io.git
  branch: master
```
注意将代码里面的yourname换成你账号的username。

执行以下指令进行部署
```
hexo d -g
```

如果提示没找到git，请先执行以下指令
```
cnpm install hexo-deployer-git --save
```
重新部署，再次访问`https://yourname.githud.io`, 就可以看见本地一样的网页了

也可以看简书中 http://www.jianshu.com/p/5fc4672b7d2e?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=qq 这篇文章。

`在使用[GitHub](https://github.com/) 进行代码管理时，需要将代码先提交到本地的仓库中，否则不能使用git提交。`
