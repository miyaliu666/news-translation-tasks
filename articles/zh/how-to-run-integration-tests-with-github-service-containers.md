```markdown 
--- 
title: 如何使用 GitHub 服务容器运行集成测试 
date: 2025-01-09T14:57:29.370Z 
author: Alex Pliutau 
authorURL: https://www.freecodecamp.org/news/author/pltvs/ 
originalURL: https://www.freecodecamp.org/news/how-to-run-integration-tests-with-github-service-containers/ 
posteditor: "" 
proofreader: "" 
--- 
 
最近，我发表了一篇关于使用 [**Testcontainers**][2] 来模拟外部依赖项（如数据库和缓存）以进行后端集成测试的[**文章**][1]。该文章还解释了运行集成测试、环境搭建的不同方法以及它们的优缺点。 
 
<!-- more --> 
 
在本文中，我想介绍另一种选择，以防你使用 GitHub Actions 作为 CI 平台（目前最流行的 CI/CD 解决方案）。这种选择被称为[**服务容器**][3]，我意识到似乎没有太多开发人员知道它。 
 
在这个实践教程中，我将演示如何使用我们在上一篇教程中创建的 [demo Go 应用][4]，为具有外部依赖项（MongoDB 和 Redis）的集成测试创建一个 GitHub Actions 工作流。我们还将回顾 GitHub 服务容器的优缺点。 
 
## 先决条件 
 
-   基本了解 GitHub Actions 工作流。 
     
-   熟悉 Docker 容器。 
     
-   具备 Go 工具链的基本知识。 
     
 
## 目录 
 
-   [什么是服务容器？][5] 
     
-   [为什么不用 Docker Compose？][6] 
     
-   [作业运行时][7] 
     
-   [就绪状态检查][8] 
     
-   [私有容器注册表][9] 
     
-   [服务间数据共享][10] 
     
-   [Golang 集成测试][11] 
     
-   [个人经验与局限性][12] 
     
-   [结论][13] 
     
-   [资源][14] 
     
 
## 什么是服务容器？ 
 
服务容器是 Docker 容器，它提供了一种简单便携的方式来托管工作流所需的依赖项，如数据库（在我们的例子中是 MongoDB）、Web 服务或缓存系统（在我们的例子中是 Redis）。 
 
本文重点是集成测试，但服务容器还有许多其他可能的应用。例如，你还可以用它们来运行工作流所需的支持工具，如代码分析工具、静态代码检查器或安全扫描器。 
 
## 为什么不用 Docker Compose？ 
 
听起来和 Docker Compose 中的 **服务** 很相似，对吧？那是因为确实如此。 
 
但尽管你可以通过安装 Docker Compose 并运行 **docker-compose up** 来在 GitHub Actions 工作流中使用 [Docker Compose][15]，服务容器提供了一种更集成和简化的方法，专为 GitHub Actions 环境设计。 
 
而且，尽管它们类似，但它们解决了不同的问题，并且有不同的一般用途： 
 
-   Docker Compose 适合在本地机器或单台服务器上管理多容器应用程序。它最适合用于长时间运行的环境。 
     
-   服务容器是短暂的，只在工作流运行期间存在，并直接在你的 GitHub Actions 工作流文件中定义。 
     
 
请记住，服务容器的功能（至少就目前而言）比 Docker Compose 更有限，所以准备好发现一些潜在的瓶颈。我们将在本文末尾讨论其中一些。 
 
## 作业运行时 
 
你可以直接在运行器机器上或在 Docker 容器中运行 GitHub 作业（通过指定 **container** 属性）。第二种选择通过使用你在 **services** 部分定义的标签简化了服务的访问。 
 
如果想直接在运行器机器上运行： 
 
**.github/workflows/test.yaml** 
 
``` 
jobs: 
  integration-tests: 
    runs-on: ubuntu-24.04 
 
    services: 
      mongo: 
        image: mongodb/mongodb-community-server:7.0-ubi8 
        ports: 
          - 27017:27017 
 
    steps: 
      - run: | 
          echo "addr 127.0.0.1:27017" 
``` 
 
或者你可以在容器中运行它（在我们的例子中是 [Chainguard Go Image][16]）： 
 
``` 
jobs: 
  integration-tests: 
    runs-on: ubuntu-24.04 
    container: cgr.dev/chainguard/go:latest 
 
    services: 
      mongo: 
        image: mongodb/mongodb-community-server:7.0-ubi8 
        ports: 
          - 27017:27017 
    steps: 
      - run: | 
          echo "addr mongo:27017" 
