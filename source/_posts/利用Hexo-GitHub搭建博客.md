---
title: 利用Hexo+GitHub搭建博客
top: false
cover: false
toc: true
mathjax: false
date: 2020-04-01 16:45:47
author: Newbiee
img:
coverImg:
password: 
- GitHub
- Hexo
summary:
tags: 
- GitHub
- Hexo
- 博客
categories: 
- 博客搭建
---



## 引言

搭建个人博客的方式不外乎有以下三种方法：

1. 由诸如`简书`、`CSDN`或者`博客园`等第三方平台提供的博客服务，但拘束略多，广告也很烦人；
2. 自行购买云服务器和域名，但成本略高且需要经常维护，没有时间搞；
3. 基于开源框架搭建博客，然后利用`github page`平台上托管我们的博客，所以本文着要介绍本人在网上学习基于`Hexo`+`github`博客搭建的过程记录，烙下学习的脚印。

接着介绍下`Hexo框架`。根据官方介绍：[Hexo]('https://hexo.io/zh-cn/docs/index.html')是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。所以此篇文章主要内容为：介绍Hexo的初步搭建以及部署到`GitHub`平台上，可选择的进行个人域名的绑定；

搭建的大体步骤：

1. 安装`Git`、`Node.js`、`Hexo`；
2. 在`GitHub`创建个人仓库以及`SSH`；
3. 将`Hexo`部署到`GitHub`；
4. 绑定个人域名；；
5. 发布新文章。

### 第一步：安装

