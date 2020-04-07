---
title: 在Android手机上安装linux系统
date: 2020年3月28日00:54:13
author: Newbiee2020
top: false
cover: true
toc: true
password: 
mathjax: 
summary: 
tags:
- Termux
- Linux
- 安卓
categories:
- 其它
---

## 引言

由于最近自己想搭建一个个人博客，在Windows上设置相关文件时老是出现无法理解的错误，所以转战Linux系统，可供选择的方案有：

* Windows系统上利用虚拟机搭建Linux系统。但问题是我的电脑无法24小时在线，所以也导致虚拟机上的Linux也会关机；
* 购买云服务器。现在阿里云学生专享的云服务器114元/年，平均一个月10元，但作为一个标致的无收入学生，月月还得交两个卡的话费（一个已使用多年的移动号，一个是专用来宽带的电信号），还得吃吃喝喝，暂且受不起；
* 最后是本文要介绍的：在Android手机上安装Linux系统然后在PC端利用SSH登录此服务器（其实主要是想瞎捣腾下，以上两个方案不用的理由可以忽略）。

自己在网上查找了相关的资料，注意到此处我使用的Android机器是无法ROOT的，所以我选择了`安装Termux app——>开启SSH服务——>PC端利用Xshell登录 `的方案。

## 操作步骤

### 1.安装`Termux`软件

