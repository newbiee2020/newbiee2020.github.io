---
title: Hexo+Coding博客搭建
date: 2020年4月4日00:54:13
author: Newbiee2020
top: true
cover: true
toc: true
password: 
mathjax: 
summary: 
tags:
- Hexo
- Coding
- 博客
categories:
- 博客搭建
---
# Hexo+Coding博客搭建

## 前言

自己使用过`windows 10`系统电脑，利用`GitHub`托管平台和`Hexo`框架搭建博客，不过遇到了一些问题：

1. 莫名语法问题：比如我们在编写`YAML`格式文件时，我明明按照语法规定，在参数后留了一个空格，但运行时还是报错，说我没留，感觉应该时`Windows`的`BUG`，所以我后面选择了``Linux`系统。
2. 访问速度太慢太慢：我们知道`GitHub`的服务器实在美国的，所以如果我们选择在`GitHub`上直接挂靠的话，国内访问会非常非常慢。当然，你可以选择使用`CDN`等方式进行加速，但又设计网站备案等等问题，所以我选择了和`GitHub`类似的国内平台——`Coding`。

本文只记录自行搭建的步骤，如果遇到其他问题，自行度娘。同时，此文是博客的初步搭建，后续会进行主题的定制、代码优化、访问优化等等内容。

## 搭建的大体步骤：

1. 安装Git、`Node.js`、`Hexo`；
2. 在Coding创建个人仓库以及SSH；
3. 将`Hexo`部署到Coding；
4. 绑定个人域名；；
5. 发布新文章。

## 第一步：安装

我是用的是`Termux`上的`Linux`，安装`git`以及`Nodejs`只需要在终端输入：

~~~bash
pkg install git -y	# git用来把本地的网页文件上传到Coding上面去
pkg install nodejs -y	# hexo 框架由nodejs编写，所以安装
pkg install npm -y
~~~

安装完毕后，可以再输入`node -v`以及`npm -v`检查是否安装完成，如果出现版本号证明无误。

下一步是添加国内镜像，加速下载速度。

   ```bash
npm config set registry https://registry.npm.taobao.org
   ```

## 第二步：安装`Hexo`

新建一个目录叫做`MyBlog`，进入此目录，然后输入以下命令进行安装hexo：

~~~bash
npm install hexo-cli -g
~~~

安装完毕后，输入`hexo -v`验证是否安装成功。

然后进行初始化：

```bash
hexo init MyBlog
```

接着输入`npm install`安装必备的组件：

```bash
cd MyBlog      
npm install
```

新建完成后，指定文件夹`MyBlog`目录下有：

- `node_modules:` 依赖包
- `public：`存放生成的页面
- `scaffolds：`生成文章的一些模板
- `source：`用来存放你的文章
- `themes：`主题
- `_config.yml:` 博客的配置文件

这样本地的网站配置也弄好啦，输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器：

```bash
hexo generate && hexo service
```

浏览器打开http://localhost:4000/，就可以看到我们的博客啦!![](https://s1.ax1x.com/2020/04/04/GdlPQx.png)

## 第三步：注册Coding账号并创建仓库

[Coding](https://coding.net/) 和 [GitHub](https://www.github.com/) 主要功能类似，都是提供代码托管服务。由于 Coding 为腾讯代理，在国内的服务器较多，所以使用 [Coding Page](https://coding.net/) 运行速度要比 [GitHub Page](https://pages.github.com/) 快很多。

coding 在2020年初进行了注册改动，注册时候需要先有一个团队才可以，也就是说你先创建一个团队然后再进行注册即可(免费注册团队)。注册地址: [Coding团队注册](https://e.coding.net/signup)

注册成功后，新建仓库：![](https://s1.ax1x.com/2020/04/04/Gd1YDK.png)

项目名称为：`[用户名].coding.me`

项目标识：`[用户名].coding.me`

把启动`readme.md`文件勾上

完成创建。

注意：在初次使用静态网站服务前，需要团队拥有者完成实名认证。点击【构建与部署】->【静态网站】->【实名认证】。按照页面提示输入认证信息，完成实名认证后，就可以正常使用静态网站功能了。![](https://help-assets.codehub.cn/enterprise/20191227154747.png)

在【构建与部署】->【静态网站】中点击【立即发布静态网站】按钮。![](https://help-assets.codehub.cn/enterprise/20191227153659.png)

在新建静态网站页面，输入【网站名称】，选择【部署来源】、【触发机制】后，点击【立即部署】按钮。

> 网站名称：长度不超过 14 个字符。
> 部署来源：可选择本项目仓库、其他项目仓库、本项目文件，默认选择本项目仓库。
> 触发机制：可选择自动部署、手动部署，默认选择自动部署。

![img](https://help-assets.codehub.cn/enterprise/20191025113853.png)

在静态网站详情页点击右上角【设置】按钮，进入静态网站设置页。

![img](https://help-assets.codehub.cn/enterprise/20191025113034.png)

在静态网站设置页可修改【网站名称】、【部署来源】、【触发机制】，还可进行【自定义域名】、【强制 HTTPS】、【删除网站】的操作。![img](https://help-assets.codehub.cn/enterprise/20191025114242.jpg)

## 第四步：配置 Git

设置你的昵称和邮箱，输入一下命令：

~~~bash
git config --global user.name "your_name" # 双引号内填入你的昵称
~~~

~~~bash
git config --global user.email "your_email@example.com" # 双引号内填入你的 CODING 账号邮箱
~~~

>  `ssh`，简单来讲，就是一个秘钥，其中，`id_rsa`是你这台电脑的私人秘钥，不能给别人看的，`id_rsa.pub`是公共秘钥，可以随便给别人看。把这个公钥放在`Coding`上，这样当你链接`Coding`自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过`git`上传你的文件到`Coding`上。

然后生成SSH密钥：

~~~bash
ssh-keygen -m PEM -t rsa -b 4096 -C "your.email@example.com" # 双引号内填入你的 CODING 账号邮箱
~~~

查看`id_rsa.pub`密钥里的内容：

~~~bash
cat ~/.ssh/id_rsa.pub # 默认存储在 ~/.ssh 目录下
~~~

下一步，把密钥的内容复制到`Coding`中：

进入【个人设置】->【SSH 公钥】，点击“新增公钥”，将公钥粘贴到“公钥内容”的框中，最后点击“添加”。![img](https://help-assets.codehub.cn/enterprise/20191105154851.png)

## 第五步：将`Hexo`部署到`Coding`

这一步，我们就可以将`hexo`和`Coding`关联起来，也就是将`hexo`生成的文章部署到`Coding`上，打开博客根目录下的`_config.yml`文件，这是博客的配置文件，在这里你可以修改与博客配置相关的各种信息。

修改最后一行的配置：

```yml
deploy:
  type: git
  repository: 
    coding: {address}
  branch: master
