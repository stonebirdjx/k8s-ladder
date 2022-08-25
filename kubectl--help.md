<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [kubectl](#kubectl)
  - [kubectl 版本号](#kubectl-%E7%89%88%E6%9C%AC%E5%8F%B7)
  - [kubectl 集群信息](#kubectl-%E9%9B%86%E7%BE%A4%E4%BF%A1%E6%81%AF)
  - [kubectl -h](#kubectl--h)
  - [kubectl 命令格式](#kubectl-%E5%91%BD%E4%BB%A4%E6%A0%BC%E5%BC%8F)
  - [kubectl 配置文件](#kubectl-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# kubectl

在 kubernetes 中，kubectl 实现全部的kubernetesapi， 我们使用 kubectl 命令与 kubernetes 交互。

## kubectl 版本号

可以使用 `kubectl version` 检查 kubernetes 的版本信息。

```bash
kubectl version
```

## kubectl 集群信息

除了版本信息，我们还可以通过 kubectl 获取更多 kubernetes 集群的相关信息：

```
kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
KubeDNS is running at https://192.168.99.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

上述信息给出了集群 API 服务的地址，如果使用 `kubectl cluster-info dump`，可以看到更多集群的信息。

## kubectl -h

kubectl 提供了非常好的帮助信息，我们可以通过 `kubectl -h` 来获取帮助信息。

```bash
kubectl -h
kubectl controls the Kubernetes cluster manager.

 Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
  create         Create a resource from a file or from stdin.
  expose         使用 replication controller, service, deployment 或者 pod 并暴露它作为一个 新的
Kubernetes Service
  run            在集群中运行一个指定的镜像
  set            为 objects 设置一个指定的特征

Basic Commands (Intermediate):
  explain        查看资源的文档
  get            显示一个或更多 resources
  edit           在服务器上编辑一个资源
  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout        Manage the rollout of a resource
  scale          为 Deployment, ReplicaSet, Replication Controller 或者 Job 设置一个新的副本数量
  autoscale      自动调整一个 Deployment, ReplicaSet, 或者 ReplicationController 的副本数量

Cluster Management Commands:
  certificate    修改 certificate 资源.
  cluster-info   显示集群信息
  top            Display Resource (CPU/Memory/Storage) usage.
  cordon         标记 node 为 unschedulable
  uncordon       标记 node 为 schedulable
  drain          Drain node in preparation for maintenance
  taint          更新一个或者多个 node 上的 taints

Troubleshooting and Debugging Commands:
  describe       显示一个指定 resource 或者 group 的 resources 详情
  logs           输出容器在 pod 中的日志
  attach         Attach 到一个运行中的 container
  exec           在一个 container 中执行一个命令
  port-forward   Forward one or more local ports to a pod
  proxy          运行一个 proxy 到 Kubernetes API server
  cp             复制 files 和 directories 到 containers 和从容器中复制 files 和 directories.
  auth           Inspect authorization

Advanced Commands:
  diff           Diff live version against would-be applied version
  apply          通过文件名或标准输入流(stdin)对资源进行配置
  patch          使用 strategic merge patch 更新一个资源的 field(s)
  replace        通过 filename 或者 stdin替换一个资源
  wait           Experimental: Wait for a specific condition on one or many resources.
  convert        在不同的 API versions 转换配置文件
  kustomize      Build a kustomization target from a directory or a remote url.

Settings Commands:
  label          更新在这个资源上的 labels
  annotate       更新一个资源的注解
  completion     Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  api-resources  Print the supported API resources on the server
  api-versions   Print the supported API versions on the server, in the form of "group/version"
  config         修改 kubeconfig 文件
  plugin         Provides utilities for interacting with plugins.
  version        输出 client 和 server 的版本信息

Usage:
  kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

## kubectl 命令格式

kubectl 命令的基本格式是 `kubectl <action> <resource>`，其中 `action` 可以是 `create`, `delete`, `get` 等等，`resource` 你可以使用 `kubectl api-resources` 获得完整列表。

例如，你可以通过 `kubectl get nodes` 获取节点信息，可以通过 `kubectl describe nodes ${NODENAME}` 获取节点详细信息。

## kubectl 配置文件

kubectl 通过读取配置文件信息与 kubernetes 集群交互，默认配置文件路径是 `~/.kube/config`，内容可能如下：

```
$ cat ~/.kube/config
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /Users/deyuandeng/.minikube/ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /Users/deyuandeng/.minikube/apiserver.crt
    client-key: /Users/deyuandeng/.minikube/apiserver.key
```

这里有三个重要的顶级概念: `clusters`, `users` 和 `contexts`。

我们使用的集群名为 `minikube`，其服务器地址为 `https://192.168.99.100:8443`，其认证证书位于 `${HOME}/.minikube/ca.crt`。

当我们使用该 kubeconfig 发送请求时，我们充当用户 `minikube` (确切地说，真正的用户名来自证书通用名，但是我们暂时跳过它)。

最后，`context` 是各种配置的组合，例如，存在两个 `context`，一个用于集群 `minikube` 以及用户 `minikube`，另一个用于集群 `example` 和用户 `minikube`，即这意味着用户 `minikube` 可以同时访问 `minikube` 和 `example` 集群。