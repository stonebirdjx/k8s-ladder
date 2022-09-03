<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Containerd 架构](#containerd-%E6%9E%B6%E6%9E%84)
- [ctr 命令 (了解即可)](#ctr-%E5%91%BD%E4%BB%A4-%E4%BA%86%E8%A7%A3%E5%8D%B3%E5%8F%AF)
  - [plugins, plugin - 查看插件信息](#plugins-plugin---%E6%9F%A5%E7%9C%8B%E6%8F%92%E4%BB%B6%E4%BF%A1%E6%81%AF)
  - [version - 版本信息](#version---%E7%89%88%E6%9C%AC%E4%BF%A1%E6%81%AF)
  - [containers, c, container - 管理容器](#containers-c-container---%E7%AE%A1%E7%90%86%E5%AE%B9%E5%99%A8)
- [其他](#%E5%85%B6%E4%BB%96)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Containerd 架构

containerd 是一个工业级标准的容器运行时，它强调**简单性**、**健壮性**和**可移植性**，containerd 可以负责干下面这些事情：

- 管理容器的生命周期（从创建容器到销毁容器）
- 拉取/推送容器镜像
- 存储管理（管理镜像及容器数据的存储）
- 调用 runc 运行容器（与 runc 等容器运行时交互）
- 管理容器网络接口及网络

整体架构图

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/containerd-how.png)

总体来看 containerd 可以分为三个大块：Storage、Metadata 和 Runtime。

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/containerd_how2.png)

容器运行时从docker 切换到containerd ，可以看出少了dockershim 和docker

![](https://github.com/stonebirdjx/static/blob/master/k8s-ladder/docker2containerd.png)

> 总之，万物皆插件，插件就是模块，模块就是插件，比如 `libcontainer`、`runc`、`containerd`、`CRI`、`OCI` 等等
>
> 顺序：Containerd -> Containerdshim -> runc

# ctr 命令 (了解即可)

  ## plugins, plugin - 查看插件信息

```bash
COMMANDS:
ls  # 列出使用的插件

# 例子
ctr plugins ls
```

##  version - 版本信息

```bash
ctr version
```

## containers, c, container - 管理容器

```bash
COMMANDS:
   create                   # 创建容器。options 和docker差不多
   delete, del, remove, rm  # 删除一个或多个存在的容器
   info                     # 获取一个容器的信息
   list, ls                 # 容器列表
   label                    # 设置或删除容器的lable
   checkpoint               checkpoint a container
   restore                  restore a container from checkpoint

```



   content                    manage content
   events, event              display containerd events
   images, image, i           manage images
   leases                     manage leases
   namespaces, namespace, ns  manage namespaces
   pprof                      provide golang pprof outputs for containerd
   run                        run a container
   snapshots, snapshot        manage snapshots
   tasks, t, task             manage tasks
   install                    install a new package
   oci                        OCI tools
   shim                       interact with a shim directly

```bash
```



# 其他

 配置文件：/etc/containerd/config.toml