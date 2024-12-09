```markdown
---
title: 初学者的 Docker 和容器简介
date: 2024-12-09T07:59:17.223Z
author: Kedar Makode
authorURL: https://www.freecodecamp.org/news/author/Kedar98/
originalURL: https://www.freecodecamp.org/news/an-introduction-to-docker-and-containers-for-beginners/
posteditor: ""
proofreader: ""
---

在现代软件开发的世界中，效率和一致性是关键。开发人员和运维团队需要能够帮助他们无缝管理、部署和运行跨不同环境的应用程序的解决方案。

<!-- more -->

容器和 Docker 是彻底改变软件构建、测试和部署的技术。

无论您是技术新手还是只想了解 Docker 的基础知识，本文将引导您了解基本要素。

## 内容目录

-   [什么是容器？][1]
    
-   [什么是 Docker？][2]
    
-   [为什么选择 Docker？][3]
    
-   [Docker 架构][4]
    
-   [Docker 的容器运行时：containerd][5]
    
-   [如何使用 Docker 创建一个简单的容器][6]
    
-   [总结][7]
    

## 什么是容器？

在深入了解 Docker 之前，让我们首先了解容器。想象一下，你正在进行一个项目，你的应用程序在你的笔记本电脑上完美运行。但当你尝试在另一台机器上运行相同的应用程序时，它失败了。这通常是由于环境之间的差异：不同的操作系统、安装的软件版本或配置。

容器通过将应用程序及其所有依赖项（如库、框架和配置文件）打包成单个标准化的单元来解决这个问题。这确保了无论应用程序部署在哪里——无论是在你的笔记本电脑上、服务器上还是在云中，它都能以相同的方式运行。

容器的关键特性：

-   **轻量级**：容器共享主机系统的内核，不像虚拟机（VM）需要单独的操作系统实例，使其更快、更高效。
    
-   **可移植**：一旦构建，容器可以在各种环境中一致运行。
    
-   **隔离**：容器在隔离的进程中运行，这意味着它们不会干扰在同一系统上运行的其他应用程序。
    

## 什么是 Docker？

现在我们了解了容器，让我们谈谈 Docker，这是一个使容器技术成为主流的平台。

Docker 是一个开源工具，旨在简化创建、管理和部署容器的过程。自 2013 年推出以来，Docker 迅速成为容器化的首选解决方案，因为它易于使用、社区支持强大并拥有强大的工具生态系统。

### Docker 的关键概念

1.  **Docker 镜像**：可以将 Docker 镜像视为容器的蓝图。它包含运行应用程序所需的一切，包括代码、库和系统依赖。镜像是根据 Dockerfile 中编写的一组指令构建的。
    
2.  **Docker 容器**：容器是 Docker 镜像的运行实例。当您创建并启动一个容器时，Docker 将镜像启动到一个隔离的环境中，在那里您的应用程序可以运行。
    
3.  **Dockerfile**：这是一个文本文件，包含创建 Docker 镜像所需的步骤。它用于定义您的容器的外观，包括基础镜像、应用程序代码和任何附加的依赖。
    
4.  **Docker Hub**：Docker Hub 是一个公共注册表，开发人员可以在其中共享和访问预构建的镜像。如果您正在处理一个常见的应用程序或技术栈，那么很可能在 Docker Hub 上已经有现成的镜像，可以节省您的时间。
    
5.  **Docker Compose**：对于需要多个容器的应用程序（例如，一个网站服务器和一个数据库），Docker Compose 允许您使用简单的 YAML 文件定义和管理多容器环境。
    

## 为什么选择 Docker？

Docker 的流行源于它能够解决开发人员如今面临的多种挑战：

-   **跨环境的一致性**：开发者可以“构建一次，到处运行”，确保相同的应用程序在不同的环境中以相同的方式工作，无论是本地开发还是生产。
    
-   **速度**：Docker 容器启动和停止迅速，成为测试和部署管道的理想选择。
    
-   **资源的高效使用**：由于容器比虚拟机更有效地共享主机系统的资源，它们减少了开销并允许更高密度的部署。
    
-   **应用程序的版本控制**：Docker 允许您不仅对代码进行版本控制，还包括运行代码的环境。这在回滚到以前的版本或调试生产环境中的问题时特别有用。
    

## Docker 架构

当您第一次开始使用 Docker 时，您可能会把它视作一个“即插即用”的工具。虽然这样可以帮助您上手，但深入了解 Docker 的架构将帮助您排除问题、优化性能，并在您的容器化策略中做出明智的决策。

Docker 的架构旨在确保效率、灵活性和可扩展性。它由多个组件组成，这些组件共同协作以创建、管理和运行容器。让我们更详细地了解每个组件。
```

