---
title: jekyll-TeXt-theme 再探
permalink: /5cae91be9b46c0ae5e5c8973cd5cdea1
mermaid: true
mathjax: true
mathjax_autoNumber: true
tags: 
  - jekyll
---

依据以下资料：

- [jekyll-TeXt-theme中文官方教程](https://kitian616.github.io/jekyll-TeXt-theme/docs/zh/quick-start)
- [Yulei's blog-用 Github pages 和 Jekyll 搭建博客](https://yuleii.github.io/2020/06/09/build-blog-with-github-pages-and-jekyll.html)
- [error's blog-使用 GitHub Page + Jekyll + TeXt theme 制作博客](https://zhuanlan.zhihu.com/p/385384830)

本文对 jekyll-TeXt-theme 再做进一步使用，并将其形成文档于此。

<!--more-->

## Jekyll 目录结构

| 文件或文件夹  | 含义                          |
| :------------ | :---------------------------- |
| *_posts/*     | 博客内容                      |
| *_pages/*     | 其他需要生成的网页，如About页 |
| *_layouts/*   | 网页排版模板                  |
| *_includes/*  | 模板包含的HTML片段等          |
| *_data/*      | 动态数据                      |
| *_sites/*     | 最终生成的静态网页            |
| *_config.yml* | 网站全局配置信息              |
| *index.html*  | 网站入口                      |

## 网站配置

我们可以在根目录的 *_config.yml* 文件中对网站进行全局配置，常用的变量有：

| 变量                 | 含义               | 示例                                              |
| :------------------- | :----------------- | :------------------------------------------------ |
| `text_skin`          | 网站皮肤           | `text_skin: default`                              |
| `highlight_theme`    | 代码高亮主题       | `highlight_theme: default`                        |
| `title`              | 网站的标题         | `title: Meiting-Wang`                             |
| `description`        | 网站描述           | `description: Truth sets us free`                 |
| `lang`               | 网站的语言         | `lang: zh`                                        |
| `timezone`           | 网站时区           | `timezone: Asia/Shanghai`                         |
| `author`             | 网站作者           | `--`                                              |
| `repository`         | GitHub 源码仓库    | `repository: Meiting-Wang/Meiting-Wang.github.io` |
| `repository_tree`    | GitHub 仓库分支    | `repository_tree: main`                           |
| `toc: selectors`     | 文章目录           | `toc: selectors: h1,h2,h3`                        |
| `mathjax`            | 数学公式           | `mathjax: true`                                   |
| `mathjax_autoNumber` | 数学公式的自动编号 | `mathjax_autoNumber: true`                        |
| `mermaid`            | 文档图形           | `mermaid: true`                                   |
| `chart`              | 统计图表           | `chart: true`                                     |
| `paginate`           | 每页显示的文章数   | `paginate: 8`                                     |
| `comments`           | 评论               | `--`                                              |
| `pageview`           | 文章点击量         | `--`                                              |

## YAML 头信息

YAML 头信息必须放在文章开头部分，以实现对文章的局部设定，例如：

```markdown
---
title: Document - Writing Posts
mathjax: true
---
```

在这里可以设置在_config.yml文件中预定义的变量或针对文章特有的变量，常用的变量有：

| 变量                  | 含义                   | 示例                              |
| --------------------- | ---------------------- | --------------------------------- |
| `show_author_profile` | 在文章末尾展示作者信息 | `show_author_profile: true`       |
| `permalink`           | 永久链接               | `permalink: /docs/en/quick-start` |
| `author`              | 本文作者               | `author: Meiting Wang`            |
| `show_title`          | 展示 title             | `show_title: true`                |
| `show_edit_on_github` | 展示在 github 中修改   | `show_edit_on_github: true`       |
| `show_date`           | 展示该文章的发布日期   | `show_date: true`                 |
| `show_tags`           | 展示该文章的标签       | `show_tags: true`                 |
| `full_width`          | 文章内容占据全部宽度   | `full_width: false`               |
| `footer`              | 显示底部栏             | `footer: false`                   |
| `lightbox`            | 文章的大图点击可预览   | `lightbox: true`                  |
| `aside: toc`          | 文章右侧展示大纲       | `aside: toc: true`                |
| `show_author_profile` | 文章末尾显示作者卡片   | `show_author_profile: false`      |
| `show_subscribe`      | 文章末尾显示订阅消息   | `show_subscribe: true`            |
| `license`             | 文章的许可协议         | `license: CC-BY-NC-4.0`           |
| `layout`              | 文章的排版模板         | `layout: article`                 |

> 我个人博客的订阅地址为：[https://meiting-wang.github.io/feed.xml](https://meiting-wang.github.io/feed.xml)。

## 导航栏配置

在 TeXt 中有两种导航栏：**头部导航栏**（Header Navigation）和**侧边栏导航栏**（Sidebar Navigation），它们均在 *_data/navigation.yml* 中配置。

### 头部导航栏

头部导航栏在 *_data/navigation.yml* 文件的 `header` 项定义，它是一个包含标题和 URL 项的数组。我自己设置的头部导航栏如下：

```yaml
header:
  - title:      LaTeX
    url:        /latex/begin
  - title:      Stata
    url:        /stata/begin
  - titles:
      # @start locale config
      en      : &EN       Archive
      en-GB   : *EN
      en-US   : *EN
      en-CA   : *EN
      en-AU   : *EN
      zh-Hans : &ZH_HANS  归档
      zh      : *ZH_HANS
      zh-CN   : *ZH_HANS
      zh-SG   : *ZH_HANS
      zh-Hant : &ZH_HANT  歸檔
      zh-TW   : *ZH_HANT
      zh-HK   : *ZH_HANT
      ko      : &KO       아카이브
      ko-KR   : *KO
      fr      : &FR       Archives
      fr-BE   : *FR
      fr-CA   : *FR
      fr-CH   : *FR
      fr-FR   : *FR
      fr-LU   : *FR
      tr      : &TR       Arşivdekiler
      # @end locale config
    url: /archive.html
  - titles:
      # @start locale config
      en      : &EN       About
      en-GB   : *EN
      en-US   : *EN
      en-CA   : *EN
      en-AU   : *EN
      zh-Hans : &ZH_HANS  关于
      zh      : *ZH_HANS
      zh-CN   : *ZH_HANS
      zh-SG   : *ZH_HANS
      zh-Hant : &ZH_HANT  關於
      zh-TW   : *ZH_HANT
      zh-HK   : *ZH_HANT
      ko      : &KO       소개
      ko-KR   : *KO
      fr      : &FR       À propos
      fr-BE   : *FR
      fr-CA   : *FR
      fr-CH   : *FR
      fr-FR   : *FR
      fr-LU   : *FR
      tr      : &TR       Hakkında
      # @end locale config
    url: /about.html
  - title:      GitHub
    url:        https://github.com/Meiting-Wang/Meiting-Wang.github.io
```

### 侧边导航栏

要想在某篇文章或页面中使用侧边栏导航栏，首先你需要在 *_data/navigation.yml* 中定义一个导航栏，如我想设置导航栏 `latexs-nav` 和 `statas-nav`：

```yaml
latexs-nav:
  - title:      LaTeX 开始
    children:
      - title:  LaTeX 开始 1
        url:    /latex/begin
      - title:  LaTeX 开始 2
        url:    /latex/begin2
      - title:  LaTeX 开始 3
        url:    /latex/begin3
  - title:      LaTeX 其次
    children:
      - title:  LaTeX 其次 1
        url:    /latex/next
      - title:  LaTeX 其次 2
        url:    /latex/next2
      - title:  LaTeX 其次 3
        url:    /latex/next3

statas-nav:
  - title:      Stata 开始
    children:
      - title:  Stata 开始 1
        url:    /stata/begin
      - title:  Stata 开始 2
        url:    /stata/begin2
      - title:  Stata 开始 3
        url:    /stata/begin3
  - title:      Stata 其次
    children:
      - title:  Stata 其次 1
        url:    /stata/next
      - title:  Stata 其次 2
        url:    /stata/next2
      - title:  Stata 其次 3
        url:    /stata/next3
```

然后在文章的 YAML 头信息中放入以下内容，即可将定义的导航栏作为该文章的导航栏，如我需要在某篇文章中将上面定义的 `latexs-nav` 作为侧边导航栏：

```yaml
sidebar:
  nav: latexs-nav
```

## 撰写普通博客

普通博客所有的文章都在 */_posts* 文件夹中。这些文件可以用 Markdown 或 HTML 编写。撰写一篇普通博客，无非就是在 */_posts* 文件中创建一个新文件。Jekyll 要求一篇普通博客的文件名需遵循以下格式：

```text
年-月-日-标题.MARKUP
```

下面是一些合法的文件名的例子：

```text
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.markdown
```

所有博客文章顶部必须有一段 YAML 头信息(YAML front-matter)，如

```yaml
---
layout: article
title: Document - Writing Posts
mathjax: true
---
```

然而，一般而言，我们可以在 *_config.yaml* 为这类普通博客全局设定一些默认值，如：

```yaml
defaults: #为对应的部分设置默认值
  ## posts
  - scope:
      path: ""
      type: posts
    values:
      layout: article
      sharing: true
      license: true
      aside:
        toc: true
      show_edit_on_github: true
      show_subscribe: true
      pageview: false
```

当然，优先级为局部设定（文章本身的 YAML 头信息）>全局设定。

## 撰写 Docs

撰写这类文章会与前面撰写普通博客不一样，而且一般都会伴随着头部导航栏和侧边导航栏。下面以我想部署一个 LaTeX Docs 为例，给出流程（示例）：

- 在根目录下新建 *_latex* 文件夹

- 在里面写一些文章，它们的 YAML 头信息为

  ```yaml
  # 第一篇文章
  ---
  title: LaTeX 开始 1
  permalink: /latex/begin
  date: 2023-08-12
  ---
  
  # 第二篇文章
  ---
  title: LaTeX 开始 2
  permalink: /latex/begin2
  date: 2023-08-12
  ---
  
  # 第三篇文章
  ---
  title: LaTeX 开始 3
  permalink: /latex/begin3
  date: 2023-08-12
  ---
  
  # 第四篇文章
  ---
  title: LaTeX 其次 1
  permalink: /latex/next
  date: 2023-08-13
  ---
  
  # 第五篇文章
  ---
  title: LaTeX 其次 2
  permalink: /latex/next2
  date: 2023-08-13
  ---
  
  # 第六篇文章
  ---
  title: LaTeX 其次 3
  permalink: /latex/next3
  date: 2023-08-13
  ---
  ```

- 在 *_data/navigation.yml* 文件中为以上文章部署一个头部导航栏：

  ```yaml
  header:
    - title:      LaTeX
      url:        /latex/begin
  ```

- 在 *_data/navigation.yml* 文件中为以上文章部署侧边导航栏 `latexs-nav`：

  ```yaml
  latexs-nav:
    - title:      LaTeX 开始
      children:
        - title:  LaTeX 开始 1
          url:    /latex/begin
        - title:  LaTeX 开始 2
          url:    /latex/begin2
        - title:  LaTeX 开始 3
          url:    /latex/begin3
    - title:      LaTeX 其次
      children:
        - title:  LaTeX 其次 1
          url:    /latex/next
        - title:  LaTeX 其次 2
          url:    /latex/next2
        - title:  LaTeX 其次 3
          url:    /latex/next3
  ```

- 在 *_config.yaml* 中为上面系列文章添加 collections:

  ```yaml
  collections:
    latex:
      output: true
  ```

- 在 *_config.yaml* 中为目录 */_latex* 下所有文章设置以下默认格式：

  ```yaml
  defaults: #为对应的部分设置默认值
    ## _latex
    - scope:
        path: "_latex"
      values:
        layout: article
        nav_key: latex
        sidebar:
          nav: latexs-nav
        license: true
        aside:
          toc: true
        show_edit_on_github: true
        show_date: true
        # lightbox: true
  ```

- 至此，LaTeX Docs 部署完毕，编译后效果如下：

  ![image-20230813183916358](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308131839412.png){:.shadow}

## 添加 Home 主页

我想把 home 主页添加至头部导航栏上，且想将其摘录类型设置为 html。流程如下：

- 在 *_data/navigation.yml* 文件中为以上文章部署一个头部导航栏：

  ```yaml
  header:
    - title:      Home
      url:        /index.html
  ```

- 在文件 *_config.yaml* 中，将其摘录类型设置为 html：

  ```yaml
  defaults: #为对应的部分设置默认值
    ## home
    - scope:
        path: "index.html" # index.html 为网站入口，其 layout 为 home
      values:
        articles:
          excerpt_type: html
  ```

  除了以上方案，我们也可以直接在 *index.html* 文件添加以下 YAML 头信息来达到同样的目的：

  ```yaml
  ---
  layout: home
  articles:
    excerpt_type: html
  ---
  ```

- 至此，Home 主页部署完毕，编译后效果如下：

  ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308140211615.png){:.shadow}

## 为文章添加作者

想为文章添加之前，我们需要有一些准备工作：

- 在 *_config.yaml* 中添加全局作者，如：

  ```yaml
  ## => Author and Social
  ##############################
  author:
    type      : # "person" (default), "organization"
    name      : Meiting Wang
    url       :
    avatar    : https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308110137093.jpg # path or url of avatar image (square)
    bio       : The truth sets us free.
    email     : wangmeiting92@gmail.com
    facebook  : # "user_name" the last part of your profile url, e.g. https://www.facebook.com/user_name
    twitter   : # "user_name" the last part of your profile url, e.g. https://twitter.com/user_name
    weibo     : # "user_id"   the last part of your profile url, e.g. https://www.weibo.com/user_id/profile?...
    googleplus: # "user_id"   the last part of your profile url, e.g. https://plus.google.com/u/0/user_id
    telegram  : # "user_name" the last part of your profile url, e.g. https://t.me/user_name
    medium    : # "user_name" the last part of your profile url, e.g. https://medium.com/user_name
    zhihu     : # "user_name" the last part of your profile url, e.g. https://www.zhihu.com/people/user_name
    douban    : # "user_name" the last part of your profile url, e.g. https://www.douban.com/people/user_name
    linkedin  : # "user_name" the last part of your profile url, e.g. https://www.linkedin.com/in/user_name
    github    : Meiting-Wang # "user_name" the last part of your profile url, e.g. https://github.com/user_name
    npm       : # "user_name" the last part of your profile url, e.g. https://www.npmjs.com/~user_name
  ```

- 在 *_data/authors.yml* 文件中添加其他作者，如：

  ```yaml
  Meiting Wang:
    name      : Meiting Wang
    url       :
    avatar    : https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308110137093.jpg # path or url of avatar image (square)
    bio       : The truth sets us free.
    email     : wangmeiting92@gmail.com
    facebook  : # "user_name" the last part of your profile url, e.g. https://www.facebook.com/user_name
    twitter   : # "user_name" the last part of your profile url, e.g. https://twitter.com/user_name
    weibo     : # "user_id"   the last part of your profile url, e.g. https://www.weibo.com/user_id/profile?...
    googleplus: # "user_id"   the last part of your profile url, e.g. https://plus.google.com/u/0/user_id
    telegram  : # "user_name" the last part of your profile url, e.g. https://t.me/user_name
    medium    : # "user_name" the last part of your profile url, e.g. https://medium.com/user_name
    zhihu     : # "user_name" the last part of your profile url, e.g. https://www.zhihu.com/people/user_name
    douban    : # "user_name" the last part of your profile url, e.g. https://www.douban.com/people/user_name
    linkedin  : # "user_name" the last part of your profile url, e.g. https://www.linkedin.com/in/user_name
    github    : Meiting-Wang # "user_name" the last part of your profile url, e.g. https://github.com/user_name
    npm       : # "user_name" the last part of your profile url, e.g. https://www.npmjs.com/~user_name
  
  Tian Qi:
    name      : Tian Qi
    url       : https://tianqi.name
    avatar    : https://wx3.sinaimg.cn/large/73bd9e13ly1fjkqy66hl8j208c08c0td.jpg
    bio       : Author of TeXt.
    email     : kitian616@outlook.com
    facebook  : # "user_name" the last part of your profile url, e.g. https://www.facebook.com/user_name
    twitter   : kitian616 # "user_name" the last part of your profile url, e.g. https://twitter.com/user_name
    weibo     : 234695683 # "user_id"   the last part of your profile url, e.g. https://www.weibo.com/user_id/profile?...
    googleplus: 101827554735084402671 # "user_id"   the last part of your profile url, e.g. https://plus.google.com/u/0/user_id
    telegram  : # "user_name" the last part of your profile url, e.g. https://t.me/user_name
    medium    : # "user_name" the last part of your profile url, e.g. https://medium.com/user_name
    zhihu     : # "user_name" the last part of your profile url, e.g. https://www.zhihu.com/people/user_name
    douban    : # "user_name" the last part of your profile url, e.g. https://www.douban.com/people/user_name
    linkedin  : # "user_name" the last part of your profile url, e.g. https://www.linkedin.com/in/user_name
    github    : kitian616 # "user_name" the last part of your profile url, e.g. https://github.com/user_name
    npm       : # "user_name" the last part of your profile url, e.g. https://www.npmjs.com/~user_name
  ```

有了以上准备工作，我们就可以在写文章中添加作者了。方式有两种：

- 在特定的文章中添加特定的作者（对应作者信息需要先备案，即前面所述的准备工作）：

  ```yaml
  author: Tian Qi
  ```

- 全局设定默认作者，有需要更改时再按上一种方法在对应文章中进行添加即可。比如说我需要为撰写普通博客设定默认作者，则我们需要在 *_config.yaml* 文件中添加以下代码：

  ```yaml
  defaults: #为对应的部分设置默认值
    ## posts
    - scope:
        path: ""
        type: posts
      values:
        author: Meiting Wang
  ```

  > 如果本身已经有这个 scope 了，我们只需要将 `author: Meiting Wang` 这行代码放到对应位置即可。如果想为其他 scope 设置默认作者，也是类似操作。

## 数学公式

可以在文章 YAML 头信息中加入

```yaml
mathjax: true
```

以使得该文章支持数学公式。当 mathjax 被激活后，可以设置

```yaml
mathjax_autoNumber: true
```

让公式自动编号。这时，我们也可以在某个特定公式后面加上 `\notag` 来阻止其编号。例如，代码如下时：

```markdown
$\sin 2x$ 可以被展开成如下形式：
$$
\begin{gather}
\sin 2x = 2\sin x \cos x
\end{gather}
$$
$\cos 2x$ 可以被展开成如下形式：
$$
\begin{align}
\cos 2x &= \cos^2 x - \sin^2 x \\
	&= 2\cos^2 x - 1 \\
	&= 1 - 2\sin^2 x \notag
\end{align}
$$
```

效果如下：

$\sin 2x$ 可以被展开成如下形式：
$$
\begin{gather}
\sin 2x = 2\sin x \cos x
\end{gather}
$$
$\cos 2x$ 可以被展开成如下形式：
$$
\begin{align}
\cos 2x &= \cos^2 x - \sin^2 x \\
	&= 2\cos^2 x - 1 \\
	&= 1 - 2\sin^2 x \notag
\end{align}
$$
{:.success}

## Mermaid

Mermaid 可以被用来画文档图形，使用它之前，需要现在文章的 YAML 头信息中加上以下语句：

```yaml
mermaid: true
```

下面举出两个示例，代码 I 为：

```markdown
graph TB;
    A[Do you have a problem in your life?]
    B[Then don't worry]
    C[Can you do something about it?]
    A--no-->B;
    A--yes-->C;
    C--no-->B;
    C--yes-->B;
```

效果 I 为：

```mermaid
graph TB;
    A[Do you have a problem in your life?]
    B[Then don't worry]
    C[Can you do something about it?]
    A--no-->B;
    A--yes-->C;
    C--no-->B;
    C--yes-->B;
```

代码 II 为：

```markdown
graph TD;
    A-->B;
    A-->C;
    A-->D;
    B[It is a text]-->E;
    B-->F;
    B-->G;
    C-->H;
    C-->I;
    C-->J;
```

效果 II 为：

```mermaid
graph TD;
    A-->B;
    A-->C;
    A-->D;
    B[It is a text]-->E;
    B-->F;
    B-->G;
    C-->H;
    C-->I;
    C-->J;
```

> 更多使用方法参见 Mermaid 的官方网页：[http://mermaid.js.org/](http://mermaid.js.org/)


## 附加样式

Jekyll 使用 kramdown 作为默认 Markdown 解释器。kramdown 可以通过 ALDs 来设置块级元素或行内元素的属性。例如，可以通过 {:.class-name1.class-name-2} 来给元素定义样式类。TeXt 定义了一些样式类，你可以在文章和页面的方便的使用。

### 文字样式

TeXt 定义了四种可用文字样式，分别为：success、info、warning、error。

代码如下：

```css
Success Text.
{:.success}

Info Text.
{:.info}

Warning Text.
{:.warning}

Error Text.
{:.error}
```

效果如下：

Success Text.
{:.success}

Info Text.
{:.info}

Warning Text.
{:.warning}

Error Text.
{:.error}

### 标签样式

TeXt 定义了四种可用标签样式，分别为：success、info、warning、error。

代码如下：

```css
`Success`{:.success}

`Info`{:.info}

`Warning`{:.warning}

`Error`{:.error}
```

效果如下：

`Success`{:.success}

`Info`{:.info}

`Warning`{:.warning}

`Error`{:.error}

### 图片样式

TeXt 定义了四种可用图片样式，分别为：border、shadow、rounded、circle。

|  样式   |                 语法                 |                             效果                             |
| :-----: | :----------------------------------: | :----------------------------------------------------------: |
|  默认   |      `![Image](path-to-image)`       | ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png) |
| border  | `![Image](path-to-image){:.border}`  | ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.border} |
| shadow  | `![Image](path-to-image){:.shadow}`  | ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.shadow} |
| rounded | `![Image](path-to-image){:.rounded}` | ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.rounded} |
| circle  | `![Image](path-to-image){:.circle}`  | ![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.circle} |