```

把`{address}`替换成你的仓库地址就可以了。

想要得到你的仓库地址，参看下图，先点击「`HTTPS`」，然后点击后面的「复制」按钮就得到地址了。

![img](https://upload-images.jianshu.io/upload_images/2668873-c7b0b5b297d47980.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

这个时候需要先安装`deploy-git` ，也就是部署的命令,这样你才能用命令部署到`Coding`。

```bash
npm install hexo-deployer-git --save
```

然后：

```bash
hexo clean
hexo generate
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。 `hexo generate`顾名思义，生成静态文章，可以用 `hexo g`缩写 ，`hexo deploy`部署文章，可以用`hexo d`缩写

> 注意`deploy`时可能要你输入`username`和`password`

## 第六步：自定义域名

绑定正确的 `CNAME` 记录类型的自定义域名后，静态网站的访问地址将变为自定义域名。

1. 在域名服务商的域名解析列表内选择所需域名，进入该域名的解析页面，点击【添加记录】按钮。![](https://help-assets.codehub.cn/enterprise/20190801183551.png)

2. 需要添加两条记录：![](https://s1.ax1x.com/2020/04/04/Gd10Cd.png)

   其中记录值为**静态网站详情页中的访问地址（例如：f5ku56.coding-pages.com）**，点击【保存】按钮后即可完成该条解析记录添加。

3. 在【静态网站设置页】->【自定义域名】->【绑定新域名】中输入上述操作所完成解析的域名，点击【绑定】按钮。   ![img](https://help-assets.codehub.cn/enterprise/20190801185507.png)

4. 可在【静态网站设置页】->【`强制 HTTPS` 】中开启强制 `HTTPS` 访问按钮。

   > 启用强制 `HTTPS` 访问后，任何尝试用 HTTP 协议访问您网站的请求都会被强制跳转到使用 `HTTPS` 加密协议访问，为保护您的数据安全。

   ![img](https://help-assets.codehub.cn/enterprise/20190802141601.png)

这时候你的项目根目录应该会出现一个名为`CNAME`的文件了。如果没有的话，打开你本地博客`/source`目录，我的是`D:\Study\MyBlog\source`，新建`CNAME`文件，注意没有后缀。然后在里面写上你的域名，保存。最后运行`hexo g`、`hexo d`上传到`Coding`。

## 第七步：发布新文章

1. 安装一个扩展`npm i hexo-deployer-git`。

2. 然后输入`hexo new post "article title"`，新建一篇文章。

3. 然后打开`MyBlog\source\_posts`的目录，可以发现下面多了一个文件夹和一个`.md`文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。
4. 编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到`Coding`上。这时打开你的域名主页就能看到发布的文章啦。![](https://s1.ax1x.com/2020/04/04/GdlPQx.png)




> 参考链接：
>
> [快速使用代码仓库]([https://help.coding.net/docs/start/repository.html#%E6%9C%AC%E5%9C%B0-Git-%E4%B8%8E-CODING-%E6%8E%88%E6%9D%83%E8%BF%9E%E6%8E%A5](https://help.coding.net/docs/start/repository.html#本地-Git-与-CODING-授权连接))
>
> [如何搭建静态网站？](https://help.coding.net/docs/devops/cd/static-website.html)
>
> [Hexo+Github博客搭建完全教程](https://sunhwee.com/posts/6e8839eb.html#toc-heading-4)
>
> [写给朋友的Hexo建站指南，含Coding Pages、域名解析、日常使用](https://www.jianshu.com/p/e44b5161ba2c)
