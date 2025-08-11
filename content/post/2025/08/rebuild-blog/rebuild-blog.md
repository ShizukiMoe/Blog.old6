+++
title = '使用 Hugo + Stack 重新搭建博客'
slug = "rebuild-blog"
date = '2025-08-12T00:00:00+08:00'
image = "post/2025/08/rebuild-blog/img/cover.webp"
license = ""
categories = ["IT"]
tags = ["Hugo"]
+++

> 本文为`Shizukiの魔法小屋`搭建记录，仅供参考，不宜完全照搬，请根据需求自行调整。

## 前言
在去年 3 月，我是搭建过一次博客的，用 Wordpress + Sakurairo 搭建（Sakurairo 真好看吧）。
不过最后毁于忘记续费，虽说也没写什么有用的东西，不过倒也积攒了经验。

也算是一个新的开始，而这次，我打算用 Hugo + Stack 来重新搭建我的博客！~

### 为什么选择 Hugo
从 Hexo 到 Hugo，再从 Wordpress 到 Halo，兜兜转转又回到了 Hugo。
与其说为什么选择 Hugo，不如说为什么不选择其他主流博客框架。

Wordpress 和 Halo 是动态博客，需要服务器或主机来部署，这意味着要花不必要的钱，并且还要花时间来管理，因此不符合我的需求。
而静态博客只需要生成页面后免费托管至 GitHub Pages 或 Cloudflare Pages，避免浪费时间与金钱。
Hexo 虽然是静态博客，但是太重又太慢，配环境还麻烦，也不符合我的需求。

综上所述，我选择了 Hugo。

