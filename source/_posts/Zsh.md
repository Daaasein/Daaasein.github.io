---
title: Zsh
date: 2023-09-30 00:37:19
category: Skills
tags:
description: "展示Zsh设置代理、清除代理缓存、检查当前代理的方式。"
---

# Proxy
## 配置
在```~/.zshrc```中配置如下：
```bash
# Use clash proxy 
export http_proxy=http://192.168.5.9:7890   #根据本机ip地址和端口号填写代理设置
export https_proxy=http://192.168.5.9:7890  #注意，虽然前面是https，但后面仍是http
```
## 问题
### 为什么前面是```https```，后面仍然是```http```?
	通信路径：客户端——代理服务器——目标服务器
	
	在代理设置中，`http_proxy` 和 `https_proxy` 环境变量的名称分别用于指导客户端的 HTTP 和 HTTPS 连接如何使用代理。这两个变量只是告诉客户端（比如，一个Web浏览器或命令行工具）如何路由不同类型的流量（HTTP或HTTPS）。
	
	也就是说，尽管 `https_proxy` 用于客户端的 HTTPS 连接，但它可以使用 HTTP 协议与代理服务器通信。通常，代理服务器会处理与目标服务器之间的实际加密，而客户端与代理服务器之间可能不需要加密。

### 为什么注释并重启```zsh```后，仍然使用代理？
像这样注释后，重启```zsh```或```source ~/.zshrc```，仍然使用代理怎么办？
```bash
# Use clash proxy 
# export http_proxy=http://192.168.5.9:7890 
# export https_proxy=http://192.168.5.9:7890
```
1. 先检查是否有代理：
	```bash
	env | grep -i proxy  #有输出就是有代理，没输出就是没代理。
	```
2. 有代理则清除缓存：
	```bash
	 unset http_proxy https_proxy
	```
3. 再次检查是否有代理：
	```bash
	env | grep -i proxy
	```