``` 
 
你也可以省略主机端口，因此容器端口将随机分配给主机上的一个空闲端口。然后你可以使用变量访问端口。 
 
省略主机端口的好处： 
 
-   避免端口冲突——例如当你在同一主机上运行许多服务时。 
     
-   增强可移植性——你的配置变得不那么依赖于特定的主机环境。 
     
 
``` 
jobs: 
  integration-tests: 
    runs-on: ubuntu-24.04 
    container: cgr.dev/chainguard/go:1.23 
 
    services: 
      mongo: 
        image: mongodb/mongodb-community-server:7.0-ubi8 
        ports: 
          - 27017/tcp 
    steps: 
      - run: | 
          echo "addr mongo:${{ job.services.mongo.ports['27017'] }}" 
``` 
 
当然，每种方法都有利弊。 
 
在容器中运行： 
 
-   **优点**：简化的网络访问（使用标签作为主机名），以及在容器网络内自动端口暴露。作业在隔离环境中运行，你也可以获得更好的隔离性和安全性。 
     
-   **缺点**：容器化带来的隐含开销。 
     
``` 
 
-   **优点**: 比在容器中运行作业可能会有更少的开销。 
     
-   **缺点**: 需要手动映射服务容器的端口（使用 localhost:）。由于作业直接在运行器机器上运行，隔离/安全性较差。如果出现问题，可能会影响其他作业或运行器本身。 
     
 
## 就绪健康检查 
 
在运行连接到已配置容器的集成测试之前，您通常需要确保服务已准备就绪。您可以通过指定 [docker create 选项][17]，比如 **health-cmd**，来实现这一点。 
 
这点非常重要，否则服务在您开始访问它们时可能尚未准备好。 
 
对于 MongoDB 和 Redis，可以这样做： 
 
``` 
    services: 
      mongo: 
        image: mongodb/mongodb-community-server:7.0-ubi8 
        ports: 
          - 27017:27017 
        options: >- 
          --health-cmd "echo 'db.runCommand("ping").ok' | mongosh mongodb://localhost:27017/test --quiet" 
          --health-interval 5s 
          --health-timeout 10s 
          --health-retries 10 
 
      redis: 
        image: redis:7 
        ports: 
          - 6379:6379 
        options: >- 
          --health-cmd "redis-cli ping" 
          --health-interval 5s 
          --health-timeout 10s 
          --health-retries 10 
``` 
 
在 Action 日志中，您可以看到就绪状态： 
 
