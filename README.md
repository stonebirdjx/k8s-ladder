<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [k8s-ladder](#k8s-ladder)
  - [概念](#%E6%A6%82%E5%BF%B5)
  - [了解常用资源和概念](#%E4%BA%86%E8%A7%A3%E5%B8%B8%E7%94%A8%E8%B5%84%E6%BA%90%E5%92%8C%E6%A6%82%E5%BF%B5)
    - [Node](#node)
    - [Label](#label)
    - [Event](#event)
    - [Configmap & Secret](#configmap--secret)
    - [Namespace](#namespace)
    - [Pod & Deployment](#pod--deployment)
    - [Service](#service)
- [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# k8s-ladder
from introduction to specialist with k8s

## 概念

[什么是k8s](https://kubernetes.io/zh-cn/docs/concepts/overview/what-is-kubernetes/)

> k8s优点：服务发现和负载均衡，存储编排，自动部署和回滚，自动完成装箱计算，自我修复，密钥与配置管理。
>
> 容器虽然比vm更轻（属于进程级别），但隔离性没有vm好。

传统-VM-容器架构图

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/container_evolution.svg)

:point_right:你需要了解什么是[资源隔离](https://github.com/stonebirdjx/notebook/blob/master/%E8%B5%84%E6%BA%90%E9%9A%94%E7%A6%BB.md)

## 了解常用资源和概念

先把k8s当作一个黑盒，知道怎么使用就好了。首先跟着 [官方的例子](https://kubernetes.io/zh-cn/docs/tutorials/kubernetes-basics/) 熟悉一下流程。

[熟悉 kubectl 相关的命令]()

> 然后需要了解下面的常用资源概念，炼气阶段，请不要尝试学习过多的资源类型。我们这里熟悉最核心的概念即可，否则易走火入魔 👻，切记。当我们打通任督二脉之时，所有的新概念都不过尔尔。--by caicloud

### Node

节点（Node）是物理机或虚拟机，他们组成了 kubernetes 的资源池。Master 节点负责资源调度、集群状态控制等， Node 节点负责运行用户应用，承接负载。

在 kubernetes 中，我们可以通过 `kubectl get nodes` 命令来获取所有节点信息：

```bash
$ kubectl get nodes
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   27h   v1.15.0
```

> 一般在生产环境中，我们不会在 master 上运行用户容器。但是如果主机数量非常少的情况下可以考虑，前提是预留足够的资源给 master。

```bash
# 不承载用户容器的主机需要加上 SchedulingDisabled 的状态
kubectl cordon <node name>   # uncordon 标记 node 为 schedulable
```

可以通过 `kubectl describe nodes` 来了解节点的详情。

### Label

### Event

### Configmap & Secret

### Namespace

namespace 是用来构建虚拟的资源池；管理员可以将 kubernetes 划分成多个虚拟的区域，不同的项目或者团队可以使用不同的 namespace，达到了共享 kubernetes 集群资源的目的。

> 此外， namespace 也被用来划分命名空间，即不同 namespace 里的资源可以取相同的名字，相同 namespace 内的资源不能重名。

相关命令

```bash
kubectl get ns
kubectl describe ns ${ns_name}
```

这里 `ns` 是 `namespace` 的缩写。输出内容中的 `default`, `kube-node-lease`, `kube-public` 和 `kube-system`，都是 kubernetes 默认创建的 namespace，用来放置系统相关的资源。

### Pod & Deployment

在 kubernetes 的世界里，Pod 是运行应用的载体。 **Pod 是由多个容器组成、是 kubernetes 的最小调度单元、Pod 共享底层资源、由 kubernetes 来管理生命周期**。然而，一般情况下，我们并不直接创建 Pod，而是通过 Deployment 来创建 Pod，由 Deployment 来负责创建、更新、维护其所管理的所有 Pods。

一旦我们通过 Deployment 创建 Pod，会有一个 Deployment 控制器不断监控所有 Pod 的状态。例如，如果 Pod 运行的机器宕机了，那么 Deployment 控制器会在另一台机器上重新启动一个 Pod。

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/kubernetes_cluster.png)

相关命令

```bash
# create deployment
kubectl apply -f resources/deployment_nginx.yaml -n tutorial

# get deployment
kubectl get deployment -n tutorial

# 可以通过 kubectl get pods 来查看当前创建的 Pod。
kubectl get pods -n tutorial -o wide

# 通过 kubectl get pods -o wide 来查看 Pod 运行的主机
kubectl get pods -n tutorial -o wide

# 通过 kubectl describe pods nginx-77cd46f788-bd5kk 来查看 Pod 更加详细的信息
kubectl describe pods nginx-77cd46f788-bd5kk -n tutorial

# 通过 kubectl logs <pod name> 查看日志
kubectl logs nginx-646b46d648-hbwg2 -n tutorial

# 通过 kubectl exec <pod name> 与 Pod 交互。
## 我们通过双横线（“--”）区分本地终端命令和容器中执行的命令；当执行的命令只有一个单词的时候，可以省略。如果容器中有 shell，我们也可以启动一个远程终端：
kubectl exec nginx-646b46d648-hbwg2 -n tutorial -- ls -l
kubectl exec nginx-646b46d648-hbwg2 -n tutorial bash
```

### Service

service 有以下几个作用：

- 提供固定的 IP。由于 Pod 可以随时启停，Pod IP 可能随时都会变化，例如上面 nginx pod 重启之后 IP 可能不再是 172.17.0.11。Service 为 Pods 提供的固定 IP，其他服务可以通过 Service IP 找到提供服务的 Pods。
- 提供负载均衡。Service 由多个 Pods 组成，kubernetes 对组成 Service 的 Pods 提供的负载均衡方案，例如随机访问、基于 Client IP 的 session affinity。
- 服务发现。集群中其他服务可以通过 Service 名字访问后端服务（DNS），也可以通过环境变量访问。

相关命令

```bash
```





# 参考资料

:point_right:最好的资料是[官方文档](https://kubernetes.io/zh-cn/)

[caicould](https://github.com/caicloud/kube-ladder)
