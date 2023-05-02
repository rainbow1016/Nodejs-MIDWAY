# 介绍

## Midway Serverless 能做什么

Midway Serverless 是用于构建 Node.js 云函数的 Serverless 框架。帮助您在云原生时代大幅降低维护成本，更专注于产品研发。

- **跨云厂商：**一份代码可在多个云平台间快速部署，不用担心你的产品会被云厂商所绑定。
- **云端一体化：**提供了多套和社区前端 React、Vue 等融合一体化开发的方案。
- **代码复用：**通过框架的依赖注入能力，让每一部分逻辑单元都天然可复用，可以快速方便地组合以生成复杂的应用。
- **传统迁移：**通过框架的运行时扩展能力，让 Egg.js 、Koa、Express.js 等传统应用无缝迁移至各云厂商的云函数

你可以使用 Midway 来构建你的**全栈应用**，也可以发布的**函数服务** ，Restful 接口等，也可以加上前端（react，vue）代码**构建中后台项目**，也可以使用 Midway 提供的方案**迁移传统**的 Egg/Koa/Express 应用上弹性容器。

## Midway Serverless 和 Midway 的关系

Midway Serverless 是 Midway 产出的一套面向 Serverless 云平台的开发方案。其内容主要包括函数框架 `@midwayjs/faas`，以及一系列跟平台配套的工具链，启动器等。

在 Midway Serverless 2.0 之后，Midway Serverless 和 Midway 的能力复用，有着相同的 CLI 工具链，编译器，装饰器等等。

当前，Midway Serverless 主要面向的是 函数（FaaS）场景。

## 函数（FaaS）能做什么

很多人对函数还不是很清楚或者不了解他能做什么。当前的函数，可以当做一个小容器，原来我们要写一个完整的应用来承载能力，现在只需要写中间的逻辑部分，以及考虑输入和输出的数据。

通过绑定平台的触发器，可以承载例如 HTTP，Socket 等流量。

通过平台提供的 BaaS SDK，可以对外调用数据库，Redis 等服务。

通过函数，能提供传统的 HTTP API 服务，结合现有的前端框架（react，vue 等）渲染出一个个美丽的页面，也可以做为一个独立的数据模块，等待被调用（触发），比如常见的文件上传变更，解压等等，也能作为定时任务的逻辑部分，到了指定的时间或者时间间隔被执行。

随着时间的更替，平台的迭代，函数的能力会越来越强，而用户的上手成本，服务器成本则会越来越低。

## 函数不能做什么

函数的架构决定了，有些需求是无法支持的，另外，函数和应用在能力上还是有一定的区别。

函数不适用：

- 执行时间超过函数配置下限制的（最好不超过 5s）
- 有状态，在本地存储数据的
- 长链接，比如 ws 等
- 后台任务，有大数据执行的
- 依赖多进程通信的
- 大文件上传（比如网关限制的 2M 以上）
- 自定义环境的，比如 nginx 配置，c++ 库(c++ addon 动态链接库等)，python 版本依赖的
- 大量服务端缓存的
- 固定 ip 的情况

## 术语描述

### 函数

逻辑意义上的一段代码片段，通过常见的入口文件包裹起来执行。函数是单一链路，并且无状态的，现在很多人认为，Serverless = FaaS + BaaS ，而 FaaS 则是无状态的函数，BaaS 解决带状态的服务。

### 函数组

多个函数聚合到一起的逻辑分组名，对应原有的应用概念。

### 触发器

触发器，也叫 Event（事件），Trigger 等，特指触发函数的方式。
与传统的开发理念不同，函数不需要自己启动一个服务去监听数据，而是通过绑定一个（或者多个）触发器，数据是通过类似事件触发的机制来调用到函数。

### 函数运行时

英文叫 Runtime，具体指执行函数的环境，具体在各个平台可能是镜像，也可能是 Node.js 代码包，比如常见的社区运行时有 kubeless 等，该代码包会实现对接平台的各种接口，处理异常，转发日志等能力。

### 发布平台

函数最后承载的平台，现在社区最常见的有阿里云 FC 、腾讯云 SCF，AWS 的 Lambda 等等。

### Layer

由于运行时的代码比较简单，且需要保证稳定性无法经常性的更新，Layer 被设计出来扩展运行时的能力，并且可以精简本地的函数代码量（有一些平台限制了上传压缩包的大小）。