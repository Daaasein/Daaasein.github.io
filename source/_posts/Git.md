---
title: Git
date: 2023-09-30 00:37:25
category: Skills
tags:
description: "展示Git的基本操作步骤，以及如何连接Github等远程仓库。"
---

# 远程仓库身份验证
Git连接远程仓库需要身份验证，直接使用Github账户和密码验证的方式已被弃用，目前官方指南有两种方式：`GitHub CLI` 或 `Git 凭据管理器（GCM）`。
## GCM（推荐）
Git 凭据管理器 （GCM） 是另一种安全存储凭据并通过 HTTPS 连接到 GitHub 的方法。使用 GCM，您无需手动创建和存储个人访问令牌，因为 GCM 代表您管理身份验证，包括 2FA（双因素身份验证）。

1. Install Git using [Homebrew](https://brew.sh/):  
    使用 Homebrew 安装 Git：
    
    ```bash
    brew install git
    ```
    
2. Install GCM using Homebrew:  
    使用Homebrew安装 GCM：
    
    ```bash
    brew install --cask git-credential-manager
    ```
    
    For MacOS, you don't need to run `git config` because GCM automatically configures Git for you.  
    对于 MacOS，您无需运行 `git config` ，因为 GCM 会自动为您配置 Git。

The next time you clone an HTTPS URL that requires authentication, Git will prompt you to log in using a browser window. You may first be asked to authorize an OAuth app. If your account or organization requires [two-factor auth](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa), you'll also need to complete the 2FA challenge.  
下次克隆需要身份验证的 HTTPS URL 时，Git 将提示您使用浏览器窗口登录。系统可能会首先要求您授权 OAuth 应用程序。如果您的帐户或组织需要双重身份验证，您还需要完成 2FA 挑战。

Once you've authenticated successfully, your credentials are stored in the macOS keychain and will be used every time you clone an HTTPS URL. Git will not require you to type your credentials in the command line again unless you change your credentials.  
成功进行身份验证后，您的凭据将存储在 macOS 钥匙串中，并在每次克隆 HTTPS URL 时使用。Git 不会要求您再次在命令行中键入凭据，除非您更改凭据。
## Github CLI
当您选择 `HTTPS` 作为 Git 操作的首选协议时，GitHub CLI 将自动为您存储 Git 凭据，并对询问您是否要使用 GitHub 凭据向 Git 进行身份验证的提示回答“是”。

1. [Install](https://github.com/cli/cli#installation) GitHub CLI on macOS, Windows, or Linux.  
    在 macOS、Windows 或 Linux 上安装 GitHub CLI。
    
2. In the command line, enter `gh auth login`, then follow the prompts.  
    在命令行中，输入 `gh auth login` ，然后按照提示操作。
    
    - When prompted for your preferred protocol for Git operations, select `HTTPS`.  
        当系统提示您输入 Git 操作的首选协议时，请选择 `HTTPS` 。
    - When asked if you would like to authenticate to Git with your GitHub credentials, enter `Y`.  
        当系统询问您是否要使用 GitHub 凭据向 Git 进行身份验证时，请输入 `Y` 。


# Init

1. 改变当前目录为iCloud的Documents目录。
	```bash
	cd ~/Documents
	```
2. 您可以使用 `git` 创建一个仓库来保存它们。
    ```bash
    git init      # 在当前目录创建仓库
    git status    # 查看文件状态，应该会显示所有文件都没有被添加
    git add --all # 添加所有文件（包含已删除文件）到暂存区
    git commit -m "Save zsh config, Homebrew config and Obsidian files"
                  # 提交与提示信息
    ```
## 创建忽略规则
在使用Git和GitHub同步Obsidian文件时，如果你想忽略Obsidian的配置文件夹（通常是 `.obsidian` 文件夹），你可以使用`.gitignore`文件来达到这个目的。

1. 在你的本地Git仓库的根目录下创建一个名为`.gitignore`的文件。

	1. 切换到您的本地Git 仓库目录，例如：
	    ```bash
	    cd ~/path/to/your/GitRespository
	    ```
	2. 创建 `.gitignore` 文件：创建一个名为 `.gitignore` 的空文件。
	    ```bash
	    touch .gitignore
	    ```
2. 编辑这个文件，并加入您想要 Git 忽略的文件或目录规则。
	1. 例如，如果您想使用 `nano` 编辑 `.gitignore` 文件，您可以运行：

		```bash
		nano .gitignore
		```

	2. 然后，在打开的编辑器里加入想忽略的目录。
	
	    ```
	    .obsidian/    # 加入这一行告诉Git忽略.obsidian文件夹及其所有内容。
	    ```

	3. `Ctrl+O`保存`.gitignore`文件，`Enter`确认，`Ctrl+X`退出。

4. 运行以下Git命令以添加和提交`.gitignore`文件：

    ```bash
    git add .gitignore
    git commit -m "Add .gitignore to exclude .obsidian folder"
    ```

之后，`.obsidian`文件夹及其所有内容就不会被Git追踪，也就不会出现在GitHub仓库中。

这样做的好处是在多台设备上使用Obsidian打开同一个Vault时，不会因为设备特定的配置或插件设置而导致冲突。

## 从远程仓库移除文件夹
如果你在设置`.gitignore`文件之前已经上传了`.obsidian`文件夹到GitHub，那么该文件夹会继续停留在远程仓库中，即使你后来在`.gitignore`文件中指定了忽略它。`.gitignore`只会防止未来对指定文件或文件夹的追踪，对于已经被追踪（tracked）或已经提交（committed）的文件，它不会有影响。

**因此，我们需要将它从Git索引中移除，再提交一次：**

1. **在本地仓库的Git索引中删除`.obsidian`文件夹**：

    ```bash
    git rm -r --cached .obsidian/
    ```
    
    这个命令会将`.obsidian`文件夹从Git的索引（staging area）中移除，但不会删除你本地的`.obsidian`文件夹。通过`--cached`实现这一点。

2. **提交这次更改**：

    ```bash
    git commit -m "Remove .obsidian folder"
    ```

3. **将这次更改推送到远程仓库**：

    ```bash
    git push origin [your-branch-name]
    ```

现在，`.obsidian`文件夹应该已经从你的远程GitHub仓库中被移除，而你本地的`.obsidian`文件夹应该依然存在并保持不变。从这一点开始，由于`.gitignore`文件的存在，`.obsidian`文件夹将不会被再次上传。

# Remote

1. **创建一个远程 Git 仓库：** 您可以在 GitHub、GitLab 或其他平台上创建一个新的远程仓库。

2. **推送本地仓库到远程：** 这样，即使您的本地机器和iCloud同时出现问题，您的文件也不会丢失。
	1. **添加一个新的远程仓库**：将其命名为 "origin"。
	```bash
	git remote add origin <your-remote-repository-url>
	```
	2. **重命名当前分支**：强制重命名为 "main"。
	```bash
	git branch -M main 
	# -M：选项意味着如果 "main" 分支已经存在，那么将会强制执行该操作
	# 由于历史原因，仓库主分支命名可能为master或main，这可能导致冲突，因此需要统一。
	```
	3. **将本地的 "main" 分支推送到远程仓库 "origin"**：在这里，就是前面添加的 GitHub 仓库。
    ```bash
	git push -u origin main
	# -u：选项用于设置本地 "main" 分支和远程 "origin/main" 分支之间的上游跟踪关系，这样在未来执行 `git pull` 或 `git push` 时，不需要指定远程仓库和分支。
    ```

# Clone
1. **克隆远程仓库：** 克隆您之前创建的远程仓库。

    ```bash
    git clone <your-remote-repository-url>
    ```

# Update
## update repository
在Git中，从远程仓库更新本地文件通常可以通过以下几个步骤来实现：

1. **确定当前状态**: 首先，使用`git status`命令来检查本地工作目录和暂存区的状态。这将告诉您是否有未提交的更改。
   
2. **保存本地更改**: 如果您有未提交的更改，并且想要保留它们，您有几个选项：
   
    - 使用`git stash`将更改暂存起来。
    - 使用`git commit`将更改提交。
3. **拉取远程更改**: 然后，使用以下命令从远程仓库拉取最新的更改：
    ```bash
    git pull origin <branch_name>
    ```
    这实际上是`git fetch`和`git merge`的一个组合操作。`git fetch`会从远程获取最新版本到本地，然后`git merge`会将这些更改合并到您当前的工作分支中。
    
4. **处理合并冲突**: 如果出现合并冲突，您需要手动解决这些冲突，并然后使用`git add`将解决后的文件标记为已解决状态，再执行`git commit`。
   
5. **应用暂存的更改**: 如果您在第2步中使用了`git stash`，现在可以使用`git stash apply`或`git stash pop`来应用这些暂存的更改。

以下是几点需要考虑的因素：
- **分支**: 确保您在正确的分支上执行这些操作。如果需要，您可以使用`git checkout <branch_name>`切换到正确的分支。
## update submodule
```bash
git submodule update --init --recursive
```

# Branch
## 步骤 1：创建并切换到新的分支

1. 克隆您现有的GitHub仓库（这里假设您已有一个托管在`main`分支的仓库）。
	```bash
	git clone https://github.com/your_username/your_repository.git
	```
2. 创建一个新的分支，比如命名为`development`。
	```bash
	git checkout development
	```
## 步骤 2：存储完整的Hexo项目文件

1. 删除```main```分支clone的本地文件（不要删除```.git```），存储希望push到新分支的所有文件。
2. 提交这些更改并推送到GitHub的`development`分支。
	```bash
	git add . 
	git commit -m "Store complete Hexo project" 
	git push origin development
	```

# CI/CD
CI/CD代表持续集成（Continuous Integration）和持续部署（Continuous Deployment）。

- **持续集成（CI）**: 是开发团队经常（通常是每天）将代码更改集成到共享仓库中的实践。之后会自动运行各种测试和其他检查工具。
- **持续部署（CD）**: 是一个自动将代码更改从共享仓库部署到生产环境的过程。

这些工具（如GitHub Actions, Jenkins等）能自动生成静态页面，因为您可以编写脚本或配置文件来自动执行一系列命令。这些命令可以包括安装依赖项、运行测试、构建项目（如使用`hexo generate`生成静态页面）以及将构建结果推送到指定的服务器或分支。这些步骤是自动化的，通常会在您提交代码或合并分支之后立即执行。

使用CI/CD工具，您可以确保代码更改是有效的，并且它们能快速、可靠地被部署到生产环境，减少了人工操作带来的风险和延迟。
## CI/CD原理和安全性

#### 原理：

CI/CD 工具通常运行在一个预配置的环境（通常称为 "Runner" 或 "Agent"）中，这个环境具备执行构建、测试和部署任务所需的所有依赖和权限。当触发某个事件（如代码推送或合并请求）时，这些工具会按照您在配置文件中定义的步骤执行一系列命令。

#### 运行环境：

这些 Runner 或 Agent 可能运行在各种环境中，从完全托管的云实例到您自己的硬件。在 GitHub Actions 的情况下，它们通常运行在 GitHub 托管的虚拟环境（如 `ubuntu-latest` 或 `windows-latest`）中。

#### 安全性：

作为服务提供方，GitHub 采取了多种措施以保证 CI/CD 环境的安全：

1. 隔离：每个工作流程在其自己的环境中运行，与其他工作流程相隔离。
2. 限制权限：默认情况下，Runner 的权限被严格限制。
3. 审计和监控：所有 CI/CD 操作都可以被审计和监控。
4. 密码和密钥管理：使用诸如 `secrets` 的机制，安全地管理敏感信息。

## 配置CI/CD

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