* 首先下载`Termux`：[下载](https://f-droid.org/packages/com.termux/)，然后手机安装（安装过程与其他软件安装一样）。

![](http://q6u4ibui1.bkt.clouddn.com/termux%E5%AE%98%E7%BD%91.png)

* 在酷安应用商场下载安装（推荐，因为下载速度很快，而且有相关的讨论，氛围很重要）

### 2.相关配置

安装完毕后，就可以直接打开啦，首当其冲的就是黑色背景的终端，然后我们要做的是：

更变为国内源（清华），在手机里的Linux系统输入：

~~~bash
sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux stable main@' $PREFIX/etc/apt/sources.list

apt update && apt upgrade
~~~

这样就把`Termux`的官方源更改为国内的清华源了，安装速度直线上升！（如果出错，建议在[清华大学开源软件镜像站-Termux 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/termux/)中找到相关的配置帮助）

安装SSH服务

~~~bash
pkg install openssh
~~~

开启SSH服务，为了电脑可以访问此系统

~~~bash
sshd
~~~

安装`proot`，获得类似于`root`的功能

~~~bash
pkg install proot
~~~

安装完毕，输入一下命令获得`root`权限：

~~~bash
termux-chroot
~~~

如果你手机已经`root`，可以安装`tsu`，和`su`功能类似

~~~bash
pkg install tsu

tsu	# 进入管理员模式
exit # 回到用户模式
~~~

然后给`termux`对应的用户设置一个密码：

~~~bash
passwd
~~~

系统会出现以下提示，你自己设置下密码（输入的时候不会显示）：

~~~bash
New password:
~~~

接下来需要获得你手机上这个Linux系统的`IP`地址，框中的就是你需要记下来的`ip`：

~~~bash
ifconfig
~~~

![](http://q6u4ibui1.bkt.clouddn.com/Ip.png)



### 3.远端连接

我们利用的软件是`Xshell`，首先`新建会话`，在新建界面需要你输入的内容有：

![](http://q6u4ibui1.bkt.clouddn.com/xshell%E6%96%B0%E5%BB%BA%E5%9B%BE%E6%A0%B7.png)

接着点击`用户身份验证`，填写`用户名`和`密码`：

![](http://q6u4ibui1.bkt.clouddn.com/xshell%E6%96%B0%E5%BB%BA2.png)

然后点击连接，就可以连接到手机上的Linux系统啦：

![](http://q6u4ibui1.bkt.clouddn.com/%E8%BF%9B%E5%85%A5%E7%95%8C%E9%9D%A2.png)



### 4.安装`ubuntu`系统

在安装好`wget`以及`proot`后，自创建一个目录然后输入：

~~~bash
curl -sL www.termux.xyz/Linux/Ubuntu/ubuntu-ports.sh | bash
~~~

安装完毕后，若要执行`ubuntu`，输入：

~~~bash
~/start-ubuntu.sh
~~~

接下来就进入`ubuntu`系统

### 5.安装`Ubuntu`图形化界面

如果想要图形化界面，需要`VNC Viewer`软件，可以在Google下载，或者使用[链接下载](https://www.lanzous.com/ialfvre)

安装完毕后，创建：用户和密码见`termux`界面，默认为：`127.0.0.1：1`，密码为12345678


### 6.玩转`Termux`软件

在安装完毕后，自己发现了一个问题，那就是如果手机息屏的话，`PC`的远端连接就会被断开，当手机解锁了之后，`PC`又自动连接上了，非常的不方便，现在还没找到比较好的解决办法。However，自己在摸索这个软件一段时间后，我感觉没必要使用`PC`远端连接了，我们可以采用直接运行软件，因为只要我们把这个软件的后台运行添加到白名单，只要你不关机，不管你洗不洗屏，都保持着运行，而且也支持多开`sessions`。同时，此软件还采用了手机音量键+手机输入的组合，提供了非常方便的快捷键。

* 常按屏幕：

  ~~~bash
  长按屏幕
  ├── COPY:复制
  ├── PASTE:更多
  ├── More:更多
     ├── Select URL: 选择网址
     └── Share transcipt: 分享命令脚本
     └── Reset: 重置
     └── Kill process: 杀掉当前终端会话进程
     └── Style: 风格配色
     └── Help: 帮助文档
  ~~~

* 屏幕由左向右滑动：

  可以新建、切换、重命名会话session和调用弹出keyboard。

  ![](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHBzL2ltYWdlLjMwMDEubmV0L2ltYWdlcy8yMDE4MDUwMS8xNTI1MTQyNTcwNjE4NC5wbmc=.jpg)

* 常用快捷键：

  `Ctrl`键是终端用户常用的按键 ，但我们手机触摸键盘都没有这个按键。为此，`Termux`使用音量减小按钮来模拟`Ctrl`键。
  例如，在触摸键盘上按`音量减小`+ `L`发送与在硬件键盘上按`Ctrl + L`相同的输入。

  > `Ctrl+A` -> 将光标移动到行首
  >
  > `Ctrl+C` -> 中止当前进程
  >
  > `Ctrl+D` -> 注销终端会话
  >
  > `Ctrl+E` -> 将光标移动到行尾
  >
  > `Ctrl+K` -> 从光标删除到行尾
  >
  > `Ctrl+L` -> 清除终端
  >
  > `Ctrl+Z` -> 挂起（发送`SIGTSTP`到）当前进程

  `音量加键`也可以作为产生特定输入的`特殊键`：

  > `音量加+E` -> `Esc键`
  >
  > `音量加+T` -> `Tab键`
  >
  > `音量加+1` -> `F1（和音量增加+ 2→F2等）`
  >
  > `音量加+0` -> `F10`
  >
  > `音量加+B` -> `Alt + B，使用readline时返回一个单词`
  >
  > `音量加+F` -> `Alt + F，使用readline时转发一个单词`
  >
  > `音量加+X` -> `Alt+X`
  >
  > `音量加+W` -> `向上箭头键`
  >
  > `音量加+A` -> `向左箭头键`
  >
  > `音量加+S` -> `向下箭头键`
  >
  > `音量加+D` -> `向右箭头键`
  >
  > `音量加+L` -> `| （管道字符）`
  >
  > `音量加+H` -> `〜（波浪号字符）`
  >
  > `音量加+U` -> `_ (下划线字符)`
  >
  > `音量加+P` -> `上一页`
  >
  > `音量加+N` -> `下一页`
  >
  > `音量加+.` -> `Ctrl + \（SIGQUIT）`
  >
  > `音量加+V` -> `显示音量控制`
  >
  > `音量加+Q` -> `显示额外的按键视图`

* 基本命令

  `Termux`除了支持`apt`命令外,还在此基础上封装了`pkg`命令,`pkg`命令向下兼容`apt`命令：

  ~~~bash
  pkg search <query>              搜索包
  pkg install <package>           安装包
  pkg uninstall <package>         卸载包
  pkg reinstall <package>         重新安装包
  pkg update                      更新源
  pkg upgrade                     升级软件包
  pkg list-all                    列出可供安装的所有包
  pkg list-installed              列出已经安装的包
  pkg shoe <package>              显示某个包的详细信息
  pkg files <package>             显示某个包的相关文件夹路径
  ~~~

  常用的目录路径：

  ~~~bash
  ~ > echo $HOME
  /data/data/com.termux/files/home
   ~ > echo $PREFIX
  /data/data/com.termux/files/usr
   ~ > echo $TMPPREFIX
  /data/data/com.termux/files/usr/tmp/zsh 
  ~~~


## 番外 安装Python并运行程序：

1. 首先安装Python以及编辑器vim

   ~~~bash
   pkg install python
   pkg install vim
   pkg install vim-python
   ~~~

2. 导入我们要运行的程序：

   此程序我在电脑上已运行过一段时间，所用的配置都已完成所以可以利用`xshell`携带的`xftp`工具直接导入。此处贴上运行的项目`GitHub`地址：[Python3 实现的 bilibili 直播助手（多用户）](https://github.com/yjqiang/bili2.0/)。或者你也可以`pkg install wget`下载`wget`，然后下载
   
3. 安装`requirements.txt`依赖库

   ~~~bash
   python -m pip install --upgrade pip
   
   pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
   ~~~

   以上命令分别为更新`pip`版本，然后间接利用清华源高速下载依赖库。

4. 万事俱备，开冲。

   ~~~bash
   python run.py
   ~~~
   
   <video id="video" controls="" preload="none">
       <source id="mp4" src="http://q6u4ibui1.bkt.clouddn.com/%E6%89%8B%E6%9C%BA%E8%BF%90%E8%A1%8C%E5%BD%95%E5%B1%8F.mp4" type="video/mp4">
   </video>










参考连接：

> * [安卓手机上安装termux，把手机当linux服务器用](https://blog.csdn.net/yinmingxuan/article/details/90511558)
>* [清华大学开源软件镜像站-Termux 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/termux/)
> * [Termux 高级终端安装使用配置教程](https://www.sqlsec.com/2018/05/termux.html)