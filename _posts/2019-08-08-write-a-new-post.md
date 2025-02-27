---
title: 撰写新文章
author: cotes
date: 2019-08-08 14:10:00 +0800
categories: [博客, 教程]
tags: [写作]
render_with_liquid: false
---

本教程将指导您如何在 _Chirpy_ 模板中撰写文章，即使您之前使用过 Jekyll，也值得一读，因为许多功能需要设置特定的变量。

## 文件命名和路径

在项目根目录下的 `_posts`{: .filepath} 中创建一个名为 `YYYY-MM-DD-TITLE.EXTENSION`{: .filepath} 的新文件。请注意，`EXTENSION`{: .filepath} 必须是 `md`{: .filepath} 或 `markdown`{: .filepath} 之一。如果您希望节省创建文件的时间，请考虑使用插件 [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose) 来实现。

## Front Matter

基本上，您需要在文章顶部填写如下 [Front Matter](https://jekyllrb.com/docs/front-matter/)：

```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [主分类, 子分类]
tags: [标签]     # 标签名称应始终为小写
---
```

> 文章的 _layout_ 已默认设置为 `post`，因此无需在 Front Matter 块中添加 _layout_ 变量。
{: .prompt-tip }

### 日期的时区

为了准确记录文章发布时间，您不仅需要在 `_config.yml`{: .filepath} 中设置 `timezone`，还需要在文章 Front Matter 的 `date` 变量中提供文章的时区。格式为 `+/-TTTT`，例如 `+0800`。

### 分类和标签

每篇文章的 `categories` 设计为最多包含两个元素，而 `tags` 的元素个数可以为零或任意多个。例如：

```yaml
---
categories: [动物, 昆虫]
tags: [bee]
---
```

### 作者信息

文章的作者信息通常不需要在 Front Matter 中填写，默认会从配置文件中的 `social.name` 和 `social.links` 的第一项获取。但您也可以按如下方式覆盖它：

在 `_data/authors.yml` 中添加作者信息（如果您的网站还没有该文件，请随时创建一个）。

```yaml
<author_id>:
  name: <全名>
  twitter: <作者的twitter>
  url: <作者的主页>
```
{: file="_data/authors.yml" }

然后使用 `author` 指定单个条目或使用 `authors` 指定多个条目：

```yaml
---
author: <author_id>                     # 单个条目
# 或者
authors: [<author1_id>, <author2_id>]     # 多个条目
---
```

需要说明的是，键 `author` 也可以识别多个条目。

> 从文件 `_data/authors.yml`{: .filepath} 中读取作者信息的好处在于，页面会带有 `twitter:creator` 的 meta 标签，这丰富了 [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution) 信息，对 SEO 有益。
{: .prompt-info }

### 文章描述

默认情况下，文章的前几句话会用于在主页文章列表、“更多阅读”部分以及 RSS 的 XML 中显示。如果您不希望显示自动生成的文章描述，可以通过在 Front Matter 中使用 `description` 字段自定义描述，如下：

```yaml
---
description: 文章的简短摘要。
---
```

另外，`description` 文本也会显示在文章页面的标题下方。

## 目录

默认情况下，文章右侧会显示 **T**able **o**f **C**ontents (TOC)。如果您想全局关闭目录，请前往 `_config.yml`{: .filepath} 并将变量 `toc` 的值设置为 `false`。如果只想对某篇文章关闭目录，在该文章的 [Front Matter](https://jekyllrb.com/docs/front-matter/) 中添加以下内容：

```yaml
---
toc: false
---
```

## 评论

评论的全局设置由 `_config.yml`{: .filepath} 文件中的 `comments.provider` 选项定义。一旦为此变量选择了评论系统，则所有文章的评论将被启用。

如果您想对某篇文章关闭评论，在该文章的 **Front Matter** 中添加以下内容：

```yaml
---
comments: false
---
```

## 媒体

在 _Chirpy_ 中，我们将图片、音频和视频统称为媒体资源。

### URL 前缀

有时我们需要为文章中的多个资源定义重复的 URL 前缀，这是一项繁琐的任务，可以通过设置两个参数来避免。

- 如果您使用 CDN 来托管媒体文件，可以在 `_config.yml`{: .filepath} 中指定 `cdn`。这样，站点头像和文章中媒体资源的 URL 将以 CDN 域名作为前缀。

  ```yaml
  cdn: https://cdn.com
  ```
  {: file='_config.yml' .nolineno }

- 若要为当前文章/页面的资源路径指定前缀，请在文章的 Front Matter 中设置 `media_subpath`：

  ```yaml
  ---
  media_subpath: /path/to/media/
  ---
  ```
  {: .nolineno }

选项 `site.cdn` 和 `page.media_subpath` 可以单独使用，也可以组合使用，以灵活构成最终的资源 URL：`[site.cdn/][page.media_subpath/]file.ext`

### 图片

#### 标题说明

在图片下一行添加斜体文本，便会成为图片标题，并显示在图片底部：

```markdown
![img-description](/path/to/image)
_Image Caption_
```
{: .nolineno}

#### 大小

为防止加载图片时页面内容布局发生位移，应为每张图片设置宽度和高度。

```markdown
![Desktop View](/assets/img/sample/mockup.png){: width="700" height="400" }
```
{: .nolineno}

> 对于 SVG 图像，至少需要指定其 _width_，否则不会被渲染。
{: .prompt-info }

从 _Chirpy v5.0.0_ 开始，`height` 和 `width` 支持缩写（`height` → `h`，`width` → `w`）。以下示例与上例效果相同：

```markdown
![Desktop View](/assets/img/sample/mockup.png){: w="700" h="400" }
```
{: .nolineno}

#### 位置

默认情况下，图片居中显示，但您可以使用 `normal`、`left` 和 `right` 类来指定图片位置。

> 一旦指定了位置，就不应再添加图片标题。
{: .prompt-warning }

- **正常位置**

  以下示例中图片将左对齐：

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .normal }
  ```
  {: .nolineno}

- **向左浮动**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .left }
  ```
  {: .nolineno}

- **向右浮动**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .right }
  ```
  {: .nolineno}

#### 暗/亮模式

您可以使图片根据主题的暗/亮模式切换显示。这需要您准备两张图片，一张用于暗模式，一张用于亮模式，然后分别为它们指定特定的类（`dark` 或 `light`）：

```markdown
![仅亮模式](/path/to/light-mode.png){: .light }
![仅暗模式](/path/to/dark-mode.png){: .dark }
```

#### 阴影效果

程序窗口的截屏可以通过添加阴影效果来显示：

```markdown
![Desktop View](/assets/img/sample/mockup.png){: .shadow }
```
{: .nolineno}

#### 预览图片

如果您想在文章顶部添加一张图片，请提供分辨率为 `1200 x 630` 的图片。请注意，如果图片的宽高比不符合 `1.91 : 1`，图片将被缩放和裁剪。

了解上述前提后，您可以开始设置图片的属性：

```yaml
---
image:
  path: /path/to/image
  alt: 图片替代文本
