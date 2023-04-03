<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [名词](#%E5%90%8D%E8%AF%8D)
- [:point_right:集群](#point_right%E9%9B%86%E7%BE%A4)
  - [:point_right:环境变量](#point_right%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [:point_right:查看成员member](#point_right%E6%9F%A5%E7%9C%8B%E6%88%90%E5%91%98member)
- [:point_right:访问etcd](#point_right%E8%AE%BF%E9%97%AEetcd)
- [:point_right:通过前缀获取key](#point_right%E9%80%9A%E8%BF%87%E5%89%8D%E7%BC%80%E8%8E%B7%E5%8F%96key)
- [:point_right:删除key](#point_right%E5%88%A0%E9%99%A4key)
- [:point_right:事务](#point_right%E4%BA%8B%E5%8A%A1)
- [:point_right:查看keys](#point_right%E6%9F%A5%E7%9C%8Bkeys)
- [:point_right:租约TTL](#point_right%E7%A7%9F%E7%BA%A6ttl)
- [:point_right:LOCK锁](#point_rightlock%E9%94%81)
- [:point_right:leader选举](#point_rightleader%E9%80%89%E4%B8%BE)
- [:point_right:检查集群状态](#point_right%E6%A3%80%E6%9F%A5%E9%9B%86%E7%BE%A4%E7%8A%B6%E6%80%81)
- [:point_right:快照](#point_right%E5%BF%AB%E7%85%A7)
- [:point_right:添加或者删除成员](#point_right%E6%B7%BB%E5%8A%A0%E6%88%96%E8%80%85%E5%88%A0%E9%99%A4%E6%88%90%E5%91%98)
- [:point_right:权限](#point_right%E6%9D%83%E9%99%90)
- [:point_right:**Metrics**](#point_rightmetrics)
  - [Server](#server)
  - [Disk](#disk)
  - [Network](#network)
- [:point_right:Restful API](#point_rightrestful-api)
  - [常用](#%E5%B8%B8%E7%94%A8)
- [:point_right:etcdctl 访问k8s 数据](#point_rightetcdctl-%E8%AE%BF%E9%97%AEk8s-%E6%95%B0%E6%8D%AE)
- [:point_right:日志](#point_right%E6%97%A5%E5%BF%97)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

ETCD是一个高可用的键值存储系统，被广泛用于服务发现、配置共享、动态配置等场景。它是CoreOS公司开发的一个分布式的、可靠的、高可用的键值存储系统，其内部采用Raft算法来保证数据一致性和高可用性。

ETCD提供了HTTP/JSON、gRPC等接口，使得开发者可以很容易地与其进行交互。

ETCD的特点包括：

1. 分布式：数据可以存储在多个节点上，保证数据的可用性和可靠性；
2. 高可用：支持多主节点，节点之间可以进行选主，保证整个系统的高可用性；
3. 数据一致性：采用Raft算法来保证数据的一致性；
4. 可观察性：可以通过metrics、log来监控etcd的运行状态；
5. 安全性：支持TLS、RBAC等安全机制；
6. 简单易用：提供HTTP/JSON、gRPC等接口，使用起来很方便。

> etcd可以扮演两大角色：
>
> 持久化的键值存储系统，分布式系统数据一致性服务提供者

# 名词

| 术语             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| Node（节点）     | Node（节点） 是 raft 状态机的一个实例。它有唯一标识，如果它是leader，会内部记录其他节点的信息。 |
| Member（成员）   | Member（成员是） etcd 的一个实例。它承载一个 node/节点，并为 client （客户端）提供服务。 |
| Cluster（集群）  | Cluster（集群）由多个　member（成员）组成。每个成员的节点遵循 raft 一致性协议来复制日志。集群从成员中接收提案，提交他们并应用到本地存储。 |
| Peer（同伴）     | Peer（同伴）是同一个集群中的其他成员。                       |
| Proposal（提议） | 提议是一个需要完成　raft　协议的请求（例如写请求，配置修改请求）。 |
| Client（客户端） | Client（客户端）是集群　HTTP API 的调用者。对于 V3 版本，应该包括 gRPC API。 |
| Machine（机器）  | 该术语已弃用。在 2.0 版本之前，在　etcd　中的成员备选。      |
| etcd HTTP Server | 用于处理用户发送的 API 请求以及其它 etcd 节点的同步与心跳信息请求。 |
| etcd Store       | 用于处理 etcd 支持的各类功能的事务，包括数据索引、节点状态变更、监控与反馈、事件处理与执行等等，是 etcd 对用户提供的大多数 API 功能的具体实现。 |
| Raft             | Raft 强一致性算法的具体实现，是 etcd 的核心。                |
| WAL              | Write Ahead Log（预写式日志），是 etcd 的数据存储方式。除了在内存中存有所有数据的状态以及节点的索引以外，etcd 就通过 WAL 进行持久化存储。WAL 中， 所有的数据提交前都会事先记录日志。Snapshot 是为了防止数据过多而进行的状态快照；Entry 表示存储的具体日志内容。 |

# :point_right:集群

```bash
TOKEN=token-01
CLUSTER_STATE=new
NAME_1=machine-1
NAME_2=machine-2
NAME_3=machine-3
HOST_1=10.240.0.17
HOST_2=10.240.0.18
HOST_3=10.240.0.19
CLUSTER=${NAME_1}=http://${HOST_1}:2380,${NAME_2}=http://${HOST_2}:2380,${NAME_3}=http://${HOST_3}:2380

# For machine 1THIS_NAME=${NAME_1}THIS_IP=${HOST_1}
etcd --data-dir=data.etcd --name ${THIS_NAME} \
        --initial-advertise-peer-urls http://${THIS_IP}:2380 --listen-peer-urls http://${THIS_IP}:2380 \
        --advertise-client-urls http://${THIS_IP}:2379 --listen-client-urls http://${THIS_IP}:2379 \
        --initial-cluster ${CLUSTER} \
        --initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}# For machine 2THIS_NAME=${NAME_2}THIS_IP=${HOST_2}
etcd --data-dir=data.etcd --name ${THIS_NAME} \
        --initial-advertise-peer-urls http://${THIS_IP}:2380 --listen-peer-urls http://${THIS_IP}:2380 \
        --advertise-client-urls http://${THIS_IP}:2379 --listen-client-urls http://${THIS_IP}:2379 \
        --initial-cluster ${CLUSTER} \
        --initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}# For machine 3THIS_NAME=${NAME_3}THIS_IP=${HOST_3}
etcd --data-dir=data.etcd --name ${THIS_NAME} \
        --initial-advertise-peer-urls http://${THIS_IP}:2380 --listen-peer-urls http://${THIS_IP}:2380 \
        --advertise-client-urls http://${THIS_IP}:2379 --listen-client-urls http://${THIS_IP}:2379 \
        --initial-cluster ${CLUSTER} \
        --initial-cluster-state ${CLUSTER_STATE} --initial-cluster-token ${TOKEN}
```

> 参数可以使用环境变量代替。

## :point_right:环境变量

```bash
[root@kube-control-1 ~]# cat /etc/etcd.env
# Environment file for etcd v3.4.13
ETCD_STRICT_RECONFIG_CHECK=True
ETCD_DATA_DIR=/var/lib/etcd
ETCD_ADVERTISE_CLIENT_URLS=https://192.168.0.159:2379
ETCD_INITIAL_ADVERTISE_PEER_URLS=https://192.168.0.159:2380
ETCD_INITIAL_CLUSTER_STATE=existing
ETCD_METRICS=basic
ETCD_LISTEN_CLIENT_URLS=https://192.168.0.159:2379,https://127.0.0.1:2379
ETCD_ELECTION_TIMEOUT=5000
ETCD_HEARTBEAT_INTERVAL=250
ETCD_INITIAL_CLUSTER_TOKEN=k8s_etcd
ETCD_LISTEN_PEER_URLS=https://192.168.0.159:2380
ETCD_NAME=etcd1
ETCD_PROXY=off
ETCD_INITIAL_CLUSTER=etcd1=https://192.168.0.159:2380,etcd2=https://192.168.0.191:2380,etcd3=https://192.168.0.33:2380
ETCD_AUTO_COMPACTION_RETENTION=8
ETCD_SNAPSHOT_COUNT=10000
# Flannel need etcd v2 API
ETCD_ENABLE_V2=true

# TLS settings
ETCD_TRUSTED_CA_FILE=/etc/ssl/etcd/ssl/ca.pem
ETCD_CERT_FILE=/etc/ssl/etcd/ssl/member-kube-control-1.pem
ETCD_KEY_FILE=/etc/ssl/etcd/ssl/member-kube-control-1-key.pem
ETCD_CLIENT_CERT_AUTH=true

ETCD_PEER_TRUSTED_CA_FILE=/etc/ssl/etcd/ssl/ca.pem
ETCD_PEER_CERT_FILE=/etc/ssl/etcd/ssl/member-kube-control-1.pem
ETCD_PEER_KEY_FILE=/etc/ssl/etcd/ssl/member-kube-control-1-key.pem
ETCD_PEER_CLIENT_CERT_AUTH=True


ETCD_LOG_OUTPUTS=stdout
ETCD_AUTO_COMPACTION_RETENTION=1
ETCD_MAX_REQUEST_BYTES=10485760
ETCD_QUOTA_BACKEND_BYTES=8589934592


# CLI settings
ETCDCTL_ENDPOINTS=https://127.0.0.1:2379
ETCDCTL_CACERT=/etc/ssl/etcd/ssl/ca.pem
ETCDCTL_KEY=/etc/ssl/etcd/ssl/admin-kube-control-1-key.pem
ETCDCTL_CERT=/etc/ssl/etcd/ssl/admin-kube-control-1.pem
```

## :point_right:查看成员member

```bash
etcdctl --endpoints=$ENDPOINTS member list

# 可以查看是否为leader
29cae73abe8480e0: name=infra3 peerURLs=http://192.168.2.188:2380 clientURLs=http://192.168.2.188:2379 isLeader=false
ddfd1a6f3be67384: name=infra1 peerURLs=http://192.168.2.186:2380 clientURLs=http://192.168.2.186:2379 isLeader=true
fd49f5e3ec73b0d2: name=infra2 peerURLs=http://192.168.2.187:2380 clientURLs=http://192.168.2.187:2379 isLeader=false
```

# :point_right:访问etcd

```Bash
# put
etcdctl --endpoints=$ENDPOINTS put foo "Hello World!"

# get 
etcdctl --endpoints=$ENDPOINTS get foo
etcdctl --endpoints=$ENDPOINTS --write-out="json" get foo
```

# :point_right:通过前缀获取key

```Bash
etcdctl --endpoints=$ENDPOINTS put web1 value1
etcdctl --endpoints=$ENDPOINTS put web2 value2
etcdctl --endpoints=$ENDPOINTS put web3 value3

etcdctl --endpoints=$ENDPOINTS get web --prefix
```

# :point_right:删除key

```Bash
etcdctl --endpoints=$ENDPOINTS put key myvalue
etcdctl --endpoints=$ENDPOINTS del key

etcdctl --endpoints=$ENDPOINTS put k1 value1
etcdctl --endpoints=$ENDPOINTS put k2 value2
etcdctl --endpoints=$ENDPOINTS del k --prefix
```

# :point_right:事务

txn 将多个请求包装到一个事务中

```Bash
etcdctl --endpoints=$ENDPOINTS put user1 bad
etcdctl --endpoints=$ENDPOINTS txn --interactive

compares:
value("user1") = "bad"

success requests (get, put, delete):
del user1

failure requests (get, put, delete):
put user1 good
```

# :point_right:查看keys

```Bash
etcdctl --endpoints=$ENDPOINTS watch stock1
etcdctl --endpoints=$ENDPOINTS put stock1 1000

etcdctl --endpoints=$ENDPOINTS watch stock --prefix
etcdctl --endpoints=$ENDPOINTS put stock1 10
etcdctl --endpoints=$ENDPOINTS put stock2 20
```

# :point_right:租约TTL

在 etcd 中，lease 是一种重要的机制，它提供了一个自动到期的 key-value 对，可以用来实现诸如分布式锁、 TTL (Time To Live) 等功能。lease 的基本使用流程是：创建一个 lease，将 lease 与一个 key 绑定，然后每次 lease 到期时，etcd 会自动将与之绑定的 key 删除。

```Bash
# 创建一个 300 秒的 TTL
etcdctl --endpoints=$ENDPOINTS lease grant 300# lease 2be7547fbc6a5afa granted with TTL(300s)
etcdctl --endpoints=$ENDPOINTS put sample value --lease=2be7547fbc6a5afa
etcdctl --endpoints=$ENDPOINTS get sample

# 撤销一个租约
etcdctl lease revoke <lease_id>

# 检查租约的过期时间：
etcdctl lease timetolive <lease_id>

# 续租一个租约
etcdctl lease keep-alive <lease_id>
```

# :point_right:LOCK锁

etcdctl lock命令用于获取分布式锁，它支持两种模式：独占锁和竞争锁。

独占锁模式是指只有一个客户端能够获取到锁，其他客户端在获取到锁之前会被阻塞。当持有锁的客户端释放锁时，其他客户端才有机会获取到锁。这种模式常用于实现类似于互斥锁的功能。

竞争锁模式是指多个客户端可以同时持有锁，当一个客户端释放锁时，其他客户端会继续竞争锁。这种模式常用于实现类似于读写锁的功能。

```Bash
etcdctl lock <lock-name> <command> [flags]
```

其中，lock-name是锁的名称，command是在获取锁后要执行的命令，flags是一些可选参数。下面是一些常用的参数：

- ttl：锁的过期时间，默认为60秒。
- timeout：获取锁的超时时间，默认为10秒。

```Bash
etcdctl lock my-lock --endpoints=https://my-etcd-server:2379 --cacert=/path/to/ca.crt --cert=/path/to/client.crt --key=/path/to/client.key -- sh -c 'echo "Acquired lock"; sleep 30; echo "Releasing lock"'
```

# :point_right:leader选举

etcd集群如何进行leader选举？

Leader选举是非常重要的机制，可以保证在一个分布式集群中只有一个节点在任期内拥有唯一的领导权。

etcd的leader选举采用Raft协议实现，Raft协议是一种基于共识算法的分布式一致性协议。在Raft协议中，每个节点都可以处于三种状态中的一种，分别是Follower、Candidate和Leader。

- Follower：处于这种状态的节点只能被动地接受来自Leader的请求，并将其转发给其他节点。如果Follower在一定时间内没有接收到Leader的心跳包，则认为Leader已经失效，将自己的状态转换为Candidate。
- Candidate：处于这种状态的节点试图成为新的Leader。Candidate首先会向其他节点发送请求投票的RPC，如果节点收到了超过一半的投票，则成为新的Leader。
- Leader：处于这种状态的节点负责处理所有客户端请求，并向Follower发送心跳包以保持自己的领导地位。

`etcdctl elect` 命令是 etcdctl 工具提供的一种在 etcd 中进行 leader 选举的机制。它主要是用于在分布式系统中选举 leader 或者 master 节点，从而保证系统的可用性和正确性

```Bash
etcdctl elect name value [flags]
```

其中，`name` 代表选举的名称，`value` 代表选举的值，`flags` 包含其他可选参数。常用的参数如下：

- `--ttl`：设置租约的生存时间，默认为 60 秒。
- `--renew`：启用自动续租，以保持租约的有效性。
- `--keep-key`：保留租约 key 在 etcd 中，这样可以使用该 key 进行 Leader 选举，而不需要重新使用名称和值。

例子

```Bash
# terminal 1
etcdctl elect myelection myvalue --ttl 30

# terminal 2
etcdctl elect myelection anothervalue --ttl 30

# 查看leader
etcdctl elect myelection leader
```

# :point_right:检查集群状态

```Bash
#  status
etcdctl --write-out=table --endpoints=$ENDPOINTS endpoint status

# health
etcdctl --endpoints=$ENDPOINTS endpoint health
```

# :point_right:快照

snapshot 保存etcd数据库的时间点快照

```Bash
etcdctl snapshot save <filename>
etcdctl snapshot restore <filename>

etcdctl --endpoints=$ENDPOINTS snapshot save snapshot.db

etcdctl snapshot restore snapshot.db \
--name etcd-new \
--initial-cluster etcd-new=http://new-node:2380 \
--initial-cluster-token etcd-cluster-1 \
--initial-advertise-peer-urls http://new-node:2380 \
--data-dir /var/lib/etcd-new
```

# :point_right:添加或者删除成员

```Bash
# get member IDexport ETCDCTL_API=3HOST_1=10.240.0.13
HOST_2=10.240.0.14
HOST_3=10.240.0.15
etcdctl --endpoints=${HOST_1}:2379,${HOST_2}:2379,${HOST_3}:2379 member list

# remove the memberMEMBER_ID=278c654c9a6dfd3b
etcdctl --endpoints=${HOST_1}:2379,${HOST_2}:2379,${HOST_3}:2379 \
        member remove ${MEMBER_ID}# add a new member (node 4)export ETCDCTL_API=3NAME_1=etcd-node-1
NAME_2=etcd-node-2
NAME_4=etcd-node-4
HOST_1=10.240.0.13
HOST_2=10.240.0.14
HOST_4=10.240.0.16 # new member
etcdctl --endpoints=${HOST_1}:2379,${HOST_2}:2379 \
        member add ${NAME_4} \
        --peer-urls=http://${HOST_4}:2380
```

# :point_right:权限

`auth`,`user`,`role` for 身份验证:

```Bash
export ETCDCTL_API=3
ENDPOINTS=localhost:2379

etcdctl --endpoints=${ENDPOINTS} role add root
etcdctl --endpoints=${ENDPOINTS} role get root

etcdctl --endpoints=${ENDPOINTS} user add root
etcdctl --endpoints=${ENDPOINTS} user grant-role root root
etcdctl --endpoints=${ENDPOINTS} user get root

etcdctl --endpoints=${ENDPOINTS} role add role0
etcdctl --endpoints=${ENDPOINTS} role grant-permission role0 readwrite foo
etcdctl --endpoints=${ENDPOINTS} user add user0
etcdctl --endpoints=${ENDPOINTS} user grant-role user0 role0

etcdctl --endpoints=${ENDPOINTS} auth enable# now all client requests go through auth

etcdctl --endpoints=${ENDPOINTS} --user=user0:123 put foo bar
etcdctl --endpoints=${ENDPOINTS} get foo
# permission denied, user name is empty because the request does not issue an authentication request
etcdctl --endpoints=${ENDPOINTS} --user=user0:123 get foo
# user0 can read the key foo
etcdctl --endpoints=${ENDPOINTS} --user=user0:123 get foo1
```

# :point_right:**Metrics**

http://127.0.0.1:2379/metrics

## Server

prefixed with `etcd_server_`

- has_leader        Whether or not a leader exists. 1 is existence, 0 is not.        Gauge
- leader_changes_seen_total        The number of leader changes seen.        Counter
- proposals_committed_total        The total number of consensus proposals committed.        Gauge
- proposals_applied_total        The total number of consensus proposals applied.        Gauge
- proposals_pending        The current number of pending proposals.        Gauge
- proposals_failed_total        The total number of failed proposals seen.        Counter

## Disk

prefixed with `etcd_disk_`

- wal_fsync_duration_seconds        The latency distributions of fsync called by wal        Histogram
- backend_commit_duration_seconds        The latency distributions of commit called by backend.        Histogram

## Network

prefixed with `etcd_network_`

...

# :point_right:Restful API

Put and get keys

Use the `/v3/kv/range` and `/v3/kv/put` services to read and write keys:

```Bash
curl -L http://localhost:2379/v3/kv/put \
  -X POST -d '{"key": "Zm9v", "value": "YmFy"}'# {"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"2","raft_term":"3"}}

curl -L http://localhost:2379/v3/kv/range \
  -X POST -d '{"key": "Zm9v"}'# {"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"2","raft_term":"3"},"kvs":[{"key":"Zm9v","create_revision":"2","mod_revision":"2","version":"1","value":"YmFy"}],"count":"1"}# get all keys prefixed with "foo"
curl -L http://localhost:2379/v3/kv/range \
  -X POST -d '{"key": "Zm9v", "range_end": "Zm9w"}'# {"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"2","raft_term":"3"},"kvs":[{"key":"Zm9v","create_revision":"2","mod_revision":"2","version":"1","value":"YmFy"}],"count":"1"}
```

Watch keys[ ](https://etcd.io/docs/v3.5/dev-guide/api_grpc_gateway/#watch-keys)

Use the `/v3/watch` service to watch keys:

```Bash
curl -N http://localhost:2379/v3/watch \
  -X POST -d '{"create_request": {"key":"Zm9v"} }' &# {"result":{"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"1","raft_term":"2"},"created":true}}

curl -L http://localhost:2379/v3/kv/put \
  -X POST -d '{"key": "Zm9v", "value": "YmFy"}' >/dev/null 2>&1# {"result":{"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"2","raft_term":"2"},"events":[{"kv":{"key":"Zm9v","create_revision":"2","mod_revision":"2","version":"1","value":"YmFy"}}]}}
```

事务Transactions with `/v3/kv/txn`

```Bash
# target CREATE
curl -L http://localhost:2379/v3/kv/txn \
  -X POST \
  -d '{"compare":[{"target":"CREATE","key":"Zm9v","createRevision":"2"}],"success":[{"requestPut":{"key":"Zm9v","value":"YmFy"}}]}'# {"header":{"cluster_id":"12585971608760269493","member_id":"13847567121247652255","revision":"3","raft_term":"2"},"succeeded":true,"responses":[{"response_put":{"header":{"revision":"3"}}}]}
 
 # target VERSION
curl -L http://localhost:2379/v3/kv/txn \
  -X POST \
  -d '{"compare":[{"version":"4","result":"EQUAL","target":"VERSION","key":"Zm9v"}],"success":[{"requestRange":{"key":"Zm9v"}}]}'# {"header":{"cluster_id":"14841639068965178418","member_id":"10276657743932975437","revision":"6","raft_term":"3"},"succeeded":true,"responses":[{"response_range":{"header":{"revision":"6"},"kvs":[{"key":"Zm9v","create_revision":"2","mod_revision":"6","version":"4","value":"YmF6"}],"count":"1"}}]}
  
```

Authentication 权限

```Bash
# create root user
curl -L http://localhost:2379/v3/auth/user/add \
  -X POST -d '{"name": "root", "password": "pass"}'# {"header":{"cluster_id":"14841639068965178418","member_id":"10276657743932975437","revision":"1","raft_term":"2"}}# create root role
curl -L http://localhost:2379/v3/auth/role/add \
  -X POST -d '{"name": "root"}'# {"header":{"cluster_id":"14841639068965178418","member_id":"10276657743932975437","revision":"1","raft_term":"2"}}# grant root role
curl -L http://localhost:2379/v3/auth/user/grant \
  -X POST -d '{"user": "root", "role": "root"}'# {"header":{"cluster_id":"14841639068965178418","member_id":"10276657743932975437","revision":"1","raft_term":"2"}}# enable auth
curl -L http://localhost:2379/v3/auth/enable -X POST -d '{}'# {"header":{"cluster_id":"14841639068965178418","member_id":"10276657743932975437","revision":"1","raft_term":"2"}}
```

[ ](https://etcd.io/docs/v3.5/dev-guide/api_grpc_gateway/#authentication)

## 常用

```Bash
Cluster API:
/v3alpha/cluster/member/add
/v3alpha/cluster/member/list
/v3alpha/cluster/member/remove
Auth API:
/v3alpha/auth/authenticate
/v3alpha/auth/authenticate/interactive
/v3alpha/auth/role/add
/v3alpha/auth/role/delete
/v3alpha/auth/role/get
/v3alpha/auth/role/grant
/v3alpha/auth/role/list
/v3alpha/auth/role/revoke
/v3alpha/auth/user/add
/v3alpha/auth/user/change-password
/v3alpha/auth/user/delete
/v3alpha/auth/user/get
/v3alpha/auth/user/grant
/v3alpha/auth/user/list
/v3alpha/auth/user/revoke
KV API:
/v3alpha/kv/range
/v3alpha/kv/put
/v3alpha/kv/delete-range
/v3alpha/kv/txn
Lease API:
/v3alpha/lease/grant
/v3alpha/lease/revoke
/v3alpha/lease/keep-alive
/v3alpha/lease/time-to-live
Watch API:
/v3alpha/watch
Maintenance API:
/v3alpha/maintenance/status
/v3alpha/maintenance/defragment
Status API:
/v3alpha/status/leader
/v3alpha/status/self
/v3alpha/status/store
```

# :point_right:etcdctl 访问k8s 数据

```Bash
# cat /etc/etcd.env 

export ETCDCTL_API=3
export ETCDCTL_CACERT=/path/to/client-ca.crt
export ETCDCTL_CERT=/path/to/client.crt
export ETCDCTL_KEY=/path/to/client.key
export ETCDCTL_ENDPOINTS=https://<etcd-service>:2379
```

key 的值是经过 base64 编码，需要解码后才能看到实际值，如

Kuberentes 的所有元数据都保存在 `/registry` 目录下，下一层就是 API 对象类型（复数形式），再下一层是 `namespace`，最后一层是对象的名字。

/registry/{resource}/{namespace}/{resource_name}

```Bash
# deployment
/registry/deployments/default/game-2048
/registry/deployments/kube-system/prometheus-operator

# replicasets
/registry/replicasets/default/game-2048-c7d589ccf

# pod
/registry/pods/default/game-2048-c7d589ccf-8lsbw

# statefulsets
/registry/statefulsets/kube-system/prometheus-k8s

# daemonsets
/registry/daemonsets/kube-system/kube-proxy

# secrets
/registry/secrets/default/default-token-tbfmb

# serviceaccounts
/registry/serviceaccounts/default/default

# service
/registry/services/specs/default/game-2048

# endpoints
/registry/services/endpoints/default/game-2048
```

资源详情

```Bash
ThirdPartyResourceData
apiextensions.k8s.io
apiregistration.k8s.io
certificatesigningrequests
clusterrolebindings
clusterroles
configmaps
controllerrevisions
controllers
daemonsets
deployments
events
horizontalpodautoscalers
ingress
limitranges
minions
monitoring.coreos.com
namespaces
persistentvolumeclaims
persistentvolumes
poddisruptionbudgets
pods
ranges
replicasets
resourcequotas
rolebindings
roles
secrets
serviceaccounts
services
statefulsets
storageclasses
thirdpartyresources
```

# :point_right:日志

```Bash
# /var/log/message
journalctl -u etcd
```