![GitHub Actions Logs](https://cdn.hashnode.com/res/hashnode/image/upload/v1736245987630/0b0bf229-b8d3-4e4e-8e0b-e3bbe5f9a6d8.png) 
 
## 私有容器注册表 
 
在我们的示例中，我们使用的是来自 Dockerhub 的公共镜像，但也可以使用来自您的私有注册表的私有镜像，例如亚马逊弹性容器注册表 (ECR)、谷歌制品注册表等。 
 
请确保将凭据存储在 [Secrets][18] 中，然后在 **credentials** 部分引用它们。 
 
``` 
services: 
  private_service: 
    image: ghcr.io/org/service_repo 
    credentials: 
      username: ${{ secrets.registry_username }} 
      password: ${{ secrets.registry_token }} 
``` 
 
## 在服务之间共享数据 
 
您可以使用卷在作业的服务或其他步骤之间共享数据。您可以指定命名的 Docker 卷、匿名 Docker 卷或主机的绑定挂载。但无法直接将源代码挂载为容器卷。更多上下文可以参考此 [开放讨论][19]。 
 
要指定卷，您需要指定源路径和目标路径：`<source>:<destinationPath>` 
 
`<source>` 是主机机器上的卷名或绝对路径，而 `<destinationPath>` 是容器内的绝对路径。 
 
``` 
volumes: 
  - /src/dir:/dst/dir 
``` 
 
Docker（以及使用 Docker 的 GitHub Actions）中的卷提供了持久的数据存储和容器或作业步骤之间的数据共享，将数据与容器镜像解耦。 
 
## 项目设置 
 
在深入研究完整源代码之前，让我们为使用 GitHub 服务容器运行集成测试设置我们的项目。 
 
1.  创建一个新的 GitHub 仓库。 
     
2.  使用 `go mod init` 初始化一个 Go 模块。 
     
3.  创建一个简单的 Go 应用。 
     
4.  在 `integration_test.go` 中添加集成测试。 
     
5.  创建一个 `.github/workflows` 目录。 
     
6.  在 `.github/workflows` 目录中创建一个名为 `integration-tests.yaml` 的文件。 
     
 
## Golang 集成测试 
 
既然我们可以配置外部依赖项了，让我们看看如何在 Go 中运行集成测试。我们将在工作流文件的 **steps** 部分进行此操作。 
 
我们将在一个使用 [Chainguard Go 镜像][20] 的容器中运行我们的测试。这意味着我们不必安装/设置 Go。如果您想直接在运行器机器上运行测试，则需要使用 [setup-go][21] Action。 
 
您可以在[这里][22]找到带有测试和此工作流的完整源代码。 
 
**.github/workflows/integration-tests.yaml** 
 
``` 
name: "integration-tests" 
 
on: 
  workflow_dispatch: 
  push: 
    branches: 
      - main 
 
jobs: 
  integration-tests: 
    runs-on: ubuntu-24.04 
    container: cgr.dev/chainguard/go:latest 
 
    env: 
      MONGO_URI: mongodb://mongo:27017 
      REDIS_URI: redis://redis:6379 
 
    services: 
      mongo: 
        image: mongodb/mongodb-community-server:7.0-ubi8 
        ports: 
          - 27017:27017 
        options: >- 
          --health-cmd "echo 'db.runCommand("ping").ok' | mongosh mongodb://localhost:27017/test --quiet" 
          --health-interval 5s 
          --health-timeout 10s 
          --health-retries 10 
 
      redis: 
        image: redis:7 
        ports: 
          - 6379:6379 
        options: >- 
          --health-cmd "redis-cli ping" 
          --health-interval 5s 
          --health-timeout 10s 
          --health-retries 10 
 
    steps: 
      - name: Check out repository code 
        uses: actions/checkout@v4 
 
      - name: Download dependencies 
        run: go mod download 
 
      - name: Run Integration Tests 
        run: go test -tags=integration -timeout=120s -v ./... 
``` 
 
总结这里发生的事情： 
 
1. 我们在一个带 Go 的容器中运行我们的作业 (**container**) 
     
2. 我们启动两个服务：MongoDB 和 Redis (**services**) 
     
3. 我们配置健康检查以确保在运行测试时我们的服务是"健康"的 (**options**) 
     
4. 我们执行标准的代码检出 
     
5. 然后我们运行 Go 测试 
     
 
![GitHub Actions Logs: full run](https://miro.medium.com/v2/resize:fit:480/0*QLl4vjotU6o1osy-.png) 
 
## 个人经验与限制 
 
我们在 [BINARLY][23] 使用服务容器进行后端集成测试已经有一段时间了，效果很好。但初始工作流的创建花费了一些时间，并且我们遇到了以下瓶颈： 
 
-   在操作服务容器中无法覆盖或运行自定义命令（就像在 Docker Compose 中使用 **command** 属性一样）。[开放的拉取请求][24] 
     
    -   解决方法：我们必须找到一个不需要覆盖命令的解决方案。在我们的案例中，我们很幸运，可以使用环境变量实现相同的目的。 
-   无法直接将源代码挂载为容器卷。[开放讨论][25] 
     
    -   尽管这确实是一个很大限制，但您可以在服务容器启动后，将代码从您的存储库复制到挂载的目录中。 
 
## 结论 
 
GitHub 服务容器是在您的 GitHub 工作流中直接配置一个临时测试环境的绝佳选择。由于配置与 Docker Compose 有些相似，因此很容易在管道中运行任何容器化的应用程序并与之通信。这确保了 GitHub 运行器在完成后会负责关闭所有东西。 
 
如果您使用 GitHub Actions，这种方法效果极佳，因为它是专门为 GitHub Actions 环境设计的。 
 
### 资源 
 
-   [源代码][26] 
     
-   [GitHub 文档][27] 
     
-   在 [packagemain.tech][28] 发现更多文章 
     
 
[1]: https://www.freecodecamp.org/news/integration-tests-using-testcontainers/ 
[2]: https://testcontainers.com/ 
[3]: https://docs.github.com/en/actions/use-cases-and-examples/using-containerized-services/about-service-containers 
[4]: https://github.com/plutov/packagemain/tree/master/testcontainers-demo 
[5]: #heading-what-are-service-containers 
[6]: #heading-why-not-docker-compose 
[7]: #heading-job-runtime 
[8]: #heading-readiness-healthcheck 
[9]: #heading-private-container-registries 
[10]: #heading-sharing-data-between-services 
[11]: #heading-golang-integration-tests 
[12]: #heading-personal-experience-amp-limitations 
[13]: #heading-conclusion 
[14]: #heading-resources 
[15]: https://github.com/marketplace/actions/docker-compose-action 
[16]: https://images.chainguard.dev/directory/image/go/overview 
[17]: https://docs.docker.com/reference/cli/docker/container/create/#options 
[18]: https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions 
[19]: https://github.com/orgs/community/discussions/42127 
[20]: https://images.chainguard.dev/directory/image/go/overview 
[21]: https://github.com/actions/setup-go 
[22]: https://github.com/plutov/service-containers 
[23]: https://www.binarly.io/ 
[24]: https://github.com/actions/runner/pull/1152 
[25]: https://github.com/orgs/community/discussions/42127 
[26]: https://github.com/plutov/service-containers 
[27]: https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idservices 
[28]: https://packagemain.tech/ 
 
 