---
title: Chocolatey
date: 2023-10-02 00:37:59
category: Skills
tags:
description: "Chocolatey是适用于Windows的包管理工具，类似Mac下的Homebrew，Linux下的APT等。"
---

Chocolatey是适用于Windows的包管理工具，类似Mac下的Homebrew，Linux下的APT等。
# Install
在Windows下安装node.js时，会同时安装npm和chocolatey（可选）。

在Windows系统下安装Chocolatey通常通过PowerShell脚本来完成，以下是一般的安装步骤：

## 准备工作

1. 确保你的系统拥有.NET Framework 4.0或更高版本。
2. 获得管理员权限。由于安装Chocolatey会更改系统设置和安装软件，因此需要管理员权限。

## 安装步骤

1. **打开PowerShell**：点击开始菜单，搜索“PowerShell”，然后以管理员身份运行Windows PowerShell。

2. **配置执行策略**（可选，但推荐）：为了能运行安装脚本，你可能需要改变PowerShell的执行策略。在PowerShell窗口中，运行以下命令：

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```
    该命令的意思是：在当前PowerShell进程中，设置执行策略为“Bypass”，从而允许所有脚本和配置文件的执行。这个操作需要管理员权限来执行。
    
	这个命令的组成部分解释如下：
	
	- `Set-ExecutionPolicy`：这是一个cmdlet（PowerShell命令），用于设置新的执行策略。
	
	- `Bypass`：这是你想设置的执行策略级别。"Bypass"表示绕过执行策略，允许所有脚本执行。这是一个非常宽松的策略，应谨慎使用。
	
	- `-Scope`：这个参数用于指定执行策略的作用范围。PowerShell允许你为不同的作用范围设置不同的执行策略。
	
	- `Process`：这指定了`-Scope`参数的值。"Process"表示这个执行策略仅适用于当前PowerShell进程。这意味着更改只会影响当前运行的PowerShell会话，不会影响其他用户或以后的会话。也可以改为LocalMachine或CurrentUser。
	要查看当前的执行策略设置，您可以运行：
	```bash
	Get-ExecutionPolicy -List
	```

3. **运行安装命令**：在PowerShell窗口中，复制并粘贴以下命令，然后回车。

    ```powershell
    iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
    ```

    这个命令会从Chocolatey网站下载一个PowerShell脚本，并立即执行它。这个脚本会下载并安装Chocolatey，以及所有必要的依赖项。

4. **检查安装**：安装完成后，在新的PowerShell窗口中运行：

    ```powershell
    choco -v
    ```

    如果返回Chocolatey的版本号，说明安装成功。

5. **关闭并重新打开PowerShell窗口**：这是为了确保所有的环境变量设置都已正确应用。

现在，你应该已经成功安装了Chocolatey，接下来你可以使用`choco install`命令来安装各种软件包，如Hugo。注意，每次使用`choco install`命令时，通常都需要管理员权限。
