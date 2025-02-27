---
title: 入门指南  
description: >-  
  通过本综合概述，快速掌握 Chirpy 的基础知识。  
  您将学习如何安装、配置和使用您的第一个基于 Chirpy 的网站，并将其部署到 Web 服务器。  
author: cotes  
date: 2019-08-09 20:55:00 +0800  
categories: [博客, 教程]  
tags: [入门]  
pin: true  
media_subpath: '/posts/20180809'  
---

## 创建站点仓库  

在创建您的站点仓库时，您可以根据需求选择以下两种方法：  

### 选项 1. 使用 Starter（推荐）  

此方法可简化升级、隔离不必要的文件，非常适合希望以最少配置专注于写作的用户。  

1. 登录 GitHub 并前往 [**starter**][starter]。  
2. 点击 <kbd>使用此模板</kbd> 按钮，然后选择 <kbd>创建新仓库</kbd>。  
3. 将新仓库命名为 `<username>.github.io`，其中 `username` 需替换为您的 GitHub 用户名（小写）。  

### 选项 2. 复刻主题  

此方法适用于想要修改功能或 UI 设计的用户，但升级时可能会遇到困难。因此，除非您熟悉 Jekyll 并计划大幅修改此主题，否则不建议使用此方法。  

1. 登录 GitHub。  
2. [复刻主题仓库](https://github.com/cotes2020/jekyll-theme-chirpy/fork)。  
3. 将新仓库命名为 `<username>.github.io`，其中 `username` 需替换为您的 GitHub 用户名（小写）。  

## 设置开发环境  

创建仓库后，接下来需要配置开发环境。主要有两种方式：  

### 使用 Dev Containers（Windows 用户推荐）  

Dev Containers 通过 Docker 提供了一个隔离的环境，避免系统冲突，并确保所有依赖项都在容器内管理。  

**步骤**：  

1. 安装 Docker：  
   - 在 Windows/macOS 上，安装 [Docker Desktop][docker-desktop]。  
   - 在 Linux 上，安装 [Docker Engine][docker-engine]。  
2. 安装 [VS Code][vscode] 和 [Dev Containers 扩展][dev-containers]。  
3. 克隆您的仓库：  
   - 对于 Docker Desktop：启动 VS Code 并[在容器卷中克隆您的仓库][dc-clone-in-vol]。  
   - 对于 Docker Engine：先在本地克隆您的仓库，然后在 VS Code 中[在容器中打开][dc-open-in-container]。  
4. 等待 Dev Containers 设置完成。  

### 本地环境搭建（Unix-like 系统推荐）  

对于 Unix-like 系统，您可以选择直接安装 Jekyll 以获得最佳性能。当然，也可以使用 Dev Containers 作为替代方案。  

**步骤**：  

1. 按照 [Jekyll 安装指南](https://jekyllrb.com/docs/installation/) 安装 Jekyll，并确保已安装 [Git](https://git-scm.com/)。  
2. 在本地克隆您的仓库。  
3. 如果您是复刻主题方式创建的仓库，需要安装 [Node.js][nodejs]，然后在仓库根目录运行 `bash tools/init.sh` 进行初始化。  
4. 在仓库根目录运行以下命令安装依赖项：  

   ```terminal
   $ bundle
   ```  

## 使用方法  

### 启动 Jekyll 服务器  

要在本地运行站点，请执行以下命令：  

```terminal
$ bundle exec jekyll serve
```  

> 如果您使用 Dev Containers，则必须在 **VS Code** 的终端中运行该命令。  
{: .prompt-info }  

几秒钟后，本地服务器将在 <http://127.0.0.1:4000> 运行。  

### 配置  

根据需要更新 `_config.yml`{: .filepath} 文件中的变量。常见选项包括：  

- `url`  
- `avatar`  
- `timezone`  
- `lang`  

### 社交联系方式  

社交联系方式显示在侧边栏底部。您可以在 `_data/contact.yml`{: .filepath} 文件中启用或禁用特定的社交链接。  

### 自定义样式表  

要自定义样式表，请将主题的 `assets/css/jekyll-theme-chirpy.scss`{: .filepath} 复制到您的 Jekyll 站点的相同路径下，并在文件末尾添加您的自定义样式。  

### 自定义静态资源  

静态资源配置是在 `5.1.0` 版本中引入的。静态资源的 CDN 在 `_data/origin/cors.yml`{: .filepath} 文件中定义。您可以根据网站发布所在地区的网络情况替换部分资源。  

如果您希望自行托管静态资源，请参考 [_chirpy-static-assets_](https://github.com/cotes2020/chirpy-static-assets#readme) 仓库。  

## 部署  

在部署前，请检查 `_config.yml`{: .filepath} 文件，并确保 `url` 已正确配置。如果您使用 [**项目站点**](https://help.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites) 且未使用自定义域名，或者希望在 **GitHub Pages** 之外的 Web 服务器上访问您的网站，请务必将 `baseurl` 设置为项目名称，并以斜杠开头，例如 `/project-name`。  

您可以选择以下 _其中一种_ 方法部署您的 Jekyll 站点。  

### 使用 GitHub Actions 自动部署  

请准备以下内容：  

- 若使用 GitHub 免费计划，您的站点仓库必须保持公开。  
- 如果您的仓库已提交 `Gemfile.lock`{: .filepath} 文件，且您的本地计算机不是运行 Linux，则需要更新锁文件的平台列表：  

  ```console
  $ bundle lock --add-platform x86_64-linux
  ```  

接下来，配置 _Pages_ 服务：  

1. 进入 GitHub 仓库，选择 _Settings_（设置）选项卡，在左侧导航栏中点击 _Pages_。在 **Source**（来源）部分（位于 _Build and deployment_ 下），从下拉菜单中选择 [**GitHub Actions**][pages-workflow-src]。  
   ![构建来源](pages-source-light.png){: .light .border .normal w='375' h='140' }  
   ![构建来源](pages-source-dark.png){: .dark .normal w='375' h='140' }  

2. 提交任意更改到 GitHub 以触发 _Actions_ 工作流。在仓库的 _Actions_ 选项卡中，您应该能看到 _Build and Deploy_ 工作流正在运行。一旦构建完成并成功，站点将自动部署。  

现在，您可以通过 GitHub 提供的 URL 访问您的网站。  

### 手动构建和部署  

对于自托管服务器，您需要在本地构建站点，然后将生成的站点文件上传到服务器。  

进入源项目的根目录，并使用以下命令构建您的站点：  

```console
$ JEKYLL_ENV=production bundle exec jekyll b
```  

除非另行指定输出路径，生成的站点文件将存放在项目根目录下的 `_site`{: .filepath} 文件夹内。您需要将这些文件上传到目标服务器。  

[nodejs]: https://nodejs.org/  
[starter]: https://github.com/cotes2020/chirpy-starter  
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow  
[docker-desktop]: https://www.docker.com/products/docker-desktop/  
[docker-engine]: https://docs.docker.com/engine/install/  
[vscode]: https://code.visualstudio.com/  
[dev-containers]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers  
[dc-clone-in-vol]: https://code.visualstudio.com/docs/devcontainers/containers#_quick-start-open-a-git-repository-or-github-pr-in-an-isolated-container-volume  
[dc-open-in-container]: https://code.visualstudio.com/docs/devcontainers/containers#_quick-start-open-an-existing-folder-in-a-container  
