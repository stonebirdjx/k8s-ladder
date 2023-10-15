<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
- [配置文件](#%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
- [执行协议](#%E6%89%A7%E8%A1%8C%E5%8D%8F%E8%AE%AE)
- [参数](#%E5%8F%82%E6%95%B0)
- [错误码](#%E9%94%99%E8%AF%AF%E7%A0%81)
- [cni的四种操作](#cni%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%93%8D%E4%BD%9C)
  - [添加](#%E6%B7%BB%E5%8A%A0)
  - [删除](#%E5%88%A0%E9%99%A4)
  - [check](#check)
  - [VERSION](#version)
- [执行网络配置](#%E6%89%A7%E8%A1%8C%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE)
  - [生命周期](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [附件参数](#%E9%99%84%E4%BB%B6%E5%8F%82%E6%95%B0)
- [Examples](#examples)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Overview

适用于 Linux 上的应用程序容器的基于插件的通用网络解决方案、容器网络接口或 CNI。

- 容器是一个网络隔离域，虽然规范中并没有定义实际的隔离技术。 例如，这可以是网络命名空间或虚拟机。
- 网络是指一组可唯一寻址、可以相互通信的端点。 这可以是单个容器（如上所述）、机器或某些其他网络设备（例如路由器）。 从概念上讲，容器可以添加到一个或多个网络或从一个或多个网络中删除。
- Runtime是负责执行CNI插件的程序。
- Plugin 是应用指定网络配置的程序。

CNI 规范定义：

- 管理员定义网络配置的格式。
- 容器运行时向网络插件发出请求的协议。
- 根据提供的配置执行插件的过程。
- 插件将功能委托给其他插件的过程。
- 插件将其结果返回到运行时的数据类型。

# 配置文件

配置文件需要放到每个节点的 /etc/cni/net.d/ 目录

```bash
{
  "cniVersion": "1.0.0",
  "name": "dbnet",
  "plugins": [
    {
      "type": "bridge",
      // plugin specific parameters
      "bridge": "cni0",
      "keyA": ["some more", "plugin specific", "configuration"],
      
      "ipam": {
        "type": "host-local",
        // ipam specific
        "subnet": "10.1.0.0/16",
        "gateway": "10.1.0.1",
        "routes": [
            {"dst": "0.0.0.0/0"}
        ]
      },
      "dns": {
        "nameservers": [ "10.1.0.1" ]
      }
    },
    {
      "type": "tuning",
      "capabilities": {
        "mac": true
      },
      "sysctl": {
        "net.core.somaxconn": "500"
      }
    },
    {
        "type": "portmap",
        "capabilities": {"portMappings": true}
    }
  ]
}
```

配置详情：https://www.cni.dev/docs/spec/#section-1-network-configuration-format

# 执行协议

CNI 协议基于容器运行时**调用的二进制文件的执行**。 CNI 定义了插件二进制文件和运行时之间的协议。

CNI 插件负责以某种方式配置容器的网络接口。 插件分为两大类：

- “Interface”插件，它在容器内创建一个网络接口并确保其具有连接性。

- “Chained”插件，调整已创建接口的配置（但可能需要创建更多接口才能实现）。

运行时通过环境变量和配置将参数传递给插件。 它通过标准输入提供配置。 如果成功，该插件将在 stdout 上返回结果；如果操作失败，则在 stderr 上返回错误。 配置和结果以 JSON 进行编码。

# 参数

- CNI_COMMAND：表示需要的操作； 添加、删除、检查或版本。
- CNI_CONTAINERID：容器ID。 容器的唯一纯文本标识符，由运行时分配。 不得为空。 必须以字母数字字符开头，可选后跟一个或多个字母数字字符、下划线 (_)、点 (.) 或连字符 (-) 的任意组合。
- CNI_NETNS：对容器“隔离域”的引用。 如果使用网络命名空间，则为网络命名空间的路径（例如 /run/netns/[nsname]）
- CNI_IFNAME：在容器内创建的接口名称； 如果插件无法使用此接口名称，则它必须返回错误。
- CNI_ARGS：用户在调用时传入的额外参数。 以分号分隔的字母数字键值对； 例如，“FOO=BAR;ABC=123”
- CNI_PATH：搜索 CNI 插件可执行文件的路径列表。 路径由操作系统特定的列表分隔符分隔； 例如，Linux 上为“:”，Windows 上为“;”

# 错误码

成功时插件必须以返回码 0 退出，失败时返回码非零。 如果插件遇到错误，它应该输出一个“错误”结果结构

```bash
{
  "cniVersion": "1.0.0",
  "code": 7,
  "msg": "Invalid Configuration",
  "details": "Network 192.168.0.0/31 too small to allocate from."
}
```

错误码：https://www.cni.dev/docs/spec/#error

# cni的四种操作

CNI 定义了 4 种操作：ADD、DEL、CHECK 和 VERSION。 这些通过 CNI_COMMAND 环境变量传递给插件。

## 添加

`ADD`: Add container to network, or apply modifications 

CNI 插件在收到 ADD 命令后，应该

- 在 CNI_NETNS 容器内创建由 CNI_IFNAME 定义的接口，或者
- 调整 CNI_NETNS 容器内 CNI_IFNAME 定义的接口配置。

运行时将在标准输入上提供 JSON 序列化的插件配置对象（定义如下）。

Required environment parameters:

- `CNI_COMMAND`
- `CNI_CONTAINERID`
- `CNI_NETNS`
- `CNI_IFNAME`

Optional environment parameters:

- `CNI_ARGS`
- `CNI_PATH`

成功返回的消息体

https://www.cni.dev/docs/spec/#success

```bash
{
    "cniVersion":"1.0.0",
    "interfaces":{
        "name":"",
        "mac":"",
        "sandbox":""
    },
    "ips":{
        "address":"",
        "gateway":"",
        "interface":""
    },
    "routes":{
        "dst":"",
        "gw":""
    },
    "dns":{
        "nameservers":"",
        "domain":"",
        "search":"",
        "options":""
    }
}
```

## 删除

CNI 插件在收到 DEL 命令后，应该

- 删除 CNI_NETNS 容器内由 CNI_IFNAME 定义的接口，或者
- 撤消插件 ADD 功能中应用的任何修改

**删除操作必须成功**：即使某些资源丢失，插件通常也应该完成 DEL 操作而不会出现错误。IPAM 插件通常也应该释放 IP 分配并返回成功，除非该网络命名空间对于 IPAM 管理至关重要。

所需环境参数：

Required environment parameters:

- `CNI_COMMAND`
- `CNI_CONTAINERID`
- `CNI_IFNAME`

Optional environment parameters:

- `CNI_NETNS`
- `CNI_ARGS`
- `CNI_PATH`

## check

参考add、del

## VERSION

参考add、del

# 执行网络配置

## 生命周期

- 在调用任何插件之前，容器运行时必须为容器创建一个新的网络命名空间。
- 容器运行时不得对同一容器调用并行操作，但允许对不同容器调用并行操作。 这包括跨多个附件。
- 插件必须处理跨不同容器并发执行的情况。 如有必要，他们必须对共享资源（例如 IPAM 数据库）实施锁定。
- 容器运行时必须确保添加最终后面跟着相应的删除。 唯一的例外是发生灾难性故障，例如节点丢失。 即使添加失败，仍必须执行删除。
- **删除之后可能会进行其他删除。**
- **在添加和删除之间网络配置不应更改。**
- 附件之间的网络配置不应更改。

## 附件参数

虽然网络配置不应在附件之间更改，但容器运行时会为每个附件提供某些参数。

- **Container ID**：容器的唯一明文标识符，由运行时分配。 不得为空。 必须以字母数字字符开头，可选后跟一个或多个字母数字字符、下划线 (_)、点 (.) 或连字符 (-) 的任意组合。 在执行期间，始终设置为 CNI_CONTAINERID 参数。
- **Namespace**：对容器“隔离域”的引用。 如果使用网络命名空间，则为网络命名空间的路径（例如 /run/netns/[nsname]）。 在执行期间，始终设置为 CNI_NETNS 参数。
- **Container interface name**: 在容器内创建的接口的名称。 在执行期间，始终设置为 CNI_IFNAME 参数。
- **Generic Arguments**：与特定附件相关的额外参数，采用键值字符串对的形式。 在执行期间，始终设置为 CNI_ARGS 参数。
- **Capability Arguments**: 这些也是键值对。 键是字符串，而值是任何 JSON 可序列化类型。 键和值是按约定定义的。

# Examples 

**add**

```json
{
    "cniVersion": "1.0.0",
    "name": "dbnet",
    "type": "bridge",
    "bridge": "cni0",
    "keyA": ["some more", "plugin specific", "configuration"],
    "ipam": {
        "type": "host-local",
        "subnet": "10.1.0.0/16",
        "gateway": "10.1.0.1"
    },
    "dns": {
        "nameservers": [ "10.1.0.1" ]
    }
}
```