Docker 的架构基于一个客户端-服务器模型，包括以下组件

-   **Docker 客户端**
    
-   **Docker 守护进程（dockerd）**
    
-   **Docker 引擎**
    
-   **Docker 镜像**
    
-   **Docker 容器**
    
-   **Docker 注册表**
    

![Docker 架构](https://docs.docker.com/get-started/images/docker-architecture.webp)

#### 1. Docker 客户端

Docker 客户端是用户与 Docker 交互的主要方式。它是一个命令行工具，通过 REST API 向 Docker 守护进程（我们将在下文中介绍）发送指令。`docker build`、`docker pull` 和 `docker run` 等命令均从 Docker 客户端执行。

当你输入像 `docker run nginx` 这样的命令时，Docker 客户端将其转换为 Docker 守护进程可以理解并执行的请求。基本上，Docker 客户端充当与 Docker 复杂后台组件交互的前端。

#### 2. Docker 守护进程（dockerd）

Docker 守护进程，也称为 **dockerd**，是整个 Docker 操作的大脑。它是一个后台进程，监听来自 Docker 客户端的请求，管理 Docker 对象如容器、镜像、网络和卷。

Docker 守护进程负责以下工作

-   **构建和运行容器**：当客户端发送运行容器的命令时，守护进程拉取镜像，创建容器并启动它。
    
-   **管理 Docker 资源**：守护进程处理网络配置和卷管理等任务。
    

Docker 守护进程运行在主机上，通过 REST API、Unix 套接字或网络接口与 Docker 客户端通信。它还负责与容器运行时交互，后者处理容器的实际执行。

#### 3. Docker 引擎

Docker 引擎是 Docker 的核心部分。它使整个平台能够正常工作，结合了客户端、守护进程和容器运行时。Docker 引擎可以在包括 Linux、Windows 和 macOS 的各种操作系统上运行。

Docker 引擎有两个版本

-   **Docker CE（社区版）**：这是免费的开源版本，广泛用于个人和小规模项目。
    
-   **Docker EE（企业版）**：付费的企业级版本，提供额外的功能，如增强的安全性、支持和认证。
    

Docker 引擎通过整合构建、运行和管理容器所需的各种组件，简化了容器编排的复杂性。

#### 4. Docker 镜像

Docker 镜像是一个只读模板，包含应用程序运行所需的一切——代码、库、依赖和配置。镜像是容器的构建基块。当你运行容器时，本质上是在 Docker 镜像之上创建一个可写层。

Docker 镜像通常是从 Dockerfile 构建的，Dockerfile 是包含构建镜像说明的文本文件。例如，一个基本的 Dockerfile 可能以 `nginx` 或 `ubuntu` 这样的基础镜像开始，并包含复制文件、安装依赖或设置环境变量的命令。

这是一个简单的 Dockerfile 示例

```
FROM nginx:latest
COPY ./html /usr/share/nginx/html
EXPOSE 80
```

在这个例子中，我们使用官方的 Nginx 镜像作为基础，并将我们的本地 HTML 文件复制到容器的 Web 目录中。

一旦镜像构建完成，它可以存储在 Docker 注册表中并与他人共享。

#### 5. Docker 容器

Docker 容器是 Docker 镜像的运行实例。它轻量并与其他容器隔离，但共享主机操作系统的内核。每个容器都有自己的文件系统、内存、CPU 分配和网络设置，这使得它们可移植且可复制。

容器可以被创建、启动、停止和销毁，甚至可以在重启之间保持。由于容器基于镜像，它们确保应用程序无论在哪里运行都会表现一致。

Docker 容器的一些关键特性：

-   **隔离**：容器彼此隔离，也与主机隔离，但它们仍然共享相同的操作系统内核。
    
-   **可移植性**：容器可以在任何地方运行，无论是在本地机器、虚拟机还是云提供商上。
    

#### 6. Docker 注册表

Docker 注册表是存储和分发 Docker 镜像的集中场所。最流行的注册表是 Docker Hub，托管着数百万个可公开获取的镜像。组织也可以设置私有注册表以安全地存储和分发自己的镜像。

Docker 注册表提供几个关键功能：

-   **镜像版本控制**：镜像使用标签进行版本控制，方便管理应用程序的不同版本。
    
-   **访问控制**：注册表可以是公共或私有的，通过基于角色的访问控制管理谁可以拉取或推送镜像。
    
-   **分发**：镜像可以从注册表中拉取并部署到任何地方，这使得共享和重用容器化应用程序变得容易。
    

Docker 架构的一个重要新进展是使用 containerd。Docker 过去有自己的容器运行时，但现在它使用 containerd，这是一种遵循行业标准的容器运行时，也被 Kubernetes 等其他平台使用。

1.  containerd 负责
    
    -   启动和停止容器
        
    -   管理容器的存储和网络
        
    -   从注册表中拉取容器镜像
        

通过将容器运行时与 Docker 的更高层次功能分离，Docker 变得更加模块化，使其他工具可以使用 containerd，而 Docker 专注于面向用户的特性。

## 如何使用 Docker 创建简单的容器

**拉取 Linux 镜像**

首先从 Docker Hub 拉取 `alpine` 镜像。`alpine` 镜像是一个最小化的 Linux 发行版，设计轻量且快速。

运行以下命令：

```
docker pull alpine
```

这将会下载 `alpine` 镜像到您的本地系统。

**运行容器**

使用 `alpine` 镜像创建并启动容器。我们还将在容器中启动一个终端会话。

```
docker run -it alpine /bin/sh
```

以下是每个选项的含义：

-   `docker run`：创建并启动一个新容器。
    
-   `-it`：允许您与容器互动（交互模式+终端）。
    
-   `alpine`：指定要使用的镜像。
    
-   `/bin/sh`：指定在容器内运行的命令（在本例中为 shell 会话）。
    

**探索容器**

一旦容器正在运行，你将看到一个 shell 提示符，看起来像这样：

```
/ #
```

这表示您在 Alpine Linux 容器内。现在可以运行 Linux 命令。例如：

检查当前目录：

```
pwd
```

列出目录中的文件：

```
ls
```

输出：一个最小的目录结构，因为 Alpine 是一个轻量化的镜像。

您还可以安装一个软件包（Alpine 使用 `apk` 作为包管理工具）：

```
apk add curl
```

**退出容器**

当你探索完毕，输入 `exit` 关闭会话并停止容器

```
bashCopy codeexit
```

**访问已停止的容器**

如果你想在停止后再次访问容器，可以使用此命令列出所有容器（包括已停止的）：

```
docker ps -a
```

你会看到带有 IDs 和状态的容器列表，然后你可以启动已停止的容器：

```
docker start <container-id>
```

可以使用此命令附加到容器的 shell：

```
docker exec -it <container-id> /bin/sh
```

如果您不再需要此容器，可以移除它

1.  停止容器（如果它仍在运行）：
    
    ```
     docker stop <container-id>
    ```
    
2.  移除容器：
    
    ```
     docker rm <container-id>
    ```
    

**Docker 关键命令回顾**

| **命令** | **描述** |
| --- | --- |
| `docker pull alpine` | 下载 Alpine Linux 镜像。 |
| `docker run -it alpine /bin/sh` | 创建并启动一个交互式容器。 |
| `docker ps -a` | 列出所有容器（运行的和已停止的）。 |
| `docker start <container-id>` | 启动一个已停止的容器。 |
| `docker exec -it <container-id>` | 附加到一个正在运行的容器。 |
| `docker stop <container-id>` | 停止一个正在运行的容器。 |
| `docker rm <container-id>` | 移除一个已停止的容器。 |

## 总结

现在您已经具备基础知识，是时候运用这些知识了。开始尝试使用 Docker，构建您的第一个容器，探索其广阔的生态系统。

您将很快明白为什么 Docker 成为现代 DevOps 和软件工程的基石。

您可以关注我

-   [Twitter][8]
    
-   [LinkedIn][9]
    

[1]: #heading-what-are-containers
[2]: #heading-what-is-docker
[3]: #heading-why-docker
[4]: #heading-docker-architecture
[5]: #heading-dockers-container-runtime-containerd
[6]: #heading-how-to-create-a-simple-container-using-docker
[7]: #heading-wrapping-up
[8]: https://twitter.com/Kedar__98
[9]: https://www.linkedin.com/in/kedar-makode-9833321ab/?originalSubdomain=in

