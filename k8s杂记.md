<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [标签选择器](#%E6%A0%87%E7%AD%BE%E9%80%89%E6%8B%A9%E5%99%A8)
- [字段选择器](#%E5%AD%97%E6%AE%B5%E9%80%89%E6%8B%A9%E5%99%A8)
- [使用 Finalizers 控制删除。](#%E4%BD%BF%E7%94%A8-finalizers-%E6%8E%A7%E5%88%B6%E5%88%A0%E9%99%A4)
- [属主ownerReferences](#%E5%B1%9E%E4%B8%BBownerreferences)
- [推荐使用的标签](#%E6%8E%A8%E8%8D%90%E4%BD%BF%E7%94%A8%E7%9A%84%E6%A0%87%E7%AD%BE)
- [节点管理](#%E8%8A%82%E7%82%B9%E7%AE%A1%E7%90%86)
  - [添加节点](#%E6%B7%BB%E5%8A%A0%E8%8A%82%E7%82%B9)
  - [设置节点不可调度](#%E8%AE%BE%E7%BD%AE%E8%8A%82%E7%82%B9%E4%B8%8D%E5%8F%AF%E8%B0%83%E5%BA%A6)
  - [节点与控制面通信](#%E8%8A%82%E7%82%B9%E4%B8%8E%E6%8E%A7%E5%88%B6%E9%9D%A2%E9%80%9A%E4%BF%A1)
- [kube-controller-manager ](#kube-controller-manager)
- [**租约（Lease）**](#%E7%A7%9F%E7%BA%A6lease)
- [容器运行时](#%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C%E6%97%B6)
- [垃圾收集](#%E5%9E%83%E5%9C%BE%E6%94%B6%E9%9B%86)
  - [已完成的 Job](#%E5%B7%B2%E5%AE%8C%E6%88%90%E7%9A%84-job)
- [镜像拉取策略](#%E9%95%9C%E5%83%8F%E6%8B%89%E5%8F%96%E7%AD%96%E7%95%A5)
- [**Runtime Class**](#runtime-class)
- [**Init 容器**](#init-%E5%AE%B9%E5%99%A8)
- [临时容器](#%E4%B8%B4%E6%97%B6%E5%AE%B9%E5%99%A8)
- [PODs目录](#pods%E7%9B%AE%E5%BD%95)
- [ReplicaSet - 不会自动更新](#replicaset---%E4%B8%8D%E4%BC%9A%E8%87%AA%E5%8A%A8%E6%9B%B4%E6%96%B0)
- [StatefulSet](#statefulset)
- [**ReplicationController**](#replicationcontroller)
- [Service](#service)
  - [EndpointSlices](#endpointslices)
  - [Endpoints ](#endpoints)
  - [服务发现环境变量](#%E6%9C%8D%E5%8A%A1%E5%8F%91%E7%8E%B0%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [Headless Services - 无头服务](#headless-services---%E6%97%A0%E5%A4%B4%E6%9C%8D%E5%8A%A1)
  - [发布服务类型](#%E5%8F%91%E5%B8%83%E6%9C%8D%E5%8A%A1%E7%B1%BB%E5%9E%8B)
  - [外部IP](#%E5%A4%96%E9%83%A8ip)
- [Ingress](#ingress)
- [**网络策略**](#%E7%BD%91%E7%BB%9C%E7%AD%96%E7%95%A5)
- [**Service 与** **Pod** **的** **DNS**](#service-%E4%B8%8E-pod-%E7%9A%84-dns)
  - [Pod 的 hostname 和 subdomain 字段](#pod-%E7%9A%84-hostname-%E5%92%8C-subdomain-%E5%AD%97%E6%AE%B5)
  - [Pod 的 DNS 配置](#pod-%E7%9A%84-dns-%E9%85%8D%E7%BD%AE)
  - [IPv4/IPv6 双协议栈](#ipv4ipv6-%E5%8F%8C%E5%8D%8F%E8%AE%AE%E6%A0%88)
  - [HostAliases 增加额外条目](#hostaliases-%E5%A2%9E%E5%8A%A0%E9%A2%9D%E5%A4%96%E6%9D%A1%E7%9B%AE)
- [Volume](#volume)
  - [emptyDir](#emptydir)
  - [hostPath](#hostpath)
  - [local - storage](#local---storage)
  - [subPath](#subpath)
- [PV&PVC](#pvpvc)
- [StorageClass&**provisioner**](#storageclassprovisioner)
- [VolumeSnapshot - 卷快照](#volumesnapshot---%E5%8D%B7%E5%BF%AB%E7%85%A7)
- [ConfigMap&Secret](#configmapsecret)
- [为 Pod 和容器管理资源](#%E4%B8%BA-pod-%E5%92%8C%E5%AE%B9%E5%99%A8%E7%AE%A1%E7%90%86%E8%B5%84%E6%BA%90)
- [Kubeconfig](#kubeconfig)
- [云原生安全的 4C](#%E4%BA%91%E5%8E%9F%E7%94%9F%E5%AE%89%E5%85%A8%E7%9A%84-4c)
- [**Pod** **安全性标准**](#pod-%E5%AE%89%E5%85%A8%E6%80%A7%E6%A0%87%E5%87%86)
- [k8s **API** **访问控制**](#k8s-api-%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
- [LimitRange - 限制范围](#limitrange---%E9%99%90%E5%88%B6%E8%8C%83%E5%9B%B4)
- [**资源配额**](#%E8%B5%84%E6%BA%90%E9%85%8D%E9%A2%9D)
- [nodeSelector  选择节点](#nodeselector--%E9%80%89%E6%8B%A9%E8%8A%82%E7%82%B9)
- [**污点和容忍度**](#%E6%B1%A1%E7%82%B9%E5%92%8C%E5%AE%B9%E5%BF%8D%E5%BA%A6)
- [**Pod** **优先级和抢占**](#pod-%E4%BC%98%E5%85%88%E7%BA%A7%E5%92%8C%E6%8A%A2%E5%8D%A0)
- [Kubectl  proxy](#kubectl--proxy)
- [apiserver proxy](#apiserver-proxy)
- [**API** **优先级和公平性**](#api-%E4%BC%98%E5%85%88%E7%BA%A7%E5%92%8C%E5%85%AC%E5%B9%B3%E6%80%A7)
  - [调试端点 ](#%E8%B0%83%E8%AF%95%E7%AB%AF%E7%82%B9)
- [**手动生成证书**](#%E6%89%8B%E5%8A%A8%E7%94%9F%E6%88%90%E8%AF%81%E4%B9%A6)
- [ResourceQuota 用于限制命名空间的总资源消耗](#resourcequota-%E7%94%A8%E4%BA%8E%E9%99%90%E5%88%B6%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E7%9A%84%E6%80%BB%E8%B5%84%E6%BA%90%E6%B6%88%E8%80%97)
- [LimitRange 用于管理项目中 pod 和容器级别的约束](#limitrange-%E7%94%A8%E4%BA%8E%E7%AE%A1%E7%90%86%E9%A1%B9%E7%9B%AE%E4%B8%AD-pod-%E5%92%8C%E5%AE%B9%E5%99%A8%E7%BA%A7%E5%88%AB%E7%9A%84%E7%BA%A6%E6%9D%9F)
  - [**配置****pod** **的cpu、内存的最大最小默认值**](#%E9%85%8D%E7%BD%AEpod-%E7%9A%84cpu%E5%86%85%E5%AD%98%E7%9A%84%E6%9C%80%E5%A4%A7%E6%9C%80%E5%B0%8F%E9%BB%98%E8%AE%A4%E5%80%BC)
- [*Cloud-controller-manager 云控制器*](#cloud-controller-manager-%E4%BA%91%E6%8E%A7%E5%88%B6%E5%99%A8)
- [网络策略](#%E7%BD%91%E7%BB%9C%E7%AD%96%E7%95%A5)
- [**为系统****守护进程****预留资源**](#%E4%B8%BA%E7%B3%BB%E7%BB%9F%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B%E9%A2%84%E7%95%99%E8%B5%84%E6%BA%90)
- [**安全地清空一个节点**](#%E5%AE%89%E5%85%A8%E5%9C%B0%E6%B8%85%E7%A9%BA%E4%B8%80%E4%B8%AA%E8%8A%82%E7%82%B9)
- [**Kubernetes** **集群中使用 sysctl**](#kubernetes-%E9%9B%86%E7%BE%A4%E4%B8%AD%E4%BD%BF%E7%94%A8-sysctl)
- [**配置存活、就绪和启动探针**](#%E9%85%8D%E7%BD%AE%E5%AD%98%E6%B4%BB%E5%B0%B1%E7%BB%AA%E5%92%8C%E5%90%AF%E5%8A%A8%E6%8E%A2%E9%92%88)
- [**为容器的生命周期事件设置处理函数**](#%E4%B8%BA%E5%AE%B9%E5%99%A8%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%BA%8B%E4%BB%B6%E8%AE%BE%E7%BD%AE%E5%A4%84%E7%90%86%E5%87%BD%E6%95%B0)
- [Pod 安全性准入](#pod-%E5%AE%89%E5%85%A8%E6%80%A7%E5%87%86%E5%85%A5)
- [YAML2JSON](#yaml2json)
- [DIFF](#diff)
- [**用插件扩展 kubectl**](#%E7%94%A8%E6%8F%92%E4%BB%B6%E6%89%A9%E5%B1%95-kubectl)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 标签选择器

```Bash
kubectl get pods -l environment=production,tier=frontend
```

# 字段选择器

```Bash
kubectl get pods --field-selector status.phase=Running
```

# 使用 Finalizers 控制删除。

```Bash
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: mymap
  finalizers:
  - kubernetes
EOF

kubectl delete configmap/mymap &

# 查看cm详情
kubectl get configmap/mymap -o yaml
apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: "2020-10-22T21:30:18Z"
  deletionGracePeriodSeconds: 0
  deletionTimestamp: "2020-10-22T21:30:34Z"
  finalizers:
  - kubernetes
  name: mymap
  namespace: default
  resourceVersion: "311456"
  selfLink: /api/v1/namespaces/default/configmaps/mymap
  uid: 93a37fed-23e3-45e8-b6ee-b2521db81638
  

#  patch 删除finalizers后正常删除
kubectl patch configmap/mymap \
    --type json \
    --patch='[ { "op": "remove", "path": "/metadata/finalizers" } ]'
```

# 属主ownerReferences

附属对象有一个 `metadata.ownerReferences` 字段，用于引用其属主对象。

# 推荐使用的标签

- app.kubernetes.io/name        应用程序的名称        
- app.kubernetes.io/instance        用于唯一确定应用实例的名称        
- app.kubernetes.io/version        应用程序的当前版本（例如语义版本 1.0、修订版哈希等）    
- app.kubernetes.io/component        架构中的组件       
- app.kubernetes.io/part-of        此级别的更高级别应用程序的名称     
- app.kubernetes.io/managed-by        用于管理应用程序的工具        

```YAML
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app.kubernetes.io/name: mysql
    app.kubernetes.io/instance: mysql-abcxzy
    app.kubernetes.io/version: "5.7.21"
    app.kubernetes.io/component: database
    app.kubernetes.io/part-of: wordpress
    app.kubernetes.io/managed-by: helm
```

# 节点管理

节点上的组件包括 `kubelet`、 容器运行时(`containerd`)以及 `kube-proxy`。

## 添加节点

- 节点上的 kubelet 向控制面执行自注册；
- 你（或者别的什么人）手动添加一个 Node 对象。

```Bash
{
  "kind": "Node",
  "apiVersion": "v1",
  "metadata": {
    "name": "10.240.79.157",
    "labels": {
      "name": "my-first-k8s-node"    }
  }
}
```

**kubelet****自注册**

当 kubelet 标志 --register-node 为 true（默认）时，它会尝试向 API 服务注册自己。 这是首选模式，被绝大多数发行版选用。

对于自注册模式，kubelet 使用下列参数启动：

- --kubeconfig - 用于向 API 服务器执行身份认证所用的凭据的路径。
- --cloud-provider - 与某云驱动 进行通信以读取与自身相关的元数据的方式。
- `--register-node` - 自动向 API 服务注册。
- --register-with-taints - 使用所给的污点列表 （逗号分隔的 <key>=<value>:<effect>）注册节点。当 register-node 为 false 时无效。
- --node-ip - 节点 IP 地址。
- --node-labels - 在集群中注册节点时要添加的标签。 （参见 NodeRestriction 准入控制插件所实施的标签限制）。
- --node-status-update-frequency - 指定 kubelet 向 API 服务器发送其节点状态的频率。

```Bash
root@kube-control-2 ~]# systemctl cat kubelet.service 
# /etc/systemd/system/kubelet.service
[Unit]
Description=Kubernetes Kubelet Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=containerd.service
Wants=containerd.service

[Service]
User=root
EnvironmentFile=-/etc/kubernetes/kubelet.env
ExecStart=/bin/sh -c "exec /usr/local/bin/kubelet \
                $KUBE_LOGTOSTDERR \
                $KUBE_LOG_LEVEL \
                $KUBELET_API_SERVER \
                $KUBELET_ADDRESS \
                $KUBELET_PORT \
                $KUBELET_HOSTNAME \
                $KUBELET_ARGS \
                $DOCKER_SOCKET \
                $KUBELET_NETWORK_PLUGIN \
                $KUBELET_VOLUME_PLUGIN \
                $KUBELET_CLOUDPROVIDER \
                >> /var/log/kubelet/kubelet.log 2>&1"
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

## 设置节点不可调度

```Bash
kubectl cordon ${node_name}

# 取消 不可调度状态
kubectl uncordon ${node_name}
```

## 节点与控制面通信

当 Pod 被实例化时，配置了一个虚拟 IP 地址， 用于（通过 `kube-proxy`）转发请求到 API 服务器的 HTTPS 末端。

> 从集群节点和节点上运行的 Pod 到控制面的连接的缺省操作模式即是安全的， 能够在不可信的网络或公网上运行。

# kube-controller-manager[ ](https://kubernetes.io/zh-cn/docs/concepts/overview/components/#kube-controller-manager)

kube-controller-manager 是控制平面的组件， 负责运行控制器进程。

从逻辑上讲， 每个控制器都是一个单独的进程， 但是为了降低复杂性，它们都被编译到同一个可执行文件，并在同一个进程中运行。

这些控制器包括：

- 节点控制器（Node Controller）：负责在节点出现故障时进行通知和响应
- 任务控制器（Job Controller）：监测代表一次性任务的 Job 对象，然后创建 Pods 来运行这些任务直至完成
- 端点分片控制器（EndpointSlice controller）：填充端点分片（EndpointSlice）对象（以提供 Service 和 Pod 之间的链接）。
- 服务账号控制器（ServiceAccount controller）：为新的命名空间创建默认的服务账号（ServiceAccount）。

# **租约（Lease）**

在 Kubernetes 中，租约概念表示为 `coordination.k8s.io`API 组中的 Lease 对象， 常用于类似节点心跳和组件级领导者选举等系统核心能力。

对于每个 `Node`，在 `kube-node-lease` 名字空间中都有一个具有匹配名称的 `Lease` 对象。 在此基础上，每个 kubelet 心跳都是对该 `Lease` 对象的 **update** 请求，更新该 Lease 的 `spec.renewTime` 字段。 Kubernetes 控制平面使用此字段的时间戳来确定此 `Node` 的可用性。

```Bash
kubectl -n kube-node-lease  get leases.coordination.k8s.io 192.168.0.190 -o yaml
apiVersion: coordination.k8s.io/v1
kind: Lease
metadata:
  creationTimestamp: "2023-02-18T04:21:03Z"
  managedFields:
  - apiVersion: coordination.k8s.io/v1
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:ownerReferences:
          .: {}
          k:{"uid":"a6f4d4c2-d926-4c3f-abc3-50a9473f3394"}:
            .: {}
            f:apiVersion: {}
            f:kind: {}
            f:name: {}
            f:uid: {}
      f:spec:
        f:holderIdentity: {}
        f:leaseDurationSeconds: {}
        f:renewTime: {}
    manager: kubelet
    operation: Update
    time: "2023-02-18T04:21:03Z"
  name: 192.168.0.190
  namespace: kube-node-lease
  ownerReferences:
  - apiVersion: v1
    kind: Node
    name: 192.168.0.190
    uid: a6f4d4c2-d926-4c3f-abc3-50a9473f3394
  resourceVersion: "98330499"
  uid: ea89ed55-fb96-4cfb-8abd-113f5b4151dc
spec:
  holderIdentity: 192.168.0.190
  leaseDurationSeconds: 40
  renewTime: "2023-04-13T03:44:49.990396Z"
```

# 容器运行时

可以使用 `--image-service-endpoint` 和 `--container-runtime-endpoint` [命令行标志](https://kubernetes.io/zh-cn/docs/reference/command-line-tools-reference/kubelet)在 kubelet 中单独配置。

# 垃圾收集

- [终止的 Pod](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/pod-lifecycle/#pod-garbage-collection)

## 已完成的 Job

TTL-after-finished 控制器只支持 Job。你可以通过指定 Job 的 `.spec.ttlSecondsAfterFinished` 字段来自动清理已结束的 Job（`Complete` 或 `Failed`）

```Bash
apiVersion: batch/v1
kind: Job
metadata:
  name: pi-with-ttl
spec:
  ttlSecondsAfterFinished: 100
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```

- [不再存在属主引用的对象](https://kubernetes.io/zh-cn/docs/concepts/architecture/garbage-collection/#owners-dependents)
- [未使用的容器和容器镜像](https://kubernetes.io/zh-cn/docs/concepts/architecture/garbage-collection/#containers-images)
- [动态制备的、StorageClass 回收策略为 Delete 的 PV 卷](https://kubernetes.io/zh-cn/docs/concepts/storage/persistent-volumes/#delete)
- [阻滞或者过期的 CertificateSigningRequest (CSR)](https://kubernetes.io/zh-cn/docs/reference/access-authn-authz/certificate-signing-requests/#request-signing-process)
- 在以下情形中删除了的[节点](https://kubernetes.io/zh-cn/docs/concepts/architecture/nodes/)对象：
  - 当集群使用[云控制器管理器](https://kubernetes.io/zh-cn/docs/concepts/architecture/cloud-controller/)运行于云端时；
  - 当集群使用类似于云控制器管理器的插件运行在本地环境中时。
- [节点租约对象](https://kubernetes.io/zh-cn/docs/concepts/architecture/nodes/#heartbeats)

# 镜像拉取策略

- **`IfNotPresent`**只有当镜像在本地不存在时才会拉取。
- **`Always`**每当 kubelet 启动一个容器时，kubelet 会查询容器的镜像仓库， 将名称解析为一个镜像[摘要](https://docs.docker.com/engine/reference/commandline/pull/#pull-an-image-by-digest-immutable-identifier)。 如果 kubelet 有一个容器镜像，并且对应的摘要已在本地缓存，kubelet 就会使用其缓存的镜像； 否则，kubelet 就会使用解析后的摘要拉取镜像，并使用该镜像来启动容器。
- **`Never`**Kubelet 不会尝试获取镜像。如果镜像已经以某种方式存在本地， kubelet 会尝试启动容器；否则，会启动失败。 更多细节见[提前拉取镜像](https://kubernetes.io/zh-cn/docs/concepts/containers/images/#pre-pulled-images)。

# **Runtime Class**

RuntimeClass 的配置依赖于运行时接口（CRI）的实现。 根据你使用的 CRI 实现

```YAML
# RuntimeClass 定义于 node.k8s.io API 组
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  # 用来引用 RuntimeClass 的名字
  # RuntimeClass 是一个集群层面的资源
  name: myclass
# 对应的 CRI 配置的名称
handler: myconfiguration

# 使用
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  runtimeClassName: myclass
  # ...

# CRI 配置 /etc/containerd/config.toml 配置文件来配置运行时 handler
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.${HANDLER_NAME}]

# CRI-O 通过 CRI-O 的 /etc/crio/crio.conf 配置文件来配置运行时 handler
[crio.runtime.runtimes.${HANDLER_NAME}]
  runtime_path = "${PATH_TO_BINARY}"
```

# **Init 容器**

Init 容器是一种特殊容器，在 [Pod](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/) 内的应用容器启动之前运行。Init 容器可以包括一些应用镜像中不存在的实用工具和安装脚本。

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  # 这些容器在 Pod 初始化期间运行
  initContainers:
  - name: install
    image: busybox:1.28
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - http://info.cern.ch
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}
```

# 临时容器

临时容器：一种特殊的容器，该容器在现有 [Pod](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/) 中临时运行，以便完成用户发起的操作，例如故障排查。

- 临时容器没有端口配置，因此像 `ports`、`livenessProbe`、`readinessProbe` 这样的字段是不允许的。
- Pod 资源分配是不可变的，因此 `resources` 配置是不允许的。
- 有关允许字段的完整列表，请参见 [EphemeralContainer 参考文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/#ephemeralcontainer-v1-core)。

```Bash
kubectl run ephemeral-demo --image=registry.k8s.io/pause:3.1 --restart=Never

# 使用 kubectl debug 添加调试容器。
kubectl debug -it ephemeral-demo --image=busybox:1.28 --target=ephemeral-demo
```

# PODs目录

/var/lib/kubelet/pods/

# ReplicaSet - 不会自动更新

我们建议使用 Deployment 而不是直接使用 ReplicaSet， 除非你需要自定义更新业务流程或根本不需要更新。

# StatefulSet 

StatefulSet 对于需要满足以下一个或多个需求的应用程序很有价值：

- 稳定的、唯一的网络标识符。
- 稳定的、持久的存储。
- 有序的、优雅的部署和扩缩。
- 有序的、自动的滚动更新。

# **ReplicationController**

**ReplicationController** 确保在任何时候都有特定数量的 Pod 副本处于运行状态。

# Service

## EndpointSlices

如果定义时没有选择算符，可以通过手动添加 EndpointSlice 对象，将服务映射到运行该服务的网络地址和端口：

```YAML
apiVersion: discovery.k8s.io/v1
kind: EndpointSlice
metadata:
  name: my-service-1 # 按惯例将服务的名称用作 EndpointSlice 名称的前缀
  labels:
    # 你应设置 "kubernetes.io/service-name" 标签。
    # 设置其值以匹配服务的名称
    kubernetes.io/service-name: my-service
addressType: IPv4
ports:
  - name: '' # 留空，因为 port 9376 未被 IANA 分配为已注册端口
    appProtocol: http
    protocol: TCP
    port: 9376
endpoints:
  - addresses:
      - "10.4.5.6" # 此列表中的 IP 地址可以按任何顺序显示
      - "10.1.2.3"
```

## Endpoints[ ](https://kubernetes.io/zh-cn/docs/concepts/services-networking/service/#endpoints)

## 服务发现环境变量

kubelet 会为每个活跃的 Service 添加一组环境变量。 kubelet 为 Pod 添加环境变量 `{SVCNAME}_SERVICE_HOST` 和 `{SVCNAME}_SERVICE_PORT`。 这里 Service 的名称需大写，横线被转换成下划线。

```Bash
REDIS_PRIMARY_SERVICE_HOST=10.0.0.11
REDIS_PRIMARY_SERVICE_PORT=6379REDIS_PRIMARY_PORT=tcp://10.0.0.11:6379
REDIS_PRIMARY_PORT_6379_TCP=tcp://10.0.0.11:6379
REDIS_PRIMARY_PORT_6379_TCP_PROTO=tcp
REDIS_PRIMARY_PORT_6379_TCP_PORT=6379REDIS_PRIMARY_PORT_6379_TCP_ADDR=10.0.0.11
```

## Headless Services - 无头服务

有时不需要或不想要负载均衡，以及单独的 Service IP。 遇到这种情况，可以通过指定 Cluster IP（`spec.clusterIP`）的值为 `"None"` 来创建 `Headless` Service。

## 发布服务类型

- `ClusterIP`：通过集群的内部 IP 暴露服务，选择该值时服务只能够在集群内部访问。 这也是你没有为服务显式指定 `type` 时使用的默认值。 你可以使用 [Ingress](https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/) 或者 [Gateway API](https://gateway-api.sigs.k8s.io/) 向公众暴露服务。
- `NodePort`：通过每个节点上的 IP 和静态端口（`NodePort`）暴露服务。 为了让节点端口可用，Kubernetes 设置了集群 IP 地址，这等同于你请求 `type: ClusterIP` 的服务。
- `LoadBalancer`：使用云提供商的负载均衡器向外部暴露服务。 外部负载均衡器可以将流量路由到自动创建的 `NodePort` 服务和 `ClusterIP` 服务上。
- `ExternalName`：通过返回 `CNAME` 记录和对应值，可以将服务映射到 `externalName` 字段的内容（例如，`foo.bar.example.com`）。 无需创建任何类型代理。

## 外部IP

外部的 IP 路由到集群中一个或多个 Node 上，Kubernetes Service 会被暴露给这些 `externalIPs`。

```YAML
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 49152
  externalIPs:
    - 198.51.100.32
```

# Ingress

[Ingress](https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/)

# **网络策略**

因为k8s有许多种网络的实现方式，企业内部可以使用简单的`flannel`、`weave`、`kube-router`等，适合公有云的方案则有`calico`等。

是一种关于 Pod 间及与其他网络端点间所允许的通信规则的规范。

NetworkPolicy 资源使用 标签 选择 Pod，并定义选定 Pod 所允许的通信规则。

如果你希望在 IP 地址或端口层面（OSI 第 3 层或第 4 层）控制网络流量

```YAML
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

- metadata 描述信息
- podSelector pod选择器，选定的pod所有的出入站流量要遵循本networkpolicy的约束
- policyTypes 策略类型。包括了Ingress和Egress，默认情况下一个policyTypes的值一定会包含Ingress，当有egress规则时，policyTypes的值中会包含Egress
- ingress 入站
- egress 出站

# **Service 与** **Pod** **的** **DNS**

DNS 查询可以使用 Pod 中的 `/etc/resolv.conf` 展开。

## Pod 的 hostname 和 subdomain 字段

- Pod 规约中包含一个可选的 `hostname` 字段，可以用来指定一个不同的主机名。 当这个字段被设置时，它将优先于 Pod 的名字成为该 Pod 的主机名（同样是从 Pod 内部观察）。 举个例子，给定一个 `spec.hostname` 设置为 `“my-host”` 的 Pod， 该 Pod 的主机名将被设置为 `“my-host”`。
- Pod 规约还有一个可选的 `subdomain` 字段，可以用来表明该 Pod 是名字空间的子组的一部分。 举个例子，某 Pod 的 `spec.hostname` 设置为 `“foo”`，`spec.subdomain` 设置为 `“bar”`， 在名字空间 `“my-namespace”` 中，主机名称被设置成 `“foo”` 并且对应的完全限定域名（FQDN）为 “`foo.bar.my-namespace.svc.cluster-domain.example`”（还是从 Pod 内部观察）。

## Pod 的 DNS 配置

`dnsConfig` 字段是可选的，它可以与任何 `dnsPolicy` 设置一起使用。 但是，当 Pod 的 `dnsPolicy` 设置为 "`None`" 时，必须指定 `dnsConfig` 字段。

```YAML
apiVersion: v1
kind: Pod
metadata:
  namespace: default
  name: dns-example
spec:
  containers:
    - name: test
      image: nginx
  dnsPolicy: "None"
  dnsConfig:
    nameservers:
      - 192.0.2.1 # 这是一个示例
    searches:
      - ns1.svc.cluster-domain.example
      - my.dns.search.suffix
    options:
      - name: ndots
        value: "2"
      - name: edns0
```

## IPv4/IPv6 双协议栈

## HostAliases 增加额外条目

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: hostaliases-pod
spec:
  restartPolicy: Never
  hostAliases:
  - ip: "127.0.0.1"
    hostnames:
    - "foo.local"
    - "bar.local"
  - ip: "10.1.2.3"
    hostnames:
    - "foo.remote"
    - "bar.remote"
  containers:
  - name: cat-hosts
    image: busybox:1.28
    command:
    - cat
    args:
    - "/etc/hosts"
```

# Volume

## emptyDir

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir:
      sizeLimit: 500Mi
```

## hostPath 

`hostPath` 卷能将主机节点文件系统上的文件或目录挂载到你的 Pod 中。 

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      # 宿主上目录位置
      path: /data
      # 此字段为可选
      type: Directory
```

## local - storage

`local` 卷所代表的是某个被挂载的本地存储设备，例如磁盘、分区或者目录。

`local` 卷所代表的是某个被挂载的本地存储设备，例如磁盘、分区或者目录。

`local` 卷只能用作静态创建的持久卷。不支持动态配置。

与 `hostPath` 卷相比，`local` 卷能够以持久和可移植的方式使用，而无需手动将 Pod 调度到节点。系统通过查看 PersistentVolume 的节点亲和性配置，就能了解卷的节点约束。

```YAML
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 100Gi
  volumeMode: Filesystem
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /mnt/disks/ssd1
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
          - example-node
```

## subPath

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: my-lamp-site
spec:
    containers:
    - name: mysql
      image: mysql
      env:
      - name: MYSQL_ROOT_PASSWORD
        value: "rootpasswd"
      volumeMounts:
      - mountPath: /var/lib/mysql
        name: site-data
        subPath: mysql
    - name: php
      image: php:7.0-apache
      volumeMounts:
      - mountPath: /var/www/html
        name: site-data
        subPath: html
    volumes:
    - name: site-data
      persistentVolumeClaim:
        claimName: my-lamp-site-data
```

# PV&PVC

# StorageClass&**provisioner**

```YAML
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: slow
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-standard
```

# VolumeSnapshot - 卷快照

VolumeSnapshotClass

```YAML
# 创建VolumeSnapshotClass对象
apiVersion: snapshot.storage.k8s.io/v1alpha1
kind: VolumeSnapshotClass
metadata:
  name: disk-snapshotclass
snapshotter: diskplugin.csi.alibabacloud.com #指定VolumeSnapshot时使用的Volume Plugin

#创建VolumeSnapshot对象
apiVersion: snapshot.storage.k8s.io/v1alpha1
kind: VolumeSnapshot
metadata:
  name: disk-snapshot
spec:
  snapshotClassName: disk-snapshotclass
  source:
    name: disk-pvc  #Snapshot的数据源, 类型是个pvc
    kind: PersistentVolumeClaim
    
#从snapshot中恢复数据到新生的pv对象中
apiVersion: v1
kind: PersisttentVolumeClaim
metadata:
  name: restore-pvc
spec:
  dataSource:
    name: disk-snapshot
    kind: VolumeSnapshot
    apiGroup: snapshot.storage.k8s.io
  accessModes:
    -ReadWriteOnce
  resource:
    requests:
      storage:20Gi
  storageClassName: csi-disk
```

# ConfigMap&Secret

# 为 Pod 和容器管理资源

- `spec.containers[].resources.limits.cpu`
- `spec.containers[].resources.limits.memory`
- `spec.containers[].resources.limits.hugepages-<size>`
- `spec.containers[].resources.requests.cpu`
- `spec.containers[].resources.requests.memory`
- `spec.containers[].resources.requests.hugepages-<size>`

# Kubeconfig

```YAML
apiVersion: v1
kind: Config

clusters:
- cluster:
    proxy-url: http://proxy.example.org:3128
    server: https://k8s.example.org/k8s/clusters/c-xxyyzz
  name: development

users:
- name: developer

contexts:
- context:
  name: development
```

# 云原生安全的 4C

云（Cloud）、集群（Cluster）、容器（Container）和代码（Code）

[云原生安全概述](https://kubernetes.io/zh-cn/docs/concepts/security/overview/)

# **Pod** **安全性标准**

容器必须以非 root 账号运行。

容器不可以将 runAsUser 设置为 0

# k8s **API** **访问控制**

用户使用 `kubectl`、客户端库或构造 REST 请求来访问 [Kubernetes API](https://kubernetes.io/zh-cn/docs/concepts/overview/kubernetes-api/)。 

# LimitRange - 限制范围

一个 **LimitRange（限制范围）** 对象提供的限制能够做到：

- 在一个命名空间中实施对每个 Pod 或 Container 最小和最大的资源使用量的限制。
- 在一个命名空间中实施对每个 [PersistentVolumeClaim](https://kubernetes.io/zh-cn/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) 能申请的最小和最大的存储空间大小的限制。
- 在一个命名空间中实施对一种资源的申请值和限制值的比值的控制。
- 设置一个命名空间中对计算资源的默认申请/限制值，并且自动的在运行时注入到多个 Container 中。

```YAML
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default: # 此处定义默认限制值
      cpu: 500m
    defaultRequest: # 此处定义默认请求值
      cpu: 500m
    max: # max 和 min 定义限制范围
      cpu: "1"
    min:
      cpu: 100m
    type: Container
```

# **资源配额**

当多个用户或团队共享具有固定节点数目的集群时，人们会担心有人使用超过其基于公平原则所分配到的资源量。

资源配额是帮助管理员解决这一问题的工具。

如果命名空间下的计算资源 （如 `cpu` 和 `memory`）的配额被启用， 则用户必须为这些资源设定请求值（request）和约束值（limit），否则配额系统将拒绝 Pod 的创建。 提示: 可使用 `LimitRanger` 准入控制器来为没有设置计算资源需求的 Pod 设置默认值。

# nodeSelector  选择节点

# **污点和容忍度**

# **Pod** **优先级和抢占**

# Kubectl  proxy

```Bash
kubectl proxy --port=8080

curl http://localhost:8080/api/
```

# apiserver proxy

如果提供合适的凭据

可以通过 `https://192.0.2.1/api/v1/namespaces/kube-system/services/elasticsearch-logging/proxy/` 访问

或通过一个 `kubectl proxy` 来访问： `http://localhost:8080/api/v1/namespaces/kube-system/services/elasticsearch-logging/proxy/`。

```Bash
# 查看所有的集群，因为你的 .kubeconfig 文件中可能包含多个上下文
kubectl config view -o jsonpath='{"Cluster name\tServer\n"}{range .clusters[*]}{.name}{"\t"}{.cluster.server}{"\n"}{end}'

# 从上述命令输出中选择你要与之交互的集群的名称
export CLUSTER_NAME="some_server_name"

# 指向引用该集群名称的 API 服务器
APISERVER=$(kubectl config view -o jsonpath="{.clusters[?(@.name==\"$CLUSTER_NAME\")].cluster.server}")

# 创建一个 secret 来保存默认服务账户的令牌
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: default-token
  annotations:
    kubernetes.io/service-account.name: default
type: kubernetes.io/service-account-token
EOF

# 等待令牌控制器使用令牌填充 secret:
while ! kubectl describe secret default-token | grep -E '^token' >/dev/null; do
  echo "waiting for token..." >&2
  sleep 1
done

# 获取令牌
TOKEN=$(kubectl get secret default-token -o jsonpath='{.data.token}' | base64 --decode)

# 使用令牌玩转 API
curl -X GET $APISERVER/api --header "Authorization: Bearer $TOKEN" --insecure
```

# **API** **优先级和公平性**

对于集群管理员来说，控制 Kubernetes API 服务器在过载情况下的行为是一项关键任务。

[kube-apiserver](https://kubernetes.io/zh-cn/docs/concepts/overview/components/#kube-apiserver) 有一些控件（例如：命令行标志 `--max-requests-inflight` 和 `--max-mutating-requests-inflight`）， 可以限制将要接受的未处理的请求，从而防止过量请求入站，潜在导致 API 服务器崩溃。 但是这些标志不足以保证在高流量期间，最重要的请求仍能被服务器接受。

 APF 的特性门控称为 `APIPriorityAndFairness`。 此特性也与某个 [API 组](https://kubernetes.io/zh-cn/docs/concepts/overview/kubernetes-api/#api-groups-and-versioning)相关： (a) `v1alpha1` 和 `v1beta1` 版本，默认被禁用； (b) `v1beta2` 和 `v1beta3` 版本，默认被启用。 你可以在启动 `kube-apiserver` 时，添加以下命令行标志来禁用此功能门控及 API Beta 组：

```Bash
kube-apiserver \
--feature-gates=APIPriorityAndFairness=false \
--runtime-config=flowcontrol.apiserver.k8s.io/v1beta2=false,flowcontrol.apiserver.k8s.io/v1beta3=false \
  # ...其他配置不变
```

## 调试端点[ ](https://kubernetes.io/zh-cn/docs/concepts/cluster-administration/flow-control/#debug-endpoints)

```Bash
# 所有优先级及其当前状态的列表
kubectl get --raw /debug/api_priority_and_fairness/dump_priority_levels

# 所有队列及其当前状态的列表
kubectl get --raw /debug/api_priority_and_fairness/dump_queues
```

# **手动生成证书**

在使用客户端证书认证的场景下，你可以通过 `easyrsa`、 `openssl` 或 `cfssl` 等工具以手工方式生成证书。

# ResourceQuota 用于限制命名空间的总资源消耗

```YAML
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu-demo
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
    pods: "2"
    persistentvolumeclaims: "1"
    services.loadbalancers: "2"
    services.nodeports: "0"
```

# LimitRange 用于管理项目中 pod 和容器级别的约束

## **配置****pod** **的cpu、内存的最大最小默认值**

如果你在具有默认内存限制 的命名空间内尝试创建一个 Pod，并且这个 Pod 中的容器没有声明自己的内存资源限制， 那么控制面会为该容器设定默认的内存限制。

```Bash
kubectl create namespace default-mem-example
```

Limit range  `memory-defaults.yaml`

```YAML
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      cpu: 1
      memory: 512Mi
    defaultRequest:
      cpu: 0.5
      memory: 256Mi
    type: Container
    
---
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-min-max-demo-lr
spec:
  limits:
  - max:
      memory: 1Gi
    min:
      memory: 500Mi
    type: Container
    
--- 
apiVersion: v1
kind: LimitRange
metadata:
  name: storagelimits
spec:
  limits:
  - type: PersistentVolumeClaim
    max:
      storage: 2Gi
    min:
      storage: 1Gi
```

Apply

```YAML
kubectl apply -f memory-defaults.yaml --namespace=default-mem-example
```

# *Cloud-controller-manager 云控制器*

# 网络策略

# **为系统****守护进程****预留资源**

- **Kubelet** **标志**：`--kube-reserved=[cpu=100m][,][memory=100Mi][,][ephemeral-storage=1Gi][,][pid=1000]`
- **Kubelet** **标志**：`--kube-reserved-cgroup=`

这是一个用于说明节点可分配（Node Allocatable）计算方式的示例：

- 节点拥有 `32Gi` `memory`、`16 CPU` 和 `100Gi` `Storage` 资源
- `--kube-reserved` 被设置为 `cpu=1,memory=2Gi,ephemeral-storage=1Gi`
- `--system-reserved` 被设置为 `cpu=500m,memory=1Gi,ephemeral-storage=1Gi`
- `--eviction-hard` 被设置为 `memory.available<500Mi,nodefs.available<10%`

# **安全地清空一个节点**

```Bash
kubectl drain --ignore-daemonsets <节点名称>

# 下线

# 安装后恢复
kubectl uncordon <node name>
```

# **Kubernetes** **集群中使用 sysctl**

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: sysctl-example
spec:
  securityContext:
    sysctls:
    - name: kernel.shm_rmid_forced
      value: "0"
    - name: net.core.somaxconn
      value: "1024"
    - name: kernel.msgmax
      value: "65536"
  ...
```

# **配置存活、就绪和启动探针**

配置存活（Liveness）、就绪（Readiness）和启动（Startup）探针

```YAML
livenessProbe:
  httpGet:
    path: /healthz
    port: liveness-port
  failureThreshold: 1
  periodSeconds: 10

startupProbe:
  httpGet:
    path: /healthz
    port: liveness-port
  failureThreshold: 30
  periodSeconds: 10
```

# **为容器的生命周期事件设置处理函数**

postStart 和 preStop 处理函数

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: lifecycle-demo
spec:
  containers:
  - name: lifecycle-demo-container
    image: nginx
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
      preStop:
        exec:
          command: ["/bin/sh","-c","nginx -s quit; while killall -0 nginx; do sleep 1; done"]
```

# Pod 安全性准入

1.27版本

通过 `--admission-control-config-file` 指定给 kube-apiserver。

```YAML
apiVersion: apiserver.config.k8s.io/v1 # 查阅兼容性说明
kind: AdmissionConfiguration
plugins:
- name: PodSecurity
  configuration:
    apiVersion: pod-security.admission.config.k8s.io/v1
    kind: PodSecurityConfiguration
    # 当未设置 mode 标签时会应用的默认设置
    #
    # level 标签必须是以下取值之一：
    # - "privileged" (默认)
    # - "baseline"
    # - "restricted"
    #
    # version 标签必须是如下取值之一：
    # - "latest" (默认) 
    # - 诸如 "v1.27" 这类版本号
    defaults:
      enforce: "privileged"
      enforce-version: "latest"
      audit: "privileged"
      audit-version: "latest"
      warn: "privileged"
      warn-version: "latest"
    exemptions:
      # 要豁免的已认证用户名列表
      usernames: []
      # 要豁免的运行时类名称列表
      runtimeClasses: []
      # 要豁免的名字空间列表
      namespaces: []
```

# YAML2JSON

```Bash
kubectl get -f https://k8s.io/examples/application/simple_deployment.yaml -o yaml
```

# DIFF

```Bash
kubectl diff -f <目录>/
```

# **用插件扩展 kubectl**

插件是一个独立的可执行文件，名称以 `kubectl-` 开头。 要安装插件，将其可执行文件移动到 `PATH` 中的任何位置。

```Bash
#!/bin/bash

# 可选的参数处理
if [[ "$1" == "version" ]]
then
    echo "1.0.0"
    exit 0
fi

# 可选的参数处理
if [[ "$1" == "config" ]]
then
    echo $KUBECONFIG
    exit 0
fi

echo "I am a plugin named kubectl-foo"


kubectl foo
```