# 基于 [mzlogin](https://github.com/mzlogin) 博客模版的补充 - bran

如需帮助，[点击此处](https://github.com/bran-nie/bran-nie.github.io/issues/new)，提 Issue 联系我。

## 概览

<!-- vim-markdown-toc GFM -->

-   [目录结构](#目录结构)
-   [启动本地服务预览博客](#启动本地服务预览博客)
-   [配置](#配置)
    -   [\_config.yml](#_config.yml)
    -   [CDN\_加速配置](#CDN_加速配置)
    -   [图床配置](#图床配置)
    -   [Github Action](#Action)

<!-- vim-markdown-toc -->

## 目录结构

```
├── Gemfile     # ruby 配置文件
├── Gemfile.lock
├── LICENSE
├── README.md   # 项目说明文档
├── _config.yml # 项目配置文件
├── _data       # 关于我页面和链接页面的模块数据配置
│   ├── links.yml
│   ├── skills.yml
│   └── social.yml
├── _drafts     # 草稿箱
│   └── template.md
├── _includes   # Jekyll 的组件，可在里面定制化修改页面
│   ├── comments.html               # 评论模块
│   ├── content-footer-ad.html      # 文章底部ad模块
│   ├── content-header-ad.html      # 文章顶部ad模块
│   ├── copyright.html              # 文章底部的文档信息
│   ├── footer-ad.html              # 底部的ad模块
│   ├── footer.html                 # 博客底部
│   ├── header.html                 # 博客顶部
│   ├── sidebar-ad.html             # 右侧栏的ad
│   ├── sidebar-categories-cloud.html   # 标签云
│   ├── sidebar-categories-nav.html     # 分类页面，右侧栏的文章统计
│   ├── sidebar-popular-repo.html       # 右侧栏的 repo 列表
│   ├── sidebar-post-nav.html           # 文章的目录结构
│   ├── sidebar-qrcode.html             # 右侧栏二维码
│   ├── sidebar-search.html             # 右侧栏的搜索模块
│   ├── sns-share.html                  # 分享模块
│   └── visit-stat.html                 # 访问量展示模块
├── _layouts    # 是包裹在文章外部的模板，标签 {{ content }} 可以将content插入页面
│   ├── categories.html
│   ├── compress.html
│   ├── default.html
│   ├── mindmap.html
│   ├── page.html
│   ├── post.html
│   └── wiki.html
├── _posts      # 自己写的博客，存放的文件夹。格式必须要 Year-Month-Day-title.md
│   ├── 2021-03-12-template.md
│   └── template.md
├── _wiki
│   └── markdown.md
├── _site  # 这个是 Jekyll 默认生成的文件夹。建议将这个放进 .gitignore 中
├── assets      # 博客所需要的静态资源文件夹
│   ├── css
│   ├── images
│   ├── js
│   ├── search_data.json
│   └── vendor
├── pages   # 博客的其他页面，如404、about等
│   ├── 404.md
│   ├── about.md
│   ├── archives.md
│   ├── categories.md
│   ├── links.md
│   ├── mindmap-viewer.md
│   ├── open-source.md
│   └── wiki.md
├── images
├── favicon.ico
├── index.html
└── ads.txt
```

## 启动本地服务预览博客

> 这部分较为繁琐，有确切需要在本地预览博客且喜爱折腾的人，请继续阅读。

### 基本要求

-   安装 Ruby
-   安装 Bundler
-   安装 Jekyll

-   安装上述环境后，在项目中执行 `bundler install`
-   使用 `yarn start` or `npm start` 开启服务
    -   实际上是使用 `bundler exec jekyll serve` 开启服务，这里我是简化了命令

详细有关本地构建网站的文档，请参考该文档 [使用 Jekyll 在本地测试 GitHub Pages 站点
](https://docs.github.com/cn/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll)

## 配置

### \_config.yml

> 依照 \_config.yml 的顺序说明

-   填写个人信息
-   是否开启 CDN 项，（PS：第一次请选择关闭，因为你的 GitHub 上面还没有博客的静态资源
-   博客上面展示的作者信息，包括仓库的地址。
-   `navs`，是博客顶部的导航链接列表
-   `comments`，评论模板配置。具体参照所选的方案的官方文档。
-   我新增了两个配置项，分别是博客所在的分支，和图床所在的分支名字，
    -   repository_branch: "master"
    -   repository_images_branch: "images"

### CDN\_加速配置

CDN 加速，即是寻找一个 CDN 服务商，将自己的的静态资源托管到他们的 CDN 服务上。

<https://www.jsdelivr.com/>，这是一个支持 GitHub 的 CDN 服务商，在其官网上是作为重要特性来宣传的。

GitHub 地址是 `https://github.com/mzlogin/mzlogin.github.io`， 那它里面的资源可以直接以 `https://cdn.jsdelivr.net/gh/mzlogin/mzlogin.github.io/ + 仓库里的文件路径` 来访问。

比如仓库里有一个 js 文件 `assets/js/main.js`，那么它可以用 CDN 链接 `https://cdn.jsdelivr.net/gh/mzlogin/mzlogin.github.io/assets/js/main.js` 来访问。

-   摘录 [使用 jsDelivr 免费加速 GitHub Pages 博客的静态资源](https://mazhuang.org/2020/05/01/cdn-for-github-pages/#jsdelivr-%E6%94%AF%E6%8C%81%E7%9A%84-github-%E8%B5%84%E6%BA%90%E7%9A%84%E6%96%B9%E5%BC%8F)

### 图床配置

> 该部分为可选项，使用图床是为了更方便的支持图片引用

首先，GitHub 可以作为一个优秀的图床来使用，但美中不足的是，在国内访问图床的速度感人。但结合上面的 CDN 加速，就可以补全这个不足，从而让 GitHub 成为完美的图床。

在写博客的过程中，可能我们采取的素材是截图，那么流程就是截图以后，还要保存到本地，那流程还是比较麻烦的，有没有什么好的方式呢？这里介绍一款图床工具，
[PicGo](https://molunerfinn.com/PicGo/)

![](https://raw.githubusercontent.com/bran-nie/bran-nie.github.io/images/images/blog/picgo.png)

具体的使用，可以查看[官方文档](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#github%E5%9B%BE%E5%BA%8A)

我的博客图床采取的做法是，新建一个空白 Git 分支- images，在 PicGo 的图床配置中，将图床的分支设置为 images

![](https://raw.githubusercontent.com/bran-nie/bran-nie.github.io/images/images/blog/PicGo_config.png)

### Action

这个配置是为了加速站内搜索引用的。
具体原因和采取方案，可以看作者的博客

[站内搜索引用的 JSON 资源加速](https://mazhuang.org/2020/10/07/cdn-for-github-pages-2/#0x02-%E7%AB%99%E5%86%85%E6%90%9C%E7%B4%A2%E5%BC%95%E7%94%A8%E7%9A%84-json-%E8%B5%84%E6%BA%90%E5%8A%A0%E9%80%9F)

---

# 码志

我的个人博客：<https://mazhuang.org>，欢迎 Star 和 Fork。

## 概览

<!-- vim-markdown-toc GFM -->

-   [效果预览](#效果预览)
-   [Fork 指南](#fork-指南)
-   [使用文档](#使用文档)
-   [经验与思考](#经验与思考)
-   [联系我](#联系我)
-   [致谢](#致谢)

<!-- vim-markdown-toc -->

## 效果预览

**[在线预览 &rarr;](https://mazhuang.org)**

![screenshot home](https://mazhuang.org/assets/images/screenshots/home.png)

## Fork 指南

Fork 本项目之后，还需要做一些事情才能让你的页面「正确」跑起来。

1. 正确设置项目名称与分支。

    按照 GitHub Pages 的规定，名称为 `username.github.io` 的项目的 master 分支，或者其它名称的项目的 gh-pages 分支可以自动生成 GitHub Pages 页面。

2. 修改域名。

    如果你需要绑定自己的域名，那么修改 CNAME 文件的内容；如果不需要绑定自己的域名，那么删掉 CNAME 文件。

3. 修改配置。

    网站的配置基本都集中在 \_config.yml 文件中，将其中与个人信息相关的部分替换成你自己的，比如网站的 url、title、subtitle 和第三方评论模块的配置等。

    **评论模块：** 目前支持 disqus、gitment 和 gitalk，选用其中一种就可以了，推荐使用 gitalk。它们各自的配置指南链接在 \_config.yml 文件的 Comments 一节里都贴出来了。

    **注意：** 如果使用 disqus，因为 disqus 处理用户名与域名白名单的策略存在缺陷，请一定将 disqus.username 修改成你自己的，否则请将该字段留空。我对该缺陷的记录见 [Issues#2][3]。

4. 删除我的文章与图片。

    如下文件夹中除了 template.md 文件外，都可以全部删除，然后添加你自己的内容。

    - \_posts 文件夹中是我已发布的博客文章。
    - \_drafts 文件夹中是我尚未发布的博客文章。
    - \_wiki 文件夹中是我已发布的 wiki 页面。
    - images 文件夹中是我的文章和页面里使用的图片。

5. 修改「关于」页面。

    pages/about.md 文件内容对应网站的「关于」页面，里面的内容多为个人相关，将它们替换成你自己的信息，包括 \_data 目录下的 skills.yml 和 social.yml 文件里的数据。

## 使用文档

-   [本博客模板常见问题 Q & A](https://mazhuang.org/2020/05/03/blog-template-qna/)。

-   在本地预览博客效果可以参考 [Setting up your Pages site locally with Jekyll][2]。

## 经验与思考

-   排版建议遵照一定的规范，推荐 [中文文案排版指北（简体中文版）][1]。

-   简约，尽量每个页面都不展示多余的内容。

-   有时一图抵千言，有时可能只会拖慢网页加载速度。

-   言之有物，不做无痛之呻吟。

-   如果写技术文章，那先将技术原理完全理清了再开始写，一边摸索技术一边组织文章效率较低。

-   杜绝难断句、难理解的长句子，如果不能将其拆分成几个简洁的短句，说明脑中的理解并不清晰。

-   可以学习一下那些高质量的博主，他们的行文，内容组织方式，有什么值得借鉴的地方。

## 联系我

如果对本博客模板或者内容有任何建议，可以通过 [Issues](https://github.com/mzlogin/mzlogin.github.io/issues) 或者微信公众号「闷骚的程序员」与我取得联系。

<img width="192px" height="192px" src="https://mazhuang.org/assets/images/qrcode.jpg"/>

## 致谢

本博客外观基于 [DONGChuan](https://dongchuan.github.io) 修改，感谢！

Thanks for JetBrains' support.

<a href="https://www.jetbrains.com/?from=mzlogin.github.io"><img src="./assets/images/jetbrains.svg"/></a>

[1]: https://github.com/mzlogin/chinese-copywriting-guidelines
[2]: https://help.github.com/articles/setting-up-your-pages-site-locally-with-jekyll/
[3]: https://github.com/mzlogin/mzlogin.github.io/issues/2
