---
title: Setup a new Mac
date: 2023-09-30 00:25:45
category: Skills
tags:
description: "本文展示了结合Homebrew,iCloud,Github等工具，设置一台新Mac的基本操作流程。"
---

# 应用程序已损坏
1. 允许任意来源的APP：
	1. 打开终端
	```zsh
	sudo spctl --master-disable
	```
	2. System Settings->Privacy & Security->Allow applications download from anywhere
2. 移除应用的安全隔离属性：
```zsh
sudo xattr -dr com.apple.quarantine [应用程序位置]
```

# 文件存储思路
- **资源类文件**存储在`国内网盘`，如：破解软件、游戏、电影。
- **有同步需求的文件**存储在`iCloud`，如：笔记文件、zsh配置文件、Homebrew配置文件。
- 接着，在iCloud的Documents目录创建一个`Git仓库`，连接到Github的private仓库，推送文件和版本记录。

这样的话，不仅笔记文件有了iCloud+Github双重保险，迁移到新Mac时也能依赖iCloud同步获得zsh和homebrew配置文件，快速配置新Mac。
# iCloud设置
在iCloud设置中开启选项“将桌面文件和文稿保存到iCloud”（大概名称）
开启后，`~/Documents`目录将会通过iCloud同步，将笔记文件放在这里。

## Zsh 配置

1. **将 `~/.zshrc` 文件复制到`~/Documents`目录下：** 这个文件包含了您的 Zsh 配置。

	```bash
	cp ~/.zshrc ~/Documents
	# cp：copy复制，默认覆盖同名文件，可以通过-i选项要求覆盖前提示
	```
## Homebrew 配置
Homebrew适用于MacOS和Linux。

1. **导出 Homebrew 包列表到`~/Documents`目录下：** 使用 `brew bundle dump` 命令，您可以将当前 Homebrew 安装的所有包、casks 和其他信息导出到一个 `Brewfile`。

    ```bash
    brew bundle dump -f --file="~/Documents/Brewfile"  
    # 将配置文件Brewfile导出到iCloud下的Documents目录内
    # -f：force强制，有同名文件则覆盖
    ```

# Git配置
## 远程仓库身份验证
Git连接远程仓库需要身份验证，直接使用Github账户和密码验证的方式已被弃用，目前官方指南有两种方式：`GitHub CLI` 或 `Git 凭据管理器（GCM）`。
### Github CLI
当您选择 `HTTPS` 作为 Git 操作的首选协议时，GitHub CLI 将自动为您存储 Git 凭据，并对询问您是否要使用 GitHub 凭据向 Git 进行身份验证的提示回答“是”。

1. [Install](https://github.com/cli/cli#installation) GitHub CLI on macOS, Windows, or Linux.  
    在 macOS、Windows 或 Linux 上安装 GitHub CLI。
    
2. In the command line, enter `gh auth login`, then follow the prompts.  
    在命令行中，输入 `gh auth login` ，然后按照提示操作。
    
    - When prompted for your preferred protocol for Git operations, select `HTTPS`.  
        当系统提示您输入 Git 操作的首选协议时，请选择 `HTTPS` 。
    - When asked if you would like to authenticate to Git with your GitHub credentials, enter `Y`.  
        当系统询问您是否要使用 GitHub 凭据向 Git 进行身份验证时，请输入 `Y` 。
### GCM（推荐）
Git 凭据管理器 （GCM） 是另一种安全存储凭据并通过 HTTPS 连接到 GitHub 的方法。使用 GCM，您无需手动创建和存储个人访问令牌，因为 GCM 代表您管理身份验证，包括 2FA（双因素身份验证）。

1. Install Git using [Homebrew](https://brew.sh/):  
    使用 Homebrew 安装 Git：
    
    ```bash
    brew install git
    ```
    
2. Install GCM using Homebrew:  
    使用自制软件安装 GCM：
    
    ```bash
    brew install --cask git-credential-manager
    ```
    
    For MacOS, you don't need to run `git config` because GCM automatically configures Git for you.  
    对于 MacOS，您无需运行 `git config` ，因为 GCM 会自动为您配置 Git。

The next time you clone an HTTPS URL that requires authentication, Git will prompt you to log in using a browser window. You may first be asked to authorize an OAuth app. If your account or organization requires [two-factor auth](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa), you'll also need to complete the 2FA challenge.  
下次克隆需要身份验证的 HTTPS URL 时，Git 将提示您使用浏览器窗口登录。系统可能会首先要求您授权 OAuth 应用程序。如果您的帐户或组织需要双重身份验证，您还需要完成 2FA 挑战。

Once you've authenticated successfully, your credentials are stored in the macOS keychain and will be used every time you clone an HTTPS URL. Git will not require you to type your credentials in the command line again unless you change your credentials.  
成功进行身份验证后，您的凭据将存储在 macOS 钥匙串中，并在每次克隆 HTTPS URL 时使用。Git 不会要求您再次在命令行中键入凭据，除非您更改凭据。


## 创建仓库

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
### 创建忽略规则
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

### 从远程仓库移除文件夹
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

## 连接远程仓库

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

# 迁移到新设备

当您需要在新的 Mac 上恢复配置时：

1. **克隆仓库：** 在新的 Mac 上克隆您之前创建的远程仓库。

    ```bash
    git clone <your-remote-repository-url>
    ```

2. **恢复 Zsh 配置：** 将 `~/.zshrc` 替换为仓库中保存的文件。

    ```bash
    cp path/to/cloned/repo/.zshrc ~/.zshrc
    # path/to/cloned/repo：指克隆下来的仓库目录，根据具体情况填写
    ```

3. **恢复 Homebrew 配置：** 使用 `brew bundle` 命令来安装所有您之前保存在 `Brewfile` 中的包。

    ```bash
    brew bundle --file=path/to/cloned/repo/Brewfile
    # path/to/cloned/repo：指克隆下来的仓库目录，根据具体情况填写
    ```

这样，您就完成了 Zsh 和 Homebrew 配置的保存和恢复。这种方法便于版本控制，也便于您在多个 Mac 设备间同步配置。