## 框架
### 安装 Hugo
前往 [GitHub](https://github.com/gohugoio/hugo/releases) 选择对应 Hugo 版本下载。
这里我下载的是 `hugo_extended_0.148.2_windows-amd64.zip`，解压后添加至环境变量。

然后验证安装

```shell
hugo version
```

得到类似以下结果

```text
hugo v0.148.2-40c3d8233d4b123eff74725e5766fc6272f0a84d+extended windows/amd64 BuildDate=2025-07-27T12:43:24Z VendorInfo=gohugoio
```

### 初始化博客
安装好 Hugo 后，就可以开始初始化博客了

```shell
hugo new site Blog
```

初始化后当前目录下会生成一个 `Blog` 目录，目录结构如下

```text
Blog/
    archetypes/
        default.md
    assets/
    content/
    data/
    i18n/
    layouts/
    static/
    themes/
    hugo.toml
```

## 主题
这里我选用的是 [Stack](https://github.com/CaiJimmy/hugo-theme-stack) 主题，其他主题可见 [Hugo Themes](https://themes.gohugo.io/)。

### 安装主题
安装主题有多种方式，这里使用 Git 子模块的方式。
这要求你的博客需要使用 Git，Git 的安装在这不多赘述。

首先在博客根目录下初始化 Git 仓库

```shell
git init
```

然后在 GitHub 上 fork Stack 主题仓库，以便于修改和使用。

将 fork 的主题仓库添加为 Git 子模块

```shell
git submodule add https://github.com/ShizukiMoe/Stack themes/Stack
```

### 配置主题
首先从 [Hugo Theme Stack Starter]() 下载 `config/_default` 下的所有文件，并复制到 `config/_default` 目录下。

然后预览主题，以便修改

```shell
hugo server -D
```

如果运行报错，尝试删除 `module.toml`。

然后可以继续根据个人喜好修改 `config/_default` 下的配置文件。
可以按 [文档](https://stack.jimmycai.com/) 或参考我的 [配置文件](https://github.com/ShizukiMoe/Blog/tree/main/config/_default)。

#### 基本设置
`hugo.toml`

```toml
baseURL = 'https://shizuki.moe/'
languageCode = 'zh-cn'
title = 'Shizukiの魔法小屋'
theme = 'Stack'
```

`config.toml`

```toml
baseurl = "https://shizuki.moe"
languageCode = "zh-cn"
title = "Shizukiの魔法小屋"

defaultContentLanguage = "zh-cn"
hasCJKLanguage = true
```

#### 页脚
`params.toml`

```toml
[footer]
since = 2025
customText = ""
```

#### 时间格式
`params.toml`

```toml
[dateFormat]
published = "2006.01.02"
lastUpdated = "2006.01.02 15:04"
```

#### 侧边栏
`params.toml`

```toml
[sidebar]
emoji = "❄️"
subtitle = "可愛いは正義です"

[sidebar.avatar]
enabled = true
local = true
src = "img/avatar.jpg"
```

#### 文章
`params.toml`

```toml
[article]
headingAnchor = false
math = false
readingTime = true

[article.license]
enabled = true
default = "CC BY-NC-SA 4.0"
```

#### 固定链接
`permalinks.toml`

```toml
post = "/post/:year/:month/:slug/"
page = "/:slug/"
```

#### 社交链接
`menu.toml`

```toml
[[social]]
name = "Bangumi"
url = "https://bgm.tv/user/shizukinatsuki"

[social.params]
icon = "brand-bangumi"

[[social]]
name = "BiliBili"
url = "https://space.bilibili.com/3546607333149385"

[social.params]
icon = "brand-bilibili"

[[social]]
name = "GitHub"
url = "https://github.com/ShizukiNatsuki"

[social.params]
icon = "brand-github"

[[social]]
name = "Steam"
url = "https://steamcommunity.com/id/ShizukiNatsuki/"

[social.params]
icon = "brand-steam"

[[social]]
name = "X"
url = "https://x.com/ShizukiNatsuki"

[social.params]
icon = "brand-x"
```

#### 菜单
菜单也有多种方式实现，这里选择 FrontMatter 的方式，好处是能跟随页面标粗。

只需要在需要作为菜单的页面前加入

```yaml
---
menu:
  main:
    name: 归档
    weight: -80
    params:
      icon: archives
---
```

#### 归档和搜索
启用归档和搜索，只需要添加页面与布局。

归档

```yaml
---
title: Archives
layout: archives
---
```

搜索

```yaml
---
title: Search
layout: search
---
```

与上一步结合。

归档

```yaml
---
menu:
  main:
    name: 归档
    weight: -80
    params:
      icon: archives

title: Archives
layout: archives
---
```

搜索

```yaml
---
menu:
  main:
    name: 搜索
    weight: -70
    params:
      icon: search

title: Search
layout: search
---
```

#### 自动修改时间
只需要在 `config.toml` 中加入以下内容即可实现根据 Git 提交记录自动添加最后修改时间

```toml
enableGitInfo = true

[frontmatter]
lastmod = ['lastmod', ':git', 'date']
```

### 更改主题
#### 移动主题著作信息
我不是很喜欢把很多东西一股脑塞到页脚下，而且在这个主题下，这样做显得很丑。
所以我把它移到了左下角侧边栏底部，切换深色模式按钮的上面。

从 `themes/Stack/layouts/partials/footer/footer.html` 中删除以下内容

```html
<section class="powerby">
    {{ with .Site.Params.footer.customText }}
    {{ . | safeHTML }} <br/>
    {{ end }}

    {{- $Generator := `<a href="https://gohugo.io/" target="_blank" rel="noopener">Hugo</a>` -}}
    {{- $Theme := printf `<b><a href="https://github.com/CaiJimmy/hugo-theme-stack" target="_blank" rel="noopener" data-version="%s">Stack</a></b>` $ThemeVersion -}}
    {{- $DesignedBy := `<a href="https://jimmycai.com" target="_blank" rel="noopener">Jimmy</a>` -}}

    {{ T "footer.builtWith" (dict "Generator" $Generator) | safeHTML }} <br />
    {{ T "footer.designedBy" (dict "Theme" $Theme "DesignedBy" $DesignedBy) | safeHTML }}
</section>
```

在 `themes/Stack/layouts/partials/sidebar/left.html` 添加以下内容

```html
<li class="menu-bottom-section">
    <section class="powerby">
        {{- $ThemeVersion := "3.30.0" -}}
        {{- $Theme := printf `<a href="https://github.com/CaiJimmy/hugo-theme-stack" target="_blank" rel="nofollow" data-version="%s">Stack</a>` $ThemeVersion -}}
        {{- $DesignedBy := `<a href="https://jimmycai.com" target="_blank" rel="nofollow">Jimmy</a>` -}}

        {{ T "footer.designedBy" (dict "Theme" $Theme "DesignedBy" $DesignedBy) | safeHTML }}

        <br>
    </section>

    <br>
</li>
```

在 `themes/Stack/assets/scss/partials/footer.scss` 添加以下内容

```scss
#main-menu {
  li {
    .powerby {
      color: var(--body-text-color);
      font-weight: normal;
      font-size: 1.2rem;

      a {
        color: var(--body-text-color);
      }
    }
  }
}
```

#### 更改页脚版权信息
我想让它显示我的昵称而并非是站点名称，直接写死就好。

更改 `themes/Stack/layouts/partials/footer/footer.html` 文件

```html
<section class="copyright">
    &copy; 
    {{ if and (.Site.Params.footer.since) (ne .Site.Params.footer.since (int (now.Format "2006"))) }}
    {{ .Site.Params.footer.since }} - 
    {{ end }}
    {{ now.Format "2006" }}
    Shizuki Natsuki
</section>
```

#### 移动最后修改时间
从 `themes/Stack/layouts/partials/article/components/footer.html` 删除以下内容

```html
{{- if ne .Lastmod .Date -}}
<section class="article-lastmod">
    {{ partial "helper/icon" "clock" }}
    <span>
        {{ T "article.lastUpdatedOn" }} {{ .Lastmod | time.Format ( or .Site.Params.dateFormat.lastUpdated "Jan 02, 2006 15:04 MST" ) }}
    </span>
</section>
{{- end -}}
```

更改 `themes/Stack/layouts/partials/article/components/details.html` 内容

```html
<footer class="article-time">
    {{- if ne .Lastmod .Date -}}
    <div>
        {{ partial "helper/icon" "clock" }}
        <time class="article-time--published">
            {{ T "article.lastUpdatedOn" }} {{ .Lastmod | time.Format ( or .Site.Params.dateFormat.lastUpdated "Jan 02, 2006 15:04 MST" ) }}
        </time>
    </div>
    {{- end -}}
</footer>
```

#### 添加字数统计
更改 `themes/Stack/layouts/partials/article/components/details.html` 内容

```html
<footer class="article-time">
    {{ if .Site.Params.article.readingTime }}
    <div>
        {{ partial "helper/icon" "brush" }}
        <time class="article-words">
            {{ T "article.wordCount" .WordCount }}
        </time>
    </div>
    {{ end }}
</footer>
```

添加 i18n 翻译 `themes/Stack/i18n/zh-cn.yaml`

```yaml
article:
    wordCount:
        other: "{{.Count}} 字"
```

#### 添加侧边栏文字
在 `themes/Stack/layouts/partials/sidebar/left.html` 添加以下内容

```html
<ol class="menu" id="main-menu">
    {{ $currentPage := . }}
    {{ range .Site.Menus.main }}
    {{ $active := or (eq $currentPage.Title .Name) (or ($currentPage.HasMenuCurrent "main" .) ($currentPage.IsMenuCurrent "main" .)) }}
    <li {{ if $active }} class='current' {{ end }}>
        <a href='{{ .URL }}' {{ if eq .Params.newTab true }}target="_blank"{{ end }}>
            {{ $icon := default .Pre .Params.Icon }}
            {{ if .Pre }}
                {{ warnf "Menu item [%s] is using [pre] field to set icon, please use [params.icon] instead.\nMore information: https://stack.jimmycai.com/config/menu" .URL }}
            {{ end }}
            {{ with $icon }}
                {{ partial "helper/icon" . }}
            {{ end }}
            <span>{{- .Name -}}</span>
        </a>
    </li>
    {{ end }}

    <li class="sidebar-text">
            <br>
    </li>
</ol>
```

在 `themes/Stack/assets/scss/partials/menu.scss` 添加以下内容

```scss
#main-menu {
  li {
    &.sidebar-text {
      &:before {
        margin: auto;
        content: "";
        display: block;
        height: 3px;
        width: 100px;
        background: var(--body-text-color);
      }
    }
  }
}
```

#### 其他杂碎
添加至 `themes/Stack/assets/scss/custom.scss`

```scss
// 全局基本设置
:root {
  --main-top-padding: 30px;
  --card-border-radius: 25px;
  --tag-border-radius: 8px;
}

// 代码块样式
.highlight {
  max-width: 102% !important;
  background-color: var(--pre-background-color);
  padding: var(--card-padding);
  position: relative;
  border-radius: 20px;
  margin-left: -7px !important;
  margin-right: -12px;
  box-shadow: var(--shadow-l1) !important;

  &:hover {
    .copyCodeButton {
      opacity: 1;
    }
  }

  [dir="rtl"] & {
    direction: ltr;
  }

  pre {
    padding: 0;
    margin: 0;
    width: auto;
  }
}

// 图片样式
.article-page .main-article .article-content {
  img {
    max-width: 96% !important;
    height: auto !important;
    border-radius: 8px;
  }
}

.article-list--compact article .article-image img {
  width: var(--image-size);
  height: var(--image-size);
  object-fit: cover;
  border-radius: 17%;
}

.section-image img {
  width: var(--image-size);
  height: var(--image-size);
  object-fit: cover;
  border-radius: 17%;
}
```

## 评论系统
### 安装 Giscus
在 GitHub 上新建一个公开仓库，专门用于博客评论。
安装 [Giscus](https://github.com/apps/giscus)。
开启仓库 Discussions 功能。

这时去 [Giscus](https://giscus.app/) 检测是否安装完毕。

### 配置 Giscus
在检测安装完毕后，就可以在下方更改设置，比如 Discussion 分类，映射关系等。
随后在下方就会出现

```text
<script src="https://giscus.app/client.js"
        data-repo="[在此输入仓库]"
        data-repo-id="[在此输入仓库 ID]"
        data-category="[在此输入分类名]"
        data-category-id="[在此输入分类 ID]"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="zh-CN"
        crossorigin="anonymous"
        async>
</script>
```

最后更改 `params.toml` 文件，并将前五项字段的值复制到 `params.toml` 中，以启用 Giscus

```toml
[comments]
enabled = true
provider = "giscus"

[comments.giscus]
repo = "ShizukiMoe/Comment"
repoID = "R_kgDOPbH3Ww"
category = "Comment"
categoryID = "DIC_kwDOPbH3W84Ct-mn"
mapping = "pathname"
lightTheme = ""
darkTheme = ""
reactionsEnabled = 1
emitMetadata = 0
```

## 自动化部署
这里用 GitHub Actions 来实现自动化部署。

首先创建一个特殊的 GitHub 仓库，以 `<username>.github.io` 命名，`<username>` 改为你的 GitHub 用户名，比如我就是 `ShizukiNatsuki.github.io`。

然后在博客仓库创建 workflow 文件 `.github/workflow/deploy.yml`

```yaml
name: Deploy

on:
  push:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Theme
        run: git clone https://github.com/ShizukiMoe/Stack themes/Stack

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
          extended: true

      - name: Build Web
        run: hugo build

      - name: Deploy Web
        uses: peaceiris/actions-gh-pages@v3
        with:
          PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
          EXTERNAL_REPOSITORY: ShizukiMoe/ShizukiMoe.github.io
          PUBLISH_BRANCH: main
          PUBLISH_DIR: ./public
          commit_message: ${{ github.event.head_commit.message }}
```

然后提交推送至博客仓库，即可实现自动化部署。

## 鸣谢
> [Hugo Stack 魔改美化 | Naive Koala](https://www.xalaok.top/post/stack-modify/)  
> [使用 Hugo 对博客的重建与 Stack 主题优化记录 | Exnadio's Blog](https://oxidane-uni.github.io/p/%E4%BD%BF%E7%94%A8-hugo-%E5%AF%B9%E5%8D%9A%E5%AE%A2%E7%9A%84%E9%87%8D%E5%BB%BA%E4%B8%8E-stack-%E4%B8%BB%E9%A2%98%E4%BC%98%E5%8C%96%E8%AE%B0%E5%BD%95/)
