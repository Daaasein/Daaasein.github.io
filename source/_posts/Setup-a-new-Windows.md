---
title: Setup a new Windows
date: 2023-09-30 00:31:52
category: Skills
tags:
description: "本文展示了设置PowerShell代理，安装WSL2并设置Zsh为默认shell，以及APT的基本使用。这些可以作为新手设置一台新Windows的基本操作流程。"
---

# Shell
## Proxy

### Set by Shell (only one time)

```shell
# Windows is case-insensitive, but capital is readable.
set HTTP_PROXY=http://proxy.server.com:port/  
set HTTPS_PROXY=http://proxy.server.com:port/
```

### Set by System env (forever)
1. Set system or user env variables:
	- Variable：`HTTP_PROXY` and ```HTTPS_PROXY```, separately.
	- Value： `http://proxy.server.com:port/`
2. Restart Shell.

### Set by User env using Batch file (forever)
1. Create a `.bat` file, such as ```set_proxy.bat```

2. Enter and save:
	```bat
	setx HTTP_PROXY http://proxy.server.com:port/
	setx HTTPS_PROXY http://proxy.server.com:port/
	```

3. Double click to run. It will set proxy as user env.

# WSL 2

## Install

1. Open powershell as an admin

2. Install
	```bash
	wsl --install
	```
	or
	```bash
	wsl --install -d <DistroName>	# specify Linux distributions
	```
	Can use ```wsl --list --online``` to show available Linux distributions.

3. Check WSL version
	```bash
	wsl --list --verbose
	```
	If don’t use `--verbose` , it will only list Linux distributions but will not display the WSL version.

## Command
### Start WSL with ~ dir

```bash
wsl ~
```

### Update WSL

```bash
wsl --update
```

### Run as a specific user
```bash
wsl -u <Username>
```
- ```-u```: --user

### Set default WSL version

 ```bash
wsl --setdefault <DistributionName> # when use wsl command
 ```

## Dir

### Linux user dir
- `\\wsl$\<DistroName>\home\<UserName>` in Windows
- ```/home/<UserName>``` in Linux.
### Windows user dir
- `C:\Users\<UserName>` in  Windows
- `/mnt/c/Users/<UserName>` in Linux.

# Linux

## Set zsh as default shell

1. Check which shell is using:
	```bash
	# will print present shell, and it is deault shell if there is '-' prefix.
	bash
	Copy code
	echo $0
	```

2. Check if zsh is already installed:
	```bash
	which zsh  # if no return, no zsh currently
	```

3. Install zsh:

	```bash
	sudo apt install zsh
	```

4. Set zsh as default:

	```bash
	chsh -s $(which zsh)
	```
   - ```chsh```: change shell.
   - ```-s```: claim it is a shell.
   - ```$(which zsh)```: run ```which zsh``` and replace the result here.

5. Modify ```~/.zshrc``` to make it act like bash in WSL：

   ```zsh
	   # Check if Terminal support color
	   case "$TERM" in
       xterm-color|*-256color) color_prompt=yes;;
	   esac
	   
	   # Set prompt
	   if [ "$color_prompt" = "yes" ]; then
       PROMPT='%F{green}%n@%m%f:%F{blue}%~%f%# '
	   else
       PROMPT='%n@%m:%~%# '
	   fi
	```

	Then run ```source ~/.zshrc```.

## APT Command
APT is for Linux, generally as default package tool in Ubuntu (one of Linux distribution).
### Update

Update packages list && Update packages：

```bash
sudo apt update  # keep list latest
sudo apt upgrade  # indeed update packages
```

### Install

```bash
sudo apt install <package-name>
```

### Remove

1. Remove package:
	```bash
	sudo apt remove <package-name>  # remove but keep configurations
	```
	or
	```bash
	sudo apt purge <package-name>  # remove configurations at the same time
	```

2. Remove  unused dependency package:

   ```bash
   sudo apt autoremove
   ```

3. update packages list (optional)

   ```bash
   sudo apt update
   ```

   
# AutoHotkey
## Open PowerShell
```ahk
^Space:: ; Ctrl + Space
{
    Run, powershell.exe
}
return
```
