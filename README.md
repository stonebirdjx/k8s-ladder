<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [k8s-ladder](#k8s-ladder)
  - [概念](#%E6%A6%82%E5%BF%B5)
  - [了解常用资源和概念（简单了解）](#%E4%BA%86%E8%A7%A3%E5%B8%B8%E7%94%A8%E8%B5%84%E6%BA%90%E5%92%8C%E6%A6%82%E5%BF%B5%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3)
    - [Node](#node)
    - [Namespace](#namespace)
    - [Pod & Deployment](#pod--deployment)
      - [水平扩展应用和更新应用](#%E6%B0%B4%E5%B9%B3%E6%89%A9%E5%B1%95%E5%BA%94%E7%94%A8%E5%92%8C%E6%9B%B4%E6%96%B0%E5%BA%94%E7%94%A8)
      - [Pod 生命周期](#pod-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [Service](#service)
    - [Label](#label)
    - [Yaml/Json File](#yamljson-file)
    - [Event](#event)
    - [Configmap & Secret](#configmap--secret)
  - [:point_right:k8s核心组件（简单了解）](#point_rightk8s%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3)
  - [:point_right:Container Runtime（简单了解）](#point_rightcontainer-runtime%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3)
  - [安装k8s（简单了解）](#%E5%AE%89%E8%A3%85k8s%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3)
  - [:point_right:kubernetes API（先简单了解，后仔细查阅ing）](#point_rightkubernetes-api%E5%85%88%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3%E5%90%8E%E4%BB%94%E7%BB%86%E6%9F%A5%E9%98%85ing)
  - [Kubernetes 对象（简单了解）](#kubernetes-%E5%AF%B9%E8%B1%A1%E7%AE%80%E5%8D%95%E4%BA%86%E8%A7%A3)
  - [:point_right:kubernetes 排错 (知道套路)](#point_rightkubernetes-%E6%8E%92%E9%94%99-%E7%9F%A5%E9%81%93%E5%A5%97%E8%B7%AF)
  - [接下来就是比较重要的东西了](#%E6%8E%A5%E4%B8%8B%E6%9D%A5%E5%B0%B1%E6%98%AF%E6%AF%94%E8%BE%83%E9%87%8D%E8%A6%81%E7%9A%84%E4%B8%9C%E8%A5%BF%E4%BA%86)
  - [:point_right:pod的生命周期-健康检查-重启策略-容器探针](#point_rightpod%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F-%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5-%E9%87%8D%E5%90%AF%E7%AD%96%E7%95%A5-%E5%AE%B9%E5%99%A8%E6%8E%A2%E9%92%88)
  - [:point_right:init 容器](#point_rightinit-%E5%AE%B9%E5%99%A8)
  - [:point_right:Pod 水平自动扩缩(HPA)](#point_rightpod-%E6%B0%B4%E5%B9%B3%E8%87%AA%E5%8A%A8%E6%89%A9%E7%BC%A9hpa)
  - [:point_right:service实现-拓扑感知](#point_rightservice%E5%AE%9E%E7%8E%B0-%E6%8B%93%E6%89%91%E6%84%9F%E7%9F%A5)
  - [:point_right:Ingress原理 - ingress-nginx操作](#point_rightingress%E5%8E%9F%E7%90%86---ingress-nginx%E6%93%8D%E4%BD%9C)
  - [:point_right:DNS 服务 - coredns](#point_rightdns-%E6%9C%8D%E5%8A%A1---coredns)
  - [:point_right: 卷(volume)](#point_right-%E5%8D%B7volume)
  - [:point_right:pv/pvc](#point_rightpvpvc)
  - [:point_right:StorageClass](#point_rightstorageclass)
  - [:point_right:API 访问控制](#point_rightapi-%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
  - [:point_right:调度、抢占和驱逐](#point_right%E8%B0%83%E5%BA%A6%E6%8A%A2%E5%8D%A0%E5%92%8C%E9%A9%B1%E9%80%90)
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

:point_right:你需要了解什么是[资源隔离](https://github.com/stonebirdjx/k8s-ladder/blob/master/%E8%B5%84%E6%BA%90%E9%9A%94%E7%A6%BB.md)

## 了解常用资源和概念（简单了解）

先把k8s当作一个黑盒，知道怎么使用就好了。首先跟着 [官方的例子](https://kubernetes.io/zh-cn/docs/tutorials/kubernetes-basics/) 熟悉一下流程。

[熟悉 kubectl 相关的命令](https://github.com/stonebirdjx/k8s-ladder/blob/master/kubectl%E5%91%BD%E4%BB%A4%E5%8F%82%E8%80%83.md)

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

#### 水平扩展应用和更新应用

**扩容**

可以通过 `kubectl scale` 子命令将 Pod 数量扩容到四个

```bash
kubectl scale deployments nginx --replicas=4 -n tutorial
```

Service 会感知到 Pods 的变化，在所有的 Pods 中负载均衡，我们可以通过 kubectl 查看。`Service nginx 已经将后端 Endpoints 扩容到所有的 4 个 Pods。`

**更新过程**

> 当我们更新容器镜像时，kubernetes 会启动一个新 Pod 并关闭一个老 Pod。
>
> 第一个 Pod 更新成功后，Deployment 会更新第二个 Pod。
>
> 直到更新完毕

**更新镜像**

可使用资源对象包括（不区分大小写,未提到的资源后面暂时不用了解）：

pod (po)、replicationcontroller (rc)、deployment (deploy)、daemonset (ds)、job、replicaset (rs)

```bash
kubectl set image deployments nginx nginx=cargo.caicloud.io/caicloud/nginx:1.9.3 -n tutorial
```

**rollout 子命令可以用来查询部署的状态，以及回滚等操作。**

```bash
# 使用 `kubectl rollout status` 可以查询部署的状态。
kubectl rollout status deployment nginx -n tutorial

# 如果我们想要回滚到之前的版本，可以使用 `kubectl rollout undo` 命令。
# kubectl rollout undo deployment <deployment-name> --to-revision=<revision>
# 如果 --to-revision 参数未指定，将选中最近的版本。
kubectl rollout undo deployment nginx -n tutorial
```

#### Pod 生命周期

Pod 生命周期主要包括：

- Pod Phase
- Pod Condition
- Restart Policy
- Container probes

> 我们主要实践 Restart Policy 和 Container probes。

**Restart Policy** 

Restart Policy 指定当 Pod 内容器出错或执行完毕后，是否重启。

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  restartPolicy: Always  # Never Onfailure
```

**Container probes** - 探针

livenessProbe

指示容器是否正在运行。如果存活态探测失败，则 kubelet 会杀死容器， 并且容器将根据其[重启策略](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy)决定未来。如果容器不提供存活探针， 则默认状态为 `Success`。

readinessProbe

指示容器是否准备好为请求提供服务。如果就绪态探测失败， 端点控制器将从与 Pod 匹配的所有服务的端点列表中删除该 Pod 的 IP 地址。 初始延迟之前的就绪态的状态值默认为 `Failure`。 如果容器不提供就绪态探针，则默认状态为 `Success`。

startupProbe

指示容器中的应用是否已经启动。如果提供了启动探针，则所有其他探针都会被 禁用，直到此探针成功为止。如果启动探测失败，`kubelet` 将杀死容器，而容器依其 [重启策略](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy)进行重启。 如果容器没有提供启动探测，则默认状态为 `Success`。

### Service

service 有以下几个作用：

- 提供固定的 IP。由于 Pod 可以随时启停，Pod IP 可能随时都会变化，例如上面 nginx pod 重启之后 IP 可能不再是 172.17.0.11。Service 为 Pods 提供的固定 IP，其他服务可以通过 Service IP 找到提供服务的 Pods。
- 提供负载均衡。Service 由多个 Pods 组成，kubernetes 对组成 Service 的 Pods 提供的负载均衡方案，例如随机访问、基于 Client IP 的 session affinity。
- 服务发现。集群中其他服务可以通过 Service 名字访问后端服务（DNS），也可以通过环境变量访问。

相关命令

```bash
# 可以通过 kubectl create -f 加文件名创建 Service
# 类似Deployment，kubernetes 提供了快捷命令让我们能快速创建 Service。
kubectl expose deployment nginx --port 80 -n tutorial
kubectl expose deployment nginx --port 8080 --target-port 80 -n tutorial

# 通过 kubectl get service 命令可以查看 service 的详细信息
kubectl get svc nginx -n tutorial

# 同样，通过 describe 可以看到更多详细的信息
kubectl describe svc nginx -n tutorial
Name:              nginx
Namespace:         tutorial
...
Endpoints:         172.17.0.11:80 # Endpoint 表明 Service 所选中的 PodIP:PodPort。

# 我们可以通过两种方式暴露服务：NodePort 和 LoadBalancer。
# NodePort 通过在每个节点打开一个端口对外提供服务，LoadBalancer 通过创建一个外部负载均衡器（例如公有云负载均衡器）来对外提供服务。这里我们尝试使用 NodePort。
kubectl expose deployment nginx --port 80 --type NodePort -n tutorial
```

### Label

Service 通过 selector & label 来选取它所管理的 Pod，同样 Deployment 也是通过 selector & label 选取它所管理的 Pod。因为我们是通过 Deployment 创建的 Pod，因此 Deployment 的 selector 一定是匹配 Pod 的 label。

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/labels.png)

> 我们使用 `kubectl expose deployment nginx` 的时候，kubernetes 默认将 Service 的 selector 设置成与 Deployment 相同的 selector。

相关命令

```bash
# 可以通过 -l 选项查看仅具有某个 label 的资源
kubectl get pods -l run=nginx -n tutorial

# 对资源的 label 进行操作
kubectl label pod nginx-646b46d648-hbwg2 app=v1 -n tutorial

# 查看
kubectl describe pods nginx-646b46d648-hbwg2 -n tutorial
```

### Yaml/Json File

> 所有的操作都是以 yaml 文件为主。我们之前所使用的命令，只是方便用户快速修改 yaml 中经常需要修改的字段。

kubernetes yaml 整体分为 5 个部分：`apiVersion`,` kind`, `metadata`, `spec`, `status`；其中 apiVersion 表明当前 kubernetes API 的分组；kind 表明当前操作的资源类型； metadata 是资源的元数据，对于每种资源都是固定的，例如资源的名字，所处的 namespace, label 等；spec 是用户对资源的 “说明书”，即用户对资源的各种配置信息；status 是资源当前的状态，kubernetes 会尽最大努力使 spec 和 status 相匹配。

相关命令

```bash
# 可以通过 kubectl get -o yaml 来获取已经部署的资源的 Yaml 文件
kubectl get pods nginx-646b46d648-7c457 -n tutorial -o yaml
kubectl get svc nginx -n tutorial -o yaml

# 用户可通过 kubectl create -f 创建 kubernetes 资源。
kubectl create -f resources/pod.yaml -n tutorial

# 修改yaml后使用 kubectl apply -f 更新 kubernetes 资源。
kubectl apply -f resources/pod.yaml -n tutorial

# 使用 kubectl edit resource 在线修改
kubectl edit pods nginx -n tutorial
```

### Event

Kubernetes events 显示了 kubernetes 集群中所有的事件。不同于其他资源，kubernetes events 并不是由用户创建的资源，而是由 kubernetes 系统组件创建，用以提示用户集群发生的各种事件。我们可以通过 kubectl get 命令来查询集群的事件。默认情况下，event 会有 TTL，超过 TTL 之后 kubernetes 会将事件删掉。

相关命令

```bash
# 查询集群events
kubectl get events -n tutorial

# kubectl describe pod <pod name> 会返回 Pod 的 event 信息
kubectl describe pod nginx -n tutorial
```

### Configmap & Secret

ConfigMap 是 kubernetes 用来管理配置信息的资源类型。我们通过单独创建 ConfigMap，再将 ConfigMap 挂载到 Pod 内的方式分离配置和应用。

相关命令

```bash
# 通过命令的方式创建
kubectl create configmap game-config --from-file=resources/game.properties --from-file=resources/ui.properties -n tutorial

# 查看cm描述
kubectl describe configmap game-config -n tutorial

# 查看详情
kubectl get configmap game-config -o yaml -n tutorial
```

## :point_right:k8s核心组件（简单了解）

[官方组件说明](https://kubernetes.io/zh-cn/docs/concepts/overview/components/)

[个人组件记录](https://github.com/stonebirdjx/k8s-ladder/blob/master/k8s%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6.md)

- master组件：etcd、api server、controller
- node组件：kubelet、kube-proxy
- 网络插件：flannel、calico

:point_right:了解基本组件、基本功能

## :point_right:Container Runtime（简单了解）

**概念**

`镜像`：镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 **不包含** 任何动态数据，其内容在构建之后也不会被改变。

`容器`：容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的命名空间。因此容器可以拥有自己的 `root` 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。

`镜像仓`：仓库用于存储镜像。可以推荐使用dockerbub，或者自建harbor

> 容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。

**常用的容器运行时**：

- containerd  -- 命令行（ctr、crictl、nerdctl）-> [containerd笔记](https://github.com/stonebirdjx/k8s-ladder/blob/master/containerd.md)
- docker -- 命令行 docker ->  [docker笔记](https://github.com/stonebirdjx/k8s-ladder/blob/master/docker.md)

**:point_right:containerd 优势**：

- containerd是docker的基础组件之一，更轻量
- containerd 调用链更短，组件更少，更稳定，占用节点资源更少。
- containerd 相比于docker , 多了 namespace 概念，每个 image 和 container 都会在各自的namespace下可见。

:point_right:了解镜像及tag、容器、镜像仓等概率，了解容器运行时接口（CRI），查看了解容器状态。

> 对于docker能熟练的使用docker命令，编码dockerfile，了解docker-compose
>
> 对于containerd相关命令，熟练使用crictl，nerdctl，了解ctr

## 安装k8s（简单了解）

> 个人使用minikube就行

- kubeadm 
- rke
- kubespray
- [手动安装-caicloud](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md)

## :point_right:kubernetes API（先简单了解，后仔细查阅ing）

Kubernetes API 是 k8 最引以为傲的设计，API 服务器负责提供 HTTP API，以供用户、集群中的不同部分和集群外部组件相互通信。

API 组和版本控制：每个版本位于不同的 API 路径， 例如 `/api/v1` 或 `/apis/rbac.authorization.k8s.io/v1alpha1`

[kubernetes Api官方介绍](https://kubernetes.io/zh-cn/docs/concepts/overview/kubernetes-api/)

[kubernetes API访问控制](https://kubernetes.io/zh-cn/docs/concepts/security/controlling-access/)

[kubernetes API代码库](https://github.com/kubernetes/api)

通过浏览 [Kubernetes/Community](https://github.com/kubernetes/community) 代码仓库来了解各个兴趣小组（SIG），"SIG" 是 Special Interest Group 的简称。一般来讲，一个 SIG 对应着一个 Kubernetes 子系统，需要了解以下子系统：

- 架构 Architecture
- 应用 Apps
- 存储 Storage
- 网络 Network
- 权限 Auth
- 节点 Node
- 调度 Scheduling
- 命令行 CLI
- 多集群 MultiCluster
- 云平台 CloudProvider
- 扩展性 Scalability
- 弹性伸缩 Autoscaling
- 监控日志 Instrumentation

## Kubernetes 对象（简单了解）

在 Kubernetes 系统中，**Kubernetes 对象** 是持久化的实体。 Kubernetes 使用这些实体去表示整个集群的状态。 比较特别地是，它们描述了如下信息：

- 哪些容器化应用正在运行（以及在哪些节点上运行）
- 可以被应用使用的资源
- 关于应用运行时表现的策略，比如重启策略、升级策略以及容错策略

**对象规约（Spec）与状态（Status）**:

在想要创建的 Kubernetes 对象所对应的 `.yaml` 文件中，需要配置的字段如下：

- `apiVersion` - 创建该对象所使用的 Kubernetes API 的版本
- `kind` - 想要创建的对象的类别
- `metadata` - 帮助唯一标识对象的一些数据，包括一个 `name` 字符串、`UID` 和可选的 `namespace`
- `spec` - 你所期望的该对象的状态

[k8s对象](https://kubernetes.io/zh-cn/docs/concepts/overview/working-with-objects/kubernetes-objects/)

## :point_right:kubernetes 排错 (知道套路)

日常过程中报错是不可避免的，需要懂得排查报错，大致步骤如下

**查看集群事件**

```bash
kubectl events
```

**查看 Node 状态**

```bash
kubectl get nodes
kubectl describe node ${node_name}
```

**查看 Pod 状态以及运行节点**

```bash
kubectl get pods -o wide
kubectl -n kube-system get pods -o wide
```

**查看 Pod 事件**

```bash
kubectl describe pod ${pod_name}
```

**kube-apiserver 日志**

```bash
pod_name=$(kubectl -n kube-system get pod -l component=kube-apiserver -o jsonpath='{.items[0].metadata.name}')
kubectl -n kube-system logs ${pod_name} --tail 100
```

> 可以使用 journalctl -u kube-apiserver 查看其日志

**kube-controller-manager 日志**

```bash
pod_name=$(kubectl -n kube-system get pod -l component=kube-controller-manager -o jsonpath='{.items[0].metadata.name}')

kubectl -n kube-system logs ${pod_name} --tail 100
```

> 可以使用 journalctl -u kube-controller-manager

**kube-scheduler 日志**

```bash
pod_name=$(kubectl -n kube-system get pod -l component=kube-scheduler -o jsonpath='{.items[0].metadata.name}')

kubectl -n kube-system logs ${pod_name} --tail 100
```

> 可以使用 journalctl -u kube-scheduler 

**kube-dns 日志**

> kube-dns 通常以 Addon 的方式部署，每个 Pod 包含三个容器，最关键的是 kubedns 容器的日志

```sh
pod_name=$(kubectl -n kube-system get pod -l k8s-app=kube-dns -o jsonpath='{.items[0].metadata.name}')

kubectl -n kube-system logs ${pod_name} -c kubedns
```

**Kube-proxy 日志**

> Kube-proxy 通常以 DaemonSet 的方式部署，可以直接用 kubectl 查询其日志

```bash
kubectl -n kube-system get pod -l component=kube-proxy

kubectl -n kube-system logs ${pod_name}
```

**kubelet 日志**

日志文件： /var/log/kubelet/kubelet.log ，可以直接cat

> Kubelet 通常以 systemd 管理。查看 Kubelet 日志需要首先 SSH 登录到 Node 上，推荐使用 [kubectl-node-shell](https://github.com/kvaps/kubectl-node-shell) 

```bash
curl -LO https://github.com/kvaps/kubectl-node-shell/raw/master/kubectl-node_shell
chmod +x ./kubectl-node_shell
sudo mv ./kubectl-node_shell /usr/local/bin/kubectl-node_shell

kubectl node-shell <node>
journalctl -l -u kubelet
```

[排错官方指南](https://kubernetes.io/zh-cn/docs/tasks/debug/)

[个人笔记-详细排查指南](https://github.com/stonebirdjx/k8s-ladder/blob/master/k8s%E6%8E%92%E9%94%99%E6%95%B4%E7%90%86.md)

## 接下来就是比较重要的东西了

:point_right:[先了解k8s资源对象]()

## :point_right:pod的生命周期-健康检查-重启策略-容器探针

Pod 遵循一个预定义的生命周期，起始于 `Pending` 如果至少 其中有一个主要容器正常启动，则进入 `Running`，之后取决于 Pod 中是否有容器以 失败状态结束而进入 `Succeeded` 或者 `Failed` 阶段。

**pod阶段**

| 取值                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| `Pending`（悬决）   | Pod 已被 Kubernetes 系统接受，但有一个或者多个容器尚未创建亦未运行。此阶段包括等待 Pod 被调度的时间和通过网络下载镜像的时间。 |
| `Running`（运行中） | Pod 已经绑定到了某个节点，Pod 中所有的容器都已被创建。至少有一个容器仍在运行，或者正处于启动或重启状态。 |
| `Succeeded`（成功） | Pod 中的所有容器都已成功终止，并且不会再重启。               |
| `Failed`（失败）    | Pod 中的所有容器都已终止，并且至少有一个容器是因为失败终止。也就是说，容器以非 0 状态退出或者被系统终止。 |
| `Unknown`（未知）   | 因为某些原因无法取得 Pod 的状态。这种情况通常是因为与 Pod 所在主机通信失败。 |

:point_right: **容器状态**

一旦调度器将 Pod 分派给某个节点，`kubelet` 就通过容器运行时开始为 Pod 创建容器。 容器的状态有三种：`Waiting`（等待）、`Running`（运行中）和 `Terminated`（已终止）。

:point_right:**容器重启策略**

Pod 的 `spec` 中包含一个 `restartPolicy` 字段，其可能取值包括 Always、OnFailure 和 Never。默认值是 Always。

**pod状况**

Pod 有一个 PodStatus 对象，其中包含一个 [PodConditions](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.25/#podcondition-v1-core) 数组。Pod 可能通过也可能未通过其中的一些状况测试。

- `PodScheduled`：Pod 已经被调度到某节点；
- `ContainersReady`：Pod 中所有容器都已就绪；
- `Initialized`：所有的 [Init 容器](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/init-containers/) 都已成功完成；
- `Ready`：Pod 可以为请求提供服务，并且应该被添加到对应服务的负载均衡池中。

**:point_right:容器探针**

```
livenessProbe
```

指示容器是否正在运行。如果存活态探测失败，则 kubelet 会杀死容器， 并且容器将根据其重启策略决定未来。如果容器不提供存活探针， 则默认状态为 `Success`。

```
readinessProbe
```

指示容器是否准备好为请求提供服务。如果就绪态探测失败， 端点控制器将从与 Pod 匹配的所有服务的端点列表中删除该 Pod 的 IP 地址。 初始延迟之前的就绪态的状态值默认为 `Failure`。 如果容器不提供就绪态探针，则默认状态为 `Success`。

```
startupProbe
```

指示容器中的应用是否已经启动。如果提供了启动探针，则所有其他探针都会被 禁用，直到此探针成功为止。如果启动探测失败，`kubelet` 将杀死容器，而容器依其 重启策略进行重启。 如果容器没有提供启动探测，则默认状态为 `Success`。

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/pod-lifecycle/)

## :point_right:init 容器

Init 容器是一种特殊容器，在Pod内的应用容器启动之前运行。Init 容器可以包括一些应用镜像中不存在的实用工具和安装脚本

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/init-containers/)

## :point_right:Pod 水平自动扩缩(HPA)

在 Kubernetes 中，*HorizontalPodAutoscaler* 自动更新工作负载资源 （例如 Deployment或者 StatefulSet）， 目的是自动扩缩工作负载以满足需求。

**HorizontalPodAutoscaler 是如何工作的**

![https://github.com/stonebirdjx/static/blob/master/k8s-ladder/horizontal-pod-autoscaler.svg](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/horizontal-pod-autoscaler.svg)

[官方文档](https://kubernetes.io/zh-cn/docs/tasks/run-application/horizontal-pod-autoscale/)

## :point_right:service实现-拓扑感知

Kubernetes Service 是一种可以访问策略，通过selector能抽象能够解耦pod和ip的联系。防止pod变化带来的影响。

> 在 Kubernetes 集群中，每个 Node 运行一个 `kube-proxy` 进程。 `kube-proxy` 负责为 Service 实现了一种 VIP（虚拟 IP）的形式。

**userspace 代理模式**

对每个 Service，它会在本地 Node 上打开一个端口（随机选择）。 任何连接到“代理端口”的请求，都会被代理到 Service 的后端 `Pods` 中的某个上面（如 `Endpoints` 所报告的一样）。 使用哪个后端 Pod，是 kube-proxy 基于 `SessionAffinity` 来确定的。

**:point_right:iptables 代理模式**

对每个 Service，它会配置 iptables 规则，从而捕获到达该 Service 的 `clusterIP` 和端口的请求，进而将请求重定向到 Service 的一组后端中的某个 Pod 上面。 对于每个 Endpoints 对象，它也会配置 iptables 规则，这个规则会选择一个后端组合。

:point_right:**IPVS 代理模式**

在 `ipvs` 模式下，kube-proxy 监视 Kubernetes 服务和端点，调用 `netlink` 接口相应地创建 IPVS 规则， 并定期将 IPVS 规则与 Kubernetes 服务和端点同步。该控制循环可确保 IPVS 状态与所需状态匹配。访问服务时，IPVS 将流量定向到后端 Pod 之一。

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/services-networking/service/)

[拓扑感知](https://kubernetes.io/zh-cn/docs/concepts/services-networking/service-topology/)

## :point_right:Ingress原理 - ingress-nginx操作

Ingress 是对集群中服务的外部访问进行管理的 API 对象，典型的访问方式是 HTTP。

Ingress 可以提供负载均衡、SSL 终结和基于名称的虚拟托管。

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/ingress.svg)

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/)

## :point_right:DNS 服务 - coredns

可以使用一致的 DNS 名称而非 IP 地址访问 Service。

DNS 查询可以使用 Pod 中的 `/etc/resolv.conf` 展开。

```bash
<namespace>.svc.cluster.local
```

对于集群要访问其他的域名可以配置coredns的configmap

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/services-networking/dns-pod-service/#pod-dns-config)

## :point_right: 卷(volume) 

永久存放数据，多个pod共享。

使用卷时, 在 `.spec.volumes` 字段中设置为 Pod 提供的卷，并在 `.spec.containers[*].volumeMounts` 字段中声明卷在容器中的挂载位置。 

常用的有：cephfs、configMap、emptyDir、hostPath、nfs、secret

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/storage/volumes/)

## :point_right:pv/pvc

**持久卷（PersistentVolume，PV）** 是集群中的一块存储，可以由管理员事先制备， 或者使用存储类（Storage Class）来动态制备。

**持久卷申领（PersistentVolumeClaim，PVC）** 表达的是用户对存储的请求。概念上与 Pod 类似。 Pod 会耗用节点资源，而 PVC 申领会耗用 PV 资源。

PVC 申领与 PV 卷之间的绑定是一种一对一的映射，实现上使用 ClaimRef 来记述 PV 卷与 PVC 申领间的双向绑定关系。

**如果找不到匹配的 PV 卷，PVC 申领会无限期地处于未绑定状态。** 当与之匹配的 PV 卷可用时，PVC 申领会被绑定。 例如，即使某集群上制备了很多 50 Gi 大小的 PV 卷，也无法与请求 100 Gi 大小的存储的 PVC 匹配。当新的 100 Gi PV 卷被加入到集群时， 该 PVC 才有可能被绑定。

用户通过在 Pod 的 `volumes` 块中包含 `persistentVolumeClaim` 节区来调度 Pod，访问所申领的 PV 卷。 

**数据卷策略**

**保留（Retain）**

回收策略 `Retain` 使得用户可以手动回收资源。当 PersistentVolumeClaim 对象被删除时，PersistentVolume 卷仍然存在，对应的数据卷被视为"已释放（released）"。 由于卷上仍然存在这前一申领人的数据，该卷还不能用于其他申领。 管理员可以通过下面的步骤来手动回收该卷：

1. 删除 PersistentVolume 对象。与之相关的、位于外部基础设施中的存储资产 （例如 AWS EBS、GCE PD、Azure Disk 或 Cinder 卷）在 PV 删除之后仍然存在。
2. 根据情况，手动清除所关联的存储资产上的数据。
3. 手动删除所关联的存储资产。

如果你希望重用该存储资产，可以基于存储资产的定义创建新的 PersistentVolume 卷对象。

**删除（Delete）**

对于支持 `Delete` 回收策略的卷插件，删除动作会将 PersistentVolume 对象从 Kubernetes 中移除，同时也会从外部基础设施（如 AWS EBS、GCE PD、Azure Disk 或 Cinder 卷）中移除所关联的存储资产。 

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/storage/persistent-volumes/)

## :point_right:StorageClass

StorageClass 为管理员提供了描述存储 "类" 的方法。 不同的类型可能会映射到不同的服务质量等级或备份策略，或是由集群管理员制定的任意策略。

每个 StorageClass 都有一个制备器（Provisioner），用来决定使用哪个卷插件制备 PV。 该字段必须指定。

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
...
```

[官方文档](https://kubernetes.io/zh-cn/docs/concepts/storage/storage-classes/)

## :point_right:API 访问控制

- [AuthN](https://kubernetes.io/docs/reference/access-authn-authz/authentication/), [AuthZ](https://kubernetes.io/docs/reference/access-authn-authz/authorization/) & [Admission Control](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)
- [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [ServiceAccount](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)
- [Pod/Container SecurityContext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
- [Pod Security Policy (PSP)](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)

## :point_right:调度、抢占和驱逐

- [Pod/Node Affinity & Anti-affinity](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity)
- [Taint & Toleration](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)
- [Priority & Preemption](https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/)
- [Pod Disruption Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)



# 参考资料

:point_right:最好的资料是[官方文档](https://kubernetes.io/zh-cn/)

feisky 的博客：[Kubernetes 指南之核心原理](https://kubernetes.feisky.xyz/concepts/index)

feisky 的博客：[Kubernetes 集群排错指南](https://feisky.gitbooks.io/kubernetes/troubleshooting/)

[caicould](https://github.com/caicloud/kube-ladder)
