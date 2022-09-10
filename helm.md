<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [:point_right:三大概念](#point_right%E4%B8%89%E5%A4%A7%E6%A6%82%E5%BF%B5)
- [Helm安装资源顺序：](#helm%E5%AE%89%E8%A3%85%E8%B5%84%E6%BA%90%E9%A1%BA%E5%BA%8F)
- [Helm的一般操作](#helm%E7%9A%84%E4%B8%80%E8%88%AC%E6%93%8D%E4%BD%9C)
- [helm命令行](#helm%E5%91%BD%E4%BB%A4%E8%A1%8C)
  - [helm version - 版本](#helm-version---%E7%89%88%E6%9C%AC)
  - [helm registry - 登录/登出注册表](#helm-registry---%E7%99%BB%E5%BD%95%E7%99%BB%E5%87%BA%E6%B3%A8%E5%86%8C%E8%A1%A8)
    - [:point_right:helm registry login - 登录注册表](#point_righthelm-registry-login---%E7%99%BB%E5%BD%95%E6%B3%A8%E5%86%8C%E8%A1%A8)
    - [:point_right:helm registry logout - 从注册表登出](#point_righthelm-registry-logout---%E4%BB%8E%E6%B3%A8%E5%86%8C%E8%A1%A8%E7%99%BB%E5%87%BA)
  - [:point_right:helm pull - 从仓库下载chart](#point_righthelm-pull---%E4%BB%8E%E4%BB%93%E5%BA%93%E4%B8%8B%E8%BD%BDchart)
  - [:point_right:helm push - 将chart推送到远端](#point_righthelm-push---%E5%B0%86chart%E6%8E%A8%E9%80%81%E5%88%B0%E8%BF%9C%E7%AB%AF)
  - [helm repo - 仓库](#helm-repo---%E4%BB%93%E5%BA%93)
    - [:point_right:helm repo add - 添加chart仓库](#point_righthelm-repo-add---%E6%B7%BB%E5%8A%A0chart%E4%BB%93%E5%BA%93)
    - [:point_right:helm repo index - 基于包含打包chart的目录，生成索引文件](#point_righthelm-repo-index---%E5%9F%BA%E4%BA%8E%E5%8C%85%E5%90%AB%E6%89%93%E5%8C%85chart%E7%9A%84%E7%9B%AE%E5%BD%95%E7%94%9F%E6%88%90%E7%B4%A2%E5%BC%95%E6%96%87%E4%BB%B6)
    - [:point_right:helm repo list - 仓库列表](#point_righthelm-repo-list---%E4%BB%93%E5%BA%93%E5%88%97%E8%A1%A8)
    - [:point_right:helm repo remove - 从chart仓库中更新本地可用chart的信息](#point_righthelm-repo-remove---%E4%BB%8Echart%E4%BB%93%E5%BA%93%E4%B8%AD%E6%9B%B4%E6%96%B0%E6%9C%AC%E5%9C%B0%E5%8F%AF%E7%94%A8chart%E7%9A%84%E4%BF%A1%E6%81%AF)
    - [:point_right:helm repo update - 从chart仓库中更新本地可用chart的信息](#point_righthelm-repo-update---%E4%BB%8Echart%E4%BB%93%E5%BA%93%E4%B8%AD%E6%9B%B4%E6%96%B0%E6%9C%AC%E5%9C%B0%E5%8F%AF%E7%94%A8chart%E7%9A%84%E4%BF%A1%E6%81%AF)
  - [:point_right:helm create - 使用给定名称创建新的chart](#point_righthelm-create---%E4%BD%BF%E7%94%A8%E7%BB%99%E5%AE%9A%E5%90%8D%E7%A7%B0%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84chart)
  - [:point_right:helm install  - chart 安装](#point_righthelm-install----chart-%E5%AE%89%E8%A3%85)
  - [:point_right:helm uninstall - 卸载版本](#point_righthelm-uninstall---%E5%8D%B8%E8%BD%BD%E7%89%88%E6%9C%AC)
  - [:point_right:helm history - 历史版本](#point_righthelm-history---%E5%8E%86%E5%8F%B2%E7%89%88%E6%9C%AC)
  - [:point_right:helm rollback - 版本回退](#point_righthelm-rollback---%E7%89%88%E6%9C%AC%E5%9B%9E%E9%80%80)
  - [helm dependency - 依赖](#helm-dependency---%E4%BE%9D%E8%B5%96)
    - [helm dependency build - 基于Chart.lock文件重新构建charts/目录](#helm-dependency-build---%E5%9F%BA%E4%BA%8Echartlock%E6%96%87%E4%BB%B6%E9%87%8D%E6%96%B0%E6%9E%84%E5%BB%BAcharts%E7%9B%AE%E5%BD%95)
    - [:point_right:helm dependency list - 列出给定chart的依赖](#point_righthelm-dependency-list---%E5%88%97%E5%87%BA%E7%BB%99%E5%AE%9Achart%E7%9A%84%E4%BE%9D%E8%B5%96)
    - [:point_right:helm dependency update - 基于Chart.yaml内容升级charts/](#point_righthelm-dependency-update---%E5%9F%BA%E4%BA%8Echartyaml%E5%86%85%E5%AE%B9%E5%8D%87%E7%BA%A7charts)
  - [:point_right:helm list - 发布版本列表](#point_righthelm-list---%E5%8F%91%E5%B8%83%E7%89%88%E6%9C%AC%E5%88%97%E8%A1%A8)
  - [:point_right:helm package - 打包](#point_righthelm-package---%E6%89%93%E5%8C%85)
  - [:point_right:helm upgrade - 升级版本](#point_righthelm-upgrade---%E5%8D%87%E7%BA%A7%E7%89%88%E6%9C%AC)
  - [:point_right:helm env - 打印环境变量](#point_righthelm-env---%E6%89%93%E5%8D%B0%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [helm show - 显示chart信息](#helm-show---%E6%98%BE%E7%A4%BAchart%E4%BF%A1%E6%81%AF)
    - [:point_right:helm show all - 显示chart的所有信息](#point_righthelm-show-all---%E6%98%BE%E7%A4%BAchart%E7%9A%84%E6%89%80%E6%9C%89%E4%BF%A1%E6%81%AF)
    - [:point_right:helm show chart - 显示chart定义](#point_righthelm-show-chart---%E6%98%BE%E7%A4%BAchart%E5%AE%9A%E4%B9%89)
    - [:point_right:helm show crds - 显示chart的CRD](#point_righthelm-show-crds---%E6%98%BE%E7%A4%BAchart%E7%9A%84crd)
    - [:point_right:helm show readme- 显示chart的README](#point_righthelm-show-readme--%E6%98%BE%E7%A4%BAchart%E7%9A%84readme)
    - [:point_right:helm show values - 显示chart的values](#point_righthelm-show-values---%E6%98%BE%E7%A4%BAchart%E7%9A%84values)
  - [helm plugin - 插件](#helm-plugin---%E6%8F%92%E4%BB%B6)
    - [helm plugin install - 安装Helm插件](#helm-plugin-install---%E5%AE%89%E8%A3%85helm%E6%8F%92%E4%BB%B6)
    - [helm plugin list - 列举已安装的Helm插件](#helm-plugin-list---%E5%88%97%E4%B8%BE%E5%B7%B2%E5%AE%89%E8%A3%85%E7%9A%84helm%E6%8F%92%E4%BB%B6)
    - [helm plugin uninstall  - 卸载Helm插件](#helm-plugin-uninstall----%E5%8D%B8%E8%BD%BDhelm%E6%8F%92%E4%BB%B6)
    - [helm plugin update - 升级Helm插件](#helm-plugin-update---%E5%8D%87%E7%BA%A7helm%E6%8F%92%E4%BB%B6)
  - [helm search - 搜索](#helm-search---%E6%90%9C%E7%B4%A2)
    - [:point_right:helm search hub  - 在Artifact Hub或自己的hub实例中搜索chart](#point_righthelm-search-hub----%E5%9C%A8artifact-hub%E6%88%96%E8%87%AA%E5%B7%B1%E7%9A%84hub%E5%AE%9E%E4%BE%8B%E4%B8%AD%E6%90%9C%E7%B4%A2chart)
    - [:point_right:helm search repo - 用chart中关键字搜索仓库](#point_righthelm-search-repo---%E7%94%A8chart%E4%B8%AD%E5%85%B3%E9%94%AE%E5%AD%97%E6%90%9C%E7%B4%A2%E4%BB%93%E5%BA%93)
  - [:point_right:helm template - 本地模板渲染](#point_righthelm-template---%E6%9C%AC%E5%9C%B0%E6%A8%A1%E6%9D%BF%E6%B8%B2%E6%9F%93)
  - [helm test - 执行发布版本的测试](#helm-test---%E6%89%A7%E8%A1%8C%E5%8F%91%E5%B8%83%E7%89%88%E6%9C%AC%E7%9A%84%E6%B5%8B%E8%AF%95)
  - [:point_right:helm status - 显示已命名发布的状态](#point_righthelm-status---%E6%98%BE%E7%A4%BA%E5%B7%B2%E5%91%BD%E5%90%8D%E5%8F%91%E5%B8%83%E7%9A%84%E7%8A%B6%E6%80%81)
  - [helm get - 获取扩展相关](#helm-get---%E8%8E%B7%E5%8F%96%E6%89%A9%E5%B1%95%E7%9B%B8%E5%85%B3)
    - [helm get all - 获取命名版本的所有信息](#helm-get-all---%E8%8E%B7%E5%8F%96%E5%91%BD%E5%90%8D%E7%89%88%E6%9C%AC%E7%9A%84%E6%89%80%E6%9C%89%E4%BF%A1%E6%81%AF)
    - [helm get hooks - 获取命名版本的所有钩子](#helm-get-hooks---%E8%8E%B7%E5%8F%96%E5%91%BD%E5%90%8D%E7%89%88%E6%9C%AC%E7%9A%84%E6%89%80%E6%9C%89%E9%92%A9%E5%AD%90)
    - [helm get manifest - 获取命名版本的清单](#helm-get-manifest---%E8%8E%B7%E5%8F%96%E5%91%BD%E5%90%8D%E7%89%88%E6%9C%AC%E7%9A%84%E6%B8%85%E5%8D%95)
    - [helm get notes - 获取命名版本的注释](#helm-get-notes---%E8%8E%B7%E5%8F%96%E5%91%BD%E5%90%8D%E7%89%88%E6%9C%AC%E7%9A%84%E6%B3%A8%E9%87%8A)
    - [helm get values - 获取命名版本的values文件](#helm-get-values---%E8%8E%B7%E5%8F%96%E5%91%BD%E5%90%8D%E7%89%88%E6%9C%AC%E7%9A%84values%E6%96%87%E4%BB%B6)
  - [helm completion - 命令自动补全](#helm-completion---%E5%91%BD%E4%BB%A4%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8)
    - [helm completion bash - 为bash生成自动补全脚本](#helm-completion-bash---%E4%B8%BAbash%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8%E8%84%9A%E6%9C%AC)
  - [helm completion fish - 为fish生成自动补全脚本](#helm-completion-fish---%E4%B8%BAfish%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8%E8%84%9A%E6%9C%AC)
    - [helm completion powershell- 为powershell生成自动补全脚本](#helm-completion-powershell--%E4%B8%BApowershell%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8%E8%84%9A%E6%9C%AC)
    - [helm completion zsh - 为zsh生成自动补全脚本](#helm-completion-zsh---%E4%B8%BAzsh%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8%E8%84%9A%E6%9C%AC)
  - [helm lint - 验证chart是否存在问题](#helm-lint---%E9%AA%8C%E8%AF%81chart%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%E9%97%AE%E9%A2%98)
  - [helm verify - 验证给定路径的chart已经被签名且有效](#helm-verify---%E9%AA%8C%E8%AF%81%E7%BB%99%E5%AE%9A%E8%B7%AF%E5%BE%84%E7%9A%84chart%E5%B7%B2%E7%BB%8F%E8%A2%AB%E7%AD%BE%E5%90%8D%E4%B8%94%E6%9C%89%E6%95%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

Helm 帮助您管理 Kubernetes 应用—— Helm Chart，即使是最复杂的 Kubernetes 应用程序，都可以帮助您定义，安装和升级。

**复杂性管理**

即使是最复杂的应用，Helm Chart 依然可以描述， 提供使用单点授权的可重复安装应用程序。

**易于升级**

随时随地升级和自定义的钩子消除您升级的痛苦。

**分发简单**

Helm Chart 很容易在公共或私有化服务器上发版，分发和部署站点。

**回滚**

使用 `helm rollback` 可以轻松回滚到之前的发布版本。

# :point_right:三大概念

`Chart` 代表着 Helm 包。它包含在 Kubernetes 集群内部运行应用程序，工具或服务所需的**所有资源定义**。

`Repository（仓库）` 是用来存放和共享 charts 的地方。

`Release` 是运行在 Kubernetes 集群中的 chart 的实例。一个 chart 通常可以在同一个集群中安装多次。每一次安装都会创建一个新的 *release*。以 MySQL chart为例，如果你想在你的集群中运行两个数据库，你可以安装该chart两次。每一个数据库都会拥有它自己的 *release* 和 *release name*。

# Helm安装资源顺序：

- Namespace
- NetworkPolicy
- ResourceQuota
- LimitRange
- PodSecurityPolicy
- PodDisruptionBudget
- ServiceAccount
- Secret
- SecretList
- ConfigMap
- StorageClass
- PersistentVolume
- PersistentVolumeClaim
- CustomResourceDefinition
- ClusterRole
- ClusterRoleList
- ClusterRoleBinding
- ClusterRoleBindingList
- Role
- RoleList
- RoleBinding
- RoleBindingList
- Service
- DaemonSet
- Pod
- ReplicationController
- ReplicaSet
- Deployment
- HorizontalPodAutoscaler
- StatefulSet
- Job
- CronJob
- Ingress
- APIService

# Helm的一般操作

- helm search:   搜索chart
- helm pull:    下载chart到本地目录查看
- helm install:   上传chart到Kubernetes
- helm list:     列出已发布的chart

# helm命令行

## helm version - 版本

打印客户端版本信息

```bash
helm version [flags]
```

## helm registry - 登录/登出注册表

### :point_right:helm registry login - 登录注册表

用远程注册表进行身份验证。

```bash
helm registry login [host] [flags]
```

### :point_right:helm registry logout - 从注册表登出

从远程注册表移除认证信息。

```b
helm registry logout [host] [flags]
```

## :point_right:helm pull - 从仓库下载chart

（可选）在本地目录解压

```bash
helm pull [chart URL | repo/chartname] [...] [flags]

# 例子
helm pull stable/traefik --untar
```

## :point_right:helm push - 将chart推送到远端

```bash 
helm push [chart] [remote] [flags]

# 例子
helm push seldon-mab harbor-test-helm
helm push seldon-core-operator-1.5.1.tgz harbor-test-helm
```

## helm repo - 仓库

### :point_right:helm repo add - 添加chart仓库

```bash
helm repo add [NAME] [URL] [flags]

# 例子
helm repo add local_chart http://127.0.0.1:8879
```

### :point_right:helm repo index - 基于包含打包chart的目录，生成索引文件

读取当前目录，并根据找到的 chart 生成索引文件。本地chart的优先级高于已有chart。

```bash
helm repo index [DIR] [flags]

# 例子
helm repo index fantastic-charts --url https://fantastic-charts.storage.googleapis.com
helm repo add myrepo https://fantastic-charts.storage.googleapis.com
helm update
```

### :point_right:helm repo list - 仓库列表

```bash
helm repo list [flags]
```

### :point_right:helm repo remove - 从chart仓库中更新本地可用chart的信息

删除一个或多个仓库

```bash
helm repo remove [REPO1 [REPO2 ...]] [flags]

# 例子
helm repo remove local_chart
```

### :point_right:helm repo update - 从chart仓库中更新本地可用chart的信息

从chart仓库中更新本地可用chart的信息

```bash
helm repo update [REPO1 [REPO2 ...]] [flags]

# 例子
helm repo update local_chart
```

## :point_right:helm create - 使用给定名称创建新的chart

该命令创建chart目录和chart用到的公共文件目录

```bash
helm create NAME [flags]
```

项目结构

```b
foo/
├── .helmignore   # Contains patterns to ignore when packaging Helm charts.
├── Chart.yaml    # Information about your chart
├── values.yaml   # The default values for your templates
├── charts/       # Charts that this chart depends on
└── templates/    # The template files
    └── tests/    # The test files
```

## :point_right:helm install  - chart 安装

安装参数必须是chart的引用，一个打包后的chart路径，未打包的chart目录或者是一个URL。

要重写chart中的值，使用'--values'参数传递一个文件或者使用'--set'参数在命令行传递配置，强制使用字符串要用'--set-string'。 当值本身对于命令行太长或者是动态生成的时候，可以使用 '--set-file' 设置独立的值。

```bash
helm install [NAME] [CHART] [flags]

# 例子
helm install -f myvalues.yaml myredis ./redis
helm install --set name=prod myredis ./redis
```

有五种不同的方式来标识需要安装的chart：

1. 通过chart引用： helm install mymaria example/mariadb
2. 通过chart包： helm install mynginx ./nginx-1.2.3.tgz
3. 通过未打包chart目录的路径： helm install mynginx ./nginx
4. 通过URL绝对路径： helm install mynginx https://example.com/charts/nginx-1.2.3.tgz
5. 通过chart引用和仓库url： helm install --repo https://example.com/charts/ mynginx nginx

## :point_right:helm uninstall - 卸载版本

该命令使用版本名称卸载版本

--dry-run  模拟卸载

```bash
helm uninstall RELEASE_NAME [...] [flags]
```

##  :point_right:helm history - 历史版本

检索发布历史

```bash
helm history RELEASE_NAME [flags]

# 例子
helm history angry-bird
```

## :point_right:helm rollback - 版本回退

回滚命令的第一个参数是发布的名称，第二是修订（版本）号，如果省略此参数，会回滚到上一个版本。

```bash
helm rollback <RELEASE> [REVISION] [flags]
```

## helm dependency - 依赖

Helm chart将依赖存储在'charts/'。对于chart开发者，管理依赖比声明了所有依赖的'Chart.yaml'文件更容易。

```yaml
# Chart.yaml
dependencies:
- name: nginx
    version: "1.2.3"
    repository: "https://example.com/charts"
- name: memcached
    version: "3.2.1"
    repository: "https://another.example.com/charts"
```

从2.2.0开始，仓库可以被定义为本地存储的依赖chart的目录路径。路径应该以"file://"前缀开头，比如：

```yaml
# Chart.yaml
dependencies:
- name: nginx
    version: "1.2.3"
    repository: "file://../dependency_chart/nginx"
```

### helm dependency build - 基于Chart.lock文件重新构建charts/目录

从Chart.lock构建输出到charts/目录

该构建是用来将chart的依赖项重建为锁定文件中的指定状态。就像'helm dependency update'一样并不会调整依赖状态。

如果没找到锁定文件，'helm dependency build'映射到'helm dependency update'

```bash
helm dependency build CHART [flags]
```

### :point_right:helm dependency list - 列出给定chart的依赖

列举所有的chart中声明的依赖

```bash
helm dependency list CHART [flags]
```

### :point_right:helm dependency update - 基于Chart.yaml内容升级charts/

更新磁盘上的依赖为Chart.yaml指定内容

```bash
helm dependency update CHART [flags]
```

## :point_right:helm list - 发布版本列表

该命令会列举出指定命名空间的所有发布版本，(如果没有指定命名空间，会使用当前命名空间)。

```bash
helm list [flags]

# 常用options
-a, --all                  show all releases without any filter applied
-A, --all-namespaces       list releases across all namespaces
-d, --date                 sort by release date
```

## :point_right:helm package - 打包

将chart目录打包到chart归档中

该命令将chart打包成一个chart版本包文件。如果给定路径，就会在该路径中查找chart（必须包含Chart.yaml文件）然后将目录打包。

chart版本包会用于Helm包仓库。

```bash
helm package [CHART_PATH] [...] [flags]

# 例子
helm package deis-workflow   # deis-workflow-0.1.0.tgz
helm install ./deis-workflow-0.1.0.tgz # 轻松安装
```

## :point_right:helm upgrade - 升级版本

要在chart中重写value，需要使用'--values'参数并传一个文件或者从命令行使用'--set'参数传个配置， 要强制字符串值，使用'--set-string'。当值本身对于命令行太长或者是动态生成的时候，可以使用 '--set-file' 设置独立的值。

```bash
helm upgrade [RELEASE] [CHART] [flags]

# 例子
helm upgrade --set foo=bar --set foo=newbar redis ./redis
helm upgrade -f myvalues.yaml -f override.yaml redis ./redis
```

## :point_right:helm env - 打印环境变量

Env 打印所有Helm使用的环境信息

```bash
helm env [flags]
```

## helm show - 显示chart信息

###  :point_right:helm show all - 显示chart的所有信息

该命令检查chart(目录、文件或URL)并显示所有的内容（values.yaml, Chart.yaml, README）

```bash
helm show all [CHART] [flags]
```

### :point_right:helm show chart - 显示chart定义

该命令检查chart(目录、文件或URL)并显示Chart.yaml文件的内容

```bash
helm show chart [CHART] [flags]
```

### :point_right:helm show crds - 显示chart的CRD

该命令检查chart(目录、文件或URL)并显示自定义资源（CustomResourceDefinition）文件的内容。

```bash
helm show crds [CHART] [flags]
```

### :point_right:helm show readme- 显示chart的README

该命令检查chart(目录、文件或URL)并显示README文件内容

```bash
helm show readme [CHART] [flags]
```

### :point_right:helm show values - 显示chart的values

该命令检查chart(目录、文件或URL)并显示values.yaml文件的内容

```yaml
helm show values [CHART] [flags]
```

## helm plugin - 插件

### helm plugin install - 安装Helm插件

一个或多个

该命令允许您通过VCS仓库url或者本地路径安装插件。

[官方插件指南](https://helm.sh/zh/docs/topics/plugins/)

```bash
helm plugin install [options] <path|url>... [flags]
# 例子
helm plugin install https://github.com/adamreese/helm-env
```

### helm plugin list - 列举已安装的Helm插件

```bash
helm plugin list [flags]
```

### helm plugin uninstall  - 卸载Helm插件

一个或多个

```bash
helm plugin uninstall <plugin>... [flags]
```

### helm plugin update - 升级Helm插件

一个或多个

```bash
helm plugin update <plugin>... [flags]
```

## helm search - 搜索

### :point_right:helm search hub  - 在Artifact Hub或自己的hub实例中搜索chart

在Artifact Hub或自己的hub实例中搜索Helm charts。

Artifact Hub 是基于web页面的应用，支持CNCF项目的查找、安装和发布包及配置项，包括了公开发布的Helm chart。它是CNCF的沙盒项目。可以访问https://artifacthub.io/

```bash
helm search hub [KEYWORD] [flags]
```

### :point_right:helm search repo - 用chart中关键字搜索仓库

搜索会读取系统上配置的所有仓库，并查找匹配。搜索这些仓库会使用存储在系统中的元数据。

它会展示找到最新稳定版本的chart。如果指定了--devel参数，输出会包括预发布版本。

```bash
helm search repo [keyword] [flags]

# 例子
helm search repo nginx
helm search repo nginx --devel
```

## :point_right:helm template - 本地模板渲染

本地渲染模板并显示输出,这个命令可以在本地渲染出 template 来检查是否正确

通常在集群中查找或检索到的任何值都可以在本地伪造。另外，没有对chart有效性进行服务端测试。

```bash
helm template [NAME] [CHART] [flags]

# 例子
helm template mychart -x templates/deployment.yaml
```

## helm test - 执行发布版本的测试

测试命令执行发布版本的测试

该命令使用的参数是部署版本的名称，要运行的测试在已安装的chart中定义。

```b
helm test [RELEASE] [flags]
```

## :point_right:helm status - 显示已命名发布的状态

```bash
helm status RELEASE_NAME [flags]
```

## helm get - 获取扩展相关

### helm get all - 获取命名版本的所有信息

该命令打印一个具有可读性的信息集合，包括注释，钩子，提供的values，以及给定版本生成的清单文件。

```bash
helm get all RELEASE_NAME [flags]
```

### helm get hooks - 获取命名版本的所有钩子

该命令下载指定版本的钩子

钩子被格式化为YAML格式并以YAML的'---\n'分隔符分隔

```bash
helm get hooks RELEASE_NAME [flags]
```

### helm get manifest - 获取命名版本的清单

该命令用来获取指定版本的清单文件

清单是由该版本的chart生成的Kubernetes资源的YAML编码表示。

```bash
helm get manifest RELEASE_NAME [flags]
```

### helm get notes - 获取命名版本的注释

该命令显示了由指定版本chart提供的注释

```bash
helm get notes RELEASE_NAME [flags]
```

### helm get values - 获取命名版本的values文件

该命令下载给定版本的values文件

```yaml
helm get values RELEASE_NAME [flags]
```

## helm completion - 命令自动补全

### helm completion bash - 为bash生成自动补全脚本

```bash
source <(helm completion bash)
helm completion bash > /etc/bash_completion.d/helm
```

## helm completion fish - 为fish生成自动补全脚本

```bash
helm completion fish | source
```

### helm completion powershell- 为powershell生成自动补全脚本

### helm completion zsh - 为zsh生成自动补全脚本

```bash
source <(helm completion zsh)
helm completion zsh > "${fpath[1]}/_helm"
```

## helm lint - 验证chart是否存在问题

命令使用一个chart路径并运行一系列的测试来验证chart的格式是否正确。

如果遇到引起chart安装失败的情况，会触发[ERROR]信息，如果遇到违反惯例或建议的问题，会触发[WARNING]。

```bash
helm lint PATH [flags]
```

## helm verify - 验证给定路径的chart已经被签名且有效

验证指定的chart有合法的源文件。

源文件提供了加密验证保证chart未被篡改，且由可信提供商打包。

该命令用于验证本地chart，其他一些命令提供'--verify'参数执行同样的验证。要生成一个签名包，使用'helm package --sign'命令。

```bash
helm verify PATH [flags]
```

