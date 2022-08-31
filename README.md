<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [k8s-ladder](#k8s-ladder)
  - [概念](#%E6%A6%82%E5%BF%B5)
  - [了解常用资源和概念](#%E4%BA%86%E8%A7%A3%E5%B8%B8%E7%94%A8%E8%B5%84%E6%BA%90%E5%92%8C%E6%A6%82%E5%BF%B5)
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
  - [k8s核心组件](#k8s%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6)
  - [Container Runtime](#container-runtime)
  - [安装k8s](#%E5%AE%89%E8%A3%85k8s)
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

## k8s核心组件

[官方组件说明](https://kubernetes.io/zh-cn/docs/concepts/overview/components/)

[个人组件记录](https://github.com/stonebirdjx/k8s-ladder/blob/master/k8s%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6.md)

- master组件：etcd、api server、controller
- node组件：kubelet、kube-proxy
- 网络插件：flannel、calico

:point_right:了解基本组件、基本功能

## Container Runtime

- containerd
- docker

:point_right:容器运行时接口（CRI），能熟练的使用docker、containerd相关命令

## 安装k8s

- kubeadm
- rke
- kubespray
- 手动安装





# 参考资料

:point_right:最好的资料是[官方文档](https://kubernetes.io/zh-cn/)

[caicould](https://github.com/caicloud/kube-ladder)
