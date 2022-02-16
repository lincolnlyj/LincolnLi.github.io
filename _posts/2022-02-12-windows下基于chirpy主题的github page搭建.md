---
layout: post
title: windows下基于chirpy主题的github page搭建
description:
category: [环境配置]
tags:
math: false
mermaid: false
comments: true
date: 2022-02-12 22:57 +0800
---
## 网页风格选择

利用jekyll实现github page的搭建，页面的风格选择的是chirpy，主题代码可以参考[github 仓库](https://github.com/cotes2020/jekyll-theme-chirpy)，chirpy也在[官方文档](https://chirpy.cotes.page/)里提供了相关的搭建教程。

## 仓库配置

### 创建仓库

首先利用[Chirpy Starter](https://github.com/cotes2020/chirpy-starter/generate)创建自己的github仓库，需确保之前已经有了github账号，仓库名称设置为`github账户名称.github.io`，注意github账户名称不是在profile中设置的name，之后将仓库clone到本地（需要提前在本地安装git，git的安装和使用在网上有很多的教程教程）。

### 本地部署（Windows系统）

#### 安装依赖

1. 安装Ruby

    从[Ruby官网](https://rubyinstaller.org/downloads/)下载最新的*RubyInstaller*，默认安装即可，注意将Ruby添加到PATH，保证在控制台可以使用，安装完成后要勾选运行`ridk install`，会弹出如下窗口，这里选择选项3进行安装

    <img src="https://gitee.com/LincolnLi/blog-images/raw/master/img/20220212093127.png" style="zoom: 80%;" />

2. 安装RubyGems

   从[官网](https://rubygems.org/pages/download)下载zip压缩包，解压后进入解压路径打开控制台运行`ruby setup.rb`。

3. 安装Jekyll和bundle

   运行如下命令

   ``` shell
   gem install jekyll
   gem install bundler
   ```

   如果需要使用分页功能可以使用`gem install jekyll-paginate`安装jekyll-paginate插件。

4. 验证

   运行`jekyll -v`，如果输出了Jekyll的版本说明安装成功

#### 运行本地服务器

第一次运行前需要在clone下的仓库中运行`bundle`命令初始化，之后运行`bundle exec jekyll s`即可，在docker中运行可以参考官网的教程，此时服务器就会被部署在<http://127.0.0.1:4000>

### 远程部署

#### 准备工作

1. 修改`_config.yml`文件

    在`_config.yml`文件中，将url这一行填入自己的域名

    <img src="https://gitee.com/LincolnLi/blog-images/raw/master/img/20220212143644.png" style="zoom:67%;" />

    如果仓库名称并不是自己github的用户名，则在baseurl中填入自己的仓库名称，如下图

    <img src="https://gitee.com/LincolnLi/blog-images/raw/master/img/20220212144210.png" style="zoom:67%;" />

2. 确保文件夹中有如下文件`.github/workflows/pages-deploy.yml`，如果没有，则从[此网址](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/.github/workflows/pages-deploy.yml.hook)复制创建一个文件，并确保`on.push.branches`和自己仓库的主要分支相同。
3. 确保自己的问价夹中含有`tools/deploy.sh`文件，如果没有，可以从我的仓库中复制一份
4. 由于运行的操作系统为Windows，需要在`Gemfile.lock`中加入Linux平台，运行`bundle lock --add-platform x86_64-linux`代码即可。

#### 使用GitHub Actions进行部署

将当前的修改push到github仓库中中，会自动生成一个`gh-pages`的分支，其对应的就是本地的`_site`和`.jekyll-cache`文件夹中的内容，由Jekyll自动生成。在`settings`中找到`pages`选项，进行如下设置，就可以通过浏览器访问自己的github page了。

<img src="https://gitee.com/LincolnLi/blog-images/raw/master/img/20220212173203.png" style="zoom: 67%;" />

## 其他配置

### 修改图标和头像

选择一张大小大于等于512×512的图片上传到[此网站](https://realfavicongenerator.net/)（点击<kbd>Select your Favicon image</kbd>），拉到底部选择<kbd>Generate your Favicons and HTML code</kbd>，在新页面下载压缩文件，将其中的`png`和`ico`文件复制到`assets/img/favicons/`文件夹，如果没有就创建即可。

对于头像，在`_config.yml`文件中将`avatar`设置为图片的相对地址或者网络地址即可

<img src="https://gitee.com/LincolnLi/blog-images/raw/master/img/20220212180626.png" style="zoom: 80%;" />

### 评论区配置

本人的评论区采用的是`utterances`实现的，通过[utterances提供的链接](https://github.com/apps/utterances)给自己的仓库安装`utterances app`，之后将`_config.yml`文件中的`comments.active`设置为`utterances`，将`comments.utterances.repo`设置为`username/repo_name`即可开启评论，`issue_term`可以根据需求修改。

### 个人资料配置

个人资料可以在`_config.yml`的`social`中进行修改，进行展示与否可以在`contact.yml`中找到对应项进行注释或取消注释。

## 发布博客

博客的markdown文件需要放在`_posts`中，命名为`YYYY-MM-DD-TITLE.md`，文件的首部需要设置一些参数，采用的是yaml格式，基础的格式如下

```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [TOP_CATEGORIE, SUB_CATEGORIE]
tags: [TAG]     # TAG names should always be lowercase
---
```

修改`_config.yml`中的`timezone`变量可以修改时区，数学公式支持可以通过添加`math: true`来开启，目录默认开启，添加`toc: false`可以关闭，评论可以通过`comments: false`来关闭，mermaid流程图也需要手动添加`mermaid: true`开启。

### 利用插件辅助

也可以利用一些插件来辅助发布博客，本次选用了[jekyll-compose](https://github.com/jekyll/jekyll-compose)插件，安装时在Gemfile中加入如下代码

```
gem 'jekyll-compose', group: [:jekyll_plugins]
```

再在控制台中执行`bundle`命令即可，使用方法参见[jekyll-compose](https://github.com/jekyll/jekyll-compose)的README.md文件即可即可。

## 参考链接

1. [chirpy官方文档](https://chirpy.cotes.page/)
1. [在Windows上安装Jekyll](https://www.jianshu.com/p/58e2c5ea3103)
2. [Install Jekyll on Windows](https://idratherbewriting.com/documentation-theme-jekyll/mydoc_install_jekyll_on_windows.html#bundler)
2. [utterances参考文档](https://utteranc.es/)
