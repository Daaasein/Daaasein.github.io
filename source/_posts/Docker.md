---
title: Docker
date: 2023-09-29 00:37:44
category: Skills
tags:
description: "Docker 是一个开源的应用容器引擎，用于容器化应用并自动化部署。"
---

## Introduction

Docker 是一个开源的应用容器引擎，用于容器化应用并自动化部署。

## Basic

### 镜像Image

#### Pull

```bash
docker pull <image-name>:<tag>
```
- `<image-name>`: 镜像的名称
- `<tag>`: 镜像的版本标签，通常默认为 `latest`，可以省略。

示例：
```bash
docker pull ubuntu:latest
```

#### Remove

```bash
docker image rm <image-name or id>:<tag>
```

#### List

```bash
docker images
```

or

```bash
docker image ls
```


### 容器Container

#### Run

```bash
docker run <options> <image-name>:<tag>
```
示例：
```bash
docker run -it ubuntu:latest /bin/bash
```

- `-i` 或 `--interactive`: 保持标准输入（STDIN）打开，即使不附加到容器的终端。这让你可以与容器进行交互。
- `-t` 或 `--tty`: 分配一个伪终端（pseudo-TTY）。这模拟一个终端，使得你可以运行需要终端的程序，如 Bash。
- `/bin/bash`: 这是当容器启动后要运行的命令。启动一个 Bash shell。

#### Stop

```bash
docker stop <container-name or id>
```

#### List

```bash
docker ps
```

or

```bash
docker ps -a # include unused
```

#### Show log

```bash
docker logs <container-name or id>
```

### 数据持久化

您可以通过挂载卷（Volumes）或宿主机目录（Bind Mounts）来实现更新镜像和容器时保留数据，但挂载操作只能在创建container时进行。同一个 Volume或Mount可以被挂载到多个容器上，但并发读写可能导致数据不一致。

#### Volumes

- **创建volume**：
  
  ```bash
	docker volume create my-volume
  ```
  
- **运行时挂载volume**：
  
  ```bash
  docker run -v my-volume:/path/in/container my-image
	```

#### Bind Mounts

- **运行时挂载mount**：
  
  ```bash
  docker run -v /path/on/host:/path/in/container my-image
  ```

#### Copy files

```bash
docker cp <source-path> <container>:<destination-path> #从主机复制到container
```

or

```bash
docker cp <container>:<source-path> <destination-path> #从container复制到主机
```

## Containerize your application

1. run in your project folder：

   ```bash
   docker init
   ```

   Then select a language and set sendible defaults......Dockerfile and compose.yaml will be created.

2. read [Dockerfile reference⁠](https://docs.docker.com/engine/reference/builder/) and [Compose file reference⁠](https://docs.docker.com/compose/compose-file/) ~

### Dockerfile

Dockerfile is used to define image.

[Dockerfile reference⁠](https://docs.docker.com/engine/reference/builder/)

### Compose file

compose.yaml in root dir of project tells Docker how to run applications. 

[Compose file reference⁠](https://docs.docker.com/compose/compose-file/)

It can be used to define:

- **Service Dependencies**: You can define dependencies between containers, ensuring they start up in the correct order and configuration.
- **Networking and Storage**: You can define private networks and storage volumes, enabling efficient communication and data persistence between containers.
- **Resource Limits**: You can set CPU, memory, and other resource limits for each service (container).
- **Scaling and Load Balancing**: You can easily scale services to multiple instances and distribute traffic through load balancing.

#### Details

1. **版本（`version`）**: 指定 Docker Compose 文件的版本。版本决定了你能使用哪些字段和设置。

   ```bash
   version: '3'
   ```

2. **服务（`services`）**: 描述应用程序的各个部分（即容器），包括它们的镜像、端口、挂载卷等。

   ```bash
   services:
     web:
       image: nginx:latest
       volumes:
         - my_volume:/path/in/container	 #use volume
         - /path/on/host:/path/in/container #bind mounts, will overwrite the contents of the container's path
         - /usr/src/app/node_modules		 #prevent the bind mount from overwriting the container's path
       ports:
         - "<host-port>:<container-port>"
   ```

3. **网络（`networks`）**: 定义私有网络，容器可以加入这些网络进行相互通信。

   ```bash
   networks:
     my_network:
       driver: bridge
   ```

4. **卷（`volumes`）**: 用于数据持久化的存储卷。

   ```bash
   volumes:
     my_volume:
       driver: local
   ```

#### Run

1. run in your project folder：

   ```bash
   docker compose up -d
   ```

   - ```-d``` or ```--detached```: Run in detached mode, which will not print output to host terminal.
   
   **In this way, image will be built, container will be created and run by following compose.yaml rules.**

#### Stop

1. run in your project folder：
	```bash
	docker compose down
	```

## Publish image

### 1.Build image

run in your project folder：

```bash
docker build -t <YOUR-Image-Name> .	#don't forget the '.'
```
- ```-t```: give ```name:tag``` to your image. The default tag is “latest”
- ```.```: let Docker know where it can find the Dockerfile

**In this way, image will just be built but doesn’t run.**
### 2.sign in to Docker

### 3.rename your image

```bash
docker tag <YOUR-Image-Name> YOUR-Docker-ID/YOUR-Image-Name
```
### 4.push your image to Docker Hub

Go to the **Images** tab, find your image, click **Actions**, select **Push to Hub**.

Go to [Docker Hub⁠](https://hub.docker.com/) and you should see the welcome-to-docker repository.
