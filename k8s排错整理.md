<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [集群排错](#%E9%9B%86%E7%BE%A4%E6%8E%92%E9%94%99)
  - [:point_right:查看node状态](#point_right%E6%9F%A5%E7%9C%8Bnode%E7%8A%B6%E6%80%81)
  - [:point_right:查看系统日志](#point_right%E6%9F%A5%E7%9C%8B%E7%B3%BB%E7%BB%9F%E6%97%A5%E5%BF%97)
  - [Node NotReady](#node-notready)
  - [/sys/fs/cgroup 空间不足](#sysfscgroup-%E7%A9%BA%E9%97%B4%E4%B8%8D%E8%B6%B3)
  - [内存泄漏](#%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F)
- [Pod排错](#pod%E6%8E%92%E9%94%99)
  - [:point_right:查看pod日志](#point_right%E6%9F%A5%E7%9C%8Bpod%E6%97%A5%E5%BF%97)
  - [:point_right:Pod 一直处于 Pending 状态](#point_rightpod-%E4%B8%80%E7%9B%B4%E5%A4%84%E4%BA%8E-pending-%E7%8A%B6%E6%80%81)
  - [:point_right:Pod 一直处于 Waiting 或 ContainerCreating 状态](#point_rightpod-%E4%B8%80%E7%9B%B4%E5%A4%84%E4%BA%8E-waiting-%E6%88%96-containercreating-%E7%8A%B6%E6%80%81)
  - [Pod 处于 Error 状态](#pod-%E5%A4%84%E4%BA%8E-error-%E7%8A%B6%E6%80%81)
- [网络排错](#%E7%BD%91%E7%BB%9C%E6%8E%92%E9%94%99)
  - [查看网络的POD是否正常](#%E6%9F%A5%E7%9C%8B%E7%BD%91%E7%BB%9C%E7%9A%84pod%E6%98%AF%E5%90%A6%E6%AD%A3%E5%B8%B8)
  - [:point_right:Pod 无法解析 DNS](#point_rightpod-%E6%97%A0%E6%B3%95%E8%A7%A3%E6%9E%90-dns)
  - [Service 无法访问](#service-%E6%97%A0%E6%B3%95%E8%AE%BF%E9%97%AE)
- [存储排错](#%E5%AD%98%E5%82%A8%E6%8E%92%E9%94%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 集群排错

排查集群问题通常从 Node 和 Kubernetes 服务的状态出发，定位出具体的异常服务，再进而寻找解决方法。集群状态异常可能的原因比较多，常见的有：**设备异常、网络异常、服务异常、数据异常、操作失误**等

按照不同的组件来说，具体的原因可能包括

- kube-apiserver 无法启动会导致
  - 集群不可访问
  - 已有的 Pod 和服务正常运行（依赖于 Kubernetes API 的除外）
- etcd 集群异常会导致
  - kube-apiserver 无法正常读写集群状态，进而导致 Kubernetes API 访问出错
  - kubelet 无法周期性更新状态
- kube-controller-manager/kube-scheduler 异常会导致
  - 复制控制器、节点控制器、云服务控制器等无法工作，从而导致 Deployment、Service 等无法工作，也无法注册新的 Node 到集群中来
  - 新创建的 Pod 无法调度（总是 Pending 状态）
- Node 本身宕机或者 Kubelet 无法启动会导致
  - Node 上面的 Pod 无法正常运行
  - 已在运行的 Pod 无法正常终止
- 网络分区会导致 Kubelet 等与控制平面通信异常以及 Pod 之间通信异常

## :point_right:查看node状态

```bash
kubectl get nodes -o wide
kubectl describe node ${node_name}
```

## :point_right:查看系统日志

**systemd管理**

日志文件

- /var/log/kube-apiserver.log
- /var/log/kube-scheduler.log
- /var/log/kube-controller-manager.log
- /var/log/kubelet.log
- /var/log/kube-proxy.log

或者使用`journactl`

```bash
journalctl -l -u kube-apiserver
journalctl -l -u kube-controller-manager
journalctl -l -u kube-scheduler
journalctl -l -u kubelet
journalctl -l -u kube-proxy
```

static 静态管理

```bash
# pod_name 改成 kube-apiserver、 kube-controller-manager、kube-scheduler、kube-proxy、kube-dns
kubectl -n kube-system logs ${pod_name} 
```

## Node NotReady

排查时最重要的就是执行 `kubectl describe node <node name>` 并查看 Kubelet 日志中的错误信息。

常见问题

- Kubelet 未启动或者异常挂起：重新启动 Kubelet。
- CNI 网络插件未部署：部署 CNI 插件。
- Docker或者containerd 僵死（API 不响应）：重启 Docker或者containerd。
- 磁盘空间不足：清理磁盘空间，比如镜像、临时文件等。

> 为使用的镜像尽量不要保存在设备上，如果是containerd，可以使用nerdctl image prune删除

##  /sys/fs/cgroup 空间不足

很多发行版默认的 fs.inotify.max_user_watches 太小，可以通过更改内核参数解决。

```sh
sysctl fs.inotify.max_user_watches=524288
```

## 内存泄漏

内核3.X上的老版本可能会出现

查看 `cat /sys/fs/cgroup/memory/kubepods/memory.kmem.slabinfo` 没输出说明没内存泄漏

# Pod排错

## :point_right:查看pod日志

都可以执行以下命令来查看 Pod 的状态

查看 Pod 的配置是否正确

```bash
kubectl get pod ${pod_name} -o yaml
```

查看 Pod 的事件

```bash
kubectl describe pod ${pod_name}
```

查看容器日志

```bash
kubectl logs ${pod_name} [-c $container_name}]
```

## :point_right:Pod 一直处于 Pending 状态

`kubectl describe pod <pod-name>` 查看事件可能的原因包括

- 资源不足，集群内所有的 Node 都不满足该 Pod 请求的 CPU、内存、GPU 或者临时存储空间等资源。解决方法是删除集群内不用的 Pod 或者增加新的 Node。
- HostPort 端口已被占用，通常推荐使用 Service 对外开放服务端口

## :point_right:Pod 一直处于 Waiting 或 ContainerCreating 状态

`kubectl describe pod <pod-name>` 查看事件，其次查看 Kubelet 日志。可能的原因有

- 镜像拉取失败，比如
  - 配置了错误的镜像
  - Kubelet 无法访问镜像（国内环境访问 `gcr.io` 需要特殊处理）
  - 私有镜像的密钥配置错误
  - 镜像太大，拉取超时（可以适当调整 kubelet 的 `--image-pull-progress-deadline` 和 `--runtime-request-timeout` 选项）
- CNI 网络错误，一般需要检查 CNI 网络插件的配置，比如
  - 无法配置 Pod 网络
  - 无法分配 IP 地址
- 容器无法启动，需要检查是否打包了正确的镜像或者是否配置了正确的容器参数

## Pod 处于 Error 状态

通常处于 Error 状态说明 Pod 启动过程中发生了错误。常见的原因包括

- 依赖的 ConfigMap、Secret 或者 PV 等不存在
- 请求的资源超过了管理员设置的限制，比如超过了 LimitRange 等
- 违反集群的安全策略，比如违反了 PodSecurityPolicy 等
- 容器无权操作集群内的资源，比如开启 RBAC 后，需要为 ServiceAccount 配置角色绑定



# 网络排错

说到 Kubernetes 的网络，其实无非就是以下三种情况之一

- Pod 访问容器外部网络
- 从容器外部访问 Pod 网络
- Pod 之间相互访问

原因有下

- CNI 网络插件配置错误，导致多主机网络不通，比如
  - IP 网段与现有网络冲突
  - 插件使用了底层网络不支持的协议
  - 忘记开启 IP 转发等
    - `sysctl net.ipv4.ip_forward`
    - `sysctl net.bridge.bridge-nf-call-iptables`
- Pod 网络路由丢失，比如
  - kubenet 要求网络中有 podCIDR 到主机 IP 地址的路由，这些路由如果没有正确配置会导致 Pod 网络通信等问题
  - 在公有云平台上，kube-controller-manager 会自动为所有 Node 配置路由，但如果配置不当（如认证授权失败、超出配额等），也有可能导致无法配置路由
- 主机内或者云平台的安全组、防火墙或者安全策略等阻止了 Pod 网络

## 查看网络的POD是否正常

> flannel 或者calico

```bash
kubectl -n kube-system get pod

# 查看描述或日志
kubectl -n kube-system describe ${pod_name}
kubectl -n kube-system logs ${pod_name}
```

## :point_right:Pod 无法解析 DNS

DNS 无法解析也有可能是 kube-dns 服务异常导致的，检查 kube-dns 是否处于正常运行状态

```bas
kubectl get pods -n kube-system |grep  kube-dns
```

检查service 检查endpoint

```bash
kubectl get svc kube-dns --namespace=kube-system
kubectl get ep kube-dns --namespace=kube-system
```

检查coredns 的configmap

```B
kubectl get cm -n kube-system  |grep core
```

## Service 无法访问

确认是否有对应的 Endpoints

```bash
kubectl get endpoints ${service_name}

# 查询匹配 LabelSelector 的 Pod
kubectl get pods -l key1=value1,key2=value2
```

> 最后查看iptables规则

# 存储排错

存储异常（PV、PVC、StorageClass等）的排错方法

```bash
kubectl get pv
kubectl get pvc
kubectl get sc

kubectl describe pv <pv-name>
kubectl describe pvc <pvc-name>
kubectl describe sc <storage-class-name>
```

