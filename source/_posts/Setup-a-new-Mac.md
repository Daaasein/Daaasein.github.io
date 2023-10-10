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
建议使用```oh-my-zsh```，它提供了许多主题、自动补全插件等：[Oh My Zsh](https://ohmyz.sh/)，甚至可以每次打开Terminal时显示宝可梦每日一句：[possatti/pokemonsay(github.com)](https://github.com/possatti/pokemonsay)
一些高级配置，如设置Proxy（Terminal默认不使用系统代理），可以参考：[[Zsh]]

1. **将 `~/.zshrc` 文件复制到`~/Documents`目录下：** 这个文件包含了您的 Zsh 配置。

	```bash
	cp ~/.zshrc ~/Documents
	# cp：copy复制，默认覆盖同名文件，可以通过-i选项要求覆盖前提示
	```
## Homebrew 配置
Homebrew适用于MacOS和Linux: [Homebrew — The Missing Package Manager for macOS (or Linux)](https://brew.sh/)

1. **导出 Homebrew 包列表到`~/Documents`目录下：** 使用 `brew bundle dump` 命令，您可以将当前 Homebrew 安装的所有包、casks 和其他信息导出到一个 `Brewfile`。

    ```bash
    brew bundle dump -f --file="~/Documents/Brewfile"  
    # 将配置文件Brewfile导出到iCloud下的Documents目录内
    # -f：force强制，有同名文件则覆盖
    ```

# Git配置
## 远程仓库身份验证
![[Git#GCM（推荐）]]
## 创建仓库
![[Git#创建仓库]]
## 连接远程仓库
![[Git#连接远程仓库]]
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
