---
title: 使用Hexo快速搭建静态博客.md
date: 2024-03-31 16:02:04
tags:
    - Hexo
    - 博客
---

## 使用 Hexo 快速搭建静态博客

我的这个博客就是使用Hexo在一小时不到的时间搭建的，而且几乎零成本（域名+SSL证书），日常中如果我们需要搭建一个博客，又不想敲代码，可以一试。

下面将介绍如何从零开始搭建一个类似于我这个博客的博客，本教程将教你使用 `Hexo + Next主题` 在 GitHub 上面搭建你的个人博客，并使用你自己的域名。

注：本教程只讨论将博客搭建在 GitHub 上，如果你想搭建在你自己的服务器上，请查看其它教程。

<!--more-->

### 介绍

[Hexo](https://hexo.io/zh-cn/)是一个快速、简洁且高效的博客框架。支持用 `Markdown` 来写博文， 同时有不错的插件拓展性，甚至可以一键部署到 GitHub 仓库，使用起来非常方便。

### 准备工作

* [Node.js](https://nodejs.org/en)
* [Git](https://git-scm.com/)

Node.js 和 Git 的安装方法，Windows 用户直接去相应官网下载安装程序安装即可，Linux 和 Mac 还有其他操作系统直接使用相应的软件包管理器进行安装即可。

#### 安装 Hexo

```bash
npm install hexo-cli -g
```

### 开始搭建

#### 初始化一个项目

打开终端，使用

```bash
hexo init 文件夹名
```

以初始化一个博客项目。

#### 本地测试

生成的项目默认是已经配置好了，使用命令

```bash
hexo s
```

可以开始本地测试，默认使用4000端口，访问 [`localhost:4000`](localhost:4000) 即可查看。

如果你的需求并不是很高，可以直接跳过本标题下的剩余内容，直接看 `部署`

#### 生成静态页面

使用命令

```bash
hexo g
```

会在项目跟文件下，新建一个 `public` 文件夹来存放生成的静态页面及所有的资源。

### 使用 Next 主题

默认的主题比较难看，我一直用的是 [Next主题](https://github.com/next-theme/hexo-theme-next)，你将该仓库直接拷贝到项目根目录下的 `themes` 文件夹下面，然后修改项目根目录下的 `_config.yml` 配置文件下的 `theme` 为 `next` 即可使用 Next 主题了。

同样是开箱即用，主题的配置文件为该主题根目录下的 `_config.yml`，修改该文件即可。

### 部署到 GitHub

首先你要新建一个 GitHub 仓库，仓库名是 你的ID.github.io

比如我的 ID 是 codewuren，那么我的这个仓库名就是 `codewuren.github.io`，这样这个仓库会被默认为是一个 GitHub Page 仓库。可以通过访问 `<ID>.github.io` 来访问你的博客，比如我的就是 `codewuren.github.io`。

然后修改项目根目录下的 `_config.yml` 配置文件，修改完基本信息后，修改 `deploy` 下的内容：

```bash
deploy:
  type: git
  repo: <仓库链接> #比如"git@github.com:codewuren/codewuren.github.io.git"
  branch: <分支名> #比如"master"
  message: <自定义提交信息> #默认是Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```

#### 安装 [hexo-depolyer-git](https://github.com/hexojs/hexo-deployer-git)

使用命令：

```bash
npm install hexo-deployer-git --save
```

#### 部署

使用命令：

```bash
hexo d
```

就可以把你的博客推送到 GitHub 上面去了，之后可以通过 `<ID>.github.io` 访问，如 [`codewuren.github.io`](codewuren.github.io)。

### 使用你自己的域名

#### 购买域名

首先你得购买一个域名，可以在[华为云](https://www.huaweicloud.com/)、[阿里云](https://www.aliyun.com/)、[腾讯云](https://cloud.tencent.com/)等网站购买一个域名，如果你预算不够充足，可以考虑 `.top` 这种便宜的域名。

#### 域名解析

光购买了域名还是不能直接使用的，在你购买的平台控制台里面找到域名解析的页面，然后修改解析配置。下面以我的域名解析设置为例：

| 域名 | 类型 | 线路类型 | 值 |
| ---- | ---- | ---- | ---- |
| codewuren.top | CNAME | 全网默认 | codewuren.github.io.
 |
| www.codewuren.top | CNAME | 全网默认 | codewuren.github.io.
 |

TTL时间可根据你的需求更改。

域名解析设置好之后依旧是不能直接使用，还得在你的博客项目里面设置了使用你的域名才行。

#### 使用域名

在你博客项目根目录下的 `source` 文件夹下面新建一个名为 `CNAME` 的文件，里面写上你购买的域名。

然后就可以直接通过你所购买的域名来访问你的博客了。

### 申请 SSL 证书

默认你的博客使用的是 `http`， 如果想使用 `https`，你需要申请一个 SSL 证书并绑定到你的域名上。

详细的申请方法就不说了，不同网站可能有所差异。

证书申请完了之后，要再次修改你的域名解析配置，按要求添加指定配置即可，如：

| 域名 | 类型 | 线路类型 | 值 |
| ---- | ---- | ---- | ---- |
| _dnsauth.codewuren.top | TXT | 全网默认 | <申请证书给你的密钥> |

配置完之后记得进行 DNS 验证，看你的配置是否正常。

之后在 GitHub 上面打开你之前新建的博客仓库，点 `Settings`，然后找到 `Code and automation` 里的 `Pages` 设置页面，勾选里面的 `Enforce HTTPS ` 即可。

如果发现无法勾选，可以先清空 `Custom domain` 里面的内容，再次添加你的域名，等待 `DNS 验证` 通过即可。