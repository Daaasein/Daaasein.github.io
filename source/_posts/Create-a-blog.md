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

## 准备工作

1. **安装 Node.js 和 NPM**: 请访问 [Node.js 官网](https://nodejs.org/) 下载并安装 Node.js，这将同时安装 NPM（Node Package Manager）。

2. **安装 Git**: 如果您还没有安装 Git，您需要安装它以便与 GitHub 进行交互。请访问 [Git 官网](https://git-scm.com/) 以下载和安装。



## 安装 Hexo

3. **全局安装 Hexo**: 打开命令行（Windows 下是 CMD 或 PowerShell，macOS 和 Linux 下是 Terminal），然后运行以下命令：
    ```bash
    npm install -g hexo-cli
    ```
    `-g`: global


## 初始化您的 Hexo 项目

4. **创建并初始化新项目**: 在命令行中，转到您希望创建项目的目录，然后运行：
   
    ```bash
    hexo init my-blog
    cd my-blog
    npm install # 它会查找一个名为 `package.json` 的文件。这个文件列出了项目所需的所有 NPM 包（依赖）。然后，NPM 会下载并安装这些依赖到 `my-blog` 目录下的 `node_modules` 子目录。
    ```

## 本地预览

5. **启动 Hexo 服务器**：在 `my-blog` 目录下运行以下命令，然后在浏览器中访问 `http://localhost:4000` 查看效果。
    ```bash
    hexo server
    ```

## 配置 Hexo

6. **编辑配置文件**: 在 `my-blog` 目录中，您会找到一个 `_config.yml` 文件。使用任意文本编辑器打开它并进行相关设置，例如您的博客标题、描述等。

## 创建您的第一篇文章

7. **新建文章**: 运行以下命令以创建一个新文章。
    ```bash
    hexo new "My First Post"
    ```

8. **编辑文章**: 打开 `source/_posts/My-First-Post.md` 文件，然后开始编写您的第一篇文章。您可以使用 Markdown 语法。

## 部署到 GitHub Pages

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
    `--save`: 将依赖项记录在`package.json`中，利于其他开发者克隆项目并运行`npm install`时，NPM会查看`package.json`文件，并自动安装所有列出的依赖项，无需手动一个一个安装。注意：从NPM 5.0.0版本开始，`--save`是默认行为。
    
12. **部署网站**: 在 `my-blog` 目录下运行以下命令以将网站部署到 GitHub Pages：
    ```bash
    hexo clean
    hexo generate
    hexo deploy
    ```

您应该现在可以通过访问 `https://username.github.io` 来查看您的博客了。

以上就是基础的步骤，您可以根据需要进行进一步的定制和优化，如安装主题、添加插件等。希望这对您有所帮助！

## 切换主题

[NexT - Theme for Hexo (theme-next.js.org)](https://theme-next.js.org/)

## 在任意设备修改Blog
基本思路：原有仓库的分支仅存储生成后的网页文件（public文件夹内的文件），若想在任意设备均可修改Blog，就需要Github托管所有源文件。为减少麻烦，不必创建一个新仓库，只需在原有仓库内新建一个branch存储所有源文件。甚至可以更进一步，当新branch有变更时，使用Github Action自动化部署。

### Branch
#### 步骤 1：创建并切换到新的分支

1. 克隆您现有的GitHub仓库（这里假设您已有一个托管在`main`分支的仓库）。
	```bash
	git clone https://github.com/your_username/your_repository.git
	```
2. 创建一个新的分支，比如命名为`development`。
	```bash
	git checkout -b development
	```
	`-b`: branch
#### 步骤 2：存储完整的Hexo项目文件

1. 删除`main`分支clone的本地文件（不要删除```.git```），存储希望push到新分支的所有文件。
2. 提交这些更改并推送到GitHub的`development`分支。
	```bash
	git add . 
	git commit -m "Store complete Hexo project" 
	git push origin development
	```

### CI/CD
CI/CD代表持续集成（Continuous Integration）和持续部署（Continuous Deployment）。

- **持续集成（CI）**: 是开发团队经常（通常是每天）将代码更改集成到共享仓库中的实践。之后会自动运行各种测试和其他检查工具。
- **持续部署（CD）**: 是一个自动将代码更改从共享仓库部署到生产环境的过程。

这些工具（如GitHub Actions, Jenkins等）能自动生成静态页面，因为您可以编写脚本或配置文件来自动执行一系列命令。这些命令可以包括安装依赖项、运行测试、构建项目（如使用`hexo generate`生成静态页面）以及将构建结果推送到指定的服务器或分支。这些步骤是自动化的，通常会在您提交代码或合并分支之后立即执行。

使用CI/CD工具，您可以确保代码更改是有效的，并且它们能快速、可靠地被部署到生产环境，减少了人工操作带来的风险和延迟。
#### CI/CD原理和安全性

##### 原理：

CI/CD 工具通常运行在一个预配置的环境（通常称为 "Runner" 或 "Agent"）中，这个环境具备执行构建、测试和部署任务所需的所有依赖和权限。当触发某个事件（如代码推送或合并请求）时，这些工具会按照您在配置文件中定义的步骤执行一系列命令。

##### 运行环境：

这些 Runner 或 Agent 可能运行在各种环境中，从完全托管的云实例到您自己的硬件。在 GitHub Actions 的情况下，它们通常运行在 GitHub 托管的虚拟环境（如 `ubuntu-latest` 或 `windows-latest`）中。

##### 安全性：

作为服务提供方，GitHub 采取了多种措施以保证 CI/CD 环境的安全：

1. 隔离：每个工作流程在其自己的环境中运行，与其他工作流程相隔离。
2. 限制权限：默认情况下，Runner 的权限被严格限制。
3. 审计和监控：所有 CI/CD 操作都可以被审计和监控。
4. 密码和密钥管理：使用诸如 `secrets` 的机制，安全地管理敏感信息。

#### 配置CI/CD

使用GitHub Actions或其他CI/CD工具，自动从`development`分支生成静态页面，并推送到`main`分支。这通常通过YAML文件来配置，该文件位于仓库的`.github/workflows`目录中。

以下是一个简单的GitHub Actions YAML文件示例，用于从`development`分支生成Hexo静态页面并推送到`main`分支。

```yaml
name: Deploy Hexo
on:
  push:
    branches:
      - development

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout development branch
        uses: actions/checkout@v2
        with:
          ref: development

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Cache dependencies
        uses: actions/cache@v2
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: ${{ runner.os }}-node-

      - name: Install Dependencies
        run: npm install

      - name: Generate site
        run: npx hexo generate

      - name: Deploy to main branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: main

```

这样，每次您在`development`分支做出更改并推送到GitHub后，CI/CD工具都会自动构建您的网站并将其部署到`main`分支。具体每个部分的解释如下：
当然，我会详细解释我之前提供的 GitHub Actions YAML 文件。

```yaml
name: Deploy Hexo
```
这一行定义了该 GitHub Actions 工作流的名字，名为 "Deploy Hexo"。

```yaml
on:
  push:
    branches:
      - development
```
这一部分定义了触发此工作流程的条件。在这里，任何推送（`push`）到`development`分支的操作都会触发这个工作流程。

```yaml
jobs:
  build-deploy:
```
在这里，`jobs`字段定义了一个或多个作业（job），每个作业都包含一系列的步骤（steps）。在这个例子中，有一个名为`build-deploy`的作业。

```yaml
    runs-on: ubuntu-latest
```
这一行定义了作业运行的环境，使用的是最新版本的 Ubuntu。

```yaml
    steps:
      - name: Checkout development branch
```
以下是作业的步骤。第一个步骤名为“Checkout development branch”。

```yaml
        uses: actions/checkout@v2
        with:
          ref: development
```
该步骤使用一个名为 `actions/checkout@v2` 的预建动作，用于检出代码。这里特定检出了`development`分支。

```yaml
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'
```
这个步骤用于设置 Node.js 环境。它使用了 `actions/setup-node@v2` 动作，并指定使用版本为 '14' 的 Node.js。

```yaml
      - name: Cache dependencies
        uses: actions/cache@v2
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: ${{ runner.os }}-node-
```
这个步骤用于缓存依赖项，以加速后续运行。这里缓存了 Node.js 的依赖项。

```yaml
      - name: Install Dependencies
        run: npm install
```
此步骤用于安装项目依赖项，使用了 `npm install` 命令。

```yaml
      - name: Generate site
        run: npx hexo generate
```
该步骤用于生成静态网站，使用 Hexo 的 `generate` 命令。

```yaml
      - name: Deploy to main branch
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: main
```
最后，这个步骤用于将生成的静态网站部署到 `main` 分支。它使用了一个第三方 GitHub Action：`peaceiris/actions-gh-pages@v3`。

- `github_token: ${{ secrets.GITHUB_TOKEN }}` 使用 GitHub 提供的令牌，以便于该工作流有权推送到 `main` 分支。
- `publish_dir: ./public` 指定了要部署的目录，这里是 `./public`。
- `publish_branch: main` 指定了目标部署分支，这里是 `main`。

整体而言，这个 YAML 文件描述了一个完整的 CI/CD 工作流程，从检出代码到设置环境，再到构建和部署，所有步骤都已定义。每次您向 `development` 分支推送更改时，这个工作流都会自动执行。