---
```

请注意，[`media_subpath`](#url-前缀) 同样可以传递给预览图片，也就是说，当它已被设置时，属性 `path` 只需要图片文件名。

简单使用时，您也可以直接使用 `image` 来定义路径。

```yaml
---
image: /path/to/image
---
```

#### LQIP

对于预览图片：

```yaml
---
image:
  lqip: /path/to/lqip-file # 或者 base64 URI
---
```

> 您可以在文章 “[文本与排版](../text-and-typography/)” 的预览图片中观察到 LQIP 效果。
  
对于普通图片：

```markdown
![图片描述](/path/to/image){: lqip="/path/to/lqip-file" }
```
{: .nolineno }

### 视频

#### 社交媒体平台

您可以使用以下语法嵌入来自社交媒体平台的视频：

```liquid
{% include embed/{Platform}.html id='{ID}' %}
```

其中 `Platform` 为平台名称的小写，`ID` 为视频 ID。

下表展示了如何从给定视频 URL 中获取这两个参数，同时您也可以了解当前支持的视频平台。

| 视频 URL                                                                                           | 平台       | ID             |
| -------------------------------------------------------------------------------------------------- | ---------- | :------------- |
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=H-B46URT4mg) | `youtube`  | `H-B46URT4mg`  |
| [https://www.**twitch**.tv/videos/**1634779211**](https://www.twitch.tv/videos/1634779211)         | `twitch`   | `1634779211`   |
| [https://www.**bilibili**.com/video/**BV1Q44y1B7Wf**](https://www.bilibili.com/video/BV1Q44y1B7Wf) | `bilibili` | `BV1Q44y1B7Wf` |

#### 视频文件

如果您想直接嵌入视频文件，请使用以下语法：

```liquid
{% include embed/video.html src='{URL}' %}
```

其中 `URL` 是指向视频文件的 URL，例如 `/path/to/sample/video.mp4`。

您还可以为嵌入的视频文件指定其他属性。以下是允许的所有属性列表。

- `poster='/path/to/poster.png'` — 视频下载时显示的封面图片
- `title='文本'` — 显示在视频下方的标题，与图片效果相同
- `autoplay=true` — 视频一旦加载完成自动开始播放
- `loop=true` — 播放结束后自动返回视频开头
- `muted=true` — 音频初始静音
- `types` — 指定额外视频格式的扩展名，多个格式之间用 `|` 分隔。确保这些文件与主视频文件位于同一目录下。

下面是一个包含所有上述属性的示例：

```liquid
{%
  include embed/video.html
  src='/path/to/video.mp4'
  types='ogg|mov'
  poster='poster.png'
  title='演示视频'
  autoplay=true
  loop=true
  muted=true
%}
```

### 音频

如果您想直接嵌入音频文件，请使用以下语法：

```liquid
{% include embed/audio.html src='{URL}' %}
```

其中 `URL` 是指向音频文件的 URL，例如 `/path/to/audio.mp3`。

您还可以为嵌入的音频文件指定其他属性。以下是允许的所有属性列表。

- `title='文本'` — 显示在音频下方的标题，与图片效果相同
- `types` — 指定额外音频格式的扩展名，多个格式之间用 `|` 分隔。确保这些文件与主音频文件位于同一目录下。

下面是一个包含所有上述属性的示例：

```liquid
{%
  include embed/audio.html
  src='/path/to/audio.mp3'
  types='ogg|wav|aac'
  title='演示音频'
%}
```

## 置顶文章

您可以将一篇或多篇文章置顶显示在主页顶部，置顶文章会按照发布时间的倒序排列。启用方法如下：

```yaml
---
pin: true
---
```

## 提示

提示分为几种类型：`tip`、`info`、`warning` 和 `danger`。只需为引用块添加类 `prompt-{type}` 即可生成对应类型的提示。例如，定义一个 `info` 类型的提示如下：

```md
> 示例提示内容。
{: .prompt-info }
```
{: .nolineno }

## 语法

### 行内代码

```md
`内联代码部分`
```
{: .nolineno }

### 文件路径高亮

```md
`/path/to/a/file.extend`{: .filepath}
```
{: .nolineno }

### 代码块

Markdown 语法 ```` ``` ```` 可轻松创建代码块，如下所示：

