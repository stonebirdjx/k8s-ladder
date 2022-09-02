<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Containerd 架构](#containerd-%E6%9E%B6%E6%9E%84)

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