我们也可将样式混合使用，如

|            样式            |                       语法                        |                             效果                             |
| :------------------------: | :-----------------------------------------------: | :----------------------------------------------------------: |
|     rounded and border     |    `![Image](path-to-image){:.rounded.border}`    | ![img](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.rounded.border} |
|     rounded and shadow     |    `![Image](path-to-image){:.rounded.shadow}`    | ![img](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.rounded.shadow} |
|     circle and border      |    `![Image](path-to-image){:.circle.border}`     | ![img](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.circle.border} |
|     circle and shadow      |    `![Image](path-to-image){:.circle.shadow}`     | ![img](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.circle.shadow} |
| circle, border, and shadow | `![Image](path-to-image){:.circle.border.shadow}` | ![img](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141544186.png){:.circle.border.shadow} |

## 扩展

我们可以在这里添加音频、视频等，细节如下：

### 音频

#### SoundCloud

添加 SoundCloud 的流程如下：

- 点击[这里](https://www.merlinschumacher.de/get-soundcloud-id/)进入 Soundcloud，打开对应歌曲分享页面

- 复制粘贴对应歌曲的 code，移至 [Get Soundcloud ID](https://www.merlinschumacher.de/get-soundcloud-id/) 网页获取该歌曲 id：


![](https://cdn.staticaly.com/gh/Meiting-Wang/pictures@main/picgo/202308141644613.png){:.shadow}

- 将歌曲 id 填入以下代码：

{% raw %}

```markdown
<div>{%- include extensions/soundcloud.html id='input your id' -%}</div>
```

如

```markdown
<div>{%- include extensions/soundcloud.html id='230333838' -%}</div>
```
{% endraw %}

可得

<div>{%- include extensions/soundcloud.html id='230333838' -%}</div>

#### 网易云音乐

添加网易云音乐的流程如下：

- 点击[这里](https://music.163.com/)进入网易云音乐，打开对应歌曲页面

- 歌曲链接自带 id，将 id 信息填入以下代码：

{% raw %}

```markdown
<div>{%- include extensions/netease-cloud-music.html id='input your id' -%}</div>
```

如

```markdown
<div>{%- include extensions/netease-cloud-music.html id='1938248779' -%}</div>
```

{% endraw %}

可得

<div>{%- include extensions/netease-cloud-music.html id='1938248779' -%}</div>

### 视频

#### 哔哩哔哩

添加哔哩哔哩的流程如下：

- 点击[这里](https://www.bilibili.com/)进入 B 站，打开对应视频页面

- 从转发>嵌入代码中找到页面 id，将 id 信息填入以下代码：

{% raw %}

```markdown
<div>{%- include extensions/bilibili.html id='input your id' -%}</div>
```

如

```markdown
<div>{%- include extensions/bilibili.html id='35800554' -%}</div>
```

{% endraw %}

可得（只有最低画质，如想看更高清画质，需进入 B 站原网页）

<div>{%- include extensions/bilibili.html id='35800554' -%}</div>

#### YouTube

添加 YouTube 的流程如下：

- 点击[这里](https://www.youtube.com/)进入 YouTube，打开对应视频页面

- 从页面链接中找到 id 信息（字符串`v=`后面的部分），将 id 信息填入以下代码：

{% raw %}

```markdown
<div>{%- include extensions/youtube.html id='input your id' -%}</div>
```

如

```markdown
<div>{%- include extensions/youtube.html id='svoThjjIzKE' -%}</div>
```

{% endraw %}

可得（可选择 4K 画质）

<div>{%- include extensions/youtube.html id='svoThjjIzKE' -%}</div>
