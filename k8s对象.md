<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [必需字段](#%E5%BF%85%E9%9C%80%E5%AD%97%E6%AE%B5)
- [资源对象](#%E8%B5%84%E6%BA%90%E5%AF%B9%E8%B1%A1)
  - [:point_right:Pod(po)](#point_rightpodpo)
    - [:point_right:pod 生命周期](#point_rightpod-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [:point_right:重启策略](#point_right%E9%87%8D%E5%90%AF%E7%AD%96%E7%95%A5)
    - [私有镜像](#%E7%A7%81%E6%9C%89%E9%95%9C%E5%83%8F)
    - [:point_right:使用volume](#point_right%E4%BD%BF%E7%94%A8volume)
    - [:point_right:环境变量](#point_right%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
    - [:point_right:镜像拉取策略](#point_right%E9%95%9C%E5%83%8F%E6%8B%89%E5%8F%96%E7%AD%96%E7%95%A5)
    - [访问dns策略](#%E8%AE%BF%E9%97%AEdns%E7%AD%96%E7%95%A5)
    - [使用主机空间](#%E4%BD%BF%E7%94%A8%E4%B8%BB%E6%9C%BA%E7%A9%BA%E9%97%B4)
    - [设置pod的hostname](#%E8%AE%BE%E7%BD%AEpod%E7%9A%84hostname)
    - [设置 Pod 的子域名](#%E8%AE%BE%E7%BD%AE-pod-%E7%9A%84%E5%AD%90%E5%9F%9F%E5%90%8D)
    - [:point_right:资源限制](#point_right%E8%B5%84%E6%BA%90%E9%99%90%E5%88%B6)
    - [:point_right:健康检查](#point_right%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
    - [:point_right:init容器](#point_rightinit%E5%AE%B9%E5%99%A8)
    - [:point_right:容器生命周期钩子](#point_right%E5%AE%B9%E5%99%A8%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)
    - [:point_right:Capabilities 容器能力](#point_rightcapabilities-%E5%AE%B9%E5%99%A8%E8%83%BD%E5%8A%9B)
    - [:point_right:来限制 Pod 的网络带宽](#point_right%E6%9D%A5%E9%99%90%E5%88%B6-pod-%E7%9A%84%E7%BD%91%E7%BB%9C%E5%B8%A6%E5%AE%BD)
    - [:point_right:调度到指定的 Node 上](#point_right%E8%B0%83%E5%BA%A6%E5%88%B0%E6%8C%87%E5%AE%9A%E7%9A%84-node-%E4%B8%8A)
    - [:point_right:自定义pod的hosts文件](#point_right%E8%87%AA%E5%AE%9A%E4%B9%89pod%E7%9A%84hosts%E6%96%87%E4%BB%B6)
    - [pod大页内存-HugePages](#pod%E5%A4%A7%E9%A1%B5%E5%86%85%E5%AD%98-hugepages)
    - [:point_right:pod优先级](#point_rightpod%E4%BC%98%E5%85%88%E7%BA%A7)
    - [:point_right:pod内核参数-Sysctls](#point_rightpod%E5%86%85%E6%A0%B8%E5%8F%82%E6%95%B0-sysctls)
    - [:point_right:pod时区](#point_rightpod%E6%97%B6%E5%8C%BA)
  - [ReplicaSet(rs)](#replicasetrs)
  - [ReplicationController(rc)](#replicationcontrollerrc)
  - [:point_right:Deployment(deploy) - 无状态服务](#point_rightdeploymentdeploy---%E6%97%A0%E7%8A%B6%E6%80%81%E6%9C%8D%E5%8A%A1)
    - [:point_right:Deployment 扩容](#point_rightdeployment-%E6%89%A9%E5%AE%B9)
    - [:point_right:暂停和恢复deployment](#point_right%E6%9A%82%E5%81%9C%E5%92%8C%E6%81%A2%E5%A4%8Ddeployment)
    - [:point_right:deployment 状态](#point_rightdeployment-%E7%8A%B6%E6%80%81)
    - [:point_right:Deployment Spec](#point_rightdeployment-spec)
  - [:point_right:StatefulSet - 有状态服务](#point_rightstatefulset---%E6%9C%89%E7%8A%B6%E6%80%81%E6%9C%8D%E5%8A%A1)
    - [:point_right:pod标识](#point_rightpod%E6%A0%87%E8%AF%86)
    - [:point_right:有序索引](#point_right%E6%9C%89%E5%BA%8F%E7%B4%A2%E5%BC%95)
    - [:point_right:稳定的存储](#point_right%E7%A8%B3%E5%AE%9A%E7%9A%84%E5%AD%98%E5%82%A8)
    - [:point_right:更新策略](#point_right%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5)
  - [:point_right:DaemonSet](#point_rightdaemonset)
    - [更新策略](#%E6%9B%B4%E6%96%B0%E7%AD%96%E7%95%A5)
  - [Job](#job)
  - [CronJob](#cronjob)
    - [CronJob Spec](#cronjob-spec)
  - [:point_right:HorizontalPodAutoscaling(hpa) - 水平扩缩容](#point_righthorizontalpodautoscalinghpa---%E6%B0%B4%E5%B9%B3%E6%89%A9%E7%BC%A9%E5%AE%B9)
  - [:point_right:Node](#point_rightnode)
    - [:point_right:node 状态](#point_rightnode-%E7%8A%B6%E6%80%81)
    - [:point_right:Taints 和 tolerations](#point_righttaints-%E5%92%8C-tolerations)
    - [:point_right:node维护模式](#point_rightnode%E7%BB%B4%E6%8A%A4%E6%A8%A1%E5%BC%8F)
  - [:point_right:Namespace(ns)](#point_rightnamespacens)
  - [:point_right:Service(svc)](#point_rightservicesvc)
    - [:point_right:service 定义](#point_rightservice-%E5%AE%9A%E4%B9%89)
    - [:point_right:service 协议](#point_rightservice-%E5%8D%8F%E8%AE%AE)
    - [:point_right:不指定 Selectors 的服务](#point_right%E4%B8%8D%E6%8C%87%E5%AE%9A-selectors-%E7%9A%84%E6%9C%8D%E5%8A%A1)
    - [:point_right:Headless 服务](#point_rightheadless-%E6%9C%8D%E5%8A%A1)
    - [:point_right:保留源ip](#point_right%E4%BF%9D%E7%95%99%E6%BA%90ip)
  - [:point_right:Ingress - 暴露服务](#point_rightingress---%E6%9A%B4%E9%9C%B2%E6%9C%8D%E5%8A%A1)
  - [:point_right:Label - 标签](#point_rightlabel---%E6%A0%87%E7%AD%BE)
  - [CustomResourceDefinition(crd) - 创建自定义资源](#customresourcedefinitioncrd---%E5%88%9B%E5%BB%BA%E8%87%AA%E5%AE%9A%E4%B9%89%E8%B5%84%E6%BA%90)
    - [Finalizer - 预删除](#finalizer---%E9%A2%84%E5%88%A0%E9%99%A4)
    - [Validation - 验证资源是否合规](#validation---%E9%AA%8C%E8%AF%81%E8%B5%84%E6%BA%90%E6%98%AF%E5%90%A6%E5%90%88%E8%A7%84)
    - [Subresources - 状态和副本](#subresources---%E7%8A%B6%E6%80%81%E5%92%8C%E5%89%AF%E6%9C%AC)
    - [Categories - 对象分组](#categories---%E5%AF%B9%E8%B1%A1%E5%88%86%E7%BB%84)
  - [:point_right:kubebuilder - 创建自定义资源（自己安装）](#point_rightkubebuilder---%E5%88%9B%E5%BB%BA%E8%87%AA%E5%AE%9A%E4%B9%89%E8%B5%84%E6%BA%90%E8%87%AA%E5%B7%B1%E5%AE%89%E8%A3%85)
- [存储对象](#%E5%AD%98%E5%82%A8%E5%AF%B9%E8%B1%A1)
  - [:point_right:Volume - 卷](#point_rightvolume---%E5%8D%B7)
    - [:point_right:emptyDir](#point_rightemptydir)
    - [:point_right:hostPath](#point_righthostpath)
    - [:point_right:NFS](#point_rightnfs)
    - [:point_right:cephFS](#point_rightcephfs)
    - [:point_right:subPath - 子路径](#point_rightsubpath---%E5%AD%90%E8%B7%AF%E5%BE%84)
  - [:point_right:PersistentVolume(pv) - PersistentVolumeClaim (PVC) - StorageClass(sc)](#point_rightpersistentvolumepv---persistentvolumeclaim-pvc---storageclasssc)
    - [:point_right:Volume 生命周期](#point_rightvolume-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [:point_right:pv](#point_rightpv)
    - [:point_right:StorageClass](#point_rightstorageclass)
      - [:point_right:Local Volume](#point_rightlocal-volume)
    - [:point_right:PVC](#point_rightpvc)
    - [:point_right:扩展 PV 空间](#point_right%E6%89%A9%E5%B1%95-pv-%E7%A9%BA%E9%97%B4)
    - [块存储（Raw Block Volume）](#%E5%9D%97%E5%AD%98%E5%82%A8raw-block-volume)
    - [:point_right:存储快照](#point_right%E5%AD%98%E5%82%A8%E5%BF%AB%E7%85%A7)
  - [:point_right:Secret](#point_rightsecret)
    - [:point_right:Secret 类型](#point_rightsecret-%E7%B1%BB%E5%9E%8B)
    - [:point_right:Opaque Secret](#point_rightopaque-secret)
      - [:point_right:将 Secret 挂载到 Volume 中](#point_right%E5%B0%86-secret-%E6%8C%82%E8%BD%BD%E5%88%B0-volume-%E4%B8%AD)
      - [:point_right:将 Secret 导入到 env中](#point_right%E5%B0%86-secret-%E5%AF%BC%E5%85%A5%E5%88%B0-env%E4%B8%AD)
    - [kubernetes.io/dockerconfigjson](#kubernetesiodockerconfigjson)
    - [kubernetes.io/service-account-token](#kubernetesioservice-account-token)
    - [secrets加密存储到 etcd](#secrets%E5%8A%A0%E5%AF%86%E5%AD%98%E5%82%A8%E5%88%B0-etcd)
    - [:point_right:禁止更改的 Secret](#point_right%E7%A6%81%E6%AD%A2%E6%9B%B4%E6%94%B9%E7%9A%84-secret)
  - [:point_right:ConfigMap](#point_rightconfigmap)
    - [:point_right:ConfigMap 用作环境变量](#point_rightconfigmap-%E7%94%A8%E4%BD%9C%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
    - [:point_right:使用 volume 将 ConfigMap 作为文件或目录直接挂载](#point_right%E4%BD%BF%E7%94%A8-volume-%E5%B0%86-configmap-%E4%BD%9C%E4%B8%BA%E6%96%87%E4%BB%B6%E6%88%96%E7%9B%AE%E5%BD%95%E7%9B%B4%E6%8E%A5%E6%8C%82%E8%BD%BD)
    - [:point_right:使用 subpath 将 ConfigMap 作为单独的文件挂载到目录](#point_right%E4%BD%BF%E7%94%A8-subpath-%E5%B0%86-configmap-%E4%BD%9C%E4%B8%BA%E5%8D%95%E7%8B%AC%E7%9A%84%E6%96%87%E4%BB%B6%E6%8C%82%E8%BD%BD%E5%88%B0%E7%9B%AE%E5%BD%95)
    - [:point_right:禁止更改的ConfigMap](#point_right%E7%A6%81%E6%AD%A2%E6%9B%B4%E6%94%B9%E7%9A%84configmap)
- [策略对象](#%E7%AD%96%E7%95%A5%E5%AF%B9%E8%B1%A1)
  - [SecurityContext - 安全上下文](#securitycontext---%E5%AE%89%E5%85%A8%E4%B8%8A%E4%B8%8B%E6%96%87)
  - [:point_right:ResourceQuota- 限制用户资源用量](#point_rightresourcequota--%E9%99%90%E5%88%B6%E7%94%A8%E6%88%B7%E8%B5%84%E6%BA%90%E7%94%A8%E9%87%8F)
  - [:point_right:LimitRange - 资源限制](#point_rightlimitrange---%E8%B5%84%E6%BA%90%E9%99%90%E5%88%B6)
- [身份对象](#%E8%BA%AB%E4%BB%BD%E5%AF%B9%E8%B1%A1)
  - [:point_right:ServiceAccount -为 Pod 配置服务账户](#point_rightserviceaccount--%E4%B8%BA-pod-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E8%B4%A6%E6%88%B7)
  - [:point_right:Role、ClusterRole**、**RoleBinding、 **ClusterRoleBinding**](#point_rightroleclusterrolerolebinding-clusterrolebinding)
    - [:point_right:Role 和 ClusterRole](#point_rightrole-%E5%92%8C-clusterrole)
    - [:point_right:RoleBinding 和 ClusterRoleBinding](#point_rightrolebinding-%E5%92%8C-clusterrolebinding)
- [网络策略](#%E7%BD%91%E7%BB%9C%E7%AD%96%E7%95%A5)
  - [:point_right:NetworkPolicy](#point_rightnetworkpolicy)
    - [:point_right:namespace 隔离](#point_rightnamespace-%E9%9A%94%E7%A6%BB)
    - [:point_right:pod 隔离](#point_rightpod-%E9%9A%94%E7%A6%BB)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 必需字段

在想要创建的 Kubernetes 对象所对应的 `.yaml` 文件中，需要配置的字段如下：

- `apiVersion` - 创建该对象所使用的 Kubernetes API 的版本
- `kind` - 想要创建的对象的类别
- `metadata` - 帮助唯一标识对象的一些数据，包括一个 `name` 字符串、`UID` 和可选的 `namespace`
- `spec` - 你所期望的该对象的状态

# 资源对象

## :point_right:Pod(po)

**Pod** 是可以在 Kubernetes 中创建和管理的、最小的可部署的计算单元。

### :point_right:pod 生命周期

Kubernetes 以 `PodStatus.Phase` 抽象 Pod 的状态（但并不直接反映所有容器的状态）。可能的 Phase 包括

- `Pending`: API Server已经创建该Pod，但一个或多个容器还没有被创建，包括通过网络下载镜像的过程。
- `Running`: Pod中的所有容器都已经被创建且已经调度到 Node 上面，但至少有一个容器还在运行或者正在启动。
- `Succeeded`: Pod 调度到 Node 上面后均成功运行结束，并且不会重启。
- `Failed`: Pod中的所有容器都被终止了，但至少有一个容器退出失败（即退出码不为 0 或者被系统终止）。
- `Unknonwn`: 状态未知，因为一些原因Pod无法被正常获取，通常是由于 apiserver 无法与 kubelet 通信导致。

可以用 kubectl 命令查询 Pod Phase：

```bash
kubectl get pod reviews-v1-5bdc544bbd-5qgxj -o jsonpath="{.status.phase}"

# output
Running
```

### :point_right:重启策略

PodSpec 中的 `restartPolicy` 可以用来设置是否对退出的 Pod 重启，可选项包括 `Always`、`OnFailure`、以及 `Never`

- Always：当容器失效时，由Kubelet自动重启该容器。RestartPolicy的默认值。
- OnFailure：当容器终止运行且退出码不为0时由Kubelet重启。
- Never：无论何种情况下，Kubelet都不会重启该容器。

### 私有镜像

在使用私有镜像时，需要创建一个docker registry secret，并在容器中引用

```bash
kubectl create secret docker-registry regsecret --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

### :point_right:使用volume

Volume 可以为容器提供持久化存储

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis
    volumeMounts:
    - name: redis-storage
      mountPath: /data/redis
  volumes:
  - name: redis-storage
    emptyDir: {}
```

### :point_right:环境变量

spec.containers[].env 设置环境变量

### :point_right:镜像拉取策略

支持三种 ImagePullPolicy

- `Always`：不管本地镜像是否存在都会去仓库进行一次镜像拉取。校验如果镜像有变化则会覆盖本地镜像，否则不会覆盖。
- `Never`：只是用本地镜像，不会去仓库拉取镜像，如果本地镜像不存在则Pod运行失败。
- `ifNotPresent`：只有本地镜像不存在时，才会去仓库拉取镜像。ImagePullPolicy的默认值。

### 访问dns策略

通过设置 dnsPolicy 参数，设置 Pod 中容器访问 DNS 的策略

ClusterFirst：优先基于 cluster domain （如 `default.svc.cluster.local`） 后缀，通过 kube-dns 查询 (默认策略)

Default：优先从 Node 中配置的 DNS 查询

### 使用主机空间

**使用主机的 IPC 命名空间**

通过设置 `spec.hostIPC` 参数为 true，使用主机的 IPC 命名空间，默认为 false。

**使用主机的网络命名空间**

通过设置 `spec.hostNetwork` 参数为 true，使用主机的网络命名空间，默认为 false。

**使用主机的 PID 空间**

通过设置 `spec.hostPID` 参数为 true，使用主机的 PID 命名空间，默认为 false。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox1
  labels:
    name: busybox
spec:
  hostIPC: true
  hostPID: true
  hostNetwork: true
  containers:
  - image: busybox
    command:
      - sleep
      - "3600"
    name: busybox
```

### 设置pod的hostname

通过 `spec.hostname` 参数实现，如果未设置默认使用 `metadata.name` 参数的值作为 Pod 的 hostname。

### 设置 Pod 的子域名

通过 `spec.subdomain` 参数设置 Pod 的子域名，默认为空。

比如，指定 hostname 为 busybox-2 和 subdomain 为 default-subdomain，完整域名为 `busybox-2.default-subdomain.default.svc.cluster.local`，也可以简写为 `busybox-2.default-subdomain.default`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox2
  labels:
    name: busybox
spec:
  hostname: busybox-2
  subdomain: default-subdomain
  containers:
  - image: busybox
    command:
      - sleep
      - "3600"
    name: busybox
```

### :point_right:资源限制

spec.containers[].resources.limits.cpu：CPU 上限，可以短暂超过，容器也不会被停止

spec.containers[].resources.limits.memory：内存上限，不可以超过；如果超过，容器可能会被终止或调度到其他资源充足的机器上

spec.containers[].resources.limits.ephemeral-storage：临时存储（容器可写层、日志以及 EmptyDir 等）的上限，超过后 Pod 会被驱逐

spec.containers[].resources.requests.cpu：CPU 请求，也是调度 CPU 资源的依据，可以超过

spec.containers[].resources.requests.memory：内存请求，也是调度内存资源的依据，可以超过；但如果超过，容器可能会在 Node 内存不足时清理

spec.containers[].resources.requests.ephemeral-storage：临时存储（容器可写层、日志以及 EmptyDir 等）的请求，调度容器存储的依据

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      resources:
        requests:
          cpu: "300m"
          memory: "56Mi"
        limits:
          cpu: "500m"
          memory: "128Mi"
```

### :point_right:健康检查

为了确保容器在部署后确实处在正常运行状态，主要两种探针（Probe）来探测容器的状态：

- LivenessProbe：探测应用是否处于健康状态，如果不健康则删除并重新创建容器。
- ReadinessProbe：探测应用是否启动完成并且处于正常服务状态，如果不正常则不会接收来自 Kubernetes Service 的流量，即将该Pod从Service的endpoint中移除。

Kubernetes 支持三种方式来执行探针：

- exec：在容器中执行一个命令，如果 [命令退出码](http://www.tldp.org/LDP/abs/html/exitcodes.html) 返回 `0` 则表示探测成功，否则表示失败
- tcpSocket：对指定的容器 IP 及端口执行一个 TCP 检查，如果端口是开放的则表示探测成功，否则表示失败
- httpGet：对指定的容器 IP、端口及路径执行一个 HTTP Get 请求，如果返回的 [状态码](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) 在 `[200,400)` 之间则表示探测成功，否则表示失败

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: nginx
  name: nginx
spec:
    containers:
    - image: nginx
      imagePullPolicy: Always
      name: http
      livenessProbe:
        httpGet:
          path: /
          port: 80
          httpHeaders:
          - name: X-Custom-Header
            value: Awesome
        initialDelaySeconds: 15
        timeoutSeconds: 1
      readinessProbe:
        exec:
          command:
          - cat
          - /usr/share/nginx/html/index.html
        initialDelaySeconds: 5
        timeoutSeconds: 1
    - name: goproxy
      image: gcr.io/google_containers/goproxy:0.1
      ports:
      - containerPort: 8080
      readinessProbe:
        tcpSocket:
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 10
      livenessProbe:
        tcpSocket:
          port: 8080
        initialDelaySeconds: 15
        periodSeconds: 20
```

### :point_right:init容器

Pod 能够具有多个容器，应用运行在容器里面，但是它也可能有一个或多个先于应用容器启动的 Init 容器。

> Init 容器在所有容器运行之前执行（run-to-completion），常用来初始化配置。

如果为一个 Pod 指定了多个 Init 容器，那些容器会按顺序一次运行一个

```yaml
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
  # These containers are run during pod initialization
  initContainers:
  - name: install
    image: busybox
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - http://kubernetes.io
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}
```

### :point_right:容器生命周期钩子

支持两种钩子：

- postStart： 容器创建后立即执行，注意由于是异步执行，它无法保证一定在 ENTRYPOINT 之前运行。如果失败，容器会被杀死，并根据 RestartPolicy 决定是否重启
- preStop：容器终止前执行，常用于资源清理。如果失败，容器同样也会被杀死

而钩子的回调函数支持两种方式：

- exec：在容器内执行命令，如果命令的退出状态码是 `0` 表示执行成功，否则表示失败
- httpGet：向指定 URL 发起 GET 请求，如果返回的 HTTP 状态码在 `[200, 400)` 之间表示请求成功，否则表示失败

```bash
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
        httpGet:
          path: /
          port: 80
      preStop:
        exec:
          command: ["/usr/sbin/nginx","-s","quit"]
```

### :point_right:Capabilities 容器能力

Kubernetes 提供了修改 [Capabilities](http://man7.org/linux/man-pages/man7/capabilities.7.html) 的机制，可以按需要给容器增加或删除。比如下面的配置给容器增加了 `CAP_NET_ADMIN` 并删除了 `CAP_KILL`。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cap-pod
spec:
  containers:
  - name: friendly-container
    image: "alpine:3.4"
    command: ["/bin/sleep", "3600"]
    securityContext:
      capabilities:
        add:
        - NET_ADMIN
        drop:
        - KILL
```

### :point_right:来限制 Pod 的网络带宽

可以通过给 Pod 增加 `kubernetes.io/ingress-bandwidth` 和 `kubernetes.io/egress-bandwidth` 这两个 annotation 来限制 Pod 的网络带宽

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: qos
  annotations:
    kubernetes.io/ingress-bandwidth: 3M
    kubernetes.io/egress-bandwidth: 4M
spec:
  containers:
  - name: iperf3
    image: networkstatic/iperf3
    command:
    - iperf3
    - -s
```

### :point_right:调度到指定的 Node 上

使用 nodeSelector，首先给 Node 加上标签

### :point_right:自定义pod的hosts文件

pod.Spec.HostAliases

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostaliases-pod
spec:
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
    image: busybox
    command:
    - cat
    args:
    - "/etc/hosts"
```

> 一般使用coredns功能

### pod大页内存-HugePages

源格式为 `hugepages-<size>`（如 `hugepages-2Mi`）。使用前要配置

- 在所有 Node 上面预分配好 HugePage ，以便 Kubelet 统计所在 Node 的 HugePage 容量

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: huge-pages-example
spec:
  containers:
  - name: example
    image: fedora:latest
    command:
    - sleep
    - inf
    volumeMounts:
    - mountPath: /hugepages-2Mi
      name: hugepage-2mi
    - mountPath: /hugepages-1Gi
      name: hugepage-1gi
    resources:
      limits:
        hugepages-2Mi: 100Mi
        hugepages-1Gi: 2Gi
        memory: 100Mi
      requests:
        memory: 100Mi
  volumes:
  - name: hugepage-2mi
    emptyDir:
      medium: HugePages-2Mi
  - name: hugepage-1gi
    emptyDir:
      medium: HugePages-1Gi
```

> HugePages 不宜过量

### :point_right:pod优先级

可以为 Pod 设置一个优先级，保证高优先级的 Pod 优先调度。

为 Pod 设置优先级前，先创建一个 PriorityClass，并设置优先级（数值越大优先级越高）：

```yaml
apiVersion: scheduling.k8s.io/v1alpha1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class should be used for XYZ service pods only."
```

为 Pod 指定优先级

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
```

### :point_right:pod内核参数-Sysctls

从 v1.11 开始，Sysctls 升级为 Beta 版本，不再区分安全和非安全 sysctl，统一通过 podSpec.securityContext.sysctls 设置，如

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sysctl-example
spec:
  securityContext:
    sysctls:
    - name: kernel.shm_rmid_forced
      value: "0"
    - name: net.ipv4.route.min_pmtu
      value: "552"
    - name: kernel.msgmax
      value: "65536"
  ...
```

### :point_right:pod时区

很多容器都是配置了 UTC 时区，与国内集群的 Node 所在时区有可能不一致，可以通过 HostPath 存储插件给容器配置与 Node 一样的时区：

```bash
apiVersion: v1
kind: Pod
metadata:
  name: sh
  namespace: default
spec:
  containers:
  - image: alpine
    stdin: true
    tty: true
    volumeMounts:
    - mountPath: /etc/localtime
      name: time
      readOnly: true
  volumes:
  - hostPath:
      path: /etc/localtime
      type: ""
    name: time
```

## ReplicaSet(rs)

虽然也 ReplicaSet 可以独立使用，但建议使用 Deployment 来自动管理 ReplicaSet

rs示例

```bash
apiVersion: extensions/v1beta1
kind: ReplicaSet
metadata:
  name: frontend
  # these labels can be applied automatically
  # from the labels in the pod template if not set
  # labels:
    # app: guestbook
    # tier: frontend
spec:
  # this replicas value is default
  # modify it according to your case
  replicas: 3
  # selector can be applied automatically
  # from the labels in the pod template if not set,
  # but we are specifying the selector here to
  # demonstrate its usage.
  selector:
    matchLabels:
      tier: frontend
    matchExpressions:
      - {key: tier, operator: In, values: [frontend]}
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
          # If your cluster config does not include a dns service, then to
          # instead access environment variables to find service host
          # info, comment out the 'value: dns' line above, and uncomment the
          # line below.
          # value: env
        ports:
        - containerPort: 80
```



## ReplicationController(rc)

ReplicationController（也简称为 rc）用来确保容器应用的副本数始终保持在用户定义的副本数，即如果有容器异常退出，会自动创建新的 Pod 来替代；而异常多出来的容器也会自动回收。

rc示例

```bash
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

## :point_right:Deployment(deploy) - 无状态服务

Deployment 为 Pod 和 Replica Set（下一代 Replication Controller）提供声明式更新。

你只需要在 Deployment 中描述你想要的目标状态是什么，Deployment controller 就会帮你将 Pod 和 Replica Set 的实际状态改变到你的目标状态。你可以定义一个全新的 Deployment，也可以创建一个新的替换旧的 Deployment。

### :point_right:Deployment 扩容

```bash
kubectl scale deployment nginx-deployment --replicas 10

# 如果你使用了HorizontalPodAutoscaling(hpa)
kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80
```

### :point_right:暂停和恢复deployment

```bash
kubectl rollout pause deployment/nginx-deployment
kubectl rollout remove deployment/nginx-deployment
```

### :point_right:deployment 状态

```b
kubectl rollout status deployment/nginx-deployment
```

###  :point_right:Deployment Spec

`.spec.template` 是 `.spec` 中唯一要求的字段。

`.spec.replicas` 是可以选字段，指定期望的 pod 数量，默认是 1。

`.spec.selector` 是可选字段，用来指定label selector，圈定 Deployment 管理的 pod 范围。

`.spec.strategy` 指定新的 Pod 替换旧的 Pod 的策略。 `.spec.strategy.type` 可以是 "Recreate" 或者是 "RollingUpdate"。"RollingUpdate" 是默认值。

> 其他配置和pod一致

## :point_right:StatefulSet - 有状态服务

StatefulSet 是为了解决有状态服务的问题（对应 Deployments 和 ReplicaSets 是为无状态服务而设计），其应用场景包括

- 稳定的持久化存储，即 Pod 重新调度后还是能访问到相同的持久化数据，基于 PVC 来实现
- 稳定的网络标志，即 Pod 重新调度后其 PodName 和 HostName 不变，基于 Headless Service（即没有 Cluster IP 的 Service）来实现
- 有序部署，有序扩展，即 Pod 是有顺序的，在部署或者扩展的时候要依据定义的顺序依次依序进行（即从 0 到 N-1，在下一个 Pod 运行之前所有之前的 Pod 必须都是 Running 和 Ready 状态），基于 init containers 来实现
- 有序收缩，有序删除（即从 N-1 到 0）

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

其他操作

```bash
# 扩容
$ kubectl scale statefulset web --replicas=5

# 缩容
$ kubectl patch statefulset web -p '{"spec":{"replicas":3}}'

# 镜像更新（目前还不支持直接更新 image，需要 patch 来间接实现）
$ kubectl patch statefulset web --type='json' -p='[{"op":"replace","path":"/spec/template/spec/containers/0/image","value":"gcr.io/google_containers/nginx-slim:0.7"}]'

# 删除 StatefulSet 和 Headless Service
$ kubectl delete statefulset web
$ kubectl delete service nginx

# StatefulSet 删除后 PVC 还会保留着，数据不再使用的话也需要删除
$ kubectl delete pvc www-web-0 www-web-1
```



### :point_right:pod标识

StatefulSet Pod 具有唯一的标识，该标识包括顺序标识、稳定的网络标识和稳定的存储。 该标识和 Pod 是绑定的，与该 Pod 调度到哪个节点上无关。

### :point_right:有序索引

对于具有 N 个副本的 StatefulSet，该 StatefulSet 中的每个 Pod 将被分配一个从 0 到 N-1 的整数序号，该序号在此 StatefulSet 上是唯一的。

### :point_right:稳定的存储

对于 StatefulSet 中定义的每个 VolumeClaimTemplate，每个 Pod 接收到一个 PersistentVolumeClaim。

每个 Pod 将会得到基于 StorageClass 制备一个 PersistentVolume。

```yaml
spec:
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```

注意：当 Pod 或者 StatefulSet 被删除时，与 PersistentVolumeClaims 相关联的 PersistentVolume 并不会被删除。要删除它必须通过手动方式来完成。

### :point_right:更新策略

StatefulSet 的 `.spec.updateStrategy` 字段让你可以配置和禁用掉自动滚动更新 Pod 的容器、标签、资源请求或限制、以及注解。有两个允许的值：

- `OnDelete`

  当 StatefulSet 的 `.spec.updateStrategy.type` 设置为 `OnDelete` 时， 它的控制器将不会自动更新 StatefulSet 中的 Pod。 用户必须手动删除 Pod 以便让控制器创建新的 Pod，以此来对 StatefulSet 的 `.spec.template` 的变动作出反应。

- `RollingUpdate`

  `RollingUpdate` 更新策略对 StatefulSet 中的 Pod 执行自动的滚动更新。这是默认的更新策略。

## :point_right:DaemonSet

DaemonSet 保证在每个 Node 上都运行一个容器副本，常用来部署一些集群的日志、监控或者其他系统管理应用。典型的应用包括：

- 日志收集，比如 fluentd，logstash 等
- 系统监控，比如 Prometheus Node Exporter，collectd，New Relic agent，Ganglia gmond 等
- 系统程序，比如 kube-proxy, kube-dns, glusterd, ceph 等

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: gcr.io/google-containers/fluentd-elasticsearch:1.20
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

```bash
# 查询历史版本
kubectl rollout history daemonset <daemonset-name>

# 查询某个历史版本的详细信息
kubectl rollout history daemonset <daemonset-name> --revision=1

# 回滚
kubectl rollout undo daemonset <daemonset-name> --to-revision=<revision>
# 查询回滚状态
kubectl rollout status ds/<daemonset-name>
```

### 更新策略

:point_right:支持策略

- OnDelete：默认策略，更新模板后，只有手动删除了旧的 Pod 后才会创建新的 Pod
- RollingUpdate：更新 DaemonSet 模版后，自动删除旧的 Pod 并创建新的 Pod

## Job

Job 会创建一个或者多个 Pod，并将继续重试 Pod 的执行，直到指定数量的 Pod 成功终止。 随着 Pod 成功结束，Job 跟踪记录成功完成的 Pod 个数。 当数量达到指定的成功个数阈值时，任务（即 Job）结束。 删除 Job 的操作会清除所创建的全部 Pod。 挂起 Job 的操作会删除 Job 的所有活跃 Pod，直到 Job 被再次恢复执行。

yaml

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

Job 的 `.spec` 中只有 `.spec.template` 是必需的字段。

Job 中 Pod 的 [`RestartPolicy`](https://kubernetes.io/zh-cn/docs/concepts/workloads/pods/pod-lifecycle/#restart-policy) 只能设置为 `Never` 或 `OnFailure` 之一。

## CronJob

CronJob 即定时任务，就类似于 Linux 系统的 crontab，在指定的时间周期运行指定的任务。

### CronJob Spec

- `.spec.schedule` 指定任务运行周期，格式同 [Cron](https://en.wikipedia.org/wiki/Cron)
- `.spec.jobTemplate` 指定需要运行的任务，格式同 [Job]()
- `.spec.startingDeadlineSeconds` 指定任务开始的截止期限
- `.spec.concurrencyPolicy` 指定任务的并发策略，支持 Allow、Forbid 和 Replace 三个选项

```bash
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

## :point_right:HorizontalPodAutoscaling(hpa) - 水平扩缩容

```bash
kubectl run php-apache --image=k8s.gcr.io/hpa-example --requests=cpu=200m --expose --port=80

# 创建hpa
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

kubectl get hpa
```

yaml

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: php-apache
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: php-apache
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

## :point_right:Node

Node 是 Pod 真正运行的主机，可以是物理机，也可以是虚拟机。为了管理 Pod，每个 Node 节点上至少要运行 container runtime（比如 `docker` 或者 `contairnd`）、`kubelet` 和 `kube-proxy` 服务。

Node Controller 负责

- 维护 Node 状态
- 与 Cloud Provider 同步 Node
- 给 Node 分配容器 CIDR
- 删除带有 `NoExecute` taint 的 Node 上的 Pods

### :point_right:node 状态

每个 Node 都包括以下状态信息：

- 地址：包括 hostname、外网 IP 和内网 IP
- 条件（Condition）：包括 OutOfDisk、Ready、MemoryPressure 和 DiskPressure
- 容量（Capacity）：Node 上的可用资源，包括 CPU、内存和 Pod 总数
- 基本信息（Info）：包括内核版本、容器引擎版本、OS 类型等

### :point_right:Taints 和 tolerations 

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
kubectl taint nodes node1 key1=value2:NoExecute
```

### :point_right:node维护模式

```bash
kubectl cordon $NODENAME
kubectl uncordon $NODENAME
kubectl drain $NODENAME
```

## :point_right:Namespace(ns)

Namespace 是对一组资源和对象的抽象集合,Namespace 常用来隔离不同的用户

```bash
kubectl create namespace new-namespace
kubectl delete namespaces new-namespace

```

注意：

- 删除一个 namespace 会自动删除所有属于该 namespace 的资源。
- `default` 和 `kube-system` 命名空间不可删除。
- PersistentVolume 是不属于任何 namespace 的，但 PersistentVolumeClaim 是属于某个特定 namespace 的。

## :point_right:Service(svc)

针对容器的服务发现与负载均衡机制，提供了 Service 资源，并通过 kube-proxy 配合 cloud provider 来适应不同的应用场景。

Service 有四种类型：

- ClusterIP：默认类型，自动分配一个仅 cluster 内部可以访问的虚拟 IP
- NodePort：在 ClusterIP 基础上为 Service 在每台机器上绑定一个端口，这样就可以通过 `<NodeIP>:NodePort` 来访问该服务。如果 kube-proxy 设置了 `--nodeport-addresses=10.240.0.0/16`（v1.10 支持），那么仅该 NodePort 仅对设置在范围内的 IP 有效。
- LoadBalancer：在 NodePort 的基础上，借助 cloud provider 创建一个外部的负载均衡器，并将请求转发到 `<NodeIP>:NodePort`
- ExternalName：将服务通过 DNS CNAME 记录方式转发到指定的域名（通过 `spec.externlName` 设定）。需要 kube-dns 版本在 1.7 以上。

### :point_right:service 定义

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    run: nginx
  name: nginx
  namespace: default
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    run: nginx
  sessionAffinity: None
  type: ClusterIP
```

查看svc

`kubectl get service nginx`

查看自动创建的ep

kubectl get endpoints nginx

### :point_right:service 协议

Service、Endpoints 和 Pod 支持三种类型的协议：

- TCP（Transmission Control Protocol，传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议。
- UDP（User Datagram Protocol，用户数据报协议）是一种无连接的传输层协议，用于不可靠信息传送服务。
- SCTP（Stream Control Transmission Protocol，流控制传输协议），用于通过IP网传输SCN（Signaling Communication Network，信令通信网）窄带信令消息。

### :point_right:不指定 Selectors 的服务

在创建 Service 的时候，也可以不指定 Selectors，用来将 service 转发到 kubernetes 集群外部的服务（而不是 Pod）。

目前支持两种方法

（1）自定义 endpoint，即创建同名的 service 和 endpoint，在 endpoint 中设置外部服务的 IP 和端口

```yaml
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
---
kind: Endpoints
apiVersion: v1
metadata:
  name: my-service
subsets:
  - addresses:
      - ip: 1.2.3.4
    ports:
      - port: 9376
```

（2）通过 DNS 转发，在 service 定义中指定 externalName。此时 DNS 服务会给 `<service-name>.<namespace>.svc.cluster.local` 创建一个 CNAME 记录，其值为 `my.database.example.com`。并且，该服务不会自动分配 Cluster IP，需要通过 service 的 DNS 来访问。

```yaml
kind: Service
apiVersion: v1
metadata:
  name: my-service
  namespace: default
spec:
  type: ExternalName
  externalName: my.database.example.com
```

注意：Endpoints 的 IP 地址不能是 127.0.0.0/8、169.254.0.0/16 和 224.0.0.0/24，也不能是 Kubernetes 中其他服务的 clusterIP

### :point_right:Headless 服务

Headless 服务即不需要 Cluster IP 的服务，即在创建服务的时候指定 `spec.clusterIP=None`。包括两种类型

- 不指定 Selectors，但设置 externalName，即上面的（2），通过 CNAME 记录处理
- 指定 Selectors，通过 DNS A 记录设置后端 endpoint 列表

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  clusterIP: None
  ports:
  - name: tcp-80-80-3b6tl
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  sessionAffinity: None
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
  namespace: default
spec:
  replicas: 2
  revisionHistoryLimit: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        imagePullPolicy: Always
        name: nginx
        resources:
          limits:
            memory: 128Mi
          requests:
            cpu: 200m
            memory: 128Mi
      dnsPolicy: ClusterFirst
      restartPolicy: Always
```

### :point_right:保留源ip

> NodePort 和 LoadBalancer 类型能保留源ip 

各种类型的 Service 对源 IP 的处理方法不同：

- ClusterIP Service：使用 iptables 模式，集群内部的源 IP 会保留（不做 SNAT）。如果 client 和 server pod 在同一个 Node 上，那源 IP 就是 client pod 的 IP 地址；如果在不同的 Node 上，源 IP 则取决于网络插件是如何处理的，比如使用 flannel 时，源 IP 是 node flannel IP 地址。
- NodePort Service：默认情况下，源 IP 会做 SNAT，server pod 看到的源 IP 是 Node IP。为了避免这种情况，可以给 service 设置 `spec.ExternalTrafficPolicy=Local` （1.6-1.7 版本设置 Annotation `service.beta.kubernetes.io/external-traffic=OnlyLocal`），让 service 只代理本地 endpoint 的请求（如果没有本地 endpoint 则直接丢包），从而保留源 IP。
- LoadBalancer Service：默认情况下，源 IP 会做 SNAT，server pod 看到的源 IP 是 Node IP。设置 `service.spec.ExternalTrafficPolicy=Local` 后可以自动从云平台负载均衡器中删除没有本地 endpoint 的 Node，从而保留源 IP。

## :point_right:Ingress - 暴露服务

Ingress 可为 Service 提供外部可访问的 URL、负载均衡流量、终止 SSL/TLS，以及基于名称的虚拟托管。 [Ingress 控制器](https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress-controllers) 通常负责通过负载均衡器来实现 Ingress，尽管它也可以配置边缘路由器或其他前端来帮助处理流量。

Ingress 主要用来将服务暴露到 cluster 外面，并且可以自定义服务的访问策略。比如想要通过负载均衡器实现不同子域名到不同服务的访问：

```bash
foo.bar.com --|                 |-> foo.bar.com s1:80
              | 178.91.123.132  |
bar.foo.com --|                 |-> bar.foo.com s2:80
```

```bash
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - backend:
          serviceName: s1
          servicePort: 80
  - host: bar.foo.com
    http:
      paths:
      - backend:
          serviceName: s2
          servicePort: 80
```

## :point_right:Label - 标签

label 和 selector ，标签与选择器

## CustomResourceDefinition(crd) - 创建自定义资源

示例

```bash
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  # name must match the spec fields below, and be in the form: <plural>.<group>
  name: crontabs.stable.example.com
spec:
  # group name to use for REST API: /apis/<group>/<version>
  group: stable.example.com
  # versions to use for REST API: /apis/<group>/<version>
  versions:
  - name: v1beta1
    # Each version can be enabled/disabled by Served flag.
    served: true
    # One and only one version must be marked as the storage version.
    storage: true
  - name: v1
    served: true
    storage: false
  # either Namespaced or Cluster
  scope: Namespaced
  names:
    # plural name to be used in the URL: /apis/<group>/<version>/<plural>
    plural: crontabs
    # singular name to be used as an alias on the CLI and for display
    singular: crontab
    # kind is normally the CamelCased singular type. Your resource manifests use this.
    kind: CronTab
    # shortNames allow shorter string to match your resource on the CLI
    shortNames:
    - ct
```

### Finalizer - 预删除

>  用于实现控制器的异步预删除钩子，可以通过 `metadata.finalizers` 来指定 Finalizer。

```yaml
apiVersion: "stable.example.com/v1"
kind: CronTab
metadata:
  finalizers:
  - finalizer.stable.example.com
```

Finalizer 指定后，客户端删除对象的操作只会设置 `metadata.deletionTimestamp` 而不是直接删除。这会触发正在监听 CRD 的控制器，控制器执行一些删除前的清理操作，从列表中删除自己的 finalizer，然后再重新发起一个删除操作。此时，被删除的对象才会真正删除。

### Validation - 验证资源是否合规

> 使用该功能需要配置 kube-apiserver 的 `--feature-gates=CustomResourceValidation=true`。

比如下面的 CRD 要求

- `spec.cronSpec` 必须是匹配正则表达式的字符串
- `spec.replicas` 必须是从 1 到 10 的整数

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  version: v1
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
    shortNames:
    - ct
  validation:
   # openAPIV3Schema is the schema for validating custom objects.
    openAPIV3Schema:
      properties:
        spec:
          properties:
            cronSpec:
              type: string
              pattern: '^(\d+|\*)(/\d+)?(\s+(\d+|\*)(/\d+)?){4}$'
            replicas:
              type: integer
              minimum: 1
              maximum: 10
```

这样，在创建下面的 CronTab 时

```yaml
apiVersion: "stable.example.com/v1"
kind: CronTab
metadata:
  name: my-new-cron-object
spec:
  cronSpec: "* * * *"
  image: my-awesome-cron-image
  replicas: 15
```

会报验证失败的错误：

```bash
The CronTab "my-new-cron-object" is invalid: []: Invalid value: map[string]interface {}{"apiVersion":"stable.example.com/v1", "kind":"CronTab", "metadata":map[string]interface {}{"name":"my-new-cron-object", "namespace":"default", "deletionTimestamp":interface {}(nil), "deletionGracePeriodSeconds":(*int64)(nil), "creationTimestamp":"2017-09-05T05:20:07Z", "uid":"e14d79e7-91f9-11e7-a598-f0761cb232d1", "selfLink":"","clusterName":""}, "spec":map[string]interface {}{"cronSpec":"* * * *", "image":"my-awesome-cron-image", "replicas":15}}:
validation failure list:
spec.cronSpec in body should match '^(\d+|\*)(/\d+)?(\s+(\d+|\*)(/\d+)?){4}$'
spec.replicas in body should be less than or equal to 10
```

### Subresources - 状态和副本

v1.10 开始 CRD 还支持 `/status` 和 `/scale` 等两个子资源

```yaml
# resourcedefinition.yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  version: v1
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
    shortNames:
    - ct
  # subresources describes the subresources for custom resources.
  subresources:
    # status enables the status subresource.
    status: {}
    # scale enables the scale subresource.
    scale:
      # specReplicasPath defines the JSONPath inside of a custom resource that corresponds to Scale.Spec.Replicas.
      specReplicasPath: .spec.replicas
      # statusReplicasPath defines the JSONPath inside of a custom resource that corresponds to Scale.Status.Replicas.
      statusReplicasPath: .status.replicas
      # labelSelectorPath defines the JSONPath inside of a custom resource that corresponds to Scale.Status.Selector.
      labelSelectorPath: .status.labelSelector
```

设置后 可以使用 `kubectl scale --replicas=5 crontabs/my-new-cron-object`

### Categories - 对象分组

Categories 用来将 CRD 对象分组，这样就可以使用 `kubectl get <category-name>` 来查询属于该组的所有对象。

## :point_right:kubebuilder - 创建自定义资源（自己安装）

> 单独查阅

# 存储对象

## :point_right:Volume - 卷

Volume 机制以便将数据持久化存储，Kubernetes Volume 的生命周期与 Pod 绑定

- 容器挂掉后 Kubelet 再次重启容器时，Volume 的数据依然还在
- 而 Pod 删除时，Volume 才会清理。数据是否丢失取决于具体的 Volume 类型，比如 emptyDir 的数据会丢失，而 PV 的数据则不会丢

### :point_right:emptyDir 

如果 Pod 设置了 emptyDir 类型 Volume， Pod 被分配到 Node 上时候，会创建 emptyDir，只要 Pod 运行在 Node 上，emptyDir 都会存在（容器挂掉不会导致 emptyDir 丢失数据），但是如果 Pod 从 Node 上被删除（Pod 被删除，或者 Pod 发生迁移），emptyDir 也会被删除，并且永久丢失。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: gcr.io/google_containers/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir: {}
```

### :point_right:hostPath 

hostPath 允许挂载 Node 上的文件系统到 Pod 里面去。如果 Pod 需要使用 Node 上的文件，可以使用 hostPath。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: gcr.io/google_containers/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      path: /data
```

### :point_right:NFS

```yaml
volumes:
- name: nfs
  nfs:
    # FIXME: use the right hostname
    server: 10.254.234.223
    path: "/"
```

### :point_right:cephFS

```yaml
 volumes:
  - name: cephfs
    cephfs:
      monitors:
      - 192.168.139.10:6789
      - 192.168.139.11:6789
      - 192.168.139.12:6789
      user: cephfs
      path: /
      secretRef:
        name: ceph-secret
```

### :point_right:subPath - 子路径

Pod 的多个容器使用同一个 Volume 时，subPath 非常有用

```bash
apiVersion: v1
kind: Pod
metadata:
  name: my-lamp-site
spec:
    containers:
    - name: mysql
      image: mysql
      volumeMounts:
      - mountPath: /var/lib/mysql
        name: site-data
        subPath: mysql
    - name: php
      image: php
      volumeMounts:
      - mountPath: /var/www/html
        name: site-data
        subPath: html
    volumes:
    - name: site-data
      persistentVolumeClaim:
        claimName: my-lamp-site-data
```

## :point_right:PersistentVolume(pv) - PersistentVolumeClaim (PVC) - StorageClass(sc)

PersistentVolume (PV) 和 PersistentVolumeClaim (PVC) 提供了方便的持久化卷：**PV 提供网络存储资源，而 PVC 请求存储资源**。

### :point_right:Volume 生命周期

Volume 的生命周期包括 6个阶段

1. Provisioning，即 PV 的创建，可以直接创建 PV（静态方式），也可以使用 StorageClass 动态创建
2. Binding，将 PV 分配给 PVC
3. Using，Pod 通过 PVC 使用该 Volume，并可以通过准入控制 StorageObjectInUseProtection（1.9 及以前版本为 PVCProtection）阻止删除正在使用的 PVC
4. Releasing，Pod 释放 Volume 并删除 PVC
5. Reclaiming，回收 PV，可以保留 PV 以便下次使用，也可以直接从云存储中删除
6. Deleting，删除 PV 并从云存储中删除后段存储

根据这 6个阶段，Volume 的状态有以下 4 种

- Available：可用
- Bound：已经分配给 PVC
- Released：PVC 解绑但还未执行回收策略
- Failed：发生错误

### :point_right:pv

PersistentVolume（PV）是集群之中的一块网络存储。跟 Node 一样，也是集群的资源。PV 跟 Volume (卷) 类似，不过会有独立于 Pod 的生命周期。如nfs

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  nfs:
    path: /tmp
    server: 172.17.0.2
```

PV 的访问模式（accessModes）有三种：

- ReadWriteOnce（RWO）：是最基本的方式，可读可写，但只支持被单个节点挂载。
- ReadOnlyMany（ROX）：可以以只读的方式被多个节点挂载。
- ReadWriteMany（RWX）：这种存储可以以读写的方式被多个节点共享。不是每一种存储都支持这三种方式，像共享方式，目前支持的还比较少，比较常用的是 NFS。在 PVC 绑定 PV 时通常根据两个条件来绑定，一个是存储的大小，另一个就是访问模式。

PV 的回收策略（persistentVolumeReclaimPolicy，即 PVC 释放卷的时候 PV 该如何操作）也有三种

- Retain，不清理, 保留 Volume（需要手动清理）
- Recycle，删除数据，即 `rm -rf /thevolume/*`（只有 NFS 和 HostPath 支持）
- Delete，删除存储资源，比如删除 AWS EBS 卷（只有 AWS EBS, GCE PD, Azure Disk 和 Cinder 支持）

### :point_right:StorageClass 

动态创建pv

Kubernetes 还提供了 StorageClass来动态创建 PV，不仅节省了管理员的时间，还可以封装不同类型的存储供 PVC 选用。

StorageClass 包括四个部分

- provisioner：指定 Volume 插件的类型，包括内置插件（如 `kubernetes.io/glusterfs`）和外部插件（如 external-storage 提供的 `ceph.com/cephfs`）。
- mountOptions：指定挂载选项，当 PV 不支持指定的选项时会直接失败。比如 NFS 支持 `hard` 和 `nfsvers=4.1` 等选项。
- parameters：指定 provisioner 的选项，比如 `kubernetes.io/aws-ebs` 支持 `type`、`zone`、`iopsPerGB` 等参数。
- reclaimPolicy：指定回收策略，同 PV 的回收策略。

cephFS

```yaml
apiVersion: storage.k8s.io/v1
  kind: StorageClass
  metadata:
    name: fast
  provisioner: kubernetes.io/rbd
  parameters:
    monitors: 10.16.153.105:6789
    adminId: kube
    adminSecretName: ceph-secret
    adminSecretNamespace: kube-system
    pool: kube
    userId: kube
    userSecretName: ceph-secret-user
```

#### :point_right:Local Volume 

Local Volume 允许将 Node 本地的磁盘、分区或者目录作为持久化存储使用。注意，Local Volume 不支持动态创建，使用前需要预先创建好 PV。

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 100Gi
  # volumeMode field requires BlockVolume Alpha feature gate to be enabled.
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
---
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

### :point_right:PVC 

PV 是存储资源，而 PersistentVolumeClaim (PVC) 是对 PV 的请求。PVC 跟 Pod 类似：Pod 消费 Node 资源，而 PVC 消费 PV 资源；Pod 能够请求 CPU 和内存资源，而 PVC 请求特定大小和访问模式的数据卷。

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```

pvc可以直接挂载pod中

```bash
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: dockerfile/nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

### :point_right:扩展 PV 空间

明确配置 `allowVolumeExpansion=true` 的 StorageClass

```bash
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: gluster-vol-default
provisioner: kubernetes.io/glusterfs
parameters:
  resturl: "http://192.168.10.100:8080"
  restuser: ""
  secretNamespace: ""
  secretName: ""
allowVolumeExpansion: true
```

用户就可以修改 PVC 中请求存储的大小（如通过 `kubectl edit` 命令）请求更大的存储空间。

### 块存储（Raw Block Volume）

可通过设置 `volumeMode: Block`（可选项为 `Filesystem` 和 `Block`）来使用块存储。

```yaml
# Persistent Volumes using a Raw Block Volume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: block-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  volumeMode: Block # 块存储
  persistentVolumeReclaimPolicy: Retain
  fc:
    targetWWNs: ["50060e801049cfd1"]
    lun: 0
    readOnly: false
---
# Persistent Volume Claim requesting a Raw Block Volume
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: block-pvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Block  # 块存储
  resources:
    requests:
      storage: 10Gi
---
# Pod specification adding Raw Block Device path in container
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-block-volume
  annotations:
    # apparmor should be unconfied for mounting the device inside container.
    container.apparmor.security.beta.kubernetes.io/fc-container: unconfined
spec:
  containers:
    - name: fc-container
      image: fedora:26
      command: ["/bin/sh", "-c"]
      args: ["tail -f /dev/null"]
      securityContext:
        capabilities:
          # CAP_SYS_ADMIN is required for mount() syscall.
          add: ["SYS_ADMIN"]
      volumeDevices:
        - name: data
          devicePath: /dev/xvda
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: block-pvc
```

### :point_right:存储快照

卷快照能力为 Kubernetes 用户提供了一种标准的方式来在指定时间点复制卷的内容，并且不需要创建全新的卷。

API 对象 `VolumeSnapshot`，`VolumeSnapshotContent` 和 `VolumeSnapshotClass` 是 [CRD](https://kubernetes.io/zh-cn/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/)， 不属于核心 API。

```yaml
# create snapshot
apiVersion: snapshot.storage.k8s.io/v1alpha1
kind: VolumeSnapshot
metadata:
  name: new-snapshot-demo
  namespace: demo-namespace
spec:
  snapshotClassName: csi-snapclass
  source:
    name: mypvc
    kind: PersistentVolumeClaim

# import from snapshot
apiVersion: snapshot.storage.k8s.io/v1alpha1
kind: VolumeSnapshotContent
metadata:
  name: static-snapshot-content
spec:
  csiVolumeSnapshotSource:
    driver: com.example.csi-driver
    snapshotHandle: snapshotcontent-example-id
  volumeSnapshotRef:
    kind: VolumeSnapshot
    name: static-snapshot-demo
    namespace: demo-namespace

# provision volume from snapshot
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-restore
  Namespace: demo-namespace
spec:
  storageClassName: csi-storageclass
  dataSource:
    name: new-snapshot-demo
    kind: VolumeSnapshot
    apiGroup: snapshot.storage.k8s.io
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

## :point_right:Secret

Secret 解决了密码、token、密钥等敏感数据的配置问题，而不需要把这些敏感数据暴露到镜像或者 Pod Spec 中。Secret 可以以 Volume 或者环境变量的方式使用。

### :point_right:Secret 类型

Secret 有三种类型：

- Opaque：base64 编码格式的 Secret，用来存储密码、密钥等；但数据也通过 base64 --decode 解码得到原始数据，所有加密性很弱。
- `kubernetes.io/dockerconfigjson`：用来存储私有 docker registry 的认证信息。
- `kubernetes.io/service-account-token`： 用于被 serviceaccount 引用。serviceaccout 创建时 Kubernetes 会默认创建对应的 secret。Pod 如果使用了 serviceaccount，对应的 secret 会自动挂载到 Pod 的 `/run/secrets/kubernetes.io/serviceaccount` 目录中。

> serviceaccount 用来使得 Pod 能够访问 Kubernetes API

### :point_right:Opaque Secret

Opaque 类型的数据是一个 map 类型，要求 value 是 base64 编码格式：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  password: MWYyZDFlMmU2N2Rm
  username: YWRtaW4=
```

#### :point_right:将 Secret 挂载到 Volume 中

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: db
  name: db
spec:
  volumes:
  - name: secrets
    secret:
      secretName: mysecret
  containers:
  - image: gcr.io/my_project_id/pg:v1
    name: db
    volumeMounts:
    - name: secrets
      mountPath: "/etc/secrets"
      readOnly: true
    ports:
    - name: cp
      containerPort: 5432
      hostPort: 5432  
```

```bash
# ls /etc/secrets
password  username
# cat  /etc/secrets/username
admin
# cat  /etc/secrets/password
1f2d1e2e67df
```

#### :point_right:将 Secret 导入到 env中

```bash
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: wordpress-deployment
spec:
  replicas: 2
  strategy:
      type: RollingUpdate
  template:
    metadata:
      labels:
        app: wordpress
        visualize: "true"
    spec:
      containers:
      - name: "wordpress"
        image: "wordpress"
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_USER
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: username
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: password
```

```bash
env |grep WORDPRESS_DB_USER
env |grep WORDPRESS_DB_PASSWORD
```

### kubernetes.io/dockerconfigjson

创建

```bash
kubectl create secret docker-registry myregistrykey --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
secret "myregistrykey" created.
```

在创建 Pod 的时候，通过 `imagePullSecrets` 来引用刚创建的 `myregistrykey`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: foo
spec:
  containers:
    - name: foo
      image: janedoe/awesomeapp:v1
  imagePullSecrets:
    - name: myregistrykey
```

### kubernetes.io/service-account-token

`kubernetes.io/service-account-token`： 用于被 serviceaccount 引用。serviceaccout 创建时 Kubernetes 会默认创建对应的 secret。Pod 如果使用了 serviceaccount，对应的 secret 会自动挂载到 Pod 的 `/run/secrets/kubernetes.io/serviceaccount` 目录中。

```bash
kubectl exec nginx-3137573019-md1u2 ls /run/secrets/kubernetes.io/serviceaccount
ca.crt
namespace
token
```

### secrets加密存储到 etcd 

```yaml
kind: EncryptionConfig
apiVersion: v1
resources:
  - resources:
    - secrets
    providers:
    - aescbc:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - identity: {}
    - aesgcm:
        keys:
        - name: key1
          secret: c2VjcmV0IGlzIHNlY3VyZQ==
        - name: key2
          secret: dGhpcyBpcyBwYXNzd29yZA==
    - secretbox:
        keys:
        - name: key1
          secret: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXoxMjM0NTY=
```

其中

- resources.resources 是 Kubernetes 的资源名
- resources.providers 是加密方法，支持以下几种
  - identity：不加密
  - aescbc：AES-CBC 加密
  - secretbox：XSalsa20 和 Poly1305 加密
  - aesgcm：AES-GCM 加密

### :point_right:禁止更改的 Secret

和 Secret 设置 `immutable: true` 就可以禁止更改Secret 

好处：

- 保护应用，使之免受意外更新所带来的负面影响。
- 通过大幅降低对 kube-apiserver 的压力提升集群性能，这是因为 Kubernetes 会关闭不可变 Secret 的监视操作。

```yaml
apiVersion: v1
kind: Secret
metadata:
  ...
data:
  ...
immutable: true
```

## :point_right:ConfigMap

ConfigMap组件可以很好的帮助我们实现应用和配置分离，避免因为修改配置项而重新构建镜像。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: special-config
  namespace: default
data:
  special.how: very
  special.type: charm
```

### :point_right:ConfigMap 用作环境变量

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: ["/bin/sh", "-c", "env"]
      env:
        - name: SPECIAL_LEVEL_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: special.how
        - name: SPECIAL_TYPE_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: special.type
      envFrom:
        - configMapRef:
            name: env-config
  restartPolicy: Never
```

### :point_right:使用 volume 将 ConfigMap 作为文件或目录直接挂载

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: vol-test-pod
spec:
  containers:
    - name: test-container
      image: gcr.io/google_containers/busybox
      command: ["/bin/sh", "-c", "cat /etc/config/special.how"]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: special-config
  restartPolicy: Never
```

### :point_right:使用 subpath 将 ConfigMap 作为单独的文件挂载到目录

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: nginx
      command: ["/bin/sh","-c","sleep 36000"]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/nginx/special.how
        subPath: special.how
  volumes:
    - name: config-volume
      configMap:
        name: special-config
        items:
        - key: special.how
          path: special.how
  restartPolicy: Never
```

### :point_right:禁止更改的ConfigMap

给不需要经常修改的 ConfigMap `immutable: true` 就可以避免类似的问题。

不可变 ConfigMap 的好处包括：

- 保护应用，使之免受意外更新所带来的负面影响。
- 通过大幅降低对 kube-apiserver 的压力提升集群性能，这是因为 Kubernetes 会关闭不可变 ConfigMap 的监视操作。

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  ...
data:
  ...
immutable: true
```

# 策略对象

## SecurityContext - 安全上下文

安全上下文（Security Context）定义 Pod 或 Container 的特权与访问控制设置。 安全上下文包括但不限于：

- 自主访问控制（Discretionary Access Control）： 基于用户 ID（UID）和组 ID（GID）来判定对对象（例如文件）的访问权限。
- 安全性增强的 Linux（SELinux）： 为对象赋予安全性标签。
- 以特权模式或者非特权模式运行。
- Linux 权能: 为进程赋予 root 用户的部分特权而非全部特权。
- AppArmor：使用程序配置来限制个别程序的权能。

- Seccomp：过滤进程的系统调用。
- `allowPrivilegeEscalation`：控制进程是否可以获得超出其父进程的特权。 此布尔值直接控制是否为容器进程设置 no_new_privs标志。 当容器满足一下条件之一时，`allowPrivilegeEscalation` 总是为 true：
  - 以特权模式运行，或者
  - 具有 `CAP_SYS_ADMIN` 权能
- readOnlyRootFilesystem：以只读方式加载容器的根文件系统。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  volumes:
  - name: sec-ctx-vol
    emptyDir: {}
  containers:
  - name: sec-ctx-demo
    image: busybox:1.28
    command: [ "sh", "-c", "sleep 1h" ]
    volumeMounts:
    - name: sec-ctx-vol
      mountPath: /data/demo
    securityContext:
      allowPrivilegeEscalation: false
```

## :point_right:ResourceQuota- 限制用户资源用量

资源配额（Resource Quotas）是用来限制用户资源用量的一种机制。

它的工作原理为

- 资源配额应用在 Namespace 上，并且每个 Namespace 最多只能有一个 `ResourceQuota` 对象
- 开启计算资源配额后，创建容器时必须配置计算资源请求或限制（也可以用 [LimitRange](https://kubernetes.io/docs/tasks/administer-cluster/cpu-memory-limit/) 设置默认值）
- 用户超额后禁止创建新的资源

计算资源，包括 cpu 和 memory

- cpu, limits.cpu, requests.cpu
- memory, limits.memory, requests.memory

存储资源，包括存储资源的总量以及指定 storage class 的总量

- requests.storage：存储资源总量，如 500Gi
- persistentvolumeclaims：pvc 的个数
- .storageclass.storage.k8s.io/requests.storage
- .storageclass.storage.k8s.io/persistentvolumeclaims
- requests.ephemeral-storage 和 limits.ephemeral-storage 

对象数，即可创建的对象的个数

- pods, replicationcontrollers, configmaps, secrets
- resourcequotas, persistentvolumeclaims
- services, services.loadbalancers, services.nodeports

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    pods: "4"
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-counts
spec:
  hard:
    configmaps: "10"
    persistentvolumeclaims: "4"
    replicationcontrollers: "20"
    secrets: "10"
    services: "10"
    services.loadbalancers: "2"
```

## :point_right:LimitRange - 资源限制

默认情况下，Kubernetes 中所有容器都没有任何 CPU 和内存限制。LimitRange 用来给 Namespace 增加一个资源限制，包括最小、最大和默认资源。比如

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mylimits
spec:
  limits:
  - max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 200m
      memory: 6Mi
    type: Pod
  - default:
      cpu: 300m
      memory: 200Mi
    defaultRequest:
      cpu: 200m
      memory: 100Mi
    max:
      cpu: "2"
      memory: 1Gi
    min:
      cpu: 100m
      memory: 3Mi
    type: Container
```

# 身份对象

## :point_right:ServiceAccount -为 Pod 配置服务账户

Service account 是为了方便 Pod 里面的进程调用 Kubernetes API 或其他外部服务而设计的。

当你（自然人）访问集群时（例如，使用 `kubectl`），API 服务器将你的身份验证为 特定的用户帐户（当前这通常是 `admin`，除非你的集群管理员已经定制了你的集群配置）。 Pod 内的容器中的进程也可以与 api 服务器接触。 当它们进行身份验证时，它们被验证为特定的服务帐户（例如，`default`）。



当你创建 Pod 时，如果没有指定服务账户，Pod 会被指定给命名空间中的 `default` 服务账户。 如果你查看 Pod 的原始 JSON 或 YAML（例如：`kubectl get pods/<podname> -o yaml`）， 你可以看到 `spec.serviceAccountName` 字段已经被自动设置了。

**创建Service account** 

```bash
kubectl create serviceaccount jenkins
serviceaccount "jenkins" created
kubectl get serviceaccounts jenkins -o yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: 2017-05-27T14:32:25Z
  name: jenkins
  namespace: default
  resourceVersion: "45559"
  selfLink: /api/v1/namespaces/default/serviceaccounts/jenkins
  uid: 4d66eb4c-42e9-11e7-9860-ee7d8982865f
secrets:
- name: jenkins-token-l9v7v
```

**添加imagePullSecrets**

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: 2015-08-07T22:02:39Z
  name: default
  namespace: default
  selfLink: /api/v1/namespaces/default/serviceaccounts/default
  uid: 052fb0f4-3d50-11e5-b066-42010af0d7b6
secrets:
- name: default-token-uudge
imagePullSecrets:
- name: myregistrykey
```

**为pod分配服务账户**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /var/run/secrets/tokens
      name: vault-token
  serviceAccountName: build-robot
  volumes:
  - name: vault-token
    projected:
      sources:
      - serviceAccountToken:
          path: vault-token
          expirationSeconds: 7200
          audience: vault
```



## :point_right:Role、ClusterRole**、**RoleBinding、 **ClusterRoleBinding**

RBAC API 声明了四种 Kubernetes 对象：**Role**、**ClusterRole**、**RoleBinding** 和 **ClusterRoleBinding**。

### :point_right:Role 和 ClusterRole

RBAC 的 **Role** 或 **ClusterRole** 中包含一组代表相关权限的规则。 这些权限是纯粹累加的（不存在拒绝某操作的规则）。

Role 总是用来在某个namespace内设置访问权限； 在你创建 Role 时，你必须指定该 Role 所属的名字空间。\

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" 标明 core API 组
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```



ClusterRole 有若干用法。你可以用它来：

1. 定义对某名字空间域对象的访问权限，并将在个别名字空间内被授予访问权限；
2. 为名字空间作用域的对象设置访问权限，并被授予跨所有名字空间的访问权限；
3. 为集群作用域的资源定义访问权限。

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" 被忽略，因为 ClusterRoles 不受名字空间限制
  name: secret-reader
rules:
- apiGroups: [""]
  # 在 HTTP 层面，用来访问 Secret 资源的名称为 "secrets"
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

> 如果你希望在名字空间内定义角色，应该使用 Role； 如果你希望定义集群范围的角色，应该使用 ClusterRole

### :point_right:RoleBinding 和 ClusterRoleBinding

RoleBinding 在指定的名字空间中执行授权，而 ClusterRoleBinding 在集群范围执行授权。

**RoleBinding 例子**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# 此角色绑定允许 "jane" 读取 "default" 名字空间中的 Pod
# 你需要在该命名空间中有一个名为 “pod-reader” 的 Role
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# 你可以指定不止一个“subject（主体）”
- kind: User
  name: jane # "name" 是区分大小写的
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" 指定与某 Role 或 ClusterRole 的绑定关系
  kind: Role        # 此字段必须是 Role 或 ClusterRole
  name: pod-reader  # 此字段必须与你要绑定的 Role 或 ClusterRole 的名称匹配
  apiGroup: rbac.authorization.k8s.io
```

**ClusterRoleBinding 例子**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# 此集群角色绑定允许 “manager” 组中的任何人访问任何名字空间中的 Secret 资源
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager      # 'name' 是区分大小写的
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

# 网络策略

## :point_right:NetworkPolicy 

>  网络策略通过[网络插件](https://kubernetes.io/zh-cn/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)来实现。

Network Policy 提供了基于策略的网络控制，用于隔离应用并减少攻击面。它使用标签选择器模拟传统的分段网络，并通过策略控制它们之间的流量以及来自外部的流量。

### :point_right:namespace 隔离

默认情况下，所有 Pod 之间是全通的。每个 Namespace 可以配置独立的网络策略，来隔离 Pod 之间的流量。

比如默认拒绝所有 Pod 之间 Ingress 通信

```yaml
# 默认拒绝所有入站流量
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
spec:
  podSelector: {}
  policyTypes:
  - Ingress
--- # 默认拒绝所有入站和所有出站流量
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress

```

### :point_right:pod 隔离

通过使用标签选择器（包括 namespaceSelector 和 podSelector）来控制 Pod 之间的流量。

```yaml
# v1.6 以及更老的版本应该使用 extensions/v1beta1
# apiVersion: extensions/v1beta1
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: tcp
      port: 6379
```

