<!-- TOC -->

- [前言](#1-前言)
- [核心概念与安装配置](#2-核心概念与安装配置)
    - [核心概念](#21-核心概念)
    - [安装Docker引擎](#22-安装Docker引擎)
    - [配置Docker服务](#23-配置Docker服务)

<!-- /TOC -->






<a id="toc_anchor" name="#1-前言"></a>

# 1. 前言
* Docker三剑客：Machine、Compose、Swarm
* 容器化应用集群平台：Kubernetes
* 还有Mesos、CoreOS


<a id="toc_anchor" name="#2-核心概念与安装配置"></a>

# 2. 核心概念与安装配置
<a id="toc_anchor" name="#21-核心概念"></a>

## 2.1. 核心概念
* 三大核心概念：镜像、容器和仓库
1. Docker镜像：Docker镜像类似虚拟机的镜像，可以理解为一个只读的模板。
   * 镜像是创建Docker容器的基础。
   * 通过镜像可以创建多个容器。
   * Docker提供了一个很简单的机制来创建和使用镜像，用户甚至可以直接通过命令下载官方镜像。
2. Docker容器：类似于一个轻量级的沙箱，Docker利用容器来运行和隔离应用。
   * 容器是从镜像创建的应用运行实例。
   * 容器是独立运行的一个或一组应用，以及它们的运行态环境。
   * 可以把容器看做是一个简易版的Linux环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。
3. Docker仓库：类似于代码控制中的代码仓库，是一个集中存放镜像文件的场所。
   * 有时候会把仓库和仓库注册服务器（Registry）混为一谈
   * 仓库服务器：存放仓库的地方，其上往往存放着多个仓库


<a id="toc_anchor" name="#22-安装Docker引擎"></a>

## 2.2. 安装Docker引擎
* Docker引擎是使用Docker容器的核心组件
* Docker支持Docker引擎、Docker Hub、Docker Cloud：
  * Docker引擎：包括支持在桌面系统或云平台安装Docker，以及为企业提供简单安全弹性的容器集群编排和管理
  * Docker Hub：官方提供的云托管服务，可以提供共有或私有镜像仓库
  * Docker Cloud：r官方提供的容器云服务，可以完成容器的部署与管理，可以完整的支持容器化项目，还有CI（Continuous Integration：表示持续集成）、CD（Continuous Delivery和 Continuous Deployment，分别是持续交付和持续部署）功能。
* 使用官方脚本安装Docker引擎：
  * 稳定band版本：`curl -sSL https://get.docker.com/ | sh`
  * 测试版本：`curl -sSL https://test.docker.com/ | sh`

<a id="toc_anchor" name="#23-配置Docker服务"></a>

## 2.3. 配置Docker服务
* 避免每次使用Docker命令时都指定sudo：`sudo usermod -aG docker USER_NAME`。USER_NAME需替换成对应的用户名
* Docker服务启动时实际上是调用了dockerd命令，支持多种启动参数。因为dockerd是启动docker，所以需要在docker停止运行时执行此命令（停止docker运行：`sudo service docker stop`）
  * 如开启debug模式，并监听本地2378端口:`dockerd -D -H tcp://127.0.0.1:2376`
* 这些选项可以写入/etc/docker路径下daemon.json文件中，由docker启动时读取
  ```
  {
    "debug": true,
    "hosts": ["tcp://0.0.0.0:2376"]
  }
  ```
* Uptart来管理启动服务的ubuntu为例，Docker服务的默认配置文件为/etc/default/docker，修改此文件中的DOCKER_OPTS变量，添加启动参数
  ```
  DOCKER_OPTS="$DOCKER_OPTS --debug --host=tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock"
  ```
* 重启docker服务：`sudo service docker restart`