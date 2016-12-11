---
description: Docker explained in depth
keywords: docker, introduction, documentation, about, technology,  understanding
redirect_from:
- /introduction/understanding-docker/
- /engine/userguide/basics/
- /engine/quickstart.md
- /engine/introduction/understanding-docker/
title: Docker 概述
---

Docker 是一个用于开发、交付和运行应用的开放平台。Docker 允许你把你的应用从基础结构上分离出来，来实现快速的软件交付。使用 Docker，你可以像管理你的应用那样来管理基础环境。利用 Docker 的优势来快速交付、测试和部署代码，可以显著的减少从代码编写到生产环境上线的时间。

## Docker 平台是什么？

Docker 提供了在容器（container）中打包应用并在一个松隔离环境中运行应用的能力。这种隔离性和安全性能够允许你在指定的宿主机上同时运行多个容器。由于容器的轻量级特性而且没有额外的虚拟化管理程序的开销，在相同的硬件配置上，可以比运行虚拟机运行更多个容器。

Docker 提供了工具和平台来管理容器的生命周期：

* 封装应用（和支持组件）到 Docker 容器中。
* 为团队分发和交付这些容器，以便进一步的开发和测试。
* 部署这些容器到生产环境中，可以是本地的数据中心或是云端。

## 什么是 Docker Engine？

_Docker Engine_ 是一个客户端-服务端架构应用，包含如下的几个重要组件：

* 服务器端是一个长时间运行的程序，称其为守护进程（daemon process）
* REST API 是编程接口来告诉守护进程做什么
* 命令行接口（CLI）客户端

![Docker Engine Components Flow](article-img/engine-components-flow.png)

通过脚本或者直接使用CLI命令，命令行接口利用REST API来控制Docker守护进程或与其交互。有许多的 Docker 应用底层就使用了 API 和 CLI。

Docker 守护进程会创建和管理 Docker __objects__ （对象），例如镜像(images)、容器(container)、网络(network) 和 数据卷 (data volomue)。

> **注：** Docker使用 Apache 2.0 许可证下的开源协议

## Docker 能做什么?

*Fast, consistent delivery of your applications*

Docker can streamline the development lifecycle by allowing developers to work in
standardized environments using local containers which provide your applications
and services. You can also integrate Docker into your continuous integration and
continuous deployment (CI/CD) workflow.

Consider the following example scenario. Your developers write code locally and
share their work with their colleagues using Docker containers. They can use
Docker to push their applications into a test environment and execute automated
and manual tests. When developers find problems, they can fix them in the development
environment and redeploy them to the test environment for testing. When testing is
complete, getting the fix to the customer is as simple as pushing the updated image
to the production environment.

*Responsive deployment and scaling*

Docker's container-based platform allows for highly portable workloads. Docker
containers can run on a developer's local host, on physical or virtual machines
in a data center, in the Cloud, or in a mixture of environments.

Docker's portability and lightweight nature also make it easy to dynamically manage
workloads, scaling up or tearing down applications and services as business
needs dictate, in near real time.

*Running more workloads on the same hardware*

Docker is lightweight and fast. It provides a viable, cost-effective alternative
to hypervisor-based virtual machines, allowing you to use more of your compute
capacity to achieve your business goals. This is useful in high density
environments and for small and medium deployments where you need to do more with
fewer resources.

## What is Docker's architecture?
Docker uses a client-server architecture. The Docker *client* talks to the
Docker *daemon*, which does the heavy lifting of building, running, and
distributing your Docker containers. The Docker client and daemon *can*
run on the same system, or you can connect a Docker client to a remote Docker
daemon. The Docker client and daemon communicate using a REST API, over UNIX
sockets or a network interface.

![Docker Architecture Diagram](article-img/architecture.svg)

### The Docker daemon
The Docker daemon runs on a host machine. The user uses the Docker client to
interact with the daemon.

### The Docker client
The Docker client, in the form of the `docker` binary, is the primary user
interface to Docker. It accepts commands and configuration flags from the user and
communicates with a Docker daemon. One client can even communicate with multiple
unrelated daemons.

