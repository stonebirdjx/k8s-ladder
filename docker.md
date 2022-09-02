<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [docker 常用命令](#docker-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
  - [docker 信息](#docker-%E4%BF%A1%E6%81%AF)
    - [docker info/version - 查看docker相关信息](#docker-infoversion---%E6%9F%A5%E7%9C%8Bdocker%E7%9B%B8%E5%85%B3%E4%BF%A1%E6%81%AF)
  - [镜像仓库有关](#%E9%95%9C%E5%83%8F%E4%BB%93%E5%BA%93%E6%9C%89%E5%85%B3)
    - [:point_right:docker login/logout - 登录登出镜像仓](#point_rightdocker-loginlogout---%E7%99%BB%E5%BD%95%E7%99%BB%E5%87%BA%E9%95%9C%E5%83%8F%E4%BB%93)
    - [:point_right:docker pull - 拉取镜像](#point_rightdocker-pull---%E6%8B%89%E5%8F%96%E9%95%9C%E5%83%8F)
    - [:point_right:docker push - 推送镜像](#point_rightdocker-push---%E6%8E%A8%E9%80%81%E9%95%9C%E5%83%8F)
    - [docker search - 查找镜像](#docker-search---%E6%9F%A5%E6%89%BE%E9%95%9C%E5%83%8F)
  - [镜像管理](#%E9%95%9C%E5%83%8F%E7%AE%A1%E7%90%86)
    - [:point_right:docker images - 列出本地镜像。](#point_rightdocker-images---%E5%88%97%E5%87%BA%E6%9C%AC%E5%9C%B0%E9%95%9C%E5%83%8F)
    - [:point_right:docker rmi - 删除镜像。](#point_rightdocker-rmi---%E5%88%A0%E9%99%A4%E9%95%9C%E5%83%8F)
    - [:point_right:docker tag - 给镜像重新命名:tag](#point_rightdocker-tag---%E7%BB%99%E9%95%9C%E5%83%8F%E9%87%8D%E6%96%B0%E5%91%BD%E5%90%8Dtag)
    - [:point_right:docker build - 使用 Dockerfile 创建镜像](#point_rightdocker-build---%E4%BD%BF%E7%94%A8-dockerfile-%E5%88%9B%E5%BB%BA%E9%95%9C%E5%83%8F)
    - [docker history - 查看镜像创建历史](#docker-history---%E6%9F%A5%E7%9C%8B%E9%95%9C%E5%83%8F%E5%88%9B%E5%BB%BA%E5%8E%86%E5%8F%B2)
    - [:point_right:docker save - 将镜像保存成 tar文件](#point_rightdocker-save---%E5%B0%86%E9%95%9C%E5%83%8F%E4%BF%9D%E5%AD%98%E6%88%90-tar%E6%96%87%E4%BB%B6)
    - [:point_right:docker load  - 将文件加载成镜像](#point_rightdocker-load----%E5%B0%86%E6%96%87%E4%BB%B6%E5%8A%A0%E8%BD%BD%E6%88%90%E9%95%9C%E5%83%8F)
    - [docker import - 从归档文件中创建镜像](#docker-import---%E4%BB%8E%E5%BD%92%E6%A1%A3%E6%96%87%E4%BB%B6%E4%B8%AD%E5%88%9B%E5%BB%BA%E9%95%9C%E5%83%8F)
  - [容器管理](#%E5%AE%B9%E5%99%A8%E7%AE%A1%E7%90%86)
    - [:point_right:docker run - 创建一个新的容器](#point_rightdocker-run---%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E5%AE%B9%E5%99%A8)
    - [docker start/stop/restart - 启动/停止/重启容器](#docker-startstoprestart---%E5%90%AF%E5%8A%A8%E5%81%9C%E6%AD%A2%E9%87%8D%E5%90%AF%E5%AE%B9%E5%99%A8)
    - [docker kill - 杀掉一个运行中的容器。](#docker-kill---%E6%9D%80%E6%8E%89%E4%B8%80%E4%B8%AA%E8%BF%90%E8%A1%8C%E4%B8%AD%E7%9A%84%E5%AE%B9%E5%99%A8)
    - [:point_right:docker rm - 删除一个容器](#point_rightdocker-rm---%E5%88%A0%E9%99%A4%E4%B8%80%E4%B8%AA%E5%AE%B9%E5%99%A8)
    - [:point_right:docker pause/unpause - 暂停/恢复容器中的所有进程](#point_rightdocker-pauseunpause---%E6%9A%82%E5%81%9C%E6%81%A2%E5%A4%8D%E5%AE%B9%E5%99%A8%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E8%BF%9B%E7%A8%8B)
    - [docker create - 创建一个新的容器但不启动它](#docker-create---%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E5%AE%B9%E5%99%A8%E4%BD%86%E4%B8%8D%E5%90%AF%E5%8A%A8%E5%AE%83)
    - [:point_right:docker exec - 在运行的容器中执行命令](#point_rightdocker-exec---%E5%9C%A8%E8%BF%90%E8%A1%8C%E7%9A%84%E5%AE%B9%E5%99%A8%E4%B8%AD%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
  - [对容器操作一下操作](#%E5%AF%B9%E5%AE%B9%E5%99%A8%E6%93%8D%E4%BD%9C%E4%B8%80%E4%B8%8B%E6%93%8D%E4%BD%9C)
    - [:point_right:docker ps - 列出运行的容器](#point_rightdocker-ps---%E5%88%97%E5%87%BA%E8%BF%90%E8%A1%8C%E7%9A%84%E5%AE%B9%E5%99%A8)
    - [:point_right:docker inspect - 获取容器/镜像的元数据](#point_rightdocker-inspect---%E8%8E%B7%E5%8F%96%E5%AE%B9%E5%99%A8%E9%95%9C%E5%83%8F%E7%9A%84%E5%85%83%E6%95%B0%E6%8D%AE)
    - [:point_right:docker top - 查看容器中运行的进程信息](#point_rightdocker-top---%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E4%B8%AD%E8%BF%90%E8%A1%8C%E7%9A%84%E8%BF%9B%E7%A8%8B%E4%BF%A1%E6%81%AF)
    - [docker attach - 连接到正在运行中的容器](#docker-attach---%E8%BF%9E%E6%8E%A5%E5%88%B0%E6%AD%A3%E5%9C%A8%E8%BF%90%E8%A1%8C%E4%B8%AD%E7%9A%84%E5%AE%B9%E5%99%A8)
    - [:point_right:docker events - 从服务器获取实时事件](#point_rightdocker-events---%E4%BB%8E%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%8E%B7%E5%8F%96%E5%AE%9E%E6%97%B6%E4%BA%8B%E4%BB%B6)
    - [:point_right:docker logs - 获取容器的日志](#point_rightdocker-logs---%E8%8E%B7%E5%8F%96%E5%AE%B9%E5%99%A8%E7%9A%84%E6%97%A5%E5%BF%97)
    - [docker wait - 阻塞容器直到退出](#docker-wait---%E9%98%BB%E5%A1%9E%E5%AE%B9%E5%99%A8%E7%9B%B4%E5%88%B0%E9%80%80%E5%87%BA)
    - [docker export - 将容器的文件系统导出为 tar 文件](#docker-export---%E5%B0%86%E5%AE%B9%E5%99%A8%E7%9A%84%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E5%AF%BC%E5%87%BA%E4%B8%BA-tar-%E6%96%87%E4%BB%B6)
    - [:point_right:docker port - 查看容器的端口映射情况](#point_rightdocker-port---%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E7%9A%84%E7%AB%AF%E5%8F%A3%E6%98%A0%E5%B0%84%E6%83%85%E5%86%B5)
    - [:point_right:docker stats - 显示容器资源的使用情况](#point_rightdocker-stats---%E6%98%BE%E7%A4%BA%E5%AE%B9%E5%99%A8%E8%B5%84%E6%BA%90%E7%9A%84%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)
    - [:point_right:docker commit - 从容器创建一个新的镜像。](#point_rightdocker-commit---%E4%BB%8E%E5%AE%B9%E5%99%A8%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84%E9%95%9C%E5%83%8F)
    - [:point_right:docker diff - 对比容器与创建时文件变化](#point_rightdocker-diff---%E5%AF%B9%E6%AF%94%E5%AE%B9%E5%99%A8%E4%B8%8E%E5%88%9B%E5%BB%BA%E6%97%B6%E6%96%87%E4%BB%B6%E5%8F%98%E5%8C%96)
    - [:point_right:docker cp - 用于容器与主机之间的数据拷贝](#point_rightdocker-cp---%E7%94%A8%E4%BA%8E%E5%AE%B9%E5%99%A8%E4%B8%8E%E4%B8%BB%E6%9C%BA%E4%B9%8B%E9%97%B4%E7%9A%84%E6%95%B0%E6%8D%AE%E6%8B%B7%E8%B4%9D)
  - [数据卷](#%E6%95%B0%E6%8D%AE%E5%8D%B7)
    - [:point_right:docker volume - 数据卷](#point_rightdocker-volume---%E6%95%B0%E6%8D%AE%E5%8D%B7)
  - [容器网络](#%E5%AE%B9%E5%99%A8%E7%BD%91%E7%BB%9C)
    - [:point_right:docker network - 网络](#point_rightdocker-network---%E7%BD%91%E7%BB%9C)
- [dockerfile](#dockerfile)
  - [:point_right:FROM - 构建镜像基于哪个镜像](#point_rightfrom---%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F%E5%9F%BA%E4%BA%8E%E5%93%AA%E4%B8%AA%E9%95%9C%E5%83%8F)
  - [:point_right:MAINTAINER - 镜像维护者姓名或邮箱地址](#point_rightmaintainer---%E9%95%9C%E5%83%8F%E7%BB%B4%E6%8A%A4%E8%80%85%E5%A7%93%E5%90%8D%E6%88%96%E9%82%AE%E7%AE%B1%E5%9C%B0%E5%9D%80)
  - [:point_right:RUN - 构建镜像时运行的指令](#point_rightrun---%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F%E6%97%B6%E8%BF%90%E8%A1%8C%E7%9A%84%E6%8C%87%E4%BB%A4)
  - [:point_right:CMD - 运行容器时执行的shell环境](#point_rightcmd---%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8%E6%97%B6%E6%89%A7%E8%A1%8C%E7%9A%84shell%E7%8E%AF%E5%A2%83)
  - [:point_right:VOLUME - 指定容器挂载点到宿主机自动生成的目录或其他容器](#point_rightvolume---%E6%8C%87%E5%AE%9A%E5%AE%B9%E5%99%A8%E6%8C%82%E8%BD%BD%E7%82%B9%E5%88%B0%E5%AE%BF%E4%B8%BB%E6%9C%BA%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E7%9A%84%E7%9B%AE%E5%BD%95%E6%88%96%E5%85%B6%E4%BB%96%E5%AE%B9%E5%99%A8)
  - [:point_right:USER - 切换用户](#point_rightuser---%E5%88%87%E6%8D%A2%E7%94%A8%E6%88%B7)
  - [:point_right:WORKDIR - 切换目录](#point_rightworkdir---%E5%88%87%E6%8D%A2%E7%9B%AE%E5%BD%95)
  - [:point_right:HEALTHCHECH - 健康检查](#point_righthealthchech---%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
  - [:point_right:ARG - 构建时指定的一些参数](#point_rightarg---%E6%9E%84%E5%BB%BA%E6%97%B6%E6%8C%87%E5%AE%9A%E7%9A%84%E4%B8%80%E4%BA%9B%E5%8F%82%E6%95%B0)
  - [:point_right:EXPOSE - 声明容器的服务端口](#point_rightexpose---%E5%A3%B0%E6%98%8E%E5%AE%B9%E5%99%A8%E7%9A%84%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%8F%A3)
  - [:point_right:ENV - 设置容器环境变量](#point_rightenv---%E8%AE%BE%E7%BD%AE%E5%AE%B9%E5%99%A8%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [:point_right:ADD - 拷贝文件或目录到容器中（支持下载或解压）](#point_rightadd---%E6%8B%B7%E8%B4%9D%E6%96%87%E4%BB%B6%E6%88%96%E7%9B%AE%E5%BD%95%E5%88%B0%E5%AE%B9%E5%99%A8%E4%B8%AD%E6%94%AF%E6%8C%81%E4%B8%8B%E8%BD%BD%E6%88%96%E8%A7%A3%E5%8E%8B)
  - [:point_right:COPY - 拷贝文件或目录到容器中（不支持下载或解压）](#point_rightcopy---%E6%8B%B7%E8%B4%9D%E6%96%87%E4%BB%B6%E6%88%96%E7%9B%AE%E5%BD%95%E5%88%B0%E5%AE%B9%E5%99%A8%E4%B8%AD%E4%B8%8D%E6%94%AF%E6%8C%81%E4%B8%8B%E8%BD%BD%E6%88%96%E8%A7%A3%E5%8E%8B)
  - [:point_right:ENTRYPOINT - 运行容器时执行的shell命令](#point_rightentrypoint---%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8%E6%97%B6%E6%89%A7%E8%A1%8C%E7%9A%84shell%E5%91%BD%E4%BB%A4)
- [docker-compose](#docker-compose)
  - [services配置参考](#services%E9%85%8D%E7%BD%AE%E5%8F%82%E8%80%83)
  - [常用命令](#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# docker 常用命令

## docker 信息

### docker info/version - 查看docker相关信息

```bash
docker info
docker version
```

## 镜像仓库有关

### :point_right:docker login/logout - 登录登出镜像仓

```bash
#  如果未指定镜像仓库地址，默认为官方仓库 Docker Hub
docker login [OPTIONS] [SERVER]

# 常用options
-u 用户名
-p 密码

# 例子
docker login -u 用户名 [-p 密码] habor.com
docker logout
```

### :point_right:docker pull - 拉取镜像

```bash
# 未指定tag 默认latest
docker pull [OPTIONS] NAME[:TAG]

# 常用options
--disable-content-trust :忽略镜像的校验,默认开启
```

### :point_right:docker push - 推送镜像

```bash
docker push [OPTIONS] NAME[:TAG]

# 常用options
--disable-content-trust :忽略镜像的校验,默认开启
```

### docker search - 查找镜像

```bash
docker search nginx
```

## 镜像管理

### :point_right:docker images - 列出本地镜像。

```bash
# 常用options
-a 列出本地所有的镜像

docker images -a
```

### :point_right:docker rmi - 删除镜像。

```bash
# 可以是镜像id，可以是name:tag
docker rmi [OPTIONS] IMAGE [IMAGE...]

# 常用options
-f 强制删除

# 例子
docker rmi -f runoob/ubuntu:v4
docker rmi 1c06aa18edee
```

### :point_right:docker tag - 给镜像重新命名:tag

```bash
# old 可以是镜像id，可以是name:tag
docker tag [OPTIONS] old_IMAGE[:TAG] new_NAME[:TAG]
```

### :point_right:docker build - 使用 Dockerfile 创建镜像

```bash
# 常用options
--build-arg=[] :设置镜像创建时的变量；
--cpu-shares :设置 cpu 使用权重；
--cpu-period :限制 CPU CFS周期；
--cpu-quota :限制 CPU CFS配额；
--cpuset-cpus :指定使用的CPU id；
--cpuset-mems :指定使用的内存 id；
--disable-content-trust :忽略校验，默认开启；
-f :指定要使用的Dockerfile路径；
--force-rm :设置镜像过程中删除中间容器；
--isolation :使用容器隔离技术；
--label=[] :设置镜像使用的元数据；
-m :设置内存最大值；
--memory-swap :设置Swap的最大值为内存+swap，"-1"表示不限swap；
--no-cache :创建镜像的过程不使用缓存；
--pull :尝试去更新镜像的新版本；
--quiet, -q :安静模式，成功后只输出镜像 ID；
--rm :设置镜像成功后删除中间容器；
--shm-size :设置/dev/shm的大小，默认值是64M；
--ulimit :Ulimit配置。
--squash :将 Dockerfile 中所有的操作压缩为一层。
--tag, -t: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
--network: 默认 default。在构建期间设置RUN指令的网络模式

# 例子
docker build -t runoob/ubuntu:v1 -f /path/Dockerfil. 
```

### docker history - 查看镜像创建历史

```
docker history nginx:v1
```

### :point_right:docker save - 将镜像保存成 tar文件

```bash
# 常用options
-o :输出到的文件

# 例子
docker save -o nginx_v1.tar nginx:v1
```


### :point_right:docker load  - 将文件加载成镜像

```bash
## 常用options
--input , -i : 指定导入的文件，代替 STDIN。
--quiet , -q : 精简输出信息。

# 例子
docker load -i nginx_v1.tar
docker load < nginx_v1.tar  # 可以读取标准输入
```

### docker import - 从归档文件中创建镜像

```bash
# 从镜像归档文件 nginx_v1.tar 创建镜像，命名为stonebird/nginx:v1
docker import  nginx_v1.tar stonebird/nginx:v1 
```

## 容器管理

### :point_right:docker run - 创建一个新的容器

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

# 常用options
-a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
-d 后台运行容器，并返回容器ID；
-i 以交互模式运行容器，通常与 -t 同时使用
-P: 随机端口映射，容器内部端口随机映射到主机的端口
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-e, --env=[]        指定环境变量，容器中可以使用该环境变量    
-m, --memory=""     指定容器的内存上限    
--name="nginx-lb"   为容器指定一个名称；
--dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
--cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
-m :设置容器使用内存最大值；
--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
--volume , -v: 绑定一个卷
--restart string 失败后是否重启

## 例子
docker run -d \
    --restart always \
    -p 80:80 \
    -p 443:443 \
    -p 6620:6620 \
    --name nginx \
    --network stonebird-net \
    -v ${NGINX_DIR}/html:/usr/share/nginx/html \
    -v ${NGINX_DIR}/conf/nginx.conf:/etc/nginx/nginx.conf \
    -v ${NGINX_DIR}/conf.d:/etc/nginx/conf.d \
    -v ${NGINX_DIR}/logs:/var/log/nginx \
    -v ${NGINX_DIR}/hjxstbserver.xyz_bundle.crt:/etc/nginx/hjxstbserver.xyz_bundle.crt \
    -v ${NGINX_DIR}/hjxstbserver.xyz.key:/etc/nginx/hjxstbserver.xyz.key \
    -v ${NGINX_DIR}/download:/etc/nginx/download \
    nginx:latest
```

### docker start/stop/restart - 启动/停止/重启容器

```bash
# 可以是名称或者id
docker start|stop|restart   [OPTIONS] CONTAINER [CONTAINER...]
```

### docker kill - 杀掉一个运行中的容器。

```bash
-s :向容器发送一个信号

# 例子
docker kill nginx
docker kill -s KILL mongo
```

### :point_right:docker rm - 删除一个容器

```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]

## 常用options
-f :通过 SIGKILL 信号强制删除一个运行中的容器。
-l :移除容器间的网络连接，而非容器本身。
-v :删除与容器关联的卷。

## 例子
docker rm -f nginx01
```

### :point_right:docker pause/unpause - 暂停/恢复容器中的所有进程

```bash
docker pause|unpause nginx
```

### docker create - 创建一个新的容器但不启动它

用法和docker run 一致

### :point_right:docker exec - 在运行的容器中执行命令

```bash
## 常用OPTIONS
-i 即使没有附加也保持STDIN 打开
-t 分配一个伪终端

## 例子
docker exec nginx_c nginx -T
docker exec -ti nginx_c /bin/bash
```

## 对容器操作一下操作

### :point_right:docker ps - 列出运行的容器

```bash
## 常用options
-a :显示所有的容器，包括未运行的。
-n :列出最近创建的n个容器。

CONTAINER ID: 容器 ID。
IMAGE: 使用的镜像。
COMMAND: 启动容器时运行的命令。
CREATED: 容器的创建时间。
STATUS: 容器状态。状态有7种：
created（已创建）
    restarting（重启中）
    running（运行中）
    removing（迁移中）
    paused（暂停）
    exited（停止）
    dead（死亡）
PORTS: 容器的端口信息和使用的连接类型（tcp\udp）。
NAMES: 自动分配的容器名称。
```

### :point_right:docker inspect - 获取容器/镜像的元数据

```bash
docker inspect nginx:v1
```

### :point_right:docker top - 查看容器中运行的进程信息

和容器里ps命令一致

```bash
docker top nginx
```

### docker attach - 连接到正在运行中的容器

进入容器正在执行的终端，不常用了解即可

```bash
docker attach topdemo
docker attach --sig-proxy=false mynginx
```

### :point_right:docker events - 从服务器获取实时事件

```bash
## 常用options
-f ：根据条件过滤事件；
--since ：从指定的时间戳后显示所有事件
--until ：流水时间显示到指定的时间为止

docker events  --since="1467302400"
```

### :point_right:docker logs - 获取容器的日志

```bash
## 常用options
-f : 跟踪日志输出
--since :显示某个开始时间的所有日志
-t : 显示时间戳
--tail :仅列出最新N条容器日志

docker logs -f 493a8c89b042
```

### docker wait - 阻塞容器直到退出

打印退出代码

```bash
docker wait my_contair
```

### docker export - 将容器的文件系统导出为 tar 文件

起到备份容器的作用

```bash
docker export -o test.tar jenkins1
```

### :point_right:docker port - 查看容器的端口映射情况

```bash
docker port my_contair
```

### :point_right:docker stats - 显示容器资源的使用情况

```bash
docker stats [OPTIONS] [CONTAINER...]

# 常用options
--all , -a :显示所有的容器，包括未运行的。
--no-stream :展示当前状态就直接退出了，不再实时更新。

docker stats
docker stats nginx
```

### :point_right:docker commit - 从容器创建一个新的镜像。

```bash
# 常用options
-a :提交的镜像作者；
-m :提交时的说明文字；
-p :在commit时，将容器暂停。

docker commit -a "stonebird" -m "1245863260@qq.com" a404c6c174a2  nginx:v1 
```

### :point_right:docker diff - 对比容器与创建时文件变化

```bash
docker diff haicoder
```

### :point_right:docker cp - 用于容器与主机之间的数据拷贝

```bash
docker cp [OPTIONS] container:src_path dest_path
docker cp [OPTIONS] dest_path container:src_path 
```

## 数据卷

### :point_right:docker volume - 数据卷

```bash
docker volume --help

Commands:
  create      创建一个数据卷
  inspect     打印一个或多个数据卷的详细信息
  ls          列出所有数据卷
  prune       删除所有未使用的数据卷
  rm          删除一个或多个数据卷

# 在用 docker run 命令的时候，使用 --mount 标记来将 数据卷 挂载到容器里。
docker run -d -P \
    --name web \
    # -v my-vol:/usr/share/nginx/html \
    --mount source=my-vol,target=/usr/share/nginx/html \
    nginx:alpine

```

## 容器网络

### :point_right:docker network - 网络

网络是一门很难很重要的东西，慢慢学

```bash
docker network --help

Commands:
connect 	将某个容器连接到一个docker网络
create 		创建一个docker局域网络
disconnect 	将某个容器退出某个局域网络
inspect 	显示某个局域网络信息
ls 			显示所有docker局域网络
prune 		删除所有未引用的docker局域网络
rm 			删除docker网络

# 例子
docker network create mynet \
  --driver=bridge \
  --subnet=172.28.0.0/16 \
  --ip-range=172.28.5.0/24 \
  --gateway=172.28.5.254 \
  br0

docker run -d --name nginx -p 80:80 --network mynet
```

# dockerfile 

> Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大

## :point_right:FROM - 构建镜像基于哪个镜像

```bash
FROM scratch
```

## :point_right:MAINTAINER - 镜像维护者姓名或邮箱地址

```bash
LABEL maintainer="stonebird <1245863260@qq.com>"
```

## :point_right:RUN - 构建镜像时运行的指令

```bash
FROM centos
RUN yum -y install wget \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && tar -xvf redis.tar.gz
```

## :point_right:CMD - 运行容器时执行的shell环境

```bash
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 

# 例子
CMD [ "sh", "-c", "echo $HOME" ]
```

## :point_right:VOLUME - 指定容器挂载点到宿主机自动生成的目录或其他容器

```bash
VOLUME ["/data"] 

# docker run -v mydata:/data 
# 这里的 /data 目录就会在运行时自动挂载为匿名卷，任何向 /data 中写入的信息都不会记录进容器存储层，从而保证了容器存储层的无状态化
# 容器运行时使用 docker run -v mydata:/data  可以覆盖这个挂载设置【个人测试的时候貌似并不可以，不知道是不是姿势不对。。】
# 必须使用双引号，不能使用单引号
```

## :point_right:USER - 切换用户

为RUN、CMD、和 ENTRYPOINT 执行命令指定运行用户

`USER` 只是帮助你切换到指定用户而已，这个用户必须是事先建立好的，否则无法切换。

```bash
USER redis
```

## :point_right:WORKDIR - 切换目录

为 RUN、CMD、ENTRYPOINT、COPY 和 ADD 设置工作目录

```bash
WORKDIR /app  # 目录不存在，WORKDIR 会帮你建立目录

RUN echo "hello" > world.txt
```

## :point_right:HEALTHCHECH - 健康检查

```bash
HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令
HEALTHCHECK 支持下列选项：
--interval=<间隔>：两次健康检查的间隔，默认为 30 秒；
--timeout=<时长>：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒；
--retries=<次数>：当连续失败指定次数后，则将容器状态视为 unhealthy，默认 3 次。

HEALTHCHECK --interval=5s --timeout=3s CMD curl -fs http://localhost/ || exit 1
```

## :point_right:ARG - 构建时指定的一些参数

```bash
# 只在 FROM 中生效
ARG DOCKER_USERNAME=library
FROM ${DOCKER_USERNAME}/alpine
# 要想在 FROM 之后使用，必须再次指定
ARG DOCKER_USERNAME=library
RUN set -x ; echo ${DOCKER_USERNAME}
```

## :point_right:EXPOSE - 声明容器的服务端口

```bash
EXPOSE <端口1> [<端口2>...]
EXPOSE 端口号
EXPOSE 端口号/协议

EXPOSE 80/tcp
EXPOSE 80/udp
```

## :point_right:ENV - 设置容器环境变量

```bash
# 两种格式
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2>..

# 例子
ENV NODE_VERSION 7.2.0
ENV VERSION=1.0 DEBUG=on \
    NAME="Happy Feet"
```

## :point_right:ADD - 拷贝文件或目录到容器中（支持下载或解压）

```bash
ADD <src> <dest>
ADD nickdir.tar.gz /app
ADD http://example.com/big.tar.xz /app

# --chown=<user>:<group> 来改变文件的所属用户及所属组
ADD --chown=stonebird:stonebird files* /app/
```

## :point_right:COPY - 拷贝文件或目录到容器中（不支持下载或解压）

```bash
 COPY <src> <dest>
 COPY check* /testdir/ 
 
```

## :point_right:ENTRYPOINT - 运行容器时执行的shell命令

和CMD类似, 默认的ENTRYPOINT也在docker run时, 也可以被覆盖. 在运行时, 用--entrypoint覆盖默认的ENTRYPOINT

```bash
ENTRYPOINT [ "curl", "-s", "http://myip.ipip.net" ]

## 例子
docker run container:version -i
```

# docker-compose

> 一般使用k8来编排，了解即可

Compose 负责实现对 Docker 容器集群的快速编排,定义和运行多容器Docker应用程序的工具。通过Compose，您可以使用YAML文件来配置应用程序的服务。

docker-compose.yml。可以使用 -f 参数指定具体文件。

```bash
docker-compose version
```

Docker Compose 的 YAML 文件包含 4 个一级 key:version、services、networks、volumes

```bash
version: '3'
services:
	# 定义services 配置
	mysql:
    build:
      context: ./mysql
    environment:
      MYSQL_ROOT_PASSWORD: admin
    restart: always
    container_name: mysql
    volumes:
    - /data/edu-bom/mysql/test:/var/lib/mysql
    image: mysql/mysql:5.7
    ports:
      - 3306:3306
    networks:
      net:
networks:
    net:
volumes:
    vol:
```

## services配置参考

[菜鸟-compose](https://www.runoob.com/docker/docker-compose.html)

[Deepzz-compose](https://deepzz.com/post/docker-compose-file.html?spm=a2c6h.12873639.article-detail.6.7fe856aeCUcDxz#toc_1)

## 常用命令

> 和docker命令类似，掌握 up / down / build 即可。

```bash
docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]

# docker-compose build  构建容器
	`--force-rm` 删除构建过程中的临时容器。
	`--no-cache` 构建镜像过程中不使用 cache（这将加长构建过程）。
	`--pull` 始终尝试通过 pull 来获取更新版本的镜像。
# docker-compose config 验证Compose文件格式是否正确
# docker-compose up/down 停止启动容器
# docker-compose exec 进入指定的容器
# docker-compose images 列出 Compose 文件中包含的镜像。
# docker-compose kill  通过发送 `SIGKILL` 信号来强制停止服务容器
# docker-compose logs 查看服务容器的输出
# docker-compose pause/unpause  暂停/启动一个服务容器
# docker-compose port  打印某个容器端口所映射的公共端口。
	`--protocol=proto` 指定端口协议，tcp（默认值）或者 udp。
	`--index=index` 如果同一服务存在多个容器，指定命令对象容器的序号（默认为 1）。
# docker-compose ps	 	 # 列出项目中目前的所有容器
# docker-compose pull 	 # 拉取服务依赖的镜像。
# docker-compose push 	 # 推送服务依赖的镜像到镜像仓库
# docker-compose restart # 重启项目中的服务。
# docker-compose rm		 # 删除所有（停止状态的）服务容器。
	`-f, --force` 强制直接删除
	`-v` 删除容器所挂载的数据卷。
# docker-compose run 	 # 在指定服务上执行一个命令。
# docker-compose scale	 # 设置指定服务运行的容器副本个数。
# docker-compose start/stop # 启动/停止已经存在的服务容器。 
# docker-compose top 	 #  查看各个服务容器内运行的进程
# docker-compose up      # 
	`-d` 在后台运行服务容器。
```

