<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [k8s核心组件](#k8s%E6%A0%B8%E5%BF%83%E7%BB%84%E4%BB%B6)
  - [Master 节点](#master-%E8%8A%82%E7%82%B9)
    - [API Server](#api-server)
      - [:point_right:**访问方式**](#point_right%E8%AE%BF%E9%97%AE%E6%96%B9%E5%BC%8F)
    - [Etcd](#etcd)
    - [Controller Manager](#controller-manager)
      - [:point_right:**kube-controller-manager 由一系列的控制器组成**](#point_rightkube-controller-manager-%E7%94%B1%E4%B8%80%E7%B3%BB%E5%88%97%E7%9A%84%E6%8E%A7%E5%88%B6%E5%99%A8%E7%BB%84%E6%88%90)
      - [:point_right:**metrics**](#point_rightmetrics)
      - [:point_right:node 驱逐](#point_rightnode-%E9%A9%B1%E9%80%90)
    - [Scheduler](#scheduler)
      - [:point_right:**指定节点调度**](#point_right%E6%8C%87%E5%AE%9A%E8%8A%82%E7%82%B9%E8%B0%83%E5%BA%A6)
      - [:point_right:**Taints 和 tolerations**](#point_righttaints-%E5%92%8C-tolerations)
      - [:point_right:优先级调度](#point_right%E4%BC%98%E5%85%88%E7%BA%A7%E8%B0%83%E5%BA%A6)
      - [多调度器](#%E5%A4%9A%E8%B0%83%E5%BA%A6%E5%99%A8)
  - [Node 节点](#node-%E8%8A%82%E7%82%B9)
    - [Kubelet](#kubelet)
      - [:point_right:node节点自注册和节点状态更新](#point_rightnode%E8%8A%82%E7%82%B9%E8%87%AA%E6%B3%A8%E5%86%8C%E5%92%8C%E8%8A%82%E7%82%B9%E7%8A%B6%E6%80%81%E6%9B%B4%E6%96%B0)
      - [:point_right:pod 管理](#point_rightpod-%E7%AE%A1%E7%90%86)
      - [:point_right:容器健康检查](#point_right%E5%AE%B9%E5%99%A8%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
      - [:point_right:查询 Node 汇总指标](#point_right%E6%9F%A5%E8%AF%A2-node-%E6%B1%87%E6%80%BB%E6%8C%87%E6%A0%87)
    - [Kube-proxy](#kube-proxy)
  - [**kube-dns**](#kube-dns)
    - [CoreDNS](#coredns)
  - [Network plugin](#network-plugin)
- [k8s资源对象](#k8s%E8%B5%84%E6%BA%90%E5%AF%B9%E8%B1%A1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# k8s核心组件

## Master 节点

k8s集群大脑，所有集群的控制命令都传递给Master组件并在其上执行。每个k8s集群至少有一套Master组件（生产环境一般大于2个）。每个Master组件包括三个核心组件（API Server, Scheduler, Controller Manager）和集群数据配置中心ETCD。

### API Server

kube-apiserver 支持同时提供 https（默认监听在 6443 端口）和 http API（默认监听在 127.0.0.1 的 8080 端口），其中 http API 是非安全接口，不做任何认证授权机制，不建议生产环境启用。

作用

- 集群控制，所有资源增删改查的唯一入口，提供RESTful API控制k8s集群
- 集群内各个组件之间数据交互的通信中枢
- 提供集群控制安全机制（身份认证、授权和访问控制）

可通过 kubectl api-versions 和 kubectl api-resources 查询 Kubernetes API 支持的 API 版本以及资源对象。

```bash
kubectl api-versions
api-resources
```

#### :point_right:**访问方式**

```bash
# kubectl
kubectl get --raw /api/v1/namespaces
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes
kubectl get --raw /apis/metrics.k8s.io/v1beta1/pods

# kubectl proxy
kubectl proxy --port=8080 &
curl http://localhost:8080/api/

# 各语言sdk: go, python, java
```

### Etcd

Etcd 是 CoreOS 基于 Raft 开发的分布式 key-value 存储，可用于服务发现、共享配置以及一致性保障（如数据库选主、分布式锁等）。

作用

- k8s集群主数据库，存储着所有资源对象及状态
- 默认与Master组件部署于同一个Node上
- 所有数据变更都是通过API Server进行的

:point_right:etcd常用命令

```bash
# 帮助命令
./etcdctl -h

# set 设置键（或者叫主题）的值
./etcdctl set /root/test/keyOne "Hello etcd"

# get 获取给定键的值，如果尝试获取不存的值时会报错
./etcdctl get /root/test/keyOne   

# update 更新给定键中存储的值，如果尝试更新不存在的值时会报错
./etcdctl update /root/test/keyOne "Hello World"  

# rm 删除给定的键，如果命令参数中给定的键不存在则会报错
./etcdctl rm /root/test/keyOne

# 键值库操作
set
get
update
rm
setdir
updatedir
ls
行为操作
backup
watch
exec-watch
member
```



### Controller Manager

Controller Manager 由 kube-controller-manager 和 cloud-controller-manager 组成，是 Kubernetes 的大脑，它通过 apiserver 监控整个集群的状态，并确保集群处于预期的工作状态。

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/post-ccm-arch.png)

作用

- k8s所有资源对象的“大总管”
- 是k8s里所有资源对象的自动化控制中心（故障检测、自动扩展、滚动更新）
- 集群内各种资源controller（Deployment, RC / ReplicaSet, StatefulSet, DaemonSet等）的核心管理者
- 针对每一种具体资源都有对应的controller
- 保证其下管理的每个controller所对应的资源始终处于“期望状态”

#### :point_right:**kube-controller-manager 由一系列的控制器组成**

```bash
Replication Controller
Node Controller
CronJob Controller
Daemon Controller
Deployment Controller
Endpoint Controller
Garbage Collector
Namespace Controller
Job Controller
Pod AutoScaler
RelicaSet
Service Controller
ServiceAccount Controller
StatefulSet Controller
Volume Controller
Resource quota Controller
```

#### :point_right:**metrics** 

Controller manager metrics 提供了控制器内部逻辑的性能度量，如 Go 语言运行时度量、etcd 请求延时、云服务商 API 请求延时、云存储请求延时等。Controller manager metrics 默认监听在 `kube-controller-manager` 的 10252 端口，提供 Prometheus 格式的性能度量数据，可以通过 `http://localhost:10252/metrics` 来访问。

#### :point_right:node 驱逐

当node节点状态异常时，pod去驱逐到其他节点

### Scheduler

kube-scheduler 负责分配调度 Pod 到集群内的节点上，它监听 kube-apiserver，查询还未分配 Node 的 Pod，然后根据调度策略为这些 Pod 分配节点（更新 Pod 的 `NodeName` 字段）。

调度器需要充分考虑诸多的因素：

- 公平调度
- 资源高效利用
- QoS
- affinity 和 anti-affinity
- 数据本地化（data locality）
- 内部负载干扰（inter-workload interference）
- deadlines

#### :point_right:**指定节点调度**

- nodeSelector：只调度到匹配指定 label 的 Node 上
- nodeAffinity：功能更丰富的 Node 选择器，比如支持集合操作
- podAffinity：调度到满足条件的 Pod 所在的 Node 上

#### :point_right:**Taints 和 tolerations**

Taints 和 tolerations 用于保证 Pod 不被调度到不合适的 Node 上，**其中 Taint 应用于 Node 上，而 toleration 则应用于 Pod 上**。

目前支持的 taint 类型

- NoSchedule：新的 Pod 不调度到该 Node 上，不影响正在运行的 Pod
- PreferNoSchedule：soft 版的 NoSchedule，尽量不调度到该 Node 上
- NoExecute：新的 Pod 不调度到该 Node 上，并且删除（evict）已在运行的 Pod。Pod 可以增加一个时间（tolerationSeconds）

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
kubectl taint nodes node1 key1=value1:NoExecute
kubectl taint nodes node1 key2=value2:NoSchedule

# Pod 没有 toleratekey2=value2:NoSchedule 无法调度到 node1 上
```

 #### :point_right:优先级调度

在 Pod的Spec 中通过 PriorityClassName 设置 Pod 的优先级

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  priorityClassName: high-priority

--- # 需要先定义
apiVersion: v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class should be used for XYZ service pods only."
```

#### 多调度器

如果默认的调度器不满足要求，还可以部署自定义的调度器。并且，在整个集群中还可以同时运行多个调度器实例，通过 `podSpec.schedulerName` 来选择使用哪一个调度器（默认使用内置的调度器）。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  # 选择使用自定义调度器 my-scheduler
  schedulerName: my-scheduler
  containers:
  - name: nginx
    image: nginx:1.10
```

## Node 节点

### Kubelet

每个Node节点上都运行一个 Kubelet 服务进程，默认监听 10250 端口，接收并执行 Master 发来的指令，管理 Pod 及 Pod 中的容器。每个 Kubelet 进程会在 API Server 上注册所在Node节点的信息，定期向 Master 节点汇报该节点的资源使用情况，并通过 cAdvisor 监控节点和容器的资源。

:point_right:/var/log/kubenetes/kubelet.log

#### :point_right:node节点自注册和节点状态更新

Kubelet 在启动时通过 API Server 注册节点信息，并定时向 API Server 发送节点新消息，API Server 在接收到新消息后，将信息写入 etcd

#### :point_right:pod 管理

kubelet 采用一组通过各种机制提供的 PodSpecs（主要通过 apiserver），并确保这些 PodSpecs 中描述的 Pod 正常健康运行。

#### :point_right:容器健康检查

-  `LivenessProbe `探针：用于判断容器是否健康，告诉 Kubelet 一个容器什么时候处于不健康的状态。如果 LivenessProbe 探针探测到容器不健康，则 Kubelet 将删除该容器，并根据容器的重启策略做相应的处理。如果一个容器不包含 LivenessProbe 探针，那么 Kubelet 认为该容器的 LivenessProbe 探针返回的值永远是 “Success”；
- `ReadinessProbe`：用于判断容器是否启动完成且准备接收请求。如果 ReadinessProbe 探针探测到失败，则 Pod 的状态将被修改。Endpoint Controller 将从 Service 的 Endpoint 中删除包含该容器所在 Pod 的 IP 地址的 Endpoint 条目。

#### :point_right:查询 Node 汇总指标

在集群内部可以直接访问 kubelet 的 10255 端口，比如 `http://<node-name>:10255/stats/summary`

### Kube-proxy

kube-proxy是集群中每个节点所上运行的网络代理， 实现 Kubernetes 服务（Service）概念的一部分。

kube-proxy 维护节点上的一些网络规则， 这些网络规则会允许从集群内部或外部的网络会话与 Pod 进行网络通信。

如果操作系统提供了可用的数据包过滤层，则 kube-proxy 会通过它来实现网络规则。 否则，kube-proxy 仅做流量转发。

## **kube-dns**

DNS 是 Kubernetes 的核心功能之一，通过 kube-dns 或 CoreDNS 作为集群的必备扩展来提供命名服务。

### CoreDNS 

从 v1.13 开始成为默认 DNS 服务。CoreDNS 的特点是效率更高，资源占用率更小，推荐使用 CoreDNS 替代 kube-dns 为集群提供 DNS 服务。

## Network plugin

默认情况使用`flannel` 或者 `calico`即可

- cni bridge
- flannel
- calico

# k8s资源对象

- workload工作负载类：pod, controller(Deployment, StatefulSet, DaemonSet, Job...)
- Discovery & load balance服务功能类：service, endpoint, ingress...
- Config & storage配置存储类：configmap, secret, volume, persistentVolumeClaim...
- Cluster集群类：node, namespaces, persistentVolume, clusterRole, clusterRoleBinding, resouceQuota...

[官方组件文档](https://kubernetes.io/zh-cn/docs/reference/command-line-tools-reference/kubelet/)

