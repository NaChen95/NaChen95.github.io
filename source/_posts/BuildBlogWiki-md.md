---
title: 如何搭建个人博客网站
tags: 
- 网站
- 博客
categories: 
- 网站
- 博客
---
## 1. 博客网站的组成

搭建个人博客网站是一件很有意义的事情。它是知识整理、提炼与输出的过程，也是一个能持久性保存的、记录回顾的地方，某些内容和想法还可能帮助和启发到别人。
总的来说，博客网站包括下面几个部分：

### 1.1. 博客生成框架

网站代码包括 HTML, CSS 和 JavaScript（前端三件套）。

- HTML 是结构层，存储网站内容，包括文本，图片等。
- CSS 是样式层，控制页面布局，字体大小颜色等，从而美化网站。
- JavaScript 是交互层，使得用户能和网站进行交互。例如用户在点击一个按钮时，弹出一个窗口。

但不是所有人都熟悉前端三件套，所以就引出了博客生成框架。它是一个应用程序，输入是一些配置文件，输出就是相应的 HTML，CSS 和 JavaScript。从而我们可以不怎么了解前端知识，仅通过简单配置，就能生成复杂的博客网站的前端界面。我用的是 [Hexo](https://hexo.io/zh-cn/)，因为它是比较成熟的框架，出了问题网上基本都能搜到答案。

### 1.2. 开发环境

开发环境是指在什么地方去写文章和调试前端界面。当然可以使用个人 PC 机，但我更推荐租一台云服务器来开发。因为前者需要被携带。而后者可以随时通过 [SSH](https://stackoverflow.com/questions/51650544/what-is-the-difference-between-ssh-and-http) 通过 [VSCode 免密码连接](https://stackoverflow.com/questions/66113731/how-to-save-ssh-password-to-vscode)上去，而且还能选择 Linux 镜像供个人学习使用。

### 1.3. 版本控制与代码托管

主流的版本控制系统是 Git，知名的代码托管平台有 GitHub 和 Gitee。我用的是 GitHub。

### 1.4. 站点部署

站点部署是指将生成好的网站目录部署到互联网上，让网络上的其他主机也能正常访问。最简单的是 [Github Pages](https://docs.github.com/zh/pages/getting-started-with-github-pages/about-github-pages) ，它能直接从 GitHub 上的仓库获取 HTML、CSS 和 JavaScript 文件来发布网站。但是 GitHub 国内访问不稳定，所以其他选择有 Gitee，Netlify 或通过 Nginx 将网站部署到自己租的云服务器上。我用的是 GitHub Pages，它的域名固定是 `usename.github.io`。

### 1.5. 其他

一个完整成熟的网站还包括很多其他的东西，比如个性化域名与备案，数据库，CDN 分发加速等，本文没有涉及这些。

## 2. 搭建博客网站的步骤

### 2.1. 软件安装

我的本地开发环境是云服务器，其操作系统是 Ubuntu 18.04。不同的操作系统的安装命令可能会有所不同。

#### 2.1.1. 安装 Git

```BASH
# 首先更新系统以及安装的软件包
sudo apt update -y
sudo apt upgrade -y

# 安装 Git
sudo apt install git

# 查看成功安装的 Git 版本
git --version

# 如果是第一次使用 Git，需要配置用户名和密码
# Git 使用用户名将提交与身份关联。 Git 用户名与 GitHub 用户名不同
git config --global user.name "xxx"
git config --global user.email "xxx"
```

#### 2.1.2. 安装 Node.js 和 npm

Hexo 及其安装依赖 Node.js 和 npm。Node.js 是 JavaScript 运行环境。本来 JavaScript 只是在前端使用，但 Node.js 的出现使得 JavaScript 能脱离浏览器，像其他编程语言直接在计算机中使用，成为了和 PHP，Python 等服务端平起平坐的脚本语言。npm 是 Node.js 的包管理器。

```BASH
# 安装 Node.js
# 有可能需要按照 https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04 的方式安装最新版本的 Node.js
sudo apt install nodejs

# 查看成功安装的 Node.js 版本
node -v

# 安装 npm
sudo apt install npm
```

#### 2.1.3. 安装 Hexo

参考 [Hexo Documentation](https://hexo.io/docs/) 与 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。

```BASH
# 安装 Hexo
sudo npm install -g hexo-cli

# 安装 Hexo 的 Git 部署插件
sudo npm install hexo-deployer-git --save
```

### 2.2. 本地调试

安装完软件后，就能在本地生成博客网站内容并进行预览和调试。参考[Hexo 官方教程](https://hexo.io/zh-cn/docs/commands.html)。

#### 2.2.1. 初始化网站目录

```bash
hexo init [folder]
```

#### 2.2.2. 网站目录结构

执行完毕后，folder 里的主要文件是：

```TEXT
.
├── .deploy_git
├── _config.yml
├── node_modules
├── package.json
├── public
├── scaffolds
├── source
│      └─ _posts
└─themes
    ├─ landscape
    └─ fluid
```

##### 2.2.2.1. deploy_git

在部署到 GitHub 后自动创建。
此目录内容等于 GitHub 远端仓库内容等于最近一次上传到 GitHub 的 public 目录的内容。

##### 2.2.2.2. _config.yml

网站的配置信息文件，可以在此配置大部分的参数。

##### 2.2.2.3. node_modules

存放安装的 Hexo 扩展包。自己安装的扩展包也会在此新建目录。

##### 2.2.2.4. package.json

用来查看 Hexo 的版本以及相关依赖包的版本。

##### 2.2.2.5. public

执行 `hexo g` 命令，Hexo 程序会解析 source 和当前使用的 theme，生成的静态网页内容目录就是 public。执行 `hexo d` 则将该目录内容复制到 `.deploy_git` 目录。

##### 2.2.2.6. scaffolds

[模版](https://hexo.io/zh-cn/docs/templates.html)文件夹。当新建文章时，Hexo 会根据 scaffold 中的模板来创建。用户基本不需要关心它。

##### 2.2.2.7. source

存放博客资源。可以分成三类：

###### 2.2.2.7.1. _posts

存放博客文章。其中的 Markdown 文件、HTML 文件、org 文件等会被解析并放到 public 文件夹，发布到站点。

###### 2.2.2.7.2. 其他命名为下划线开头的文件

将会被忽略。因此可以在 source 目录下创建 _drafts 目录用于存放未完成的草稿，其中内容不会发布到网站。

###### 2.2.2.7.3. 其他命名为非下划线开头的文件

会被拷贝到 public 目录并上传到站点。因此可以创建 img 目录来存放在博客引用到的图片，添加新的页面例如 [about](https://stackoverflow.com/questions/29167023/how-to-add-route-for-hexo)。

##### 2.2.2.8. themes

存放[主题](https://hexo.io/zh-cn/docs/themes.html)。Hexo 自带的主题 landscape 不好看，大家基本上都会安装新主题。我用的是 [Fluid](https://github.com/fluid-dev/hexo-theme-fluid) 主题，因为它的[用户手册](https://fluid-dev.github.io/hexo-fluid-docs/guide/)完善。安装方式为通过下载[最新 release 版本](https://github.com/fluid-dev/hexo-theme-fluid/releases)解压到 themes 目录，并将解压出的文件夹重命名为 fluid。

#### 2.2.3. Hexo 常用命令

```BASH
# 新建一篇文章，layout 存放在 scaffolds 目录中
# layout 是指 scaffolds 文件夹中的三个文件：draft.md / page.md / post.md。默认 layout 是 post
hexo n [layout] <title>

# 生成静态文件，存放在 public 目录
hexo g

# 启动本地服务器，用于调试网页
hexo s

# 部署网站（需要先配置好 Github 和 _config.yml）
hexo d
```

关于 layout 的详细解释可参考[《如何生成一篇新的post》](https://oakland.github.io/2016/05/02/hexo-如何生成一篇新的post/)。

### 2.3. 网站部署

#### 2.3.1. 配置 [Github Pages](https://pages.github.com/)

首先注册 [GitHub](https://github.com/)，然后创建名为 `username.github.io` 的公共仓库。`username` 是自己的 GitHub 用户名。

#### 2.3.2. 配置 `_config.yml`

以我的博客网站为例，在 `_config.yml` 末尾增加：

```YML
deploy:
  type: git
  repository: https://github.com/NaChen95/NaChen95.github.io.git
  branch: main
```

注意 GitHub 现在的默认 `branch` 是 `main`，而不是之前的 `master`。这可能是由于美国的[政治、种族原因](https://www.quora.com/Why-did-GitHub-replace-master-with-the-main-branch)。

### 2.4. 配置 `_config.fluid.yml`

[Fluid 用户手册](https://fluid-dev.github.io/hexo-fluid-docs/guide/#覆盖配置)建议在博客根目录下创建 `_config.fluid.yml`，并将主题的 `_config.yml` 全部或部分配置复制过去。此外，还需要 `_config.fluid.yml` 的 `url` 将改为：

```YML
url: https://nachen95.github.io/project
```

#### 2.4.1. GitHub Authentication

[Authentication](https://auth0.com/docs/get-started/identity-fundamentals/authentication-and-authorization) 是验证用户身份的过程，通过它才有权限向自己的 GitHub 仓库推送本地修改。根据 [Authenticating with GitHub](https://mgimond.github.io/Colby-summer-git-workshop-2021/authenticating-with-github.html)，现在已经没法使用用户名和密码的方式进行认证，而需要使用 [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) 或者 [SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)。在第一次执行 `hexo d` 时，会提示要求填入用户名和密码，注意这个密码是 PAT。

### 2.5. 遇到的坑

#### 2.5.1. 本地调试时网站页面无法打开

执行 `hexo s` 后，发现访问 `http://localhost:4000/` （ localhost 需换成云服务器 IP）失败。这可能有两个原因：

##### 2.5.1.1. 4000 端口被占用

可以在 `_config.yml` 的末尾增加

```YML
server:
  port: 5000
  compress: true
  header: true
```

将默认端口改成 5000。

#### 2.5.2. 端口防火墙未开放

我用的是云服务器是阿里云轻量应用服务器，参考它的[防火墙设置教程](https://developer.aliyun.com/article/846804)开放端口。

#### 2.5.3. 在 Hexo 部署到 GitHub 时卡住

可能是阿里云服务器本身对 GitHub 的访问不良。参考 [《告别无法访问的github》](https://developer.aliyun.com/article/813040)中的修改本地 host 解决。