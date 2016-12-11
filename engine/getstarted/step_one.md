---
description: Docker 入门
keywords: beginner, getting started, Docker, install
redirect_from:
- /mac/step_one/
- /windows/step_one/
- /linux/step_one/
title: 安装 Docker 并运行 hello-world
---

- [第一步：获取 Docker](#step-1-get-docker)
- [第二步：安装 Docker](#step-2-install-docker)
- [第三步：验证安装](#step-3-verify-your-installation)

## 第一步：获取 Docker

### Docker for Mac

Docker for Mac 是在 Mac 上的最新选择。它以原生 Mac 应用方式运行并使用 <a href="https://github.com/mist64/xhyve/" target="_blank">xhyve</a> 虚拟化技术来支持 Docker Engine 环境和 Docker 守护进程所需 Linux 内核相关特性。

<a class="button" href="https://download.docker.com/mac/stable/Docker.dmg">获取 Docker for Mac</a>

**需求**

- Mac 必须是 2010 年或之后型号，包含Intel的内存管理单元（memory management unit - MMU）虚拟化的硬件支持; 比如, 扩展页面表（Extended Page Tables - EPT）

- macOS 10.10.3 Yosemite 或更高版本

- 至少 4GB 内存

- 早于 4.3.30 版本的 VirtualBox 必须没有被安装（与 Docker for Mac 不兼容）。 否则 Docker for Mac 会在安装时报错。卸载老版本 VirtualBox 并重试安装。

#### Docker Toolbox 的 Mac 版本 

如果你所拥有的早期 Mac 无法满足 Docker for Mac 的先决条件， 请<a href="https://www.docker.com/products/docker-toolbox" target="_blank">获取 Docker Toolbox</a> 。

参见 [Docker Toolbox 简介](/toolbox/overview.md) 来安装 Docker 。

### Docker for Windows

Docker for Mac 是在 PC 上的最新选择。它以原生 Windows 应用方式运行并使用  Hyper-V 虚拟化技术来支持 Docker Engine 环境和 Docker 守护进程所需 Linux 内核相关特性。

<a class="button" href="https://download.docker.com/win/stable/InstallDocker.msi">获取 Docker for Windows</a>

**需求**

* 64位 Windows 10 专业版, 企业版和教育版（1511 November update, Build 10586 或更高版本）。 在未来会支持更多的 Windows 10 版本。

* Hyper-V 支持必须开启。如果需要，Docker for Windows 安装程序会自动开启相应支持。（需要重启）

#### Docker Toolbox 的 Windows 版本 

如果你所拥有的 Windows 系统无法满足 Docker for Windows 的先决条件， 请<a href="https://www.docker.com/products/docker-toolbox" target="_blank">获取 Docker Toolbox</a> 。

参见 [Docker Toolbox 简介](/toolbox/overview.md) 来安装 Docker 。

### Docker for Linux
Docker Engine 可以在 Linux 发行版上原生运行.

关于在不同 Linux 发行版上支持 Docker 的详细描述, 参见 [安装 Docker Engine](/engine/installation/index.md).

## 第二步：安装 Docker

- **Docker for Mac** - 参见 [Docker for Mac 指南](/docker-for-mac/)中安装介绍。

- **Docker for Windows** - 参见 [Docker for Windows 指南](/docker-for-windows/)中安装介绍。

- **Docker Toolbox** - 参见 [Docker Toolbox Overview 指南](/toolbox/overview.md)中安装介绍。

- **Docker on Linux** - 这个指南通过简单示例介绍了在 Ubuntu Linux 上安装 Docker。参见 [在 Ubuntu Linux 上安装 Docker（示例）](linux_install_help.md)。在所有支持的 Linux 发行版上的详细安装介绍在 [安装 Docker Engine](/engine/installation/index.md).

## 第三步：验证安装

1. 打开命令行终端，运行一些 Docker 命令来验证 Docker 工作正确。

    一些建议的命令可以来尝试：`docker version` 来检查是否已安装最新版本， `docker ps` 来检查是否有正在运行中的容器。（因为刚启动，很可能没有运行中容器）

2. 键入 `docker run hello-world` 命令并按回车键。

    如果一切运行正常，命令的执行结果输出如下：


        $ docker run hello-world
        Unable to find image 'hello-world:latest' locally
        latest: Pulling from library/hello-world
        535020c3e8ad: Pull complete
        af340544ed62: Pull complete
        Digest: sha256:a68868bfe696c00866942e8f5ca39e3e31b79c1e50feaee4ce5e28df2f051d5c
        Status: Downloaded newer image for hello-world:latest

        Hello from Docker.
        This message shows that your installation appears to be working correctly.

        To generate this message, Docker took the following steps:
        1. The Docker Engine CLI client contacted the Docker Engine daemon.
        2. The Docker Engine daemon pulled the "hello-world" image from the Docker Hub.
        3. The Docker Engine daemon created a new container from that image which runs the
           executable that produces the output you are currently reading.
        4. The Docker Engine daemon streamed that output to the Docker Engine CLI client, which sent it
           to your terminal.

        To try something more ambitious, you can run an Ubuntu container with:
        $ docker run -it ubuntu bash

        Share images, automate workflows, and more with a free Docker Hub account:
        https://hub.docker.com

        For more examples and ideas, visit:
        https://docs.docker.com/userguide/

3. 运行 `docker ps -a` 命令来展现系统中所有的容器

        $ docker ps -a

        CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
        592376ff3eb8        hello-world         "/hello"            25 seconds ago      Exited (0) 24 seconds ago                       prickly_wozniak

    你可以发现 `hello-world` 容器出现 `docker ps -a` 命令的输出列表中.

    `docker ps` 命令只会显示当前运行中的容器。由于 `hello-world` 已经运行并退出，它不会在 `docker ps` 的结果中出现。

## 寻求排错帮助?

一般而言，在开箱即用的情况下上面的步骤会被正确执行。但是有一些场景可能引起错误。如果你的 `docker run hello-world` 命令无法正常工作并引发错误，请查看 [故障排查](/toolbox/faqs/troubleshoot.md) 来快速修复常见问题。

## 下一步

现在，你已经成功地安装了 Docker。请保持 Docker Quickstart Terminal 窗口打开。现在请前往下一页 [阅读Docker镜像和容器的简单介绍](step_two.md).

&nbsp;


