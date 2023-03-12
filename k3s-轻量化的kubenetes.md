<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
  - [:point_right:适用场景](#point_right%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF)
- [架构](#%E6%9E%B6%E6%9E%84)
  - [单节点架构](#%E5%8D%95%E8%8A%82%E7%82%B9%E6%9E%B6%E6%9E%84)
  - [:point_right:集群架构](#point_right%E9%9B%86%E7%BE%A4%E6%9E%B6%E6%9E%84)
  - [agent节点](#agent%E8%8A%82%E7%82%B9)
  - [:point_right:自动部署清单](#point_right%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E6%B8%85%E5%8D%95)
- [Autok3s](#autok3s)
- [安装](#%E5%AE%89%E8%A3%85)
  - [:point_right:国内快速安装](#point_right%E5%9B%BD%E5%86%85%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85)
  - [:point_right:配置](#point_right%E9%85%8D%E7%BD%AE)
  - [:point_right:网络选项](#point_right%E7%BD%91%E7%BB%9C%E9%80%89%E9%A1%B9)
  - [:point_right:创建高可用集群-外部数据库](#point_right%E5%88%9B%E5%BB%BA%E9%AB%98%E5%8F%AF%E7%94%A8%E9%9B%86%E7%BE%A4-%E5%A4%96%E9%83%A8%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [:point_right:创建高可用集群-嵌入式数据库](#point_right%E5%88%9B%E5%BB%BA%E9%AB%98%E5%8F%AF%E7%94%A8%E9%9B%86%E7%BE%A4-%E5%B5%8C%E5%85%A5%E5%BC%8F%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [私有镜像仓库配置](#%E7%A7%81%E6%9C%89%E9%95%9C%E5%83%8F%E4%BB%93%E5%BA%93%E9%85%8D%E7%BD%AE)
  - [禁用组件标志](#%E7%A6%81%E7%94%A8%E7%BB%84%E4%BB%B6%E6%A0%87%E5%BF%97)
  - [:point_right:使用 .skip 文件禁用组件](#point_right%E4%BD%BF%E7%94%A8-skip-%E6%96%87%E4%BB%B6%E7%A6%81%E7%94%A8%E7%BB%84%E4%BB%B6)
  - [Kubernetes 仪表板](#kubernetes-%E4%BB%AA%E8%A1%A8%E6%9D%BF)
- [:point_right:集群访问](#point_right%E9%9B%86%E7%BE%A4%E8%AE%BF%E9%97%AE)
- [升级](#%E5%8D%87%E7%BA%A7)
- [卸载](#%E5%8D%B8%E8%BD%BD)
- [:point_right:备份和恢复](#point_right%E5%A4%87%E4%BB%BD%E5%92%8C%E6%81%A2%E5%A4%8D)
- [:point_right:卷和存储](#point_right%E5%8D%B7%E5%92%8C%E5%AD%98%E5%82%A8)
  - [:point_right:Local Path](#point_rightlocal-path)
  - [:point_right:设置 Longhorn](#point_right%E8%AE%BE%E7%BD%AE-longhorn)
- [网络](#%E7%BD%91%E7%BB%9C)
- [Helm](#helm)
- [高级选项和配置](#%E9%AB%98%E7%BA%A7%E9%80%89%E9%A1%B9%E5%92%8C%E9%85%8D%E7%BD%AE)
- [:point_right:k3s 命令行](#point_rightk3s-%E5%91%BD%E4%BB%A4%E8%A1%8C)
- [k3d](#k3d)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

K3s 是一个轻量级的 Kubernetes 发行版，它针对边缘计算、物联网等场景进行了高度优化。

- 打包为单个二进制文件。

- 使用基于 sqlite3 的轻量级存储后端作为默认存储机制。同时支持使用 etcd3、MySQL 和 PostgreSQL 作为存储机制。

- 最大程度减轻了外部依赖性，K3s 仅需要 kernel 和 cgroup 挂载。 K3s 软件包需要的依赖项包括：

  - containerd
  - Flannel
  - CoreDNS
  - CNI
  - 主机实用程序（iptables、socat 等）
  - Ingress controller（Traefik）
  - 嵌入式服务负载均衡器（service load balancer）
  - 嵌入式网络策略控制器（network policy controller）

  [官方简介](https://docs.rancher.cn/docs/k3s/_index/)

## :point_right:适用场景

K3s 适用于以下场景：

- 边缘计算-Edge
- 物联网-IoT
- CI
- Development
- ARM
- 嵌入 K8s

# 架构

[官方架构说明](https://docs.rancher.cn/docs/k3s/architecture/_index)

## 单节点架构

一个master(k3s server)节点、可使用sqlite存储， 多个node(k3s agent)节点

## :point_right:集群架构

多个master(k3s server)节点、可使用mysql、etcd、postgres存储， 多个node(k3s agent)节点

##  agent节点

Agent 节点用`k3s agent`进程发起的 websocket 连接注册，连接由作为代理进程一部分运行的客户端负载均衡器维护。

Agent 将使用节点集群 secret 以及随机生成的节点密码向 k3s server 注册，密码存储在 `/etc/rancher/node/password`路径下。

server 将把各个节点的密码存储为 Kubernetes secrets，随后的任何尝试都必须使用相同的密码。节点密码秘密存储在`kube-system`命名空间中，名称使用模板`<host>.node-password.k3s`。

> 通过使用`--with-node-id`标志启动 K3s server 或 agent，可以将唯一的节点 ID 附加到主机名中。

## :point_right:自动部署清单

位于目录路径`/var/lib/rancher/k3s/server/manifests` 的清单在构建时被捆绑到 K3s 二进制文件中，将由[rancher/helm-controller](https://github.com/rancher/helm-controller#helm-controller)在运行时安装。

# Autok3s

[AutoK3s 使用指南](https://docs.rancher.cn/docs/k3s/autok3s/_index)：如果您使用的云厂商是 Alibaba、Tencent 或 AWS，建议您使用 AutoK3s 进行快速入门，详情请参考本文。

# 安装

安装前需要关闭防火墙

```bash
systemctl disable firewalld --now
```

## :point_right:国内快速安装

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
```

运行此安装后：

- K3s 服务将被配置为在节点重启后或进程崩溃或被杀死时自动重启。
- 将安装其他实用程序，包括`kubectl`、`crictl`、`ctr`、`k3s-killall.sh` 和 `k3s-uninstall.sh`。
- 将[kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)文件写入到`/etc/rancher/k3s/k3s.yaml`，由 K3s 安装的 kubectl 将自动使用该文件

要在工作节点上安装并将它们添加到集群

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -
```

设置`K3S_URL`参数会使 K3s 以 worker 模式运行。K3s agent 将在所提供的 URL 上向监听的 K3s 服务器注册。`K3S_TOKEN`使用的值存储在你的服务器节点上的`/var/lib/rancher/k3s/server/node-token`路径下。

> 每台计算机必须具有唯一的主机名。如果您的计算机没有唯一的主机名，请传递`K3S_NODE_NAME`环境变量，并为每个节点提供一个有效且唯一的主机名

## :point_right:配置

配置文件：认情况下，安装时将使用位于 `/etc/rancher/k3s/config.yaml` 的 YAML 文件中的值。

你可以通过 `K3S_` 环境变量配置 K3s：

```bash
K3S_KUBECONFIG_MODE="644" k3s server
```

或使用命令标志：

```bash
k3s server --write-kubeconfig-mode 644
```

[server配置](https://docs.k3s.io/zh/reference/server-config)

[agent配置](https://docs.k3s.io/zh/reference/agent-config)

[环境变量](https://docs.k3s.io/zh/reference/env-variables)

## :point_right:网络选项

**Flannel 选项**

Flannel 是第 3 层网络结构的轻量级提供程序

- Flannel 只能在 Server 节点上设置选项，并且集群中的所有 Server 上都必须相同。
- Flannel 的默认后端是 `vxlan`。要启用加密，请使用 `wireguard-native` 后端

**自定义CNI**

使用 `--flannel-backend=none` 启动 K3s 并安装你选择的 CNI。

以多master集群，calico 为例

```bash
# 第一台master
export INSTALL_K3S_EXEC="--flannel-backend=none --disable=traefik --cluster-cidr=172.16.2.0/24 --cluster-init"
curl -sfL https://get.k3s.io | sh -

# 其余master 添加--server参数，将其指向第一个节点的IP地址
export K3S_TOKEN="your_token"
export INSTALL_K3S_EXEC="--flannel-backend=none --disable=traefik --cluster-cidr=172.16.2.0/24 --server https://10.0.0.60:6443"
curl -sfL https://get.k3s.io | sh -

# agent 节点
export K3S_URL=https://10.0.0.60:6443
export K3S_TOKEN="secret_edgecluster_token"
curl -sfL https://get.k3s.io | sh -

# 先从其中一个server节点复制/etc/rancher/k3s/k3s.yaml到你的本地工作站并将KUBECONFIG环境变量指向它。别忘了在YAML文件中更新master URL。这提供了通过kubectl CLI对K3s集群的远程访问。

# calico
wget https://docs.projectcalico.org/manifests/tigera-operator.yaml
wget https://docs.projectcalico.org/manifests/custom-resources.yaml
kubectl create -f tigera-operator.yaml
kubectl create -f custom-resources.yaml

# 在calico-system命名空间中修改cni-config configmap以启用IP转发
kubectl edit cm cni-config -n calico-system

"container_settings": {
              "allow_ip_forwarding": true
          }
          
# kubectl get pods -n calico-system
```

## :point_right:创建高可用集群-外部数据库

你首先需要为集群创建一个外部数据存储。建议使用etcd，如果你的团队没有操作 etcd 的专业知识，你可以选择 MySQL 或 PostgreSQL 等企业级 SQL 数据库。

以mysql为例：

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - server \
  --token=SECRET \
  --datastore-endpoint="mysql://username:password@tcp(hostname:3306)/database-name"
```

如果第一个 Server 节点是在没有 `--token` CLI 标志或 `K3S_TOKEN` 变量的情况下启动的，那么可以从任何已经加入集群的 Server 节点中检索到 Token：

```bash
cat /var/lib/rancher/k3s/server/token
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -s - server \
  --token=SECRET \
  --datastore-endpoint="mysql://username:password@tcp(hostname:3306)/database-name"
```

有几个配置标志在所有 Server 节点中必须是相同的:

- 网络相关标志：`--cluster-dns`、`--cluster-domain`、`--cluster-cidr`、`--service- cidr`
- 控制某些组件部署的标志：`--disable-helm-controller`、`--disable-kube-proxy`、`--disable-network-policy` 和任何传递给 `--disable` 的组件
- 功能相关标志：`--secrets-encryption`

**加入 Agent 节点**

因为 K3s Server 节点默认是可调度的，所以 HA K3s Server 集群的最小节点数是两个 Server 节点和零个 Agent 节点。

在 HA 集群中加入 Agent 节点与在单个 Server 集群中加入 Agent 节点是一样的。你只需要指定 Agent 应该注册的 URL 和要使用的 Token 即可。

```bash
K3S_TOKEN=SECRET k3s agent --server https://fixed-registration-address:6443
```

## :point_right:创建高可用集群-嵌入式数据库

嵌入式 etcd (HA) 在速度较慢的磁盘（例如使用 SD 卡运行的 Raspberry Pi）上可能会出现性能问题。

要在这种模式下运行 K3s，你必须拥有奇数个 Server 节点。我们建议从三个节点开始。

首先，启动一个带有 `cluster-init` 标志的 Server 节点来启用集群和一个令牌，该令牌将作为共享 secret，用于将其他服务器加入集群。

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn K3S_TOKEN=SECRET sh -s - server --cluster-init
```

使用共享 secret 将第二台和第三台服务器加入集群：

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn K3S_TOKEN=SECRET sh -s - server --server https://<ip or hostname of server1>:6443
```

有几个配置标志在所有 Server 节点中必须是相同的:

- 网络相关标志：`--cluster-dns`、`--cluster-domain`、`--cluster-cidr`、`--service- cidr`
- 控制某些组件部署的标志：`--disable-helm-controller`、`--disable-kube-proxy`、`--disable-network-policy` 和任何传递给 `--disable` 的组件
- 功能相关标志：`--secrets-encryption`

如果你有一个使用默认嵌入式 SQLite 数据库的现有集群，你可以通过使用 `--cluster-init` 标志重新启动你的 K3s server，从而将其转换为 etcd。完成此操作后，你将能够如上所述添加其他实例。

## 私有镜像仓库配置

可以将 Containerd 配置为连接到私有镜像仓库，并在节点上使用私有镜像仓库拉取私有镜像。

启动时，K3s 会检查 `/etc/rancher/k3s/` 中是否存在 `registries.yaml` 文件，并指示 containerd 使用该文件中定义的镜像仓库。如果你想使用私有的镜像仓库，你需要在每个使用镜像仓库的节点上以 root 身份创建这个文件。

该文件由两个主要部分组成：

- mirrors
- configs

```bash
mirrors:
  mycustomreg.com:
    endpoint:
      - "https://mycustomreg.com:5000"
configs:
  "mycustomreg:5000":
    auth:
      username: xxxxxx # this is the registry username
      password: xxxxxx # this is the registry password
    tls:
      cert_file: # path to the cert file used in the registry
      key_file:  # path to the key file used in the registry
      ca_file:   # path to the ca file used in the registry
# 重写
mirrors:
  docker.io:
    endpoint:
      - "https://registry.example.com:5000"
    rewrite:
      "^rancher/(.*)": "mirrorproject/rancher-images/$1"
      
```

## 禁用组件标志

假设你通过将 `--cluster-init` 标志传递给 Server 进程来运行带有嵌入式 etcd 的 K3s Server。

要运行只包含 etcd 组件的 K3s Server，你可以将 `--disable-apiserver --disable-controller-manager --disable-scheduler` 标志传递给 K3s，这样就能运行只有 etcd 组件的 Server 节点。例如，使用以下标志运行 K3s Server：

```bash
curl -fL https://get.k3s.io | sh -s - server --cluster-init --disable-apiserver --disable-controller-manager --disable-scheduler
```

禁用etcd

```bash
curl -fL https://get.k3s.io | sh -s - server --token <token> --disable-etcd --server https://<etcd-only-node>:6443
```

## :point_right:使用 .skip 文件禁用组件

对于 `/var/lib/rancher/k3s/server/manifests` 下的任何 YAML 文件（coredns、traefik、local-storeage 等），你可以添加一个 `.skip` 文件，这将导致 K3s 不应用相关的 YAML 文件。 

```bash
$ ls /var/lib/rancher/k3s/server/manifests
ccm.yaml      local-storage.yaml  rolebindings.yaml  traefik.yaml.skip
coredns.yaml  traefik.yaml
```

## Kubernetes 仪表板

```bash
GITHUB_URL=https://github.com/kubernetes/dashboard/releases
VERSION_KUBE_DASHBOARD=$(curl -w '%{url_effective}' -I -L -s -S ${GITHUB_URL}/latest -o /dev/null | sed -e 's|.*/||')
sudo k3s kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/${VERSION_KUBE_DASHBOARD}/aio/deploy/recommended.yaml
```

SA 和rolebinding

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
  
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

```bash
sudo k3s kubectl create -f dashboard.admin-user.yml -f dashboard.admin-user-role.yml
```

# :point_right:集群访问

`/etc/rancher/k3s/k3s.yaml` 中存储的 kubeconfig 文件用于配置对 Kubernetes 集群的访问

```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
kubectl get pods --all-namespaces
helm ls --all-namespaces

kubectl --kubeconfig /etc/rancher/k3s/k3s.yaml get pods --all-namespaces
helm --kubeconfig /etc/rancher/k3s/k3s.yaml ls --all-namespaces
```

将 `/etc/rancher/k3s/k3s.yaml` 复制到位于集群外部的主机上的 `~/.kube/config`。

# 升级

```bash
# 停止进程
/usr/local/bin/k3s-killall.sh

# 升级
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
# 升级到指定版本
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=vX.Y.Z-rc1 sh -

# 重启
sudo systemctl restart k3s
sudo systemctl restart k3s-agent
```

# 卸载

```bash
# server
./k3s-uninstall.sh 
# agent
./k3s-agent-uninstall.sh
```

# :point_right:备份和恢复

[备份官方文档](https://docs.k3s.io/zh/backup-restore)

**外部数据库**

使用外部数据存储时，备份和恢复操作在 K3s 外面处理。pg、mysql、etcd

**使用嵌入式数据库**

快照目录默认为 `${data-dir}/server/db/snapshots`。data-dir 的默认值为 `/var/lib/rancher/k3s`，你可以通过设置 `--data-dir` 标志来更改它。

```bash
k3s server \
  --cluster-reset \
  --cluster-reset-restore-path=<PATH-TO-SNAPSHOT>
```

# :point_right:卷和存储

在部署需要保留数据的应用时，你需要创建持久存储。持久存储允许你在运行应用的 pod 之外存储应用数据。即使运行应用的 pod 发生故障，这种存储方式也能让你保留应用数据。

K3s 移除了卷插件，为了让k3s更小

##  :point_right:Local Path

K3s 自带 Rancher 的 Local Path Provisioner，这使得能够使用各自节点上的本地存储来开箱即用地创建持久卷声明。

pvc.yaml

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: local-path-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: local-path
  resources:
    requests:
      storage: 2Gi
```

pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-test
  namespace: default
spec:
  containers:
  - name: volume-test
    image: nginx:stable-alpine
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - name: volv
      mountPath: /data
    ports:
    - containerPort: 80
  volumes:
  - name: volv
    persistentVolumeClaim:
      claimName: local-path-pvc
```

kubectl create -f pvc.yaml
kubectl create -f pod.yaml

PVC 和 pv 状态都应该是 Bound。

## :point_right:设置 Longhorn

```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/master/deploy/longhorn.yaml
```

Longhorn 将安装在命名空间 `longhorn-system` 中。

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: longhorn-volv-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: longhorn
  resources:
    requests:
      storage: 2Gi
  
---
apiVersion: v1
kind: Pod
metadata:
  name: volume-test
  namespace: default
spec:
  containers:
  - name: volume-test
    image: nginx:stable-alpine
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - name: volv
      mountPath: /data
    ports:
    - containerPort: 80
  volumes:
  - name: volv
    persistentVolumeClaim:
      claimName: longhorn-volv-pvc
```

# 网络

**CoreDNS** 在 Server 启动时自动部署。要禁用它，请使用 `--disable=coredns` 选项来配置集群中的所有 Server。

[Traefik](https://traefik.io/) 是一种现代的 HTTP 反向代理和负载均衡器，可以轻松部署微服务

ServiceLB任何 Service Load Balancer (LB) 都可以在你的 K3s 集群中使用。

要排除 ServiceLB 使用的节点，请将以下标签添加到托管 ServiceLB Pod 的节点。所有未标记的节点都不会用于 ServiceLB。

```bash
svccontroller.k3s.cattle.io/enablelb
```

# Helm 

Helm Chart 为 Kubernetes YAML 清单文件提供了模板语法

**helm crd**

[HelmChart 资源定义](https://github.com/k3s-io/helm-controller#helm-controller)捕获了你通常传递给 `helm` 命令行工具的大部分选项。以下示例说明了如何从默认 Chart 仓库部署 Grafana，并覆盖某些默认的 Chart 值。请注意，HelmChart 资源本身位于 `kube-system` 命名空间中，但 Chart 的资源将部署到 `monitoring` 命名空间。]

```bash
apiVersion: helm.cattle.io/v1
kind: HelmChart
metadata:
  name: grafana
  namespace: kube-system
spec:
  chart: stable/grafana
  targetNamespace: monitoring
  set:
    adminPassword: "NotVerySafePassword"
  valuesContent: |-
    image:
      tag: master
    env:
      GF_EXPLORE_ENABLED: true
    adminUser: admin
    sidecar:
      datasources:
        enabled: true
```

# 高级选项和配置

[选项和配置官网文档](https://docs.k3s.io/zh/advanced)

# :point_right:k3s 命令行

```bash
[root@iv-yca74nicm1egg4fk7eo9 ~]# k3s --help
NAME:
   k3s - Kubernetes, but small and simple

USAGE:
   k3s [global options] command [command options] [arguments...]

VERSION:
   v1.25.6+k3s1 (9176e03c)

COMMANDS:
   server           Run management server
   agent            Run node agent
   kubectl          Run kubectl
   crictl           Run crictl
   ctr              Run ctr
   check-config     Run config check
   etcd-snapshot    Trigger an immediate etcd snapshot
   secrets-encrypt  Control secrets encryption and keys rotation
   certificate      Certificates management
   completion       Install shell completion script
   help, h          Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --debug                     (logging) Turn on debug logs [$K3S_DEBUG]
   --data-dir value, -d value  (data) Folder to hold state (default: /var/lib/rancher/k3s or ${HOME}/.rancher/k3s if not root)
   --help, -h                  show help
   --version, -v               print the version
```

# k3d

k3d 是一个轻量级的包装器，用于在 Docker 中运行 k3s（一个最小化的 Kubernetes 发行版，Rancher Desktop 也使用它）。k3d 能让你轻松在 Docker 中创建单节点和多节点 k3s 集群（例如用于 Kubernetes 上的本地开发）

安装k3d

```bas
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

运行 `k3d cluster create` 命令来启动多节点集群。

```bas
k3d cluster create two-node-cluster --agents 2
k3d cluster create three-node-cluster --agents 3
```

[k3d官网文档](https://k3d.io/v5.4.8/)