### Inside Docker
To understand Docker's internals, you need to know about _images_, _registries_,
and _containers_.

#### Docker images

A Docker _image_ is a read-only template with instructions for creating a Docker
container. For example, an image might contain an Ubuntu operating system with
Apache web server and your web application installed. You can build or update
images from scratch or download and use images created by others. An image may be
based on, or may extend, one or more other images. A docker image is described in
text file called a _Dockerfile_, which has a simple, well-defined syntax. For more
details about images, see [How does a Docker image work?](#how-does-a-docker-image-work).

Docker images are the **build** component of Docker.

#### Docker containers
A Docker container is a runnable instance of a Docker image. You can run, start,
stop, move, or delete a container using Docker API or CLI commands. When you run
a container, you can provide configuration metadata such as networking information
or environment variables. Each container is an isolated and secure application
platform, but can be given access to resources running in a different host or
container, as well as persistent storage or databases. For more details about
containers, see [How does a container work?](#how-does-a-container-work).

Docker containers are the **run** component of Docker.

#### Docker registries
A docker registry is a library of images. A registry can be public or private,
and can be on the same server as the Docker daemon or Docker client, or on a
totally separate server. For more details about registries, see
[How does a Docker registry work?](#how-does-a-docker-registry-work)

Docker registries are the **distribution** component of Docker.

#### Docker services
A Docker _service_ allows a _swarm_ of Docker nodes to work together, running a
defined number of instances of a replica task, which is itself a Docker image.
You can specify the number of concurrent replica tasks to run, and the swarm
manager ensures that the load is spread evenly across the worker nodes. To
the consumer, the Docker service appears to be a single application. Docker
Engine supports swarm mode in Docker 1.12 and higher.

Docker services are the **scalability** component of Docker.

### How does a Docker image work?
Docker images are read-only templates from which Docker containers are instantiated.
Each image consists of a series of layers. Docker uses
[union file systems](http://en.wikipedia.org/wiki/UnionFS) to
combine these layers into a single image. Union file systems allow files and
directories of separate file systems, known as branches, to be transparently
overlaid, forming a single coherent file system.

These layers are one of the reasons Docker is so lightweight. When you
change a Docker image, such as when you update an application to a new version,
a new layer is built and replaces only the layer it updates. The other layers
remain intact. To distribute the update, you only need to transfer the updated
layer. Layering speeds up distribution of Docker images. Docker determines which
layers need to be updated at runtime.

An image is defined in a Dockerfile. Every image starts from a base image, such as
`ubuntu`, a base Ubuntu image, or `fedora`, a base Fedora image. You can also use
images of your own as the basis for a new image, for example if you have a base
Apache image you could use this as the base of all your web application images. The
base image is defined using the `FROM` keyword in the dockerfile.

> **Note:** [Docker Hub](https://hub.docker.com) is a public registry and stores
images.

The docker image is built from the base image using a simple, descriptive
set of steps we call *instructions*, which are stored in a `Dockerfile`. Each
instruction creates a new layer in the image. Some examples of Dockerfile
instructions are:

* Specify the base image (`FROM`)
* Specify the maintainer (`MAINTAINER`)
* Run a command (`RUN`)
* Add a file or directory (`ADD`)
* Create an environment variable (`ENV`)
* What process to run when launching a container from this image (`CMD`)

Docker reads this `Dockerfile` when you request a build of
an image, executes the instructions, and returns the image.

### How does a Docker registry work?
A Docker registry stores Docker images. After you build a Docker image, you
can *push* it to a public registry such as [Docker Hub](https://hub.docker.com)
or to a private registry running behind your firewall. You can also search for
existing images and pull them from the registry to a host.

[Docker Hub](http://hub.docker.com) is a public Docker
registry which serves a huge collection of existing images and allows you to
contribute your own. For more information, go to
[Docker Registry](/registry/overview/) and
[Docker Trusted Registry](/docker-trusted-registry/overview/).

[Docker store](http://store.docker.com) allows you to buy and sell Docker images.
For image, you can buy a Docker image containing an application or service from
the software vendor, and use the image to deploy the application into your
testing, staging, and production environments, and upgrade the application by pulling
the new version of the image and redeploying the containers. Docker Store is currently
in private beta.

### How does a container work?
A container uses the host machine's Linux kernel, and consists of any extra files
you add when the image is created, along with metadata associated with the container
at creation or when the container is started. Each container is built from an image.
The image defines the container's contents, which process to run when the container
is launched, and a variety of other configuration details. The Docker image is
read-only. When Docker runs a container from an image, it adds a read-write layer
on top of the image (using a UnionFS as we saw earlier) in which your application
runs.

#### What happens when you run a container?
When you use the `docker run` CLI command or the equivalent API, the Docker Engine
client instructs the Docker daemon to run a container. This example tells the
Docker daemon to run a container using the `ubuntu` Docker image, to remain in
the foreground in interactive mode (`-i`), and to run the `/bin/bash` command.

    $ docker run -i -t ubuntu /bin/bash


When you run this command, Docker Engine does the following:

1. **Pulls the `ubuntu` image:** Docker Engine checks for the presence of the
    `ubuntu` image. If the image already exists locally, Docker Engine uses it for
    the new container. Otherwise, then Docker Engine pulls it from
    [Docker Hub](https://hub.docker.com).

1. **Creates a new container:** Docker uses the image to create a container.

1. **Allocates a filesystem and mounts a read-write _layer_:** The container is
    created in the file system and a read-write layer is added to the image.

1. **Allocates a network / bridge interface:** Creates a network interface that
    allows the Docker container to talk to the local host.

1. **Sets up an IP address:** Finds and attaches an available IP address from a
    pool.

1. **Executes a process that you specify:** Executes the `/bin/bash` executable.

1. **Captures and provides application output:** Connects and logs standard input,
    outputs and errors for you to see how your application is running, because you
    requested interactive mode.

Your container is now running. You can manage and interact with it, use the services
and applications it provides, and eventually stop and remove it.

## The underlying technology
Docker is written in [Go](https://golang.org/) and takes advantage of several
features of the Linux kernel to deliver its functionality.

### Namespaces
Docker uses a technology called `namespaces` to provide the isolated workspace
called the *container*.  When you run a container, Docker creates a set of
*namespaces* for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs
in a separate namespace and its access is limited to that namespace.

Docker Engine uses namespaces such as the following on Linux:

 - **The `pid` namespace:** Process isolation (PID: Process ID).
 - **The `net` namespace:** Managing network interfaces (NET:
 Networking).
 - **The `ipc` namespace:** Managing access to IPC
 resources (IPC: InterProcess Communication).
 - **The `mnt` namespace:** Managing filesystem mount points (MNT: Mount).
 - **The `uts` namespace:** Isolating kernel and version identifiers. (UTS: Unix
Timesharing System).

### Control groups
Docker Engine on Linux also relies on another technology called _control groups_
(`cgroups`). A cgroup limits an application to a specific set of resources.
Control groups allow Docker Engine to share available hardware resources to
containers and optionally enforce limits and constraints. For example,
you can limit the memory available to a specific container.

### Union file systems
Union file systems, or UnionFS, are file systems that operate by creating layers,
making them very lightweight and fast. Docker Engine uses UnionFS to provide
the building blocks for containers. Docker Engine can use multiple UnionFS variants,
including AUFS, btrfs, vfs, and DeviceMapper.

### Container format
Docker Engine combines the namespaces, control groups, and UnionFS into a wrapper
called a container format. The default container format is `libcontainer`. In
the future, Docker may support other container formats by integrating with
technologies such as BSD Jails or Solaris Zones.

## 进一步学习
- Read about [Installing Docker Engine](installation/index.md#installation).
- Get hands-on experience with the [Get Started With Docker](getstarted/index.md)
    tutorial.
- Check out examples and deep dive topics in the
    [Docker Engine User Guide](userguide/index.md).


