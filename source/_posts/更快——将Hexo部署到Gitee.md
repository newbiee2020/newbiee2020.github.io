---
title: 更快——将Hexo部署到Gitee
date: 2020年4月6日00:54:13
author: Newbiee2020
top: false
cover: true
toc: true
password: 
mathjax: 
summary: 
tags:
- Hexo
- NexT
- 博客
- Gitee
categories:
- 博客搭建
---
## 原因

之前试过将hexo部署到GitHub，但因为GitHub服务器在美国所以访问速度很慢，之后我们想到将hexo部署到Coding，但搭建完毕后，依然感觉速度不快，经网上查找，发现Coding的服务器是在香港，所以访问速度和GitHub相比也是半斤八两，所以有没有服务器是在中国大陆，且和GitHub相似的托管平台呢，当然有啦，那就是——码云Gitee。

## 步骤

由于之前我们已经搭建好了Hexo+Coding的博客，所以当我们需要部署到Gitee上时，就很方便了，我们需要做的修改有：

1. 在Gitee上新建一个仓库，记住仓库名，比如我的：yyxu2020。
2. 在根配置文件_config.yml中的最后几行修改为：

~~~yaml
deploy:
  type: git
  repository:
    # coding: 在上文hexo+coding部署你所填写的地址，我们不需要啦
    gitee: https://gitee.com/newbiee2020/yyxu2020
  branch: master
~~~

在gitee中的https://gitee.com/newbiee2020/yyxu2020，newbiee2020是你的用户名，yyxu2020是你的仓库名。

3. 在你新建的仓库页面——>服务——>Gitee Pages中进行设置：![](https://s1.ax1x.com/2020/04/04/G0JgOA.png)

4. 在设置中我们需要注意的是如果你想在gitee部署且自定义域名的话是需要会员的，一年99元，开通后部署。由于是第一次使用，所以我们可以试用一个月看看。

5. 点击试用Gitee Pages Pro后，在自定义域名中填写你购买后的域名，例如：yyxu2020.xyz。![](https://s1.ax1x.com/2020/04/04/G0YdXj.png)

6. 然后点击配置域名证书。此处需要说明：我的域名实在腾讯云上购买的，所以如果你想知道域名证书的话，需要登录腾讯云——>SSL证书——>下载。![](https://s1.ax1x.com/2020/04/04/G0tGr9.png)

7. 下载好的压缩包中，我们需要用到的的有两个文件，一个是Apache中的.crt和.key。

   我们需要用记事本打开他们，然后分别复制，粘贴到对应的以下两个部分：![](https://s1.ax1x.com/2020/04/04/G0Nki6.png)

8. 接下来就是添加自定义域名解析，我们要在腾讯云的域名管理中，对使用到的域名进行添加记录：![](https://s1.ax1x.com/2020/04/04/G0NxtP.png)

9. 然后在命令行输入：

   ~~~bash
   hexo clean
   hexo generate
   hexo deploy
   ~~~

10. 这样就可以打开你的博客了，如果不行，那就在Gitee Pages设置页面中点击启动或者更新。

   



