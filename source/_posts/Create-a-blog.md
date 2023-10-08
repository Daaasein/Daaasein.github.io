---
title: Create a blog
date: 2023-10-04 21:47:10
category: Skills
tags: 
- Hexo
- Next
description: "这里是一个使用 Hexo 和 GitHub Pages 搭建个人博客的基础步骤指南。"
---

这里是一个使用 Hexo 和 GitHub Pages 搭建个人博客的基础步骤指南：

### 准备工作

1. **安装 Node.js 和 NPM**: 请访问 [Node.js 官网](https://nodejs.org/) 下载并安装 Node.js，这将同时安装 NPM（Node Package Manager）。

2. **安装 Git**: 如果您还没有安装 Git，您需要安装它以便与 GitHub 进行交互。请访问 [Git 官网](https://git-scm.com/) 以下载和安装。

### 安装 Hexo

3. **全局安装 Hexo**: 打开命令行（Windows 下是 CMD 或 PowerShell，macOS 和 Linux 下是 Terminal），然后运行以下命令：
    ```bash
    npm install -g hexo-cli
    ```
    ```-g```: global
### 初始化您的 Hexo 项目

4. **创建并初始化新项目**: 在命令行中，转到您希望创建项目的目录，然后运行：
    ```bash
    hexo init my-blog
    cd my-blog
    npm install # 它会查找一个名为 `package.json` 的文件。这个文件列出了项目所需的所有 NPM 包（依赖）。然后，NPM 会下载并安装这些依赖到 `my-blog` 目录下的 `node_modules` 子目录。
    ```

### 本地预览

5. **启动 Hexo 服务器**：在 `my-blog` 目录下运行以下命令，然后在浏览器中访问 `http://localhost:4000` 查看效果。
    ```bash
    hexo server
    ```

### 配置 Hexo

6. **编辑配置文件**: 在 `my-blog` 目录中，您会找到一个 `_config.yml` 文件。使用任意文本编辑器打开它并进行相关设置，例如您的博客标题、描述等。

### 创建您的第一篇文章

7. **新建文章**: 运行以下命令以创建一个新文章。
    ```bash
    hexo new "My First Post"
    ```

8. **编辑文章**: 打开 `source/_posts/My-First-Post.md` 文件，然后开始编写您的第一篇文章。您可以使用 Markdown 语法。

### 部署到 GitHub Pages

9. **创建 GitHub 仓库**: 访问 [GitHub](https://github.com/)，创建一个新的仓库，名为 `username.github.io`（`username` 是您的 GitHub 用户名）。

10. **配置部署**: 在 `_config.yml` 文件中，找到 `deploy` 部分，并进行如下设置：
    ```yaml
    deploy:
      type: git
      repository: <repository url>
      branch: main
    ```

11. **安装部署插件**: 在 `my-blog` 目录下运行以下命令以安装 Hexo 的 Git 部署插件：
    ```bash
    npm install hexo-deployer-git --save
    ```
    ```--save```: 将依赖项记录在`package.json`中，利于其他开发者克隆项目并运行`npm install`时，NPM会查看`package.json`文件，并自动安装所有列出的依赖项，无需手动一个一个安装。注意：从NPM 5.0.0版本开始，`--save`是默认行为。
12. **部署网站**: 在 `my-blog` 目录下运行以下命令以将网站部署到 GitHub Pages：
    ```bash
    hexo clean
    hexo generate
    hexo deploy
    ```

您应该现在可以通过访问 `https://username.github.io` 来查看您的博客了。

以上就是基础的步骤，您可以根据需要进行进一步的定制和优化，如安装主题、添加插件等。希望这对您有所帮助！

### 切换主题
[NexT - Theme for Hexo (theme-next.js.org)](https://theme-next.js.org/)
