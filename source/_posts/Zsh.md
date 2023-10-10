---
title: Zsh
date: 2023-09-30 00:37:19
category: Skills
tags:
description: "展示Zsh设置代理、清除代理缓存、检查当前代理的方式。"
---

# Proxy
## 语法
打开Clash的Allow LAN，然后在```~/.zshrc```中配置如下：
```bash
# Use clash proxy 
export http_proxy=http://192.168.5.9:7890   #根据本机ip地址和端口号填写代理设置
export https_proxy=http://192.168.5.9:7890  #注意，虽然前面是https，但后面仍是http
```

## 自动根据网络使用不同proxy
打开Clash的Allow LAN，然后在```~/.zshrc```中配置如下：
```zsh
# 自动检测网络环境并设置代理
if ping -c 1 -W 1 192.168.0.1 > /dev/null 2>&1; then
    # 如果能ping通家里的某个设备或者IP，我们认为处于家庭网络
    export http_proxy="http://proxy.home.com:8080"
    export https_proxy="http://proxy.home.com:8080"
elif ping -c 1 -W 1 10.0.0.1 > /dev/null 2>&1; then
    # 如果能ping通公司的某个设备或者IP，我们认为处于公司网络
    export http_proxy="http://proxy.company.com:8080"
    export https_proxy="http://proxy.company.com:8080"
else
    unset http_proxy
    unset https_proxy
fi
```
### 参数解释

- **`-c 1`:** 这里的`-c`是“count”的缩写，意味着发送的ICMP数据包数量。`1`指的是发送一个数据包。
  
- **`-W 1`:** 这里的`-W`是“timeout”的缩写，表示等待响应的最长时间（以秒为单位）。`1`意味着如果1秒内没有收到响应，`ping`命令就会终止。
### 什么是ICMP包？

ICMP（Internet Control Message Protocol，因特网控制报文协议）是一个用于在IP主机和路由器之间传递控制消息的网络层协议。`ping`命令使用ICMP Echo Request和Echo Reply消息来检查两台计算机之间的连通性。
### 重定向解释
在Unix和Unix-like操作系统中（这包括Linux和macOS），每一个运行的进程都有一组文件描述符。文件描述符是非负整数，用于访问或者标识特定的文件或输入/输出资源。其中，有三个文件描述符特别重要：

- **0**: 标准输入（stdin）
- **1**: 标准输出（stdout）
- **2**: 标准错误（stderr）

在重定向语法中，`>` 默认操作的是标准输出（即文件描述符 1）。例如，`> /dev/null` 相当于 `1> /dev/null`。

因此：
**```> /dev/null 2>&1```:**
    
- `> /dev/null`: 这部分将标准输出（stdout）重定向到`/dev/null`。`/dev/null`是一个特殊的文件，写入到这个文件的数据会被操作系统丢弃。这在实际应用中等同于“忽略输出”。
  
- `2>&1`: 这部分将标准错误（stderr）重定向到标准输出（stdout）。由于标准输出已经被重定向到`/dev/null`，这意味着标准错误也会被同样处理——即被丢弃。

组合在一起，`> /dev/null 2>&1`意味着“忽略所有输出和错误”。这在脚本中通常用于当你不关心命令的输出或错误信息时。

这就是为什么经常看到 `> /dev/null 2>&1` 的组合：它会将所有输出和错误信息都导向到 `/dev/null`，实际上就是忽略这些信息。


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
