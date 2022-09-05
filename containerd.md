<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Containerd 架构](#containerd-%E6%9E%B6%E6%9E%84)
- [ctr - 基础命令(了解即可)](#ctr---%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4%E4%BA%86%E8%A7%A3%E5%8D%B3%E5%8F%AF)
  - [containers, c, container - 管理容器](#containers-c-container---%E7%AE%A1%E7%90%86%E5%AE%B9%E5%99%A8)
  - [images, image, i - 管理镜像](#images-image-i---%E7%AE%A1%E7%90%86%E9%95%9C%E5%83%8F)
  - [tasks, t, task - 对容器的一些操作](#tasks-t-task---%E5%AF%B9%E5%AE%B9%E5%99%A8%E7%9A%84%E4%B8%80%E4%BA%9B%E6%93%8D%E4%BD%9C)
  - [namespaces, namespace, ns - 管理命名空间](#namespaces-namespace-ns---%E7%AE%A1%E7%90%86%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)
  - [run - 运行一个容器](#run---%E8%BF%90%E8%A1%8C%E4%B8%80%E4%B8%AA%E5%AE%B9%E5%99%A8)
  - [其他子模块](#%E5%85%B6%E4%BB%96%E5%AD%90%E6%A8%A1%E5%9D%97)
- [crictl - k8s容器命令(值得细品)](#crictl---k8s%E5%AE%B9%E5%99%A8%E5%91%BD%E4%BB%A4%E5%80%BC%E5%BE%97%E7%BB%86%E5%93%81)
  - [POD 相关（todo：待完善）](#pod-%E7%9B%B8%E5%85%B3todo%E5%BE%85%E5%AE%8C%E5%96%84)
    - [crictl pods - 列出pods](#crictl-pods---%E5%88%97%E5%87%BApods)
    - [crictl runp - 运行pod](#crictl-runp---%E8%BF%90%E8%A1%8Cpod)
    - [crictl stopp - 停止pod](#crictl-stopp---%E5%81%9C%E6%AD%A2pod)
    - [crictl inspectp - 查看pod配置详情](#crictl-inspectp---%E6%9F%A5%E7%9C%8Bpod%E9%85%8D%E7%BD%AE%E8%AF%A6%E6%83%85)
    - [crictl statsp - 查看pod运行状态](#crictl-statsp---%E6%9F%A5%E7%9C%8Bpod%E8%BF%90%E8%A1%8C%E7%8A%B6%E6%80%81)
- [nerdctl -  docker风格的命令](#nerdctl----docker%E9%A3%8E%E6%A0%BC%E7%9A%84%E5%91%BD%E4%BB%A4)
  - [用户](#%E7%94%A8%E6%88%B7)
    - [:point_right:nerdctl login - 登录镜像仓](#point_rightnerdctl-login---%E7%99%BB%E5%BD%95%E9%95%9C%E5%83%8F%E4%BB%93)
    - [:point_right:nerdctl logout - 退出登录](#point_rightnerdctl-logout---%E9%80%80%E5%87%BA%E7%99%BB%E5%BD%95)
  - [系统管理](#%E7%B3%BB%E7%BB%9F%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl events - 获取实时事件](#point_rightnerdctl-events---%E8%8E%B7%E5%8F%96%E5%AE%9E%E6%97%B6%E4%BA%8B%E4%BB%B6)
    - [:point_right:nerdctl info - 显示系统范围的信息](#point_rightnerdctl-info---%E6%98%BE%E7%A4%BA%E7%B3%BB%E7%BB%9F%E8%8C%83%E5%9B%B4%E7%9A%84%E4%BF%A1%E6%81%AF)
    - [:point_right:nerdctl version - 版本信息](#point_rightnerdctl-version---%E7%89%88%E6%9C%AC%E4%BF%A1%E6%81%AF)
    - [:point_right:nerdctl system prune - 删除未使用的数据](#point_rightnerdctl-system-prune---%E5%88%A0%E9%99%A4%E6%9C%AA%E4%BD%BF%E7%94%A8%E7%9A%84%E6%95%B0%E6%8D%AE)
  - [镜像管理](#%E9%95%9C%E5%83%8F%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl images - 列出镜像](#point_rightnerdctl-images---%E5%88%97%E5%87%BA%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl pull - 拉取镜像](#point_rightnerdctl-pull---%E6%8B%89%E5%8F%96%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl push - 推送镜像](#point_rightnerdctl-push---%E6%8E%A8%E9%80%81%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl load -  从 tar 存档或 STDIN 加载镜像](#point_rightnerdctl-load----%E4%BB%8E-tar-%E5%AD%98%E6%A1%A3%E6%88%96-stdin-%E5%8A%A0%E8%BD%BD%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl save - 将镜像保存为归档文件](#point_rightnerdctl-save---%E5%B0%86%E9%95%9C%E5%83%8F%E4%BF%9D%E5%AD%98%E4%B8%BA%E5%BD%92%E6%A1%A3%E6%96%87%E4%BB%B6)
    - [:point_right:nerdctl tag - 给镜像创建一个新tag](#point_rightnerdctl-tag---%E7%BB%99%E9%95%9C%E5%83%8F%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%B0tag)
    - [:point_right:nerdctl rmi -  删除镜像](#point_rightnerdctl-rmi----%E5%88%A0%E9%99%A4%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl image inspect - 显示镜像详细信息](#point_rightnerdctl-image-inspect---%E6%98%BE%E7%A4%BA%E9%95%9C%E5%83%8F%E8%AF%A6%E7%BB%86%E4%BF%A1%E6%81%AF)
    - [:point_right:nerdctl image history - 显示镜像历史](#point_rightnerdctl-image-history---%E6%98%BE%E7%A4%BA%E9%95%9C%E5%83%8F%E5%8E%86%E5%8F%B2)
    - [:point_right:nerdctl image prune - 删除未使用的镜像](#point_rightnerdctl-image-prune---%E5%88%A0%E9%99%A4%E6%9C%AA%E4%BD%BF%E7%94%A8%E7%9A%84%E9%95%9C%E5%83%8F)
    - [nerdctl image convert - 镜像转换](#nerdctl-image-convert---%E9%95%9C%E5%83%8F%E8%BD%AC%E6%8D%A2)
    - [nerdctl image encrypt - 镜像加密](#nerdctl-image-encrypt---%E9%95%9C%E5%83%8F%E5%8A%A0%E5%AF%86)
    - [nerdctl image decrypt - 镜像解密](#nerdctl-image-decrypt---%E9%95%9C%E5%83%8F%E8%A7%A3%E5%AF%86)
  - [容器管理](#%E5%AE%B9%E5%99%A8%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl run - 在新容器中运行命令](#point_rightnerdctl-run---%E5%9C%A8%E6%96%B0%E5%AE%B9%E5%99%A8%E4%B8%AD%E8%BF%90%E8%A1%8C%E5%91%BD%E4%BB%A4)
    - [:point_right:nerdctl exec - 在运行的容器中执行命令](#point_rightnerdctl-exec---%E5%9C%A8%E8%BF%90%E8%A1%8C%E7%9A%84%E5%AE%B9%E5%99%A8%E4%B8%AD%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
    - [nerdctl create - 创建新容器不运行](#nerdctl-create---%E5%88%9B%E5%BB%BA%E6%96%B0%E5%AE%B9%E5%99%A8%E4%B8%8D%E8%BF%90%E8%A1%8C)
    - [:point_right:nerdctl cp - 主机与容器间文件拷贝](#point_rightnerdctl-cp---%E4%B8%BB%E6%9C%BA%E4%B8%8E%E5%AE%B9%E5%99%A8%E9%97%B4%E6%96%87%E4%BB%B6%E6%8B%B7%E8%B4%9D)
    - [:point_right:nerdctl ps - 容器列表](#point_rightnerdctl-ps---%E5%AE%B9%E5%99%A8%E5%88%97%E8%A1%A8)
    - [:point_right:nerdctl inspect - 显示容器的详细信息](#point_rightnerdctl-inspect---%E6%98%BE%E7%A4%BA%E5%AE%B9%E5%99%A8%E7%9A%84%E8%AF%A6%E7%BB%86%E4%BF%A1%E6%81%AF)
    - [:point_right:nerdctl logs - 获取容器的日志](#point_rightnerdctl-logs---%E8%8E%B7%E5%8F%96%E5%AE%B9%E5%99%A8%E7%9A%84%E6%97%A5%E5%BF%97)
    - [:point_right:nerdctl port - 显示容器端口映射](#point_rightnerdctl-port---%E6%98%BE%E7%A4%BA%E5%AE%B9%E5%99%A8%E7%AB%AF%E5%8F%A3%E6%98%A0%E5%B0%84)
    - [:point_right:nerdctl rm - 删除容器](#point_rightnerdctl-rm---%E5%88%A0%E9%99%A4%E5%AE%B9%E5%99%A8)
    - [nerdctl stop -  停止运行容器](#nerdctl-stop----%E5%81%9C%E6%AD%A2%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8)
    - [nerdctl start - 启动运行容器](#nerdctl-start---%E5%90%AF%E5%8A%A8%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8)
    - [nerdctl restart - 重新运行中容器](#nerdctl-restart---%E9%87%8D%E6%96%B0%E8%BF%90%E8%A1%8C%E4%B8%AD%E5%AE%B9%E5%99%A8)
    - [:point_right:nerdctl update - 更改容器配置](#point_rightnerdctl-update---%E6%9B%B4%E6%94%B9%E5%AE%B9%E5%99%A8%E9%85%8D%E7%BD%AE)
    - [nerdctl wait - 停止运行容器打印退出代码](#nerdctl-wait---%E5%81%9C%E6%AD%A2%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8%E6%89%93%E5%8D%B0%E9%80%80%E5%87%BA%E4%BB%A3%E7%A0%81)
    - [nerdctl kill - 杀死运行中的容器](#nerdctl-kill---%E6%9D%80%E6%AD%BB%E8%BF%90%E8%A1%8C%E4%B8%AD%E7%9A%84%E5%AE%B9%E5%99%A8)
    - [nerdctl pause - 暂停容器运行](#nerdctl-pause---%E6%9A%82%E5%81%9C%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C)
    - [nerdctl unpause - 恢复容器运行](#nerdctl-unpause---%E6%81%A2%E5%A4%8D%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C)
    - [nerdctl rename - 重命名容器](#nerdctl-rename---%E9%87%8D%E5%91%BD%E5%90%8D%E5%AE%B9%E5%99%A8)
    - [:point_right:nerdctl container prune - 移除所有停止的容器](#point_rightnerdctl-container-prune---%E7%A7%BB%E9%99%A4%E6%89%80%E6%9C%89%E5%81%9C%E6%AD%A2%E7%9A%84%E5%AE%B9%E5%99%A8)
  - [Stats - 状态](#stats---%E7%8A%B6%E6%80%81)
    - [:point_right:nerdctl stats - 显示容器使用资源](#point_rightnerdctl-stats---%E6%98%BE%E7%A4%BA%E5%AE%B9%E5%99%A8%E4%BD%BF%E7%94%A8%E8%B5%84%E6%BA%90)
    - [:point_right:nerdctl top - 显示容器运行的进程](#point_rightnerdctl-top---%E6%98%BE%E7%A4%BA%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C%E7%9A%84%E8%BF%9B%E7%A8%8B)
  - [build](#build)
    - [:point_right:nerdctl build - 从Dockerfile构建镜像](#point_rightnerdctl-build---%E4%BB%8Edockerfile%E6%9E%84%E5%BB%BA%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl commit - 将容器保存为镜像](#point_rightnerdctl-commit---%E5%B0%86%E5%AE%B9%E5%99%A8%E4%BF%9D%E5%AD%98%E4%B8%BA%E9%95%9C%E5%83%8F)
  - [网络管理](#%E7%BD%91%E7%BB%9C%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl network create - 创建网络](#point_rightnerdctl-network-create---%E5%88%9B%E5%BB%BA%E7%BD%91%E7%BB%9C)
    - [:point_right:nerdctl network ls - 网络列表](#point_rightnerdctl-network-ls---%E7%BD%91%E7%BB%9C%E5%88%97%E8%A1%A8)
    - [:point_right:nerdctl network inspect - 网络详情](#point_rightnerdctl-network-inspect---%E7%BD%91%E7%BB%9C%E8%AF%A6%E6%83%85)
    - [:point_right:nerdctl network rm - 删除网络](#point_rightnerdctl-network-rm---%E5%88%A0%E9%99%A4%E7%BD%91%E7%BB%9C)
    - [:point_right:nerdctl network prune - 删除没有使用的网络](#point_rightnerdctl-network-prune---%E5%88%A0%E9%99%A4%E6%B2%A1%E6%9C%89%E4%BD%BF%E7%94%A8%E7%9A%84%E7%BD%91%E7%BB%9C)
  - [数据卷管理](#%E6%95%B0%E6%8D%AE%E5%8D%B7%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl volume create - 创建数据卷](#point_rightnerdctl-volume-create---%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%8D%B7)
    - [:point_right:nerdctl volume ls - 数据卷列表](#point_rightnerdctl-volume-ls---%E6%95%B0%E6%8D%AE%E5%8D%B7%E5%88%97%E8%A1%A8)
    - [:point_right:nerdctl volume inspect - 数据卷详情](#point_rightnerdctl-volume-inspect---%E6%95%B0%E6%8D%AE%E5%8D%B7%E8%AF%A6%E6%83%85)
    - [:point_right:nerdctl volume rm - 删除数据卷](#point_rightnerdctl-volume-rm---%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE%E5%8D%B7)
    - [:point_right:nerdctl volume prune - 删除没有使用的数据卷](#point_rightnerdctl-volume-prune---%E5%88%A0%E9%99%A4%E6%B2%A1%E6%9C%89%E4%BD%BF%E7%94%A8%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8D%B7)
  - [命名空间管理](#%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl namespace create - 创建命名空间](#point_rightnerdctl-namespace-create---%E5%88%9B%E5%BB%BA%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)
    - [:point_right:nerdctl namespace ls - 命名空间列表](#point_rightnerdctl-namespace-ls---%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E5%88%97%E8%A1%A8)
    - [:point_right:nerdctl namespace inspect - 命名空间详情](#point_rightnerdctl-namespace-inspect---%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E8%AF%A6%E6%83%85)
    - [:point_right:nerdctl namespace remove - 删除命名空间](#point_rightnerdctl-namespace-remove---%E5%88%A0%E9%99%A4%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4)
    - [:point_right:nerdctl namespace update - 更改命名空间（label）](#point_rightnerdctl-namespace-update---%E6%9B%B4%E6%94%B9%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4label)
  - [容器资源管理](#%E5%AE%B9%E5%99%A8%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86)
    - [nerdctl apparmor inspect - 显示nerdctl-default配置详情](#nerdctl-apparmor-inspect---%E6%98%BE%E7%A4%BAnerdctl-default%E9%85%8D%E7%BD%AE%E8%AF%A6%E6%83%85)
    - [nerdctl apparmor load - 加载nerdctl-default配置详情](#nerdctl-apparmor-load---%E5%8A%A0%E8%BD%BDnerdctl-default%E9%85%8D%E7%BD%AE%E8%AF%A6%E6%83%85)
    - [nerdctl apparmor ls - 已加载AppArmor profile列表](#nerdctl-apparmor-ls---%E5%B7%B2%E5%8A%A0%E8%BD%BDapparmor-profile%E5%88%97%E8%A1%A8)
    - [nerdctl apparmor unload - 卸载 AppArmor 配置文件](#nerdctl-apparmor-unload---%E5%8D%B8%E8%BD%BD-apparmor-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
  - [Builder 管理](#builder-%E7%AE%A1%E7%90%86)
    - [nerdctl builder prune - 清理 BuildKit 构建缓存](#nerdctl-builder-prune---%E6%B8%85%E7%90%86-buildkit-%E6%9E%84%E5%BB%BA%E7%BC%93%E5%AD%98)
    - [:point_right:nerdctl builder debug - 使用 buildg 交互式调试 Dockerfile](#point_rightnerdctl-builder-debug---%E4%BD%BF%E7%94%A8-buildg-%E4%BA%A4%E4%BA%92%E5%BC%8F%E8%B0%83%E8%AF%95-dockerfile)
  - [Shell completion](#shell-completion)
    - [:point_right:nerdctl completion bash - 为 bash 生成自动完成脚本](#point_rightnerdctl-completion-bash---%E4%B8%BA-bash-%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E8%84%9A%E6%9C%AC)
    - [nerdctl completion zsh - 为 zsh生成自动完成脚本](#nerdctl-completion-zsh---%E4%B8%BA-zsh%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E8%84%9A%E6%9C%AC)
    - [nerdctl completion fish - 为 fish生成自动完成脚本](#nerdctl-completion-fish---%E4%B8%BA-fish%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E8%84%9A%E6%9C%AC)
    - [nerdctl completion powershell - 为 powershell 生成自动完成脚本](#nerdctl-completion-powershell---%E4%B8%BA-powershell-%E7%94%9F%E6%88%90%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E8%84%9A%E6%9C%AC)
  - [Compose - 容器编排](#compose---%E5%AE%B9%E5%99%A8%E7%BC%96%E6%8E%92)
    - [:point_right:nerdctl compose - 编排](#point_rightnerdctl-compose---%E7%BC%96%E6%8E%92)
    - [:point_right:nerdctl compose up  - 创建和启动容器](#point_rightnerdctl-compose-up----%E5%88%9B%E5%BB%BA%E5%92%8C%E5%90%AF%E5%8A%A8%E5%AE%B9%E5%99%A8)
    - [:point_right:nerdctl compose logs -  查看容器日志](#point_rightnerdctl-compose-logs----%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E6%97%A5%E5%BF%97)
    - [:point_right:nerdctl compose build - 创建或重新创建服务](#point_rightnerdctl-compose-build---%E5%88%9B%E5%BB%BA%E6%88%96%E9%87%8D%E6%96%B0%E5%88%9B%E5%BB%BA%E6%9C%8D%E5%8A%A1)
    - [:point_right:nerdctl compose down - 移除容器和相关资源](#point_rightnerdctl-compose-down---%E7%A7%BB%E9%99%A4%E5%AE%B9%E5%99%A8%E5%92%8C%E7%9B%B8%E5%85%B3%E8%B5%84%E6%BA%90)
    - [:point_right:nerdctl compose ps - 列出服务的相关容器](#point_rightnerdctl-compose-ps---%E5%88%97%E5%87%BA%E6%9C%8D%E5%8A%A1%E7%9A%84%E7%9B%B8%E5%85%B3%E5%AE%B9%E5%99%A8)
    - [nerdctl compose pull - 拉取服务镜像](#nerdctl-compose-pull---%E6%8B%89%E5%8F%96%E6%9C%8D%E5%8A%A1%E9%95%9C%E5%83%8F)
    - [nerdctl compose push - 推送服务镜像](#nerdctl-compose-push---%E6%8E%A8%E9%80%81%E6%9C%8D%E5%8A%A1%E9%95%9C%E5%83%8F)
    - [:point_right:nerdctl compose config - 验证并查看 Compose 文件](#point_rightnerdctl-compose-config---%E9%AA%8C%E8%AF%81%E5%B9%B6%E6%9F%A5%E7%9C%8B-compose-%E6%96%87%E4%BB%B6)
    - [nerdctl compose kill - 杀死服务的容器运行](#nerdctl-compose-kill---%E6%9D%80%E6%AD%BB%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%AE%B9%E5%99%A8%E8%BF%90%E8%A1%8C)
    - [nerdctl compose run - 启动服务运行容器（一次性命令）](#nerdctl-compose-run---%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1%E8%BF%90%E8%A1%8C%E5%AE%B9%E5%99%A8%E4%B8%80%E6%AC%A1%E6%80%A7%E5%91%BD%E4%BB%A4)
  - [IPFS 管理](#ipfs-%E7%AE%A1%E7%90%86)
    - [:point_right:nerdctl ipfs registry up - 启动由 IPFS 支持的只读本地注册表](#point_rightnerdctl-ipfs-registry-up---%E5%90%AF%E5%8A%A8%E7%94%B1-ipfs-%E6%94%AF%E6%8C%81%E7%9A%84%E5%8F%AA%E8%AF%BB%E6%9C%AC%E5%9C%B0%E6%B3%A8%E5%86%8C%E8%A1%A8)
    - [:point_right:nerdctl ipfs registry down - 停止由 IPFS 支持的只读本地注册表](#point_rightnerdctl-ipfs-registry-down---%E5%81%9C%E6%AD%A2%E7%94%B1-ipfs-%E6%94%AF%E6%8C%81%E7%9A%84%E5%8F%AA%E8%AF%BB%E6%9C%AC%E5%9C%B0%E6%B3%A8%E5%86%8C%E8%A1%A8)
    - [:point_right:nerdctl ipfs registry serve - 启动在 localhost 上提供由 IPFS 支持的只读注册表。](#point_rightnerdctl-ipfs-registry-serve---%E5%90%AF%E5%8A%A8%E5%9C%A8-localhost-%E4%B8%8A%E6%8F%90%E4%BE%9B%E7%94%B1-ipfs-%E6%94%AF%E6%8C%81%E7%9A%84%E5%8F%AA%E8%AF%BB%E6%B3%A8%E5%86%8C%E8%A1%A8)
  - [全局options](#%E5%85%A8%E5%B1%80options)
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

# ctr - 基础命令(了解即可)

> 比docker多了namepace概念

了解`namespace`、`containers`、`images`、`run`、`tasks` 就行。

```bash
COMMANDS:
   plugins, plugin            provides information about containerd plugins
   version                    print the client and server versions
   containers, c, container   manage containers
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
   help, h                    Shows a list of commands or help for one command

```

## containers, c, container - 管理容器

了解 `create` 、`delete del remove rm` 、`info` 、`list` 即可

```bash
COMMANDS:
   create                   # 创建容器。options 和docker差不多
   	`ctr c create [command options] [flags] Image|RootFS CONTAINER [COMMAND] [ARG...]`
   	
   delete, del, remove, rm  # 删除一个或多个存在的容器
   	`ctr c rm [command options] [flags] CONTAINER [CONTAINER, ...]`
   	
   info                     # 获取一个容器的信息
   	`ctr c info [command options] CONTAINER`
   	
   list, ls                 # 容器列表
   	`ctr c list [command options] [flags] [<filter>, ...]`
   	 
   label                    # 设置或删除容器的lable
   	`ctr c label [flags] CONTAINER [<key>=<value>, ...]`
   	
   checkpoint               # 检查一个容器的checkpoint事件
   	`ctr c checkpoint [command options] CONTAINER REF`
   	
   restore                  # 从检查点恢复容器
	`ctr c restore [command options] CONTAINER REF`
```

> `checkpoint` 和 `restore` 参数其实都是通过`runc`去提供的一种容器快照功能，保存容器当前状态或恢复当前状态。

##  images, image, i - 管理镜像

和docker功能类似，了解`export/import`、`ls`、`mount/unmount`、`pull/push`、`del delete remove rm`、`tag` 即可

```bash
USAGE:
   ctr images command [command options] [arguments...]

COMMANDS:
   check                    # 检查现有镜像以确保所有内容在本地可用
   	`ctr i check [command options] [flags] [<filter>, ...]`
   
   export                   # 导出镜像到tar文件
   	`ctr i export [command options] [flags] <out> <image> ...`
   	
   import                   # 从tar文件导入镜像
   	` ctr i import [command options] [flags] <in>`
   	
   list, ls                 # 镜像列表
   mount                    # 将镜像挂载到指定目录
   unmount                  # 取消镜像挂载
   pull                     # 拉取远端镜像
   push                     # 把镜像推送到远端
   delete, del, remove, rm  # 删除镜像
   tag                      # 重新设置镜像tag
   label                    # 设置或清理镜像label
   convert                  # 镜像转换
   	`ctr images convert [command options] [flags] <source_ref> <target_ref>`
```

## tasks, t, task - 对容器的一些操作

和docker功能类似，task 命令都要去了解一些

```bash
USAGE:
   ctr tasks command [command options] [arguments...]

COMMANDS:
   attach                   attach to the IO of a running container
   checkpoint               checkpoint a container
   delete, del, remove, rm  delete one or more tasks
   exec                     execute additional processes in an existing container
   list, ls                 list tasks
   kill                     signal a container (default: SIGTERM)
   pause                    pause an existing container
   ps                       list processes for container
   resume                   resume a paused container
   start                    start a container that has been created
   metrics, metric          get a single data point of metrics for a task with the built-in Linux runtime
```

## namespaces, namespace, ns - 管理命名空间

```bash
USAGE:
   ctr namespaces command [command options] [arguments...]

COMMANDS:
   create, c   create a new namespace
   list, ls    list namespaces
   remove, rm  remove one or more namespaces
   label       set and clear labels for a namespace
```

## run - 运行一个容器

运行的options 需要自己查阅

```bash
 ctr run [command options] [flags] Image|RootFS ID [COMMAND] [ARG...]
```



## 其他子模块

**plugins, plugin - 查看插件信息**

`ctr plugins ls`

**version - 版本信息**

`ctr version`

**content - 管理内容（可以修改镜像）**

**snapshots- 镜像层的操作**

**pprof - 监控**

**events - 可以监听到containerd内的所有事件**

**oci - 这个模块主要是展示OCI的规范参数**

**shim - 连接模块**

# crictl - k8s容器命令(值得细品)

> k8s环境下使用，命名空间k8s.io下的容器与镜像
>
> 除了没push外其他和docker，nerdctl命令太像，掌握与pod相关即可

[基本使用文档](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md)

`crictl` 是 CRI 兼容的容器运行时命令行接口。 你可以使用它来检查和调试 Kubernetes 节点上的容器运行时和应用程序

```bash

USAGE:
   crictl [global options] command [command options] [arguments...]

COMMANDS:
   attach              Attach to a running container
   create              Create a new container
   exec                Run a command in a running container
   version             Display runtime version information
   images, image, img  List images
   inspect             Display the status of one or more containers
   inspecti            Return the status of one or more images
   imagefsinfo         Return image filesystem info
   inspectp            Display the status of one or more pods
   logs                Fetch the logs of a container
   port-forward        Forward local port to a pod
   ps                  List containers
   pull                Pull an image from a registry
   run                 Run a new container inside a sandbox
   runp                Run a new pod
   rm                  Remove one or more containers
   rmi                 Remove one or more images
   rmp                 Remove one or more pods
   pods                List pods
   start               Start one or more created containers
   info                Display information of the container runtime
   stop                Stop one or more running containers
   stopp               Stop one or more running pods
   update              Update one or more running containers
   config              Get and set crictl client configuration options
   stats               List container(s) resource usage statistics
   statsp              List pod resource usage statistics
   completion          Output shell completion code
   checkpoint          Checkpoint one or more running containers
   help, h             Shows a list of commands or help for one command
```

## POD 相关（todo：待完善）

### crictl pods - 列出pods

### crictl runp - 运行pod

### crictl stopp - 停止pod

### crictl inspectp - 查看pod配置详情

### crictl statsp - 查看pod运行状态

# nerdctl -  docker风格的命令

[官方文档](https://github.com/containerd/nerdctl)

## 用户

### :point_right:nerdctl login - 登录镜像仓

```bash
Usage: 
	nerdctl login [OPTIONS] [SERVER]

# 常用options
-u, --username: Username
-p, --password: Password
--password-stdin: Take the password from stdin
```

### :point_right:nerdctl logout - 退出登录

```bash
Usage: 
	nerdctl logout [SERVER]
```

## 系统管理

### :point_right:nerdctl events - 获取实时事件

```bash
Usage: 
	nerdctl events [OPTIONS]

# 常用options
--format: Format the output using the given Go template, e.g, {{json .}}
```

### :point_right:nerdctl info - 显示系统范围的信息

```bash
Usage: 
	nerdctl info [OPTIONS]
```

### :point_right:nerdctl version - 版本信息

```bash
Usage: 
	nerdctl version 
```

### :point_right:nerdctl system prune - 删除未使用的数据

> 目前需要指定 --all

```bash
Usage: 
	nerdctl system prune [OPTIONS]

# 常用options
-a, --all: Remove all unused images, not just dangling ones
-f, --force: Do not prompt for confirmation
--volumes: Prune volumes
```

## 镜像管理

### :point_right:nerdctl images - 列出镜像

```bash
Usage: 
	nerdctl images [OPTIONS] [REPOSITORY[:TAG]]

# 常用options
-a, --all: Show all images
--names: Show image names
```

### :point_right:nerdctl pull - 拉取镜像

```bash
Usage: 
	nerdctl pull [OPTIONS] NAME[:TAG]
```

### :point_right:nerdctl push - 推送镜像

```bash
Usage: 
	nerdctl push [OPTIONS] NAME[:TAG]
```

### :point_right:nerdctl load -  从 tar 存档或 STDIN 加载镜像

```bash
Usage: 
	nerdctl load [OPTIONS]

# 常用options
-i, --input: Read from tar archive file, instead of STDIN
--platform=(amd64|arm64|...): Import content for a specific platform
--all-platforms: Import content for all platforms

# 例子
nerdctl load -i nginx_v1.tar
nerdctl load < nginx_v1.tar  # 可以读取标准输入
```

### :point_right:nerdctl save - 将镜像保存为归档文件

```bash
Usage: 
	nerdctl save [OPTIONS] IMAGE [IMAGE...]

# 常用options
-o, --output: Write to a file, instead of STDOUT
--platform=(amd64|arm64|...): Export content for a specific platform
--all-platforms: Export content for all platforms

# 例子
nerdctl save -o nginx_v1.tar nginx:v1
```

### :point_right:nerdctl tag - 给镜像创建一个新tag

```bash
Usage: 
	nerdctl tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

### :point_right:nerdctl rmi -  删除镜像

```bash
Usage: 
	nerdctl rmi [OPTIONS] IMAGE [IMAGE...]

# 常用options
-f, --force: Force removal of the image
```

### :point_right:nerdctl image inspect - 显示镜像详细信息

```bash
Usage: 
	nerdctl image inspect [OPTIONS] NAME|ID [NAME|ID...]

# 常用options
--mode=(dockercompat|native): Inspection mode. "native" produces more information.
--format: Format the output using the given Go template, e.g, {{json .}}
--platform=(amd64|arm64|...): Inspect a specific platform
```

### :point_right:nerdctl image history - 显示镜像历史

```bash
Usage: 
	nerdctl history [OPTIONS] IMAGE

# 常用options
--no-trunc: Don't truncate output
-q, --quiet: Only display snapshots IDs
--format: Format the output using the given Go template, e.g, {{json .}}
```

### :point_right:nerdctl image prune - 删除未使用的镜像

```bash
Usage: 
	nerdctl image prune [OPTIONS]

# 常用options
-a, --all: Remove all unused images, not just dangling ones
-f, --force: Do not prompt for confirmation
```

### nerdctl image convert - 镜像转换

```bash
Usage: 
	nerdctl image convert [OPTIONS] SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

# 例子
nerdctl image convert --estargz --oci example.com/foo:orig example.com/foo:esgz
```

### nerdctl image encrypt - 镜像加密

> [加密详情](https://github.com/containerd/nerdctl/blob/master/docs/ocicrypt.md)

```bash
Usage: 
	nerdctl image encrypt [OPTIONS] SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

### nerdctl image decrypt - 镜像解密

```bash
Usage: 
	nerdctl image decrypt [OPTIONS] SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```



## 容器管理

### :point_right:nerdctl run - 在新容器中运行命令

```bash
Usage:
	nerdctl run [OPTIONS] IMAGE [COMMAND] [ARG...]
	
# options 与docker一致
-d 
--rm 
-net
--name string
--namespace string 
```

### :point_right:nerdctl exec - 在运行的容器中执行命令

```bash
Usage: 
	nerdctl exec [OPTIONS] CONTAINER COMMAND [ARG...]

# options
-d, --detach             Detached mode: run command in the background
-e, --env stringArray    Set environment variables
	--env-file strings   Set environment variables from file
-h, --help               help for exec
-i, --interactive        Keep STDIN open even if not attached
    --privileged         Give extended privileges to the command
-t, --tty                (Currently -t needs to correspond to -i)
-u, --user string        Username or UID (format: <name|uid>[:<group|gid>])
-w, --workdir string     Working directory inside the container
```

### nerdctl create - 创建新容器不运行

与nerdctl run用法一致 

### :point_right:nerdctl cp - 主机与容器间文件拷贝

```bash
Usage:
 	nerdctl cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
 	nerdctl cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH [flags]

# 常用options
-L, --follow-link Always follow symbol link in SRC_PATH.
```

### :point_right:nerdctl ps - 容器列表

```bash
Usage: 
	nerdctl ps [OPTIONS]

# 常用options
-a Show all containers (default shows just running)
```

### :point_right:nerdctl inspect - 显示容器的详细信息

```
Usage: 
	nerdctl inspect [OPTIONS] NAME|ID [NAME|ID...]
```

### :point_right:nerdctl logs - 获取容器的日志

```bash
Usage: 
	nerdctl logs [OPTIONS] CONTAINER
	
# 常用options
	--since: Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
	--until: Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
-t, --timestamps: Show timestamps
-n, --tail: Number of lines to show from the end of the logs (default "all")
```

### :point_right:nerdctl port - 显示容器端口映射

```bash
Usage: 
	nerdctl port CONTAINER [PRIVATE_PORT[/PROTO]]
```

### :point_right:nerdctl rm - 删除容器

```bash
Usage: 
	nerdctl rm [OPTIONS] CONTAINER [CONTAINER...]

# 常用options
-f, --force: Force the removal of a running|paused|unknown container (uses SIGKILL)
-v, --volumes: Remove anonymous volumes associated with the container
```

### nerdctl stop -  停止运行容器

```bash
Usage: 
	nerdctl stop [OPTIONS] CONTAINER [CONTAINER...]

# 常用options
-t, --time=SECONDS: Seconds to wait for stop before killing it (default "10")
```

### nerdctl start - 启动运行容器

```bash
Usage: 
	nerdctl start [OPTIONS] CONTAINER [CONTAINER...]
```

### nerdctl restart - 重新运行中容器

```bash
Usage: 
	nerdctl restart [OPTIONS] CONTAINER [CONTAINER...]

# 常用options
-t, --time=SECONDS: Seconds to wait for stop before killing it (default "10")
```

### :point_right:nerdctl update - 更改容器配置

```bash
Usage: 
	nerdctl update [OPTIONS] CONTAINER [CONTAINER...]

# 常用options
--cpus: Number of CPUs
--cpu-quota: Limit the CPU CFS (Completely Fair Scheduler) quota
--cpu-period: Limit the CPU CFS (Completely Fair Scheduler) period
--cpu-shares: CPU shares (relative weight)
--cpuset-cpus: CPUs in which to allow execution (0-3, 0,1)
--cpuset-mems: Memory nodes (MEMs) in which to allow execution (0-3, 0,1). Only effective on NUMA systems
--memory: Memory limit
--memory-reservation: Memory soft limit
--memory-swap: Swap limit equal to memory plus swap: '-1' to enable unlimited swap
--kernel-memory: Kernel memory limit (deprecated)
--pids-limit: Tune container pids limit
--blkio-weight: Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
```

### nerdctl wait - 停止运行容器打印退出代码

> 一般用stop替换

```bash
Usage: 
	nerdctl wait CONTAINER [CONTAINER...]
```

### nerdctl kill - 杀死运行中的容器

> 一般用stop替换

```bash
Usage: 
	nerdctl kill [OPTIONS] CONTAINER [CONTAINER...]

# 常用options
-s, --signal: Signal to send to the container (default: "KILL")
```

### nerdctl pause - 暂停容器运行

```bash
Usage: 
	nerdctl pause CONTAINER [CONTAINER...]
```

### nerdctl unpause - 恢复容器运行

```bash
Usage: 
	nerdctl unpause CONTAINER [CONTAINER...]
```

### nerdctl rename - 重命名容器

```bash
Usage: 
	nerdctl rename CONTAINER NEW_NAME
```

### :point_right:nerdctl container prune - 移除所有停止的容器

```bash
Usage: 
	nerdctl container prune [OPTIONS]

# 常用options
-f, --force: Do not prompt for confirmation.
```

## Stats - 状态

### :point_right:nerdctl stats - 显示容器使用资源

```bash
Usage: 
	nerdctl stats [OPTIONS]

# 常用options
-a, --all: Show all containers (default shows just running)
--format=FORMAT: Pretty-print images using a Go template, e.g., {{json .}}
--no-stream: Disable streaming stats and only pull the first result
--no-trunc : Do not truncate output
```

### :point_right:nerdctl top - 显示容器运行的进程

```bash
Usage: 
	nerdctl top CONTAINER [ps OPTIONS]
```

## build

### :point_right:nerdctl build - 从Dockerfile构建镜像

```bash
Usage: 
	nerdctl build [OPTIONS] PATH

# 常用options
-t, --tag: Name and optionally a tag in the 'name:tag' format
-f, --file: Name of the Dockerfile

# 例子
nerdctl build -t runoob/ubuntu:v1 -f /path/Dockerfil. 
```

### :point_right:nerdctl commit - 将容器保存为镜像

```bash
Usage: 
	nerdctl commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

# 常用options
-a, --author: Author (e.g., "nerdctl contributor nerdctl-dev@example.com")
-m, --message: Commit message
-c, --change: Apply Dockerfile instruction to the created image (supported directives: [CMD, ENTRYPOINT])
-p, --pause: Pause container during commit (default: true)

# 例子
nerdctl commit -a "stonebird" -m "1245863260@qq.com" a404c6c174a2  nginx:v1
```

## 网络管理

### :point_right:nerdctl network create - 创建网络

```bash
Usage: 
	nerdctl network create [OPTIONS] NETWORK

# 常用options
-d, --driver=(bridge|nat|macvlan|ipvlan): Driver to manage the Network(Default bridge)
--subnet: Subnet in CIDR format that represents a network segment, e.g. "10.5.0.0/16"
--gateway: Gateway for the master subnet
```

### :point_right:nerdctl network ls - 网络列表

```bash
Usage: 
	nerdctl network ls [OPTIONS]
```

### :point_right:nerdctl network inspect - 网络详情

```bash
Usage: 
	nerdctl network inspect [OPTIONS] NETWORK [NETWORK...]
```

### :point_right:nerdctl network rm - 删除网络

```bash
Usage: 
	nerdctl network rm NETWORK [NETWORK...]
```

### :point_right:nerdctl network prune - 删除没有使用的网络

```bash
Usage: 
	nerdctl network prune [OPTIONS]

# 常用options
-f, --force: Do not prompt for confirmation
```

## 数据卷管理

### :point_right:nerdctl volume create - 创建数据卷

```bash
Usage: 
	nerdctl volume create [OPTIONS] [VOLUME]
```

### :point_right:nerdctl volume ls - 数据卷列表

```bash
Usage: 
	nerdctl volume ls [OPTIONS]
```

### :point_right:nerdctl volume inspect - 数据卷详情

```bash
Usage: 
	nerdctl volume inspect [OPTIONS] VOLUME [VOLUME...]
```

### :point_right:nerdctl volume rm - 删除数据卷

```bash
Usage: 
	nerdctl volume rm [OPTIONS] VOLUME [VOLUME...]

# 常用options
-f, --force: Force the removal of one or more volumes
```

### :point_right:nerdctl volume prune - 删除没有使用的数据卷

```bash
Usage: 
	nerdctl volume prune [OPTIONS]

# 常用options
-f, --force: Do not prompt for confirmation
```

## 命名空间管理

### :point_right:nerdctl namespace create - 创建命名空间

```bash
Usage: 
	nerdctl namespace create NAMESPACE 

# 常用options
--label: Set labels for a namespa
```

### :point_right:nerdctl namespace ls - 命名空间列表

```bash
Usage: 
	nerdctl namespace ls [OPTIONS]
```

### :point_right:nerdctl namespace inspect - 命名空间详情

```bash
Usage: 
	nerdctl namespace inspect NAMESPACE
```

### :point_right:nerdctl namespace remove - 删除命名空间

```bash
Usage: 
	nerdctl namespace remove [OPTIONS] NAMESPACE [NAMESPACE...]

# 常用options
-c, --cgroup: delete the namespace's cgroup
```

### :point_right:nerdctl namespace update - 更改命名空间（label）

```bash
Usage: 
	nerdctl namespace update NAMESPACE

# 常用options
--label: Set labels for a namespace
```



## 容器资源管理

> AppArmor限制容器对资源访问
>
> 管理 AppArmor 配置文件

### nerdctl apparmor inspect - 显示nerdctl-default配置详情

> 此命令无法显示其他配置文件。

```bash
Usage: 
	nerdctl apparmor inspect
```

### nerdctl apparmor load - 加载nerdctl-default配置详情

```bash
Usage: 
	nerdctl apparmor load
```

### nerdctl apparmor ls - 已加载AppArmor profile列表

```bash
Usage: 
	nerdctl apparmor ls [OPTIONS]
```

### nerdctl apparmor unload - 卸载 AppArmor 配置文件

```bash
Usage: 
	nerdctl apparmor unload [PROFILE]
```

## Builder 管理

### nerdctl builder prune - 清理 BuildKit 构建缓存

```bash
Usage: 
	nerdctl builder prune

# 常用options
 --buildkit-host=<BUILDKIT_HOST>: BuildKit address
```

### :point_right:nerdctl builder debug - 使用 buildg 交互式调试 Dockerfile

[buildg](https://github.com/ktock/buildg)

[builder-debug](https://github.com/containerd/nerdctl/blob/master/docs/builder-debug.md)

```bash
Usage: 
	nerdctl builder debug PATH

# 常用options
-f, --file: Name of the Dockerfile
--image: Image to use for debugging stage
--target: Set the target build stage to build
--build-arg: Set build-time variables
```

## Shell completion

### :point_right:nerdctl completion bash - 为 bash 生成自动完成脚本

`source <(nerdctl completion bash)`

```bash
# root 用户
nerdctl completion bash > /etc/bash_completion.d/nerdctl 
```

### nerdctl completion zsh - 为 zsh生成自动完成脚本

> 自行help查询

```bash
 nerdctl completion zsh [flags]
```

### nerdctl completion fish - 为 fish生成自动完成脚本

> 自行help查询

### nerdctl completion powershell - 为 powershell 生成自动完成脚本

> 自行help查询

## Compose - 容器编排

> 可以参考docker compose

### :point_right:nerdctl compose - 编排

```bash
Usage:
	nerdctl compose [OPTIONS] [COMMAND]

# 常用options
-f, --file: Specify an alternate compose file
-p, --project-name: Specify an alternate project name
```

### :point_right:nerdctl compose up  - 创建和启动容器

```bash
Usage: nerdctl compose up [OPTIONS] [SERVICE...]

# 常用options
-d, --detach: Detached mode: Run containers in the background
--no-build: Don't build an image, even if it's missing.
--no-color: Produce monochrome output
--no-log-prefix: Don't print prefix in logs
--build: Build images before starting containers.
--ipfs: Build images with pulling base images from IPFS. See ./docs/ipfs.md for details.
--quiet-pull: Pull without printing progress information
--scale: Scale SERVICE to NUM instances. Overrides the scale setting in the Compose file if present.
--remove-orphans: Remove containers for services not defined in the Compose file
```

### :point_right:nerdctl compose logs -  查看容器日志

```bash
Usage: 
	nerdctl compose logs [OPTIONS]

# 常用options
--timestamps: Show timestamps
--tail: Number of lines to show from the end of the lo
```

### :point_right:nerdctl compose build - 创建或重新创建服务

```bash
Usage: 
	nerdctl compose build [OPTIONS]
```

### :point_right:nerdctl compose down - 移除容器和相关资源

```bash
Usage: 
	nerdctl compose down [OPTIONS]
```

### :point_right:nerdctl compose ps - 列出服务的相关容器

```bash
Usage: 
	nerdctl compose ps
```

### nerdctl compose pull - 拉取服务镜像

```bash
Usage: 
	nerdctl compose pull
```

### nerdctl compose push - 推送服务镜像

```bash
Usage: 
	nerdctl compose push
```

### :point_right:nerdctl compose config - 验证并查看 Compose 文件

```bash
Usage: 
	nerdctl compose config
```

### nerdctl compose kill - 杀死服务的容器运行

```bash
Usage: 
	nerdctl compose kill

# 常用options
-s, --signal: SIGNAL to send to the container (default: "SIGKILL")
```

### nerdctl compose run - 启动服务运行容器（一次性命令）

```bash
Usage: 
	nerdctl compose run
```

## IPFS 管理

### :point_right:nerdctl ipfs registry up - 启动由 IPFS 支持的只读本地注册表

```bash
Usage: 
	nerdctl ipfs registry up [OPTIONS]

# 常用options
--listen-registry: Address to listen (default localhost:5050)
--read-retry-num: Times to retry query on IPFS (default 0 (no retry))
--read-timeout: Timeout duration of a read request to IPFS (default 0 (no timeout))
```

### :point_right:nerdctl ipfs registry down - 停止由 IPFS 支持的只读本地注册表

```bash
Usage: 
	nerdctl ipfs registry down
```

### :point_right:nerdctl ipfs registry serve - 启动在 localhost 上提供由 IPFS 支持的只读注册表。

```bash
Usage: 
	nerdctl ipfs registry serve [OPTIONS]

# 常用options
--ipfs-address: Multiaddr of IPFS API (default is pulled from $IPFS_PATH/api file. If $IPFS_PATH env var is not present, it defaults to ~/.ipfs).
--listen-registry: Address to listen (default localhost:5050).
--read-retry-num: Times to retry query on IPFS (default 0 (no retry))
--read-timeout: Timeout duration of a read request to IPFS (default 0 (no timeout))
```

## 全局options

```bash
--address: containerd address, optionally with "unix://" prefix
-a, --host, -H: deprecated aliases of --address
--namespace: containerd namespace
-n: deprecated alias of --namespace
--snapshotter: containerd snapshotter
--storage-driver: deprecated alias of --snapshotter
--cni-path: CNI binary path (default: /opt/cni/bin) [$CNI_PATH]
--cni-netconfpath: CNI netconf path (default: /etc/cni/net.d) [$NETCONFPATH]
--data-root: nerdctl data root, e.g. "/var/lib/nerdctl"
--cgroup-manager=(cgroupfs|systemd|none): cgroup manager 
	Default: "systemd" on cgroup v2 (rootful & rootless), "cgroupfs" on v1 rootful, "none" on v1 rootless
--insecure-registry: skips verifying HTTPS certs, and allows falling back to plain HTTP
```

# 其他

 配置文件：/etc/containerd/config.toml