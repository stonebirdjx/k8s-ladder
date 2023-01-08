<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [minikube安装](#minikube%E5%AE%89%E8%A3%85)
  - [设备系统信息](#%E8%AE%BE%E5%A4%87%E7%B3%BB%E7%BB%9F%E4%BF%A1%E6%81%AF)
  - [安装docker与containerd](#%E5%AE%89%E8%A3%85docker%E4%B8%8Econtainerd)
  - [安装nerdctl](#%E5%AE%89%E8%A3%85nerdctl)
  - [安装kubectl](#%E5%AE%89%E8%A3%85kubectl)
  - [安装minikube](#%E5%AE%89%E8%A3%85minikube)
- [启动minikube](#%E5%90%AF%E5%8A%A8minikube)
- [命令补全提示](#%E5%91%BD%E4%BB%A4%E8%A1%A5%E5%85%A8%E6%8F%90%E7%A4%BA)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# minikube安装

## 设备系统信息

采用centos7.9系统：4C8G100G

## 安装docker与containerd

root 用户操作

```bash
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y containerd.io docker-ce

# 系统服务
systemctl enable containerd
systemctl start containerd
systemctl enable docker
systemctl start docker
```

## 安装nerdctl

推荐使用nerdctl （docker命令类似）

```bash
# 下载地址：https://github.com/containerd/nerdctl/releases
tar Cxzvvf /usr/local/bin nerdctl-1.1.0-linux-amd64.tar.gz

# 下载cni插件地址：https://github.com/containernetworking/plugins/releases
mkdir -p /opt/cni/bin/
tar -xvf cni-plugins-linux-amd64-v1.1.1.tgz -C /opt/cni/bin/

# 设置命令自动补齐
source <(nerdctl completion bash)
echo 'source <(nerdctl completion bash)' >>~/.bashrc
```

## 安装kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# 安装
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# 设置命令自动补齐
source <(kubectl completion bash)
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

## 安装minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# 设置命令自动补齐
source <(minikube completion bash)
echo 'source <(minikube completion bash)' >>~/.bashrc
```

# 启动minikube

```bash
minikube start --force --driver=docker --container-runtime=containerd 
```

# 命令补全提示

如果系统没有 bash-completion， 可以通过 `apt-get install bash-completion` 或 `yum install bash-completion` 等命令来安装它。

上述命令将创建文件 `/usr/share/bash-completion/bash_completion`，它是 bash-completion 的主脚本。 依据包管理工具的实际情况，你需要在 `~/.bashrc` 文件中手工导入此文件。