<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [kubectl 命令格式](#kubectl-%E5%91%BD%E4%BB%A4%E6%A0%BC%E5%BC%8F)
- [最基本命令](#%E6%9C%80%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4)
  - [:point_right:kubectl create - 从文件或标准输入创建资源](#point_rightkubectl-create---%E4%BB%8E%E6%96%87%E4%BB%B6%E6%88%96%E6%A0%87%E5%87%86%E8%BE%93%E5%85%A5%E5%88%9B%E5%BB%BA%E8%B5%84%E6%BA%90)
    - [:point_right:kubectl create clusterrole - 创建集群角色](#point_rightkubectl-create-clusterrole---%E5%88%9B%E5%BB%BA%E9%9B%86%E7%BE%A4%E8%A7%92%E8%89%B2)
    - [:point_right:kubectl create clusterrolebinding - 集群角色绑定](#point_rightkubectl-create-clusterrolebinding---%E9%9B%86%E7%BE%A4%E8%A7%92%E8%89%B2%E7%BB%91%E5%AE%9A)
    - [:point_right:kubectl create configmap - 根据文件、目录或指定的文字值创建配置configmap](#point_rightkubectl-create-configmap---%E6%A0%B9%E6%8D%AE%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E6%88%96%E6%8C%87%E5%AE%9A%E7%9A%84%E6%96%87%E5%AD%97%E5%80%BC%E5%88%9B%E5%BB%BA%E9%85%8D%E7%BD%AEconfigmap)
    - [kubectl create cronjob - 创建cronjob](#kubectl-create-cronjob---%E5%88%9B%E5%BB%BAcronjob)
    - [kubectl create deployment - 创建deployment](#kubectl-create-deployment---%E5%88%9B%E5%BB%BAdeployment)
    - [kubectl create ingress - 创建ingress](#kubectl-create-ingress---%E5%88%9B%E5%BB%BAingress)
    - [kubectl create job - 创建job](#kubectl-create-job---%E5%88%9B%E5%BB%BAjob)
    - [:point_right:kubectl create namespace - 创建namespace](#point_rightkubectl-create-namespace---%E5%88%9B%E5%BB%BAnamespace)
    - [kubectl create poddisruptionbudget - 使用指定的名称、选择器创建 pod](#kubectl-create-poddisruptionbudget---%E4%BD%BF%E7%94%A8%E6%8C%87%E5%AE%9A%E7%9A%84%E5%90%8D%E7%A7%B0%E9%80%89%E6%8B%A9%E5%99%A8%E5%88%9B%E5%BB%BA-pod)
    - [:point_right:kubectl create priorityclass - 创建优先级](#point_rightkubectl-create-priorityclass---%E5%88%9B%E5%BB%BA%E4%BC%98%E5%85%88%E7%BA%A7)
    - [:point_right:kubectl create quota - 创建资源配额](#point_rightkubectl-create-quota---%E5%88%9B%E5%BB%BA%E8%B5%84%E6%BA%90%E9%85%8D%E9%A2%9D)
    - [:point_right:kubectl create role - 使用单一规则创建角色](#point_rightkubectl-create-role---%E4%BD%BF%E7%94%A8%E5%8D%95%E4%B8%80%E8%A7%84%E5%88%99%E5%88%9B%E5%BB%BA%E8%A7%92%E8%89%B2)
    - [:point_right:kubectl create rolebinding - 角色绑定](#point_rightkubectl-create-rolebinding---%E8%A7%92%E8%89%B2%E7%BB%91%E5%AE%9A)
    - [:point_right:kubectl create secret - 创建secret](#point_rightkubectl-create-secret---%E5%88%9B%E5%BB%BAsecret)
    - [kubectl create secret docker-registry - 创建docker-registry](#kubectl-create-secret-docker-registry---%E5%88%9B%E5%BB%BAdocker-registry)
    - [kubectl create secret generic - 根据文件、目录或指定的文字值创建secret](#kubectl-create-secret-generic---%E6%A0%B9%E6%8D%AE%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95%E6%88%96%E6%8C%87%E5%AE%9A%E7%9A%84%E6%96%87%E5%AD%97%E5%80%BC%E5%88%9B%E5%BB%BAsecret)
    - [kubectl create secret tls - 从给定的公钥/私钥对创建 secret TLS](#kubectl-create-secret-tls---%E4%BB%8E%E7%BB%99%E5%AE%9A%E7%9A%84%E5%85%AC%E9%92%A5%E7%A7%81%E9%92%A5%E5%AF%B9%E5%88%9B%E5%BB%BA-secret-tls)
    - [:point_right:kubectl create service - 创建service](#point_rightkubectl-create-service---%E5%88%9B%E5%BB%BAservice)
    - [:point_right:kubectl create service clusterip - 创建clusterip服务](#point_rightkubectl-create-service-clusterip---%E5%88%9B%E5%BB%BAclusterip%E6%9C%8D%E5%8A%A1)
    - [kubectl create service externalname - 创建指定名称服务](#kubectl-create-service-externalname---%E5%88%9B%E5%BB%BA%E6%8C%87%E5%AE%9A%E5%90%8D%E7%A7%B0%E6%9C%8D%E5%8A%A1)
    - [:point_right:kubectl create service loadbalancer - 创建loadbalancer 服务](#point_rightkubectl-create-service-loadbalancer---%E5%88%9B%E5%BB%BAloadbalancer-%E6%9C%8D%E5%8A%A1)
    - [:point_right:kubectl create service nodeport - 创建nodeport服务](#point_rightkubectl-create-service-nodeport---%E5%88%9B%E5%BB%BAnodeport%E6%9C%8D%E5%8A%A1)
    - [:point_right:kubectl create serviceaccount -创建具有指定名称的服务帐户](#point_rightkubectl-create-serviceaccount--%E5%88%9B%E5%BB%BA%E5%85%B7%E6%9C%89%E6%8C%87%E5%AE%9A%E5%90%8D%E7%A7%B0%E7%9A%84%E6%9C%8D%E5%8A%A1%E5%B8%90%E6%88%B7)
    - [:point_right:kubectl create token - 创建请求token](#point_rightkubectl-create-token---%E5%88%9B%E5%BB%BA%E8%AF%B7%E6%B1%82token)
  - [:point_right:kubectl get - 显示一个或多个资源](#point_rightkubectl-get---%E6%98%BE%E7%A4%BA%E4%B8%80%E4%B8%AA%E6%88%96%E5%A4%9A%E4%B8%AA%E8%B5%84%E6%BA%90)
    - [:point_right:jsonpath 表达式](#point_rightjsonpath-%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  - [kubectl run - 创建pod运行镜像](#kubectl-run---%E5%88%9B%E5%BB%BApod%E8%BF%90%E8%A1%8C%E9%95%9C%E5%83%8F)
  - [:point_right:kubectl expose - 将资源公开为k8s服务](#point_rightkubectl-expose---%E5%B0%86%E8%B5%84%E6%BA%90%E5%85%AC%E5%BC%80%E4%B8%BAk8s%E6%9C%8D%E5%8A%A1)
  - [:point_right:kubectl delete - 删除资源](#point_rightkubectl-delete---%E5%88%A0%E9%99%A4%E8%B5%84%E6%BA%90)
- [app管理](#app%E7%AE%A1%E7%90%86)
  - [:point_right:kubectl apply - 将配置应用于资源](#point_rightkubectl-apply---%E5%B0%86%E9%85%8D%E7%BD%AE%E5%BA%94%E7%94%A8%E4%BA%8E%E8%B5%84%E6%BA%90)
    - [kubectl apply edit-last-applied - 从默认编辑器配置资源](#kubectl-apply-edit-last-applied---%E4%BB%8E%E9%BB%98%E8%AE%A4%E7%BC%96%E8%BE%91%E5%99%A8%E9%85%8D%E7%BD%AE%E8%B5%84%E6%BA%90)
    - [kubectl apply set-last-applied - 通过文件更新配置](#kubectl-apply-set-last-applied---%E9%80%9A%E8%BF%87%E6%96%87%E4%BB%B6%E6%9B%B4%E6%96%B0%E9%85%8D%E7%BD%AE)
    - [kubectl apply view-last-applied - 查看最新的配置注释](#kubectl-apply-view-last-applied---%E6%9F%A5%E7%9C%8B%E6%9C%80%E6%96%B0%E7%9A%84%E9%85%8D%E7%BD%AE%E6%B3%A8%E9%87%8A)
  - [:point_right:kubectl annotate - 更新注解](#point_rightkubectl-annotate---%E6%9B%B4%E6%96%B0%E6%B3%A8%E8%A7%A3)
  - [kubectl autoscale - 为资源自动扩缩副本](#kubectl-autoscale---%E4%B8%BA%E8%B5%84%E6%BA%90%E8%87%AA%E5%8A%A8%E6%89%A9%E7%BC%A9%E5%89%AF%E6%9C%AC)
  - [kubectl debug - debug调试容器资源](#kubectl-debug---debug%E8%B0%83%E8%AF%95%E5%AE%B9%E5%99%A8%E8%B5%84%E6%BA%90)
  - [:point_right:kubectl diff - 对比pod与yaml的差异](#point_rightkubectl-diff---%E5%AF%B9%E6%AF%94pod%E4%B8%8Eyaml%E7%9A%84%E5%B7%AE%E5%BC%82)
  - [:point_right:kubectl edit - 修改编辑资源](#point_rightkubectl-edit---%E4%BF%AE%E6%94%B9%E7%BC%96%E8%BE%91%E8%B5%84%E6%BA%90)
  - [kubectl kustomize - 定制k8s配置](#kubectl-kustomize---%E5%AE%9A%E5%88%B6k8s%E9%85%8D%E7%BD%AE)
  - [:point_right:kubectl label - 修改资源label](#point_rightkubectl-label---%E4%BF%AE%E6%94%B9%E8%B5%84%E6%BA%90label)
  - [:point_right:kubectl patch - 修改资源](#point_rightkubectl-patch---%E4%BF%AE%E6%94%B9%E8%B5%84%E6%BA%90)
  - [:point_right:kubectl replace - 替换资源](#point_rightkubectl-replace---%E6%9B%BF%E6%8D%A2%E8%B5%84%E6%BA%90)
  - [kubectl rollout - 版本控制](#kubectl-rollout---%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)
    - [:point_right:kubectl rollout history - 查看历史版本](#point_rightkubectl-rollout-history---%E6%9F%A5%E7%9C%8B%E5%8E%86%E5%8F%B2%E7%89%88%E6%9C%AC)
    - [:point_right:kubectl rollout pause - 将资源标记为暂停](#point_rightkubectl-rollout-pause---%E5%B0%86%E8%B5%84%E6%BA%90%E6%A0%87%E8%AE%B0%E4%B8%BA%E6%9A%82%E5%81%9C)
    - [:point_right:kubectl rollout restart - 重启资源](#point_rightkubectl-rollout-restart---%E9%87%8D%E5%90%AF%E8%B5%84%E6%BA%90)
    - [:point_right:kubectl rollout resume - 恢复暂停的资源](#point_rightkubectl-rollout-resume---%E6%81%A2%E5%A4%8D%E6%9A%82%E5%81%9C%E7%9A%84%E8%B5%84%E6%BA%90)
    - [:point_right:kubectl rollout status - 显示资源状态](#point_rightkubectl-rollout-status---%E6%98%BE%E7%A4%BA%E8%B5%84%E6%BA%90%E7%8A%B6%E6%80%81)
    - [:point_right:kubectl rollout undo - 回退到指定版本](#point_rightkubectl-rollout-undo---%E5%9B%9E%E9%80%80%E5%88%B0%E6%8C%87%E5%AE%9A%E7%89%88%E6%9C%AC)
  - [:point_right:kubectl scale - 设置资源副本数](#point_rightkubectl-scale---%E8%AE%BE%E7%BD%AE%E8%B5%84%E6%BA%90%E5%89%AF%E6%9C%AC%E6%95%B0)
  - [kubectl set - 设置资源](#kubectl-set---%E8%AE%BE%E7%BD%AE%E8%B5%84%E6%BA%90)
    - [:point_right:kubectl set env - 更新pod上的环境变量](#point_rightkubectl-set-env---%E6%9B%B4%E6%96%B0pod%E4%B8%8A%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
    - [:point_right:kubectl set image - 更新资源的现有容器镜像](#point_rightkubectl-set-image---%E6%9B%B4%E6%96%B0%E8%B5%84%E6%BA%90%E7%9A%84%E7%8E%B0%E6%9C%89%E5%AE%B9%E5%99%A8%E9%95%9C%E5%83%8F)
    - [:point_right:kubectl set resources - 更新resources (CPU、内存)](#point_rightkubectl-set-resources---%E6%9B%B4%E6%96%B0resources-cpu%E5%86%85%E5%AD%98)
    - [kubectl set selector - 在资源上设置选择器](#kubectl-set-selector---%E5%9C%A8%E8%B5%84%E6%BA%90%E4%B8%8A%E8%AE%BE%E7%BD%AE%E9%80%89%E6%8B%A9%E5%99%A8)
    - [:point_right:kubectl set serviceaccount - 更新pod模板serviceaccount](#point_rightkubectl-set-serviceaccount---%E6%9B%B4%E6%96%B0pod%E6%A8%A1%E6%9D%BFserviceaccount)
    - [:point_right:kubectl set subject - 更新角色绑定](#point_rightkubectl-set-subject---%E6%9B%B4%E6%96%B0%E8%A7%92%E8%89%B2%E7%BB%91%E5%AE%9A)
  - [kubect wait - 等待达到某种状态](#kubect-wait---%E7%AD%89%E5%BE%85%E8%BE%BE%E5%88%B0%E6%9F%90%E7%A7%8D%E7%8A%B6%E6%80%81)
- [app使用](#app%E4%BD%BF%E7%94%A8)
  - [kubectl attach - 连接到现有容器中运行的进程。](#kubectl-attach---%E8%BF%9E%E6%8E%A5%E5%88%B0%E7%8E%B0%E6%9C%89%E5%AE%B9%E5%99%A8%E4%B8%AD%E8%BF%90%E8%A1%8C%E7%9A%84%E8%BF%9B%E7%A8%8B)
  - [kubectl auth - 检查授权](#kubectl-auth---%E6%A3%80%E6%9F%A5%E6%8E%88%E6%9D%83)
    - [:point_right:kubectl auth can-i - 检查是否允许操作](#point_rightkubectl-auth-can-i---%E6%A3%80%E6%9F%A5%E6%98%AF%E5%90%A6%E5%85%81%E8%AE%B8%E6%93%8D%E4%BD%9C)
    - [:point_right:kubectl auth reconcile - 绑定对象的规则](#point_rightkubectl-auth-reconcile---%E7%BB%91%E5%AE%9A%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%A7%84%E5%88%99)
  - [:point_right:kubectl cp - 复制文件](#point_rightkubectl-cp---%E5%A4%8D%E5%88%B6%E6%96%87%E4%BB%B6)
  - [:point_right:kubectl describe - 显示资源详细信息](#point_rightkubectl-describe---%E6%98%BE%E7%A4%BA%E8%B5%84%E6%BA%90%E8%AF%A6%E7%BB%86%E4%BF%A1%E6%81%AF)
  - [:point_right:kubectl exec - 在容器中执行命令](#point_rightkubectl-exec---%E5%9C%A8%E5%AE%B9%E5%99%A8%E4%B8%AD%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
  - [:point_right:kubectl logs - 查看容器日志](#point_rightkubectl-logs---%E6%9F%A5%E7%9C%8B%E5%AE%B9%E5%99%A8%E6%97%A5%E5%BF%97)
  - [:point_right:kubectl port-forward -  将本地端口转发到pod](#point_rightkubectl-port-forward----%E5%B0%86%E6%9C%AC%E5%9C%B0%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91%E5%88%B0pod)
  - [:point_right:kubectl proxy - local代码k8s API](#point_rightkubectl-proxy---local%E4%BB%A3%E7%A0%81k8s-api)
  - [kubectl top - 显示资源（CPU/内存）使用情况](#kubectl-top---%E6%98%BE%E7%A4%BA%E8%B5%84%E6%BA%90cpu%E5%86%85%E5%AD%98%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)
    - [:point_right:kubectl top node - 显示节点的资源（CPU/内存）使用情况](#point_rightkubectl-top-node---%E6%98%BE%E7%A4%BA%E8%8A%82%E7%82%B9%E7%9A%84%E8%B5%84%E6%BA%90cpu%E5%86%85%E5%AD%98%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)
    - [:point_right:kubectl top pod - 显示pod的资源（CPU/内存）使用情况](#point_rightkubectl-top-pod---%E6%98%BE%E7%A4%BApod%E7%9A%84%E8%B5%84%E6%BA%90cpu%E5%86%85%E5%AD%98%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)
- [集群管理](#%E9%9B%86%E7%BE%A4%E7%AE%A1%E7%90%86)
  - [:point_right:kubectl api-versions - 集群可用的api版本](#point_rightkubectl-api-versions---%E9%9B%86%E7%BE%A4%E5%8F%AF%E7%94%A8%E7%9A%84api%E7%89%88%E6%9C%AC)
  - [kubectl certificate - 修改证书资源](#kubectl-certificate---%E4%BF%AE%E6%94%B9%E8%AF%81%E4%B9%A6%E8%B5%84%E6%BA%90)
    - [:point_right: kubectl certificate approve - 批准证书签名请求](#point_right-kubectl-certificate-approve---%E6%89%B9%E5%87%86%E8%AF%81%E4%B9%A6%E7%AD%BE%E5%90%8D%E8%AF%B7%E6%B1%82)
    - [:point_right: kubectl certificate deny - 拒绝证书签名请求](#point_right-kubectl-certificate-deny---%E6%8B%92%E7%BB%9D%E8%AF%81%E4%B9%A6%E7%AD%BE%E5%90%8D%E8%AF%B7%E6%B1%82)
  - [:point_right:kubectl cluster-info - 集群信息](#point_rightkubectl-cluster-info---%E9%9B%86%E7%BE%A4%E4%BF%A1%E6%81%AF)
    - [:point_right:kubectl cluster-info dump - 导出集群信息](#point_rightkubectl-cluster-info-dump---%E5%AF%BC%E5%87%BA%E9%9B%86%E7%BE%A4%E4%BF%A1%E6%81%AF)
  - [:point_right:kubectl cordon - 将节点置为不可调度](#point_rightkubectl-cordon---%E5%B0%86%E8%8A%82%E7%82%B9%E7%BD%AE%E4%B8%BA%E4%B8%8D%E5%8F%AF%E8%B0%83%E5%BA%A6)
  - [:point_right:kubectl uncordon - 将节点置为可调度](#point_rightkubectl-uncordon---%E5%B0%86%E8%8A%82%E7%82%B9%E7%BD%AE%E4%B8%BA%E5%8F%AF%E8%B0%83%E5%BA%A6)
  - [:point_right:kubectl drain - 安全清空node](#point_rightkubectl-drain---%E5%AE%89%E5%85%A8%E6%B8%85%E7%A9%BAnode)
  - [:point_right:kubectl taint - 更新节点上的污点。](#point_rightkubectl-taint---%E6%9B%B4%E6%96%B0%E8%8A%82%E7%82%B9%E4%B8%8A%E7%9A%84%E6%B1%A1%E7%82%B9)
- [设置和使用](#%E8%AE%BE%E7%BD%AE%E5%92%8C%E4%BD%BF%E7%94%A8)
  - [kubectl alpha - alpha 功能](#kubectl-alpha---alpha-%E5%8A%9F%E8%83%BD)
    - [kubectl alpha events - 展示事件](#kubectl-alpha-events---%E5%B1%95%E7%A4%BA%E4%BA%8B%E4%BB%B6)
  - [:point_right:kubectl api-resources - 查看可用的API](#point_rightkubectl-api-resources---%E6%9F%A5%E7%9C%8B%E5%8F%AF%E7%94%A8%E7%9A%84api)
  - [:point_right:kubectl completion - 命令自动补全](#point_rightkubectl-completion---%E5%91%BD%E4%BB%A4%E8%87%AA%E5%8A%A8%E8%A1%A5%E5%85%A8)
  - [:point_right:kubectl config - 修改 kubeconfig 文件](#point_rightkubectl-config---%E4%BF%AE%E6%94%B9-kubeconfig-%E6%96%87%E4%BB%B6)
    - [kubectl config current-context - 展示current-context](#kubectl-config-current-context---%E5%B1%95%E7%A4%BAcurrent-context)
    - [kubectl config delete-cluster - 从 kubeconfig 中删除指定的集群](#kubectl-config-delete-cluster---%E4%BB%8E-kubeconfig-%E4%B8%AD%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E7%9A%84%E9%9B%86%E7%BE%A4)
    - [kubectl config delete-context - 从 kubeconfig 中删除指定的context](#kubectl-config-delete-context---%E4%BB%8E-kubeconfig-%E4%B8%AD%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E7%9A%84context)
    - [kubectl config delete-user - 从 kubeconfig 中删除指定的user](#kubectl-config-delete-user---%E4%BB%8E-kubeconfig-%E4%B8%AD%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E7%9A%84user)
    - [kubectl config get-clusters - 显示 kubeconfig 中定义的集群](#kubectl-config-get-clusters---%E6%98%BE%E7%A4%BA-kubeconfig-%E4%B8%AD%E5%AE%9A%E4%B9%89%E7%9A%84%E9%9B%86%E7%BE%A4)
    - [kubectl config get-contexts - 显示 kubeconfig 文件中的一个或多个context](#kubectl-config-get-contexts---%E6%98%BE%E7%A4%BA-kubeconfig-%E6%96%87%E4%BB%B6%E4%B8%AD%E7%9A%84%E4%B8%80%E4%B8%AA%E6%88%96%E5%A4%9A%E4%B8%AAcontext)
    - [kubectl config get-users - 显示 kubeconfig 中定义的用户](#kubectl-config-get-users---%E6%98%BE%E7%A4%BA-kubeconfig-%E4%B8%AD%E5%AE%9A%E4%B9%89%E7%9A%84%E7%94%A8%E6%88%B7)
    - [kubectl config rename-context - 从 kubeconfig 文件重命名context](#kubectl-config-rename-context---%E4%BB%8E-kubeconfig-%E6%96%87%E4%BB%B6%E9%87%8D%E5%91%BD%E5%90%8Dcontext)
    - [kubectl config set - 在 kubeconfig 文件中设置单个值](#kubectl-config-set---%E5%9C%A8-kubeconfig-%E6%96%87%E4%BB%B6%E4%B8%AD%E8%AE%BE%E7%BD%AE%E5%8D%95%E4%B8%AA%E5%80%BC)
    - [kubectl config set-cluster - 在 kubeconfig 中设置集群](#kubectl-config-set-cluster---%E5%9C%A8-kubeconfig-%E4%B8%AD%E8%AE%BE%E7%BD%AE%E9%9B%86%E7%BE%A4)
    - [kubectl config set-context - 在 kubeconfig 中设置context](#kubectl-config-set-context---%E5%9C%A8-kubeconfig-%E4%B8%AD%E8%AE%BE%E7%BD%AEcontext)
    - [kubectl config set-credentials 在 kubeconfig 中设置用户](#kubectl-config-set-credentials-%E5%9C%A8-kubeconfig-%E4%B8%AD%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7)
    - [kubectl config unset - 在 kubeconfig 文件中取消设置单个值](#kubectl-config-unset---%E5%9C%A8-kubeconfig-%E6%96%87%E4%BB%B6%E4%B8%AD%E5%8F%96%E6%B6%88%E8%AE%BE%E7%BD%AE%E5%8D%95%E4%B8%AA%E5%80%BC)
    - [kubectl config use-context - 在 kubeconfig 文件中设置use-context](#kubectl-config-use-context---%E5%9C%A8-kubeconfig-%E6%96%87%E4%BB%B6%E4%B8%AD%E8%AE%BE%E7%BD%AEuse-context)
    - [kubectl config view - 显示 kubeconfig 文件](#kubectl-config-view---%E6%98%BE%E7%A4%BA-kubeconfig-%E6%96%87%E4%BB%B6)
  - [:point_right:kubectl explain - 列出支持资源的字段](#point_rightkubectl-explain---%E5%88%97%E5%87%BA%E6%94%AF%E6%8C%81%E8%B5%84%E6%BA%90%E7%9A%84%E5%AD%97%E6%AE%B5)
  - [:point_right:kubectl options - 打印所有命令继承的标志列表](#point_rightkubectl-options---%E6%89%93%E5%8D%B0%E6%89%80%E6%9C%89%E5%91%BD%E4%BB%A4%E7%BB%A7%E6%89%BF%E7%9A%84%E6%A0%87%E5%BF%97%E5%88%97%E8%A1%A8)
  - [:point_right:kubectl plugin -  插件](#point_rightkubectl-plugin----%E6%8F%92%E4%BB%B6)
    - [kubectl plugin list - 列出用户 PATH 上所有可用的插件](#kubectl-plugin-list---%E5%88%97%E5%87%BA%E7%94%A8%E6%88%B7-path-%E4%B8%8A%E6%89%80%E6%9C%89%E5%8F%AF%E7%94%A8%E7%9A%84%E6%8F%92%E4%BB%B6)
  - [:point_right:kubectl  version - 版本信息](#point_rightkubectl--version---%E7%89%88%E6%9C%AC%E4%BF%A1%E6%81%AF)
- [kubectl 配置文件](#kubectl-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# kubectl 命令格式

kubectl 命令的基本格式是 `kubectl <action> <resource>`，其中 `action` 可以是 `create`, `delete`, `get` 等等，`resource` 你可以使用 `kubectl api-resources` 获得完整列表。

例如，你可以通过 `kubectl get nodes` 获取节点信息，可以通过 `kubectl describe nodes ${NODENAME}` 获取节点详细信息。

[官方命令详解](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-)

# 最基本命令

## :point_right:kubectl create - 从文件或标准输入创建资源

```bash
kubectl create -f FILENAME
```

> 推荐使用yaml，如果手动 create 具体的 resource 请查阅[create](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create)

### :point_right:kubectl create clusterrole - 创建集群角色

```bash
kubectl create clusterrole NAME --verb=verb --resource=resource.group [--resource-name=resourcename] [--dry-run=server|client|none]

# 例子
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods
```

### :point_right:kubectl create clusterrolebinding - 集群角色绑定

```bash
kubectl create clusterrolebinding NAME --clusterrole=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]

# 例子
kubectl create clusterrolebinding cluster-admin --clusterrole=cluster-admin --user=user1 --user=user2 --group=group1
```

### :point_right:kubectl create configmap - 根据文件、目录或指定的文字值创建配置configmap

```bash
kubectl create configmap NAME [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]

# 例子
kubectl create configmap my-config --from-file=path/to/bar
kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt
kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

### kubectl create cronjob - 创建cronjob

```bash
kubectl create cronjob NAME --image=image --schedule='0/5 * * * ?' -- [COMMAND] [args...]

kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *"
```

### kubectl create deployment - 创建deployment

```bash
kubectl create deployment NAME --image=image -- [COMMAND] [args...]

# 例子
kubectl create deployment my-dep --image=busybox
```

### kubectl create ingress - 创建ingress 

```bash
kubectl create ingress NAME --rule=host/path=service:port[,tls[=secret]]

# 例子
kubectl create ingress simple --rule="foo.com/bar=svc1:8080,tls=my-cert"
```

### kubectl create job - 创建job

```bash
kubectl create job NAME --image=image [--from=cronjob/name] -- [COMMAND] [args...]

# 例子
kubectl create job my-job --image=busybox
```

### :point_right:kubectl create namespace - 创建namespace 

```bash
kubectl create namespace NAME [--dry-run=server|client|none]

# 例子
kubectl create namespace stonebird
```

### kubectl create poddisruptionbudget - 使用指定的名称、选择器创建 pod

```bash
kubectl create poddisruptionbudget NAME --selector=SELECTOR --min-available=N [--dry-run=server|client|none]

# 例子
kubectl create poddisruptionbudget my-pdb --selector=app=rails --min-available=1
kubectl create pdb my-pdb --selector=app=nginx --min-available=50%
```

### :point_right:kubectl create priorityclass - 创建优先级

> 具有指定名称、值、globalDefault 和描述的

```bash
kubectl create priorityclass NAME --value=VALUE --global-default=BOOL [--dry-run=server|client|none]

# 例子
kubectl create priorityclass high-priority --value=1000 --description="high priority"
kubectl create priorityclass default-priority --value=1000 --global-default=true --description="default priority"
```

### :point_right:kubectl create quota - 创建资源配额

> 具有指定名称、硬限制和可选范围的

```bash
kubectl create quota NAME [--hard=key1=value1,key2=value2] [--scopes=Scope1,Scope2] [--dry-run=server|client|none]

# 例子
kubectl create quota my-quota --hard=cpu=1,memory=1G,pods=2,services=3,replicationcontrollers=2,resourcequotas=1,secrets=5,persistentvolumeclaims=10
```

### :point_right:kubectl create role - 使用单一规则创建角色

```bash
kubectl create role NAME --verb=verb --resource=resource.group/subresource [--resource-name=resourcename] [--dry-run=server|client|none]

# 例子
kubectl create role foo --verb=get,list,watch --resource=rs.extensions
```

### :point_right:kubectl create rolebinding - 角色绑定

```bash
kubectl create rolebinding NAME --clusterrole=NAME|--role=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]

# 例子
kubectl create rolebinding admin --clusterrole=admin --user=user1 --user=user2 --group=group1
Create a role binding for a particular role or cluster role
```

### :point_right:kubectl create secret - 创建secret 

```bash
kubectl create secret

# 例子
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
kubectl create secret docker-registry my-secret --from-file=.dockerconfigjson=path/to/.docker/config.json
```

### kubectl create secret docker-registry - 创建docker-registry

> 用于pod拉取镜像

```bash
kubectl create docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [--from-file=[key=]source] [--dry-run=server|client|none]

# 例子
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```

### kubectl create secret generic - 根据文件、目录或指定的文字值创建secret 

```bash
kubectl create generic NAME [--type=string] [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]

# 例子
kubectl create secret generic my-secret --from-file=path/to/bar
```

### kubectl create secret tls - 从给定的公钥/私钥对创建 secret TLS

```bash
kubectl create tls NAME --cert=path/to/cert/file --key=path/to/key/file [--dry-run=server|client|none]

# 例子
kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls.key
```

### :point_right:kubectl create service - 创建service

```bash
kubectl create service
```

### :point_right:kubectl create service clusterip - 创建clusterip服务

```bash
kubectl create clusterip NAME [--tcp=<port>:<targetPort>] [--dry-run=server|client|none]

# 例子
kubectl create service clusterip my-cs --tcp=5678:8080
# handless 无头服务
kubectl create service clusterip my-cs --clusterip="None"
```

### kubectl create service externalname - 创建指定名称服务

```bash
kubectl create externalname NAME --external-name external.name [--dry-run=server|client|none]

# 例子
kubectl create service externalname my-ns --external-name bar.com
```

### :point_right:kubectl create service loadbalancer - 创建loadbalancer 服务

```bash
kubectl create loadbalancer NAME [--tcp=port:targetPort] [--dry-run=server|client|none]

# 例子
kubectl create service loadbalancer my-lbs --tcp=5678:8080
```

### :point_right:kubectl create service nodeport - 创建nodeport服务

```bash
kubectl create nodeport NAME [--tcp=port:targetPort] [--dry-run=server|client|none]

# 例子
kubectl create service nodeport my-ns --tcp=5678:8080
```

### :point_right:kubectl create serviceaccount -创建具有指定名称的服务帐户

```bash
kubectl create serviceaccount NAME [--dry-run=server|client|none]

# 例子
kubectl create serviceaccount my-service-account
```

### :point_right:kubectl create token - 创建请求token

```bash
kubectl create token SERVICE_ACCOUNT_NAME

# 例子
kubectl create token myapp
kubectl create token myapp --namespace myns
kubectl create token myapp --duration 10m
```







## :point_right:kubectl get - 显示一个或多个资源

```bash
kubectl get [(-o|--output=)json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file|custom-columns|custom-columns-file|wide] (TYPE[.VERSION][.GROUP] [NAME | -l label] | TYPE[.VERSION][.GROUP]/NAME ...) [flags]

# 例子
# --raw k8s-api
kubectl get --raw=/api/v1/nodes/11.185.19.215/proxy/metrics/cadvisor

# yaml 转json
kubectl get -f pod.yaml -o json

# --wath 循环监听
kubectl get node -A --watch
```

### :point_right:[jsonpath 表达式](https://kubernetes.io/zh-cn/docs/reference/kubectl/jsonpath/)

| 函数                | 描述                 | 示例                                                         | 结果                                              |
| ------------------- | -------------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| `text`              | 纯文本               | `kind is {.kind}`                                            | `kind is List`                                    |
| `@`                 | 当前对象             | `{@}`                                                        | 与输入相同                                        |
| `.` or `[]`         | 子运算符             | `{.kind}`, `{['kind']}` or `{['name\.type']}`                | `List`                                            |
| `..`                | 递归下降             | `{..name}`                                                   | `127.0.0.1 127.0.0.2 myself e2e`                  |
| `*`                 | 通配符。获取所有对象 | `{.items[*].metadata.name}`                                  | `[127.0.0.1 127.0.0.2]`                           |
| `[start:end :step]` | 下标运算符           | `{.users[0].name}`                                           | `myself`                                          |
| `[,]`               | 并集运算符           | `{.items[*]['metadata.name', 'status.capacity']}`            | `127.0.0.1 127.0.0.2 map[cpu:4] map[cpu:8]`       |
| `?()`               | 过滤                 | `{.users[?(@.name=="e2e")].user.password}`                   | `secret`                                          |
| `range`, `end`      | 迭代列表             | `{range .items[*]}[{.metadata.name}, {.status.capacity}] {end}` | `[127.0.0.1, map[cpu:4]] [127.0.0.2, map[cpu:8]]` |
| `''`                | 引用解释执行字符串   | `{range .items[*]}{.metadata.name}{'\t'}{end}`               | `127.0.0.1 127.0.0.2`                             |

使用 `kubectl` 和 JSONPath 表达式的示例:

```shell
# 例子
kubectl get pods -o json
kubectl get pods -o=jsonpath='{@}'
kubectl get pods -o=jsonpath='{.items[0]}'
kubectl get pods -o=jsonpath='{.items[0].metadata.name}'
kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'status.capacity']}"
kubectl get pods -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.startTime}{"\n"}{end}'
```

## kubectl run - 创建pod运行镜像

```bash
kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]
```

## :point_right:kubectl expose - 将资源公开为k8s服务

> pod (po), service (svc), replicationcontroller (rc), deployment (deploy), replicaset (rs)

```bash
kubectl expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP|SCTP] [--target-port=number-or-name] [--name=name] [--external-ip=external-ip-of-service] [--type=type]

# 例子 
kubectl expose service nginx --port=443 --target-port=8443 --name=nginx-https
```

## :point_right:kubectl delete - 删除资源

> 按文件名、标准输入、资源和名称，或按资源和标签选择器删除资源

```bash
kubectl delete ([-f FILENAME] | [-k DIRECTORY] | TYPE [(NAME | -l label | --all)])

# 例子
# -f
kubectl delete -f ./pod.json

# stdin
cat pod.json | kubectl delete -f -

# -k yaml文件夹
kubectl delete -k dir

# -l 通过label删除
kubectl delete pods,services -l name=myLabel

# --force 立即删除
kubectl delete pod foo --force
```

# app管理

## :point_right:kubectl apply - 将配置应用于资源

> 通过文件名或标准输入

```bash
kubectl apply (-f FILENAME | -k DIRECTORY)
```

### kubectl apply edit-last-applied - 从默认编辑器配置资源

```bash
kubectl apply edit-last-applied (RESOURCE/NAME | -f FILENAME)

# 例子
kubectl apply edit-last-applied deployment/nginx
```

### kubectl apply set-last-applied - 通过文件更新配置

> 一般使用 kubectl apply -f

```bash
kubectl apply set-last-applied -f FILENAME

# 例子
kubectl apply set-last-applied -f deploy.yaml
kubectl apply set-last-applied -f path/
```

### kubectl apply view-last-applied - 查看最新的配置注释

```bash
kubectl apply view-last-applied (TYPE [NAME | -l label] | TYPE/NAME | -f FILENAME)

# 例子
kubectl apply view-last-applied deployment/nginx
kubectl apply view-last-applied -f deploy.yaml -o json
```

## :point_right:kubectl annotate - 更新注解

```bash
kubectl annotate [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]

# 例子
kubectl annotate --overwrite pods foo description='my frontend running nginx'
```

## kubectl autoscale - 为资源自动扩缩副本

```bash
kubectl autoscale (-f FILENAME | TYPE NAME | TYPE/NAME) [--min=MINPODS] --max=MAXPODS [--cpu-percent=CPU]

# 例子
kubectl autoscale deployment foo --min=2 --max=10 --cpu-percent=80
```

## kubectl debug - debug调试容器资源

```bash
kubectl debug (POD | TYPE[[.VERSION].GROUP]/NAME) [ -- COMMAND [args...] ]

# 例子
kubectl debug mypod -it --image=busybox --copy-to=my-debugger
```

## :point_right:kubectl diff - 对比pod与yaml的差异

```bash
 kubectl diff -f FILENAME
 
 # 例子
 cat service.yaml | kubectl diff -f -
```

## :point_right:kubectl edit - 修改编辑资源

```bash
kubectl edit (RESOURCE/NAME | -f FILENAME
```

##  kubectl kustomize - 定制k8s配置

```bash
kubectl kustomize DIR
```

## :point_right:kubectl label - 修改资源label

```bash
kubectl label [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]

# 例子
kubectl label pods foo unhealthy=true
```

## :point_right:kubectl patch - 修改资源

```bash
kubectl patch (-f FILENAME | TYPE NAME) [-p PATCH|--patch-file FILE]

# 例子
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'
```

## :point_right:kubectl replace - 替换资源

> 用文件名或标准输入  一般用apply替换

```bash
kubectl replace -f FILENAME
```

## kubectl rollout - 版本控制

### :point_right:kubectl rollout history - 查看历史版本

```bash
kubectl rollout history (TYPE NAME | TYPE/NAME) [flags]

# 例子
kubectl rollout history daemonset <daemonset-name>
```

### :point_right:kubectl rollout pause - 将资源标记为暂停

```bash
kubectl rollout pause RESOURCE

# 例子
kubectl rollout pause deployment/nginx
```

### :point_right:kubectl rollout restart - 重启资源

```bash
kubectl rollout restart RESOURCE

# 例子
kubectl rollout restart deployment/nginx
```

### :point_right:kubectl rollout resume - 恢复暂停的资源

```bash
kubectl rollout resume RESOURCE

# 例子
kubectl rollout resume deployment/nginx
```

### :point_right:kubectl rollout status - 显示资源状态

```bash
kubectl rollout status (TYPE NAME | TYPE/NAME) [flags]

# 例子
kubectl rollout status deployment/nginx
```

### :point_right:kubectl rollout undo - 回退到指定版本

```bash
kubectl rollout undo (TYPE NAME | TYPE/NAME) [flags]

# 例子
kubectl rollout undo daemonset <daemonset-name> --to-revision=<revision> 
#--to-revision 参数未指定，将选中最近的版本
```

## :point_right:kubectl scale - 设置资源副本数

> 为一个 Deployment、ReplicaSet 或 ReplicationController、statefulset设置一个新的规模尺寸值

```bash
kubectl scale [--resource-version=version] [--current-replicas=count] --replicas=COUNT (-f FILENAME | TYPE NAME)

# 例子
kubectl scale --replicas=3 statefulset/web
kubectl scale --replicas=3 rs/foo
kubectl scale --replicas=3 deployment/mysql
```

## [kubectl set](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set) - 设置资源

### :point_right:kubectl set env - 更新pod上的环境变量

```bash
kubectl set env RESOURCE/NAME KEY_1=VAL_1 ... KEY_N=VAL_N

# 例子
kubectl set env deployment/registry STORAGE_DIR=/local
```

### :point_right:kubectl set image - 更新资源的现有容器镜像

```bash
kubectl set image (-f FILENAME | TYPE NAME) CONTAINER_NAME_1=CONTAINER_IMAGE_1 ... CONTAINER_NAME_N=CONTAINER_IMAGE_N

# 例子
kubectl set image deployment/nginx busybox=busybox nginx=nginx:1.9.1
```

### :point_right:kubectl set resources - 更新resources (CPU、内存)

> 为定义 pod 模板的任何资源指定计算资源要求（CPU、内存）。 如果一个 pod 被成功调度，它可以保证请求的资源量，但可能会达到其指定的限制。

```bash
kubectl set resources (-f FILENAME | TYPE NAME) ([--limits=LIMITS & --requests=REQUESTS]

# 例子
kubectl set resources deployment nginx -c=nginx --limits=cpu=200m,memory=512Mi
kubectl set resources deployment nginx --limits=cpu=0,memory=0 --requests=cpu=0,memory=0
```

### kubectl set selector - 在资源上设置选择器

>  请注意，如果资源在调用“设置选择器”之前有一个，则新选择器将覆盖旧选择器

```bash
kubectl set selector (-f FILENAME | TYPE NAME) EXPRESSIONS [--resource-version=version]
```

### :point_right:kubectl set serviceaccount - 更新pod模板serviceaccount 

```bash
kubectl set serviceaccount (-f FILENAME | TYPE NAME) SERVICE_ACCOUNT

# 例子
kubectl set serviceaccount deployment nginx-deployment serviceaccount1
kubectl set sa -f nginx-deployment.yaml serviceaccount1 --local --dry-run=client -o yaml
```

### :point_right:kubectl set subject - 更新角色绑定

> 或集群角色绑定中的用户、组或服务帐户

```bash
kubectl set subject (-f FILENAME | TYPE NAME) [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]

# 例子
kubectl set subject clusterrolebinding admin --serviceaccount=namespace:serviceaccount1
kubectl set subject rolebinding admin --user=user1 --user=user2 --group=group1
```

## kubect wait - 等待达到某种状态

```bash
kubectl wait ([-f FILENAME] | resource.group/resource.name | resource.group [(-l label | --all)]) [--for=delete|--for condition=available|--for=jsonpath='{}'=value]

# 例子
kubectl wait --for=condition=Ready pod/busybox1
```

# app使用

## kubectl attach - 连接到现有容器中运行的进程。

```bash
kubectl attach (POD | TYPE/NAME) -c CONTAINER

# 例子
kubectl attach mypod -c ruby-container -i -t
```

## kubectl auth - 检查授权

```bash
kubectl auth
```

### :point_right:kubectl auth can-i - 检查是否允许操作

```bash
kubectl auth can-i VERB [TYPE | TYPE/NAME | NONRESOURCEURL]

# 例子
kubectl auth can-i create pods --all-namespaces
kubectl auth can-i list deployments.apps
```

### :point_right:kubectl auth reconcile - 绑定对象的规则

> 协调 RBAC 角色、角色绑定、集群角色和集群角色

```bash
kubectl auth reconcile -f FILENAME

# 例子
kubectl auth reconcile -f FILENAME
kubectl auth reconcile -f my-rbac-rules.yaml
```

## :point_right:kubectl cp - 复制文件

> 复制到容器 或者从容器复制

```bash
kubectl cp <file-spec-src> <file-spec-dest>

# 例子
kubectl cp /tmp/foo_dir <some-pod>:/tmp/bar_dir
kubectl cp /tmp/foo <some-pod>:/tmp/bar -c <specific-container>
kubectl cp <some-namespace>/<some-pod>:/tmp/foo /tmp/bar
```

## :point_right:kubectl describe - 显示资源详细信息

```bash
kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME)

# 例子
kubectl describe pods/nginx
kubectl describe po -l name=myLabel
```

## :point_right:kubectl exec - 在容器中执行命令

```bash
# 单个命令不带参数可以不带 --
kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...]

# 例子
kubectl exec mypod -c ruby-container -it bash 
kubectl exec nginx -- nginx -T
```

## :point_right:kubectl logs - 查看容器日志

```bash
kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER]

# 例子
kubectl logs -f nginx
kubectl logs --tail=20 nginx
kubectl logs --since=1h nginx
```

## :point_right:kubectl port-forward -  将本地端口转发到pod

> 一个或多个本地端口

```bash
kubectl port-forward TYPE/NAME [options] [LOCAL_PORT:]REMOTE_PORT [...[LOCAL_PORT_N:]REMOTE_PORT_N]

# 例子
# Listen on ports 5000 and 6000 locally, forwarding data to/from ports 5000 and 6000 in the pod
kubectl port-forward pod/mypod 5000 6000

# Listen on port 8888 on all addresses, forwarding to 5000 in the pod
kubectl port-forward --address 0.0.0.0 pod/mypod 8888:5000
```

## :point_right:kubectl proxy - local代码k8s API

```bash
kubectl proxy [--port=PORT] [--www=static-dir] [--www-prefix=prefix] [--api-prefix=prefix]

# 例子
# 默认8001
kubectl proxy --api-prefix=/ &
```

##  kubectl top - 显示资源（CPU/内存）使用情况

```bash
kubectl top
```

### :point_right:kubectl top node - 显示节点的资源（CPU/内存）使用情况

```bash
kubectl top node [NAME | -l label]

# 例子
kubectl top node # all node
kubectl top node NODE_NAME
```

### :point_right:kubectl top pod - 显示pod的资源（CPU/内存）使用情况

```bash
kubectl top pod [NAME | -l label]

# 例子
kubectl top pod # 默认default空间
kubectl top pod --namespace=NAMESPACE
```

# 集群管理

## :point_right:kubectl api-versions - 集群可用的api版本

```bash
kubectl api-versions
```

## kubectl certificate - 修改证书资源

 ### :point_right: kubectl certificate approve - 批准证书签名请求

```bash
kubectl certificate approve (-f FILENAME | NAME)

# 例子
kubectl certificate approve csr-sqgzp
```

### :point_right: kubectl certificate deny - 拒绝证书签名请求

```bash
kubectl certificate deny (-f FILENAME | NAME)

# 例子
kubectl certificate deny (-f FILENAME | NAME)
```

## :point_right:kubectl cluster-info - 集群信息

```bash
kubectl cluster-info
```

### :point_right:kubectl cluster-info dump - 导出集群信息

```bash
kubectl cluster-info dump

# 例子
kubectl cluster-info dump --output-directory=/path/to/cluster-state
```

## :point_right:kubectl cordon - 将节点置为不可调度

```b
kubectl cordon NODE
```

## :point_right:kubectl uncordon - 将节点置为可调度

```bash
kubectl drain NODE
```

## :point_right:kubectl drain - 安全清空node

```bash
kubectl drain NODE

# 例子
# 使用 --force，否则 drain 不会删除任何 pod。 如果一个或多个 pod 的管理资源丢失， --force 也将允许继续删除
kubectl drain foo --force
```

## :point_right:kubectl taint - 更新节点上的污点。

> 一个或多个node

```bash
kubectl taint NODE NAME KEY_1=VAL_1:TAINT_EFFECT_1 ... KEY_N=VAL_N:TAINT_EFFECT_N

# 例子
# Update node 'foo' with a taint with key 'dedicated' and value 'special-user' and effect 'NoSchedule' 
kubectl taint nodes foo dedicated=special-user:NoSchedule

# move from node 'foo' the taint with key 'dedicated' and effect 'NoSchedule' if one exists
kubectl taint nodes foo dedicated:NoSchedule-

# Remove from node 'foo' all the taints with key 'dedicated'
ubectl taint nodes foo dedicated-
```

# 设置和使用

## kubectl alpha - alpha 功能

```bash
kubectl alpha
```

### kubectl alpha events - 展示事件

> 但是我们会发现默认情况下 `kubectl get events` 并没有按照 events 发生的顺序进行排列，所以我们往往需要为其增加 `--sort-by='{.metadata.creationTimestamp}'` 参数来让其输出可以按时间进行排列。
>
> 这也是为何 Kubernetes v1.23 版本中会新增 `kubectl alpha events`

```bash
kubectl alpha events [--for TYPE/NAME] [--watch]
```

## :point_right:kubectl api-resources - 查看可用的API

```bash
kubectl api-resources
```

## :point_right:kubectl completion - 命令自动补全

```bash
kubectl completion SHELL

# 例子
# install bash-completion
source <(kubectl completion bash)
```

## :point_right:[kubectl config](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config) - 修改 kubeconfig 文件

### kubectl config current-context - 展示current-context

```bash
kubectl config current-context
```

### kubectl config delete-cluster - 从 kubeconfig 中删除指定的集群

```bash
kubectl config delete-cluster NAME

# 例子
kubectl config delete-cluster minikube
```

### kubectl config delete-context - 从 kubeconfig 中删除指定的context

```bash
kubectl config delete-context NAME

# 例子
kubectl config delete-context minikube
```

### kubectl config delete-user - 从 kubeconfig 中删除指定的user

```bash
kubectl config delete-user NAME

# 例子
kubectl config delete-user minikube
```

### kubectl config get-clusters - 显示 kubeconfig 中定义的集群

```bash
kubectl config get-clusters
```

### kubectl config get-contexts - 显示 kubeconfig 文件中的一个或多个context

```bash
kubectl config get-contexts [(-o|--output=)name)]

# 例子
# List all the contexts in your kubeconfig file
kubectl config get-contexts

# one
kubectl config get-contexts my-context
```

### kubectl config get-users - 显示 kubeconfig 中定义的用户

```bash
kubectl config get-users
```

### kubectl config rename-context - 从 kubeconfig 文件重命名context

```bash
kubectl config rename-context CONTEXT_NAME NEW_NAME
```

### kubectl config set - 在 kubeconfig 文件中设置单个值

```bash
kubectl config set PROPERTY_NAME PROPERTY_VALUE

# 例子
kubectl config set clusters.my-cluster.server https://1.2.3.4
```

### kubectl config set-cluster - 在 kubeconfig 中设置集群

```bash
kubectl config set-cluster NAME [--server=server] [--certificate-authority=path/to/certificate/authority] [--insecure-skip-tls-verify=true] [--tls-server-name=example.com]

# 例子
# Set only the server field on the e2e cluster entry without touching other values
kubectl config set-cluster e2e --server=https://1.2.3.4
```

### kubectl config set-context - 在 kubeconfig 中设置context

```bash
kubectl config set-context [NAME | --current] [--cluster=cluster_nickname] [--user=user_nickname] [--namespace=namespace]

# 例子
kubectl config set-context gce --user=cluster-admin
```

### kubectl config set-credentials 在 kubeconfig 中设置用户

```bash
kubectl config set-credentials NAME [--client-certificate=path/to/certfile] [--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user] [--password=basic_password] [--auth-provider=provider_name] [--auth-provider-arg=key=value] [--exec-command=exec_command] [--exec-api-version=exec_api_version] [--exec-arg=arg] [--exec-env=key=value]

# 例子
kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key
```

### kubectl config unset - 在 kubeconfig 文件中取消设置单个值

```bash
kubectl config unset PROPERTY_NAME

# 例子
kubectl config unset contexts.foo.namespace
```

### kubectl config use-context - 在 kubeconfig 文件中设置use-context

```bash
kubectl config use-context CONTEXT_NAME

# 例子
kubectl config use-context minikube
```

### kubectl config view - 显示 kubeconfig 文件

```bash
kubectl config view

# 例子
kubectl config view
kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'
```

## :point_right:kubectl explain - 列出支持资源的字段

```bash
kubectl explain RESOURCE

# 例子
kubectl explain pods
kubectl explain pods.spec.containers
```

## :point_right:kubectl options - 打印所有命令继承的标志列表

```bash
kubectl options
```

## :point_right:kubectl plugin -  插件

### kubectl plugin list - 列出用户 PATH 上所有可用的插件

```bash
kubectl plugin list
```

## :point_right:kubectl  version - 版本信息

```bash
kubectl version
```



# kubectl 配置文件

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