````md
```
这是一个纯文本代码片段。
```
````

#### 指定语言

使用 ```` ```{language} ```` 将获得具有语法高亮的代码块：

````markdown
```yaml
key: value
```
````

> Jekyll 标签 `{% highlight %}` 与本主题不兼容。
{: .prompt-danger }

#### 行号

默认情况下，除 `plaintext`、`console` 和 `terminal` 之外的所有语言都会显示行号。当您想隐藏代码块的行号时，可以为其添加 `nolineno` 类：

````markdown
```shell
echo '不再显示行号！'
```
{: .nolineno }
````

#### 指定文件名

您可能已经注意到，代码块顶部会显示代码语言。如果您想用文件名替换它，可以添加 `file` 属性来实现：

````markdown
```shell
# 内容
```
{: file="path/to/file" }
````

#### Liquid 代码

如果您想展示 **Liquid** 代码片段，请用 `{% raw %}` 和 `{% endraw %}` 包裹 Liquid 代码：

````markdown
{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  该产品标题中包含 "Pack" 一词。
{% endif %}
```
{% endraw %}
````

或者在文章的 YAML 块中添加 `render_with_liquid: false`（需要 Jekyll 4.0 或更高版本）。

## 数学公式

我们使用 [**MathJax**][mathjax] 来渲染数学公式。出于网站性能考虑，数学功能默认不加载，但可以通过以下方式启用：

[mathjax]: https://www.mathjax.org/

```yaml
---
math: true
---
```

启用数学功能后，您可以使用以下语法添加数学公式：

- **块级公式** 应该用 `$$ math $$` 包裹，并且 `$$` 前后必须有空行  
  - **插入公式编号** 可用 `$$\begin{equation} math \end{equation}$$`  
  - **引用公式编号** 应在公式块中使用 `\label{eq:label_name}`，在正文中使用 `\eqref{eq:label_name}`（见下例）
- **行内公式**（在文本行内）应使用 `$$ math $$`，且前后不需要空行
- **行内公式**（在列表中）应使用 `\$$ math $$`

```markdown
<!-- 块级公式，请确保前后有空行 -->

$$
LaTeX_math_expression
$$

<!-- 带公式编号的块级公式，请确保前后有空行  -->

$$
\begin{equation}
  LaTeX_math_expression
  \label{eq:label_name}
\end{equation}
$$

可以通过 \eqref{eq:label_name} 来引用。

<!-- 行内公式，行内使用，无空行 -->

"Lorem ipsum dolor sit amet, $$ LaTeX_math_expression $$ consectetur adipiscing elit."

<!-- 列表中的行内公式，首个 `$` 需要转义 -->

1. \$$ LaTeX_math_expression $$
2. \$$ LaTeX_math_expression $$
3. \$$ LaTeX_math_expression $$
```

> 从 `v7.0.0` 开始，**MathJax** 的配置选项已移至文件 `assets/js/data/mathjax.js`{: .filepath}，您可以根据需要更改选项，例如添加 [extensions][mathjax-exts]。  
> 如果您是通过 `chirpy-starter` 构建站点，请将该文件从 gem 安装目录（可通过命令 `bundle info --path jekyll-theme-chirpy` 检查）复制到您仓库的相同目录下。
{: .prompt-tip }

[mathjax-exts]: https://docs.mathjax.org/en/latest/input/tex/extensions/index.html

## Mermaid

[**Mermaid**](https://github.com/mermaid-js/mermaid) 是一个出色的图表生成工具。要在文章中启用它，请在 YAML 块中添加以下内容：

```yaml
---
mermaid: true
---
```

然后，您可以像其他 Markdown 语言一样使用它：用 ```` ```mermaid ```` 和 ```` ``` ```` 包裹图表代码。

## 了解更多

有关 Jekyll 文章的更多知识，请访问 [Jekyll Docs: Posts](https://jekyllrb.com/docs/posts/)。
