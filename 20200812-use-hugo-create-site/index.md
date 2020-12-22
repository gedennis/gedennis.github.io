# 使用 Hugo 搭建个人站点


## 摘要

## 为什么写博客

## Hugo 基础使用

Hugo 是一款使用 Go 编写的的静态站点生成器。它使用非常简单，你只需要关注你的内容，选择一款你喜欢的主题，通过几个简单的命令就可以快速生成一个漂亮的个人站点。关于静态网站生成器，之前使用过 Jekyll 和 Hexo。 这两款工具使用也挺简单方便，但安装设置相对来说复杂一点，最重要是生成速度比较慢。Hugo 的页面生成速度很快，目前我之前博客全部从 Hexo 转为使用 Hugo。

### 准备

本文假设您已经做好了这些准备：

- 电脑安装了 Git。
- 有 Github 账号, 并熟悉 Github 的基础操作。

### 安装

Hugo 同时支持 MacOS、Windows 和 Linux 等操作系统，安装也非常简单。你可以到[官网下载](https://gohugo.io/getting-started/installing/)相应的版本。

如果是 MacOS, 建议使用 Homebrew 安装。

```
$ brew install hugo
```

如果是 Windows 或者 Homebrew 下载比较慢的话，建议到 Hugo 的 [github release 页面](https://github.com/gohugoio/hugo/releases)下载压缩包。下载解压之后，可以得到 Hugo 的执行程序。
MacOS 或者 Linux 可以将指令放在 `/usr/local/bin` 目录。Windows 可以放在 `C:\Hugo\bin`目录。

> 注意：Windows 下需要将 hugo 的目录添加到环境变量。【开始】-> 【系统】 -> 【高级系统设置】-> 【环境变量】-> 【PATH】-> 【新增】，然后添加 Hugo 的路径 `C:\Hugo\bin`。

安装完成之后，验证安装。

```
$ hugo version
Hugo Static Site Generator v0.69.2/extended darwin/amd64 BuildDate: unknown
```

### 快速入门

下面我们介绍几个 Hugo 的常用指令来快速入门。

**生成站点**
使用 `hugo new site` 命令生成站点。

```
$ hugo new site <站点名称>
```

进入生成的目录，目录结构如下：

```
$ tree -L 2
.
├── archetypes      // hugo new 生成博客的模板，通常不需要动
│   └── default.md
├── config.toml     // 系统配置文件，后面详细介绍
├── content         // 页面和博客存放的目录
├── data            // 静态数据
├── layouts         // 布局文件
├── static          // 静态文件目录
└── themes          // 主题目录
```

**添加主题**

```
$ cd <站点目录>
$ git init
$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

下载完成之后，将主题添加到站点配置中。

```
$ echo 'theme = "LoveIt"' >> config.toml
```

**添加文章**
使用命令`hugo new content/<种类>/<文件名>.<后缀名>`创建第一篇文章。

```
$ hugo new posts/my-first-post.md
```

这里在 posts 目录下创建了一个名为`my-first-post`的 Markdown 文件。打开文件你会看到。

```
---
title: "My First Post"
date: 2020-08-21T16:17:14+08:00
draft: true
---
```

这是文章的一些信息。由 `archetypes/default.md` 文件指定的。你可以在文件里添加一些内容。

**预览**
现在我们可以通过 `hugo server` 在本地进行预览。

```
$ hugo server -D
```

浏览器打开 `http://localhost:1313/`, 就可以看到页面了。
这里只是简单的感受了一下 hugo 的使用，下面我们将正式介绍 Hugo 搭建站点的一套工作流。

## 基本框架

这一部分我们介绍如何搭建我们博客的基础框架，包括代码仓库、站点配置、Github Page 托管、站内搜索和百度统计/谷歌统计等内容。

> 这一部分基本就配置一次就可以了，平时基本不需要改动。

### 创建代码仓库

这里我们需要创建两个代码仓库 `blog` 和 `<你的github名称>.github.io` 两个仓库。

- blog 仓库： 用来托管我们站点的代码。
- \*.github.io: 用来存放托管的静态页面。

> 请到自己的 Github 创建这两个仓库。如果没有 Github 账号或者不会创建，请在网上自行搜索学习。

**本地创建站点目录**

```
// 创建 blog 目录
$ hugo new site blog

// 添加主题， 这里以 LoveIt 为例
$ cd blog
$ git init
$ git remote add origin git@github.com:<你的账号>/blog.git
```

现在我们就已经对 blog 仓库做了版本管理, 将代码推到 Github。因为有些文件我们不需要托管，在 blog 目录下创建 `.gitignore` 文件。可以参考网站 [gitignore.io](https://www.toptal.com/developers/gitignore) 生成 ignore 文件内容。

```
/public/
/resources/_gen/
hugo_stats.json

# Executable may be added to repository
hugo.exe
hugo.darwin
hugo.linux
```

现在我们就已经对 blog 仓库做了版本管理, 可以将代码推到 Github。

```
$ git add .
$ git commit -m "commit message"
$ git push -u origin master
```

### 站点配置（可选）

\$ git submodule add git@github.com:dillonzq/LoveIt.git themes/LoveIt

// 将主题示例文章和配置复制到 blog 目录
\$ cp -r themes/LoveIt/exampleSite/\* ./

### 基本工作流

- 添加主题
  以 LoveIt 为例。

````

\$ git submodule add git@github.com:dillonzq/LoveIt.git themes/LoveIt

```

通过网站 gitignore.io 生成 ignore 文件。

```

### Hugo

# Generated files by hugo

/public/
/resources/\_gen/
hugo_stats.json

# Executable may be added to repository

hugo.exe
hugo.darwin
hugo.linux

```

- 复制常用配置

```

cp -r themes/LoveIt/exampleSite/\* ./

```

> 注释 themeDir 行

```

# themesDir = "../..

```

- 预览效果

```

\$ hugo server -D

```

## 内容

### 图床

- 域名
- CDN

## 总结

## 参考

## 环境
```
````

