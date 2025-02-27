---
title: Customize the Favicon
author: cotes
date: 2019-08-11 00:34:00 +0800
categories: [Blogging, Tutorial]
tags: [favicon]
---

  [favicons](https://www.favicon-generator.org/about/) of [**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/) 被放置在目录 `assets/img/favicons/`{: .filepath} 中。你可能希望用你自己的图标替换它们。以下各部分将指导你创建并替换默认的 favicon。

## 生成 favicon

准备一张尺寸为 512x512 或更大、且为正方形的图片（PNG、JPG 或 SVG 格式），和然后前往在线工具 [**Real Favicon Generator**](https://realfavicongenerator.net/)，点击按钮 <kbd>Select your Favicon image</kbd> 上传你的图片文件。

在下一步中，网页将显示所有使用场景。你可以保留默认选项，滚动至页面底部，和点击按钮 <kbd>Generate your Favicons and HTML code</kbd> 来生成 favicon。

## 下载 & 替换

下载生成的压缩包，解压后删除以下两个文件：

- `browserconfig.xml`{: .filepath}
- `site.webmanifest`{: .filepath}

然后将剩余的图片文件（`.PNG`{: .filepath} 和 `.ICO`{: .filepath}）复制过去，覆盖你 Jekyll 站点中目录 `assets/img/favicons/`{: .filepath} 下的原有文件。如果你的 Jekyll 站点还没有该目录，请直接新建一个。

下表将帮助你理解 favicon 文件的变化：

| File(s)             | From Online Tool                  | From Chirpy |
|---------------------|:---------------------------------:|:-----------:|
| `*.PNG`             | ✓                                 | ✗           |
| `*.ICO`             | ✓                                 | ✗           |

<!-- markdownlint-disable-next-line -->
>  ✓ 表示保留，✗ 表示删除。
{: .prompt-info }

下次构建站点时，favicon 将被替换为定制版。