1. 主要介绍Windows系统的安装步骤。由于Git官网资源下载太慢，建议使用国内镜像资源：[Git国内镜像资源链接]('https://github.com/waylau/git-for-win')。![http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303210957.png](http://q6mdlpvkp.bkt.clouddn.com/搜狗截图20200303210957.png)

   安装的过程也没啥好说的，一直下一步。

2. 因为Hexo是基于`Node.js`编写的，所以需要安装`Node.js`。可以直接官网下载：[Node.js](http://nodejs.cn/download/)![Node.js资源下载预览](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303214502.png)

   安装的步骤也是一直下一步，安装完成后，可以按win+r调出cmd命令行输入`npm -v `和 `node -v`验证是否安装成功，出现版本号则证明成功。![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303215526.png)

3. 接着进行Hexo框架的安装：首先在本地电脑新建一个文件夹，取名`MyBlog`，用来存放博客的相关文件，我选择的路径为：`E:\MyBlog` 。进入此目录下，点击鼠标右键的“Git Bash Here”进入Git命令行，然后输入`npm install -g hexo-cli`进行安装Hexo。

   ~~~Bash
   npm install -g hexo-cli
   hexo -v 		#安装完成后输入验证是否安装成功。
   ~~~

4. 针对Linux系统，我采用的是`Centos8`发行版，安装`git`以及`Nodejs`只需要在终端输入：

   ~~~Bash
   sudo yum install git
   sudo yum install nodejs
   sudo yum install npm
   ~~~

   然后更改环境变量，使得我们可以再终端直接调用相关命令。

   ```bash
   sudo vim /etc/profile
   ```

   复制下面两行到刚打开的`profile`文件最底部：

   ```bash
   export NODE_HOME=你的node安装地址，如/home/shw/MySoftwares/node-v12.8.0-linux-x64
   export PATH=$PATH:$NODE_HOME/bin
   ```

   保存后退出，再执行下面命令将环境变量生效：

   ```bash
   source /etc/profile
   ```

   将目录软链接到全局环境下（命令后面的`/usr/local/bin/node`是固定的）

   ```bash
   sudo ln -s /home/shw/MySoftwares/node-v12.8.0-linux-x64/node /usr/local/bin/node
   sudo ln -s /home/shw/MySoftwares/node-v12.8.0-linux-x64/npm /usr/local/bin/npm
   ```

   这样安装好了以后使用`npm`安装的包(比如：`ionic serve`)，使用包的命令时可能会提示找不到命令，没关系，在用户目录下终端执行下面命令固定写法固定写法：

   ```bash
   echo -e "export PATH=$(npm prefix -g)/bin:$PATH" >> ~/.bashrc && source ~/.bashrc
   ```

   这样我们在所有用户下，都可以使用`npm`，也可以使用`npm`安装的包的命令。成功的将`nodejs`安装并配置到全局环境下。

   安装完毕后，可以再输入`node -v`以及`npm -v`检查是否安装完成，如果出现版本号证明无误。

   下一步是添加国内镜像，加速下载速度。

   ```bash
   npm config set registry https://registry.npm.taobao.org
   ```

### 第二步：Hexo初始化

1. 接着Hexo安装完毕，我们输入`hexo init MyBlog`进行初始化：

   ~~~bash
   hexo init MyBlog
   ~~~

2. 初始化结束后，进入到`MyBlog`目录中，进入Git Bash中进行必备组件的安装，输入命令：

   ~~~bash
   npm install
   ~~~

   经过一段时间后，你的目录中会有以下几个文件或文件夹：

   * `node_modules:` 依赖包
   * `public：`存放生成的页面
   * `scaffolds：`生成文章的一些模板
   * `source：`用来存放你的文章
   * `themes：`主题**
   * `_config.yml:` 博客的配置文件

3. 无误后，输入`hexo generate`或者`hexo g `生成静态网页，

   ~~~Bash
   hexo g
   ~~~

   然后输入` hexo service` 或者	hexo s 	可以临时打开本地服务器

   ~~~Bash
   hexo s
   ~~~

   开启本地服务器后，就可以用浏览器访问：`http://localhost：4000`，预览博客网站。![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303222506.png)

   按`Ctrl+c`结束进程。这样我们的博客就初步完成了，接下来的工作就是把我们的博客部署到GitHub page上。
   
4. 现在对根配置文件内容做下详述：

   ~~~yaml
   # Hexo Configuration
   ## Docs: https://hexo.io/docs/configuration.html
   ## Source: https://github.com/hexojs/hexo/
   
   # Site
   title: Hexo          # 网站的标题，可能用在各种布局的页面中
   subtitle:            # 网站子标题
   description:         # 网站的描述性
   keywords:            # 网站的关键字
   author: John Doe     # 网站的作者
   language:            # 网站采用语言，要跟/theme/***/languages/**.yml下的文件名对应。
   timezone:            # 网站的时区
   
   # URL
   ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
   url: http://yoursite.com              # 网站的url，如果不在域名根目录，应包含子目录，且root要设置为`/子目录/`
   root: /                               # 网站的根目录
   permalink: :year/:month/:day/:title/  #文章永久链接的形成模版。每一篇文章都有唯一的url。
   permalink_defaults:                   #文章永久链接中，各部分的默认值。
   
   # Directory
   source_dir: source          # 网站中源文件（比如Markdown啊什么的所在的文件夹）
   public_dir: public          # 生成的静态网站的目录
   tag_dir: tags               # 标签页所在的文件夹。
   archive_dir: archives       # 文档页所在的文件夹
   category_dir: categories    # 类别也所在的文件夹
   code_dir: downloads/code    # 代码也所在的文件夹
   i18n_dir: :lang             # 国际语言所在的文件夹
   skip_render:                # 忽略文档清单
   
   # Writing 写作
   new_post_name: :title.md    # 默认新建文档名，`:title`为变量，指文档标题，也可用其他变量
   default_layout: post        # 新建文档的默认布局
   titlecase: false            # 是否要把标题中的首字符大写
   external_link: true         # 是否要在新开tab中打开外链
   filename_case: 0            # 文件名是否小写敏感
   render_drafts: false        # 是否渲染草稿
   post_asset_folder: false    # 是否启用资源文件夹。如启用，新建文档同时建立同名的资源文件夹
   relative_link: false        # 是否把站内资源的链接改为站内相对链接。建议关闭。
   future: true                # 文档中指定为未来时间创建
   highlight:
     enable: true              # 是否开启代码高亮功能
     line_number: true         # 代码块中是否在前面加上行号
     auto_detect: false        # 是否自动检测代码块的语言（比如xml、JavaScript、mermaid等）
     tab_replace:              # 用什么字符来代替tab(`\t`)字符。
     
   # Home page setting
   # path: Root path for your blogs index page. (default = '')
   # per_page: Posts displayed per page. (0 = disable pagination)
   # order_by: Posts order. (Order by date descending by default)
   index_generator:       
     path: ''              # 主页所在路径，默认为''
     per_page: 10          # 主页的索引页包含文章数量，如未定义，则采用根目录下的`per_page`值
     order_by: -date       # 文章（Post类型）排序属性，`-`为降序
     
   # Category & Tag
   default_category: uncategorized      # 对文档的默认分类
   category_map:                        # 对文档中的分类字段进行映射。建立分类文件夹时采用映射后的字符串
   tag_map:                             # 对文档中的标签字段进行映射。建立标签文件夹时采用映射后的字符串
   
   # Date / Time format
   ## Hexo uses Moment.js to parse and display date
   ## You can customize the date format as defined in
   ## http://momentjs.com/docs/#/displaying/format/
   date_format: YYYY-MM-DD   # 日期格式
   time_format: HH:mm:ss     # 时间格式
   
   # Pagination
   ## Set per_page to 0 to disable pagination
   per_page: 10                      # 主页/分类/标签/存档等类型索引页包含文章数量
   pagination_dir: page              # 分页所在文件夹
   
   # Extensions                      # 扩展。放置插件和主题
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: landscape                                # 默认主题landscape
   
   # Deployment
   ## Docs: https://hexo.io/docs/deployment.html    
   deploy:                                         # 定义部署
     type:
   ~~~

   

### 第三步：将Hexo部署到`GitHub`上

1. 在GitHub中创建仓库：

   创建的过程中需要注意的是，仓库名为：`用户名.github.io`，以及把`README`初始化选中，如图所示（图中的警告是因为我已经创建过此仓库了，第一次创建不会出现这种问题的）：![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303223420.png)

2. 生成SSH添加到GitHub

   在根目录下打开git bash输入：

   ~~~Bash
   git config --global user.name "yourname"
   git config --global user.email "youremail"
   ~~~

   这里的`yourname`输入你的`GitHub`用户名，`youremail`输入你`GitHub`的邮箱。这样`GitHub`才能知道你是不是对应它的账户。

   我们可以输入一下命令，检查输入有无问题：

   ```bash
   git config user.name
   git config user.email
   ```

   接下来就是建立SSH了，把`youremail`换成你的邮箱就会生成SSH文件，并且告诉你文件的路径：

   ```bash
   ssh-keygen -t rsa -C "youremail"
   ```

   ![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303224343.png)

   这时我们只需要按照路径打开`id_rsa.pub`文件，将内容复制，接着打开GitHub，在setting中点击`SSH and GPG keys`选项，新建SSH，名字所以，内容为`id_rsa.pub`文中的所有内容。![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303225314.png)

   在git bash 中输入ssh -T git@github.com命令，出现以下结果即为成功：

   ~~~Bash
   ssh -T git@github.com
   ~~~

   ![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303225717.png)

3. 将Hexo部署到GitHub

   这一步主要是为了把Hexo生成的文章部署到GitHub上，操作为打开根目录`_config.yml`文件，这是博客的配置文件，在这里你可以修改与博客配置相关的各种信息。

   修改最后deploy部分为：

   ```yml
   deploy:
     type: git
     repository: https://github.com/newbiee2020/newbiee2020.github.io
     branch: master
   ```

   修改下repository为你新建仓库的地址，目的是部署时，告诉工具，要将生成的网页git到你的仓库中。

   下面安装deploy-git，即部署命令。

   ```bash
   npm install hexo-deployer-git --save
   ```

   接着介绍下`hexo`生成、部署的基本命令：

   ```bash
   hexo clean
   hexo generate
   hexo server
   hexo deploy
   ```

   `hexo generate` 以及`hexo server`前面已经提及，而`hexo clean `即为清除了你之前生成的东西，`hexo deploy`即为部署文章到你的GitHub中。其中部署命令执行后可能需要你输入你的GitHub用户名以及密码：

   ![](http://q6mdlpvkp.bkt.clouddn.com/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20200303230949.png)

   部署成功的标志如下图所示：![](http://q6mdlpvkp.bkt.clouddn.com/git.png)

   之后不久你就可以通过输入`yourname.github.io`访问你的博客啦。如果你想改变你的域名，则可以进行下一步，购买域名以及绑定。

### 第四步：绑定个人域名

1. 购买域名。我是在[腾讯云](https://cloud.tencent.com/act/domainsales?from=11394)上购买的，最便宜的是1元/首年，好的如.net的为50元/首年，因为可能是一时兴起吧，先买个最便宜的练练手，我选择的是`.xyz`的，一块钱~

2. 购买完成后需要在域名管理中进行实名认证，后点击解析添加两条记录：![](http://q6mdlpvkp.bkt.clouddn.com/%E6%B7%BB%E5%8A%A0%E8%AE%B0%E5%BD%95.png)

3. 然后打开你的GitHub的博客仓库，点击setting，在Custom domain中填写你的域名。![](http://q6mdlpvkp.bkt.clouddn.com/GitHub%E6%9B%B4%E6%94%B9%E5%9F%9F%E5%90%8D.png)

4. 在`MyBlog\source\`中新建无后缀的`CNAME文件`，且内容为你的域名。接着执行`hexo g && hexo d `命令，不久后，你就可以在浏览器中输入你购买的域名访问到你的博客网站。

### 第五步：发布新文章

1. 在博客根目录打开git bash命令行，安装扩展：`npm i hexo-deployer-git`

   ~~~Bash
   npm i hexo-deployer-git
   ~~~

2. 安装完成后，即可通过npm new post "article name"，新建文章

   ~~~Bash
   npm new post "Test article"
   ~~~

3. 接着就可以在`MyBlog\source\\_post`中找到对应的以`.md`为后缀的文件，我们对其进行编辑。

4. 最后通过`hexo g && hexo d `进行网页的生成和部署，之后就可以在博客网站中看到此文章。然后后续我们可以更改主题，定制我们的专属博客~



> 参考链接：
>
> 1. [Hexo+Github博客搭建完全教程](https://sunhwee.com/posts/6e8839eb.html)
> 2. [主题matery](https://github.com/shw2018/hexo-theme-matery)
> 3. [最新GitHub+HEXO搭建博客](https://www.jianshu.com/p/bad24beeb51e)
> 4. [Hexo+NexT（二）：Hexo站点配置详解](https://blog.csdn.net/loze/article/details/94209583)



