<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [client-go](#client-go)
  - [:point_right:GVR/GVK](#point_rightgvrgvk)
    - [:point_right:有组名资源组](#point_right%E6%9C%89%E7%BB%84%E5%90%8D%E8%B5%84%E6%BA%90%E7%BB%84)
    - [:point_right:无组名资源组](#point_right%E6%97%A0%E7%BB%84%E5%90%8D%E8%B5%84%E6%BA%90%E7%BB%84)
    - [请求路径](#%E8%AF%B7%E6%B1%82%E8%B7%AF%E5%BE%84)
  - [Client-Go目录讲解](#client-go%E7%9B%AE%E5%BD%95%E8%AE%B2%E8%A7%A3)
  - [Client-Go客户端](#client-go%E5%AE%A2%E6%88%B7%E7%AB%AF)
  - [:point_right:RESTClient](#point_rightrestclient)
  - [:point_right:ClientSet](#point_rightclientset)
  - [:point_right:DynamicClient](#point_rightdynamicclient)
  - [DiscoveryClient](#discoveryclient)
  - [将 GVR 缓存到本地](#%E5%B0%86-gvr-%E7%BC%93%E5%AD%98%E5%88%B0%E6%9C%AC%E5%9C%B0)
  - [Informer 组件](#informer-%E7%BB%84%E4%BB%B6)
    - [Reflector](#reflector)
    - [DeltaFIFO](#deltafifo)
    - [indexer](#indexer)
  - [ShardInformer](#shardinformer)
  - [workQueue](#workqueue)
- [crd - 自定义资源](#crd---%E8%87%AA%E5%AE%9A%E4%B9%89%E8%B5%84%E6%BA%90)
  - [code-generator - 了解即可](#code-generator---%E4%BA%86%E8%A7%A3%E5%8D%B3%E5%8F%AF)
    - [安装工具](#%E5%AE%89%E8%A3%85%E5%B7%A5%E5%85%B7)
    - [项目目录](#%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95)
  - [controller-tools - 了解即可](#controller-tools---%E4%BA%86%E8%A7%A3%E5%8D%B3%E5%8F%AF)
    - [:point_right:controller-gen - 命令行工作cli](#point_rightcontroller-gen---%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E4%BD%9Ccli)
    - [安装工具](#%E5%AE%89%E8%A3%85%E5%B7%A5%E5%85%B7-1)
  - [Apimachinery、Api、Client-go库之间的关系](#apimachineryapiclient-go%E5%BA%93%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB)
- [kubebuilder](#kubebuilder)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [:point_right:流程](#point_right%E6%B5%81%E7%A8%8B)
    - [:point_right:init - 初始化一个项目](#point_rightinit---%E5%88%9D%E5%A7%8B%E5%8C%96%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE)
    - [:point_right:create - 创建一个API](#point_rightcreate---%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAapi)
    - [:point_right:生成crd.yaml](#point_right%E7%94%9F%E6%88%90crdyaml)
    - [:point_right:将 CRD 安装到集群中](#point_right%E5%B0%86-crd-%E5%AE%89%E8%A3%85%E5%88%B0%E9%9B%86%E7%BE%A4%E4%B8%AD)
    - [:point_right:运行controller](#point_right%E8%BF%90%E8%A1%8Ccontroller)
  - [:point_right:代码生成注释](#point_right%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A)
  - [:point_right:controller-runtime](#point_rightcontroller-runtime)
    - [Scheme 和 GVK](#scheme-%E5%92%8C-gvk)
    - [Client 与 Cache](#client-%E4%B8%8E-cache)
    - [Manager](#manager)
    - [Controller 与 Reconciler (重要的资源协调 )](#controller-%E4%B8%8E-reconciler-%E9%87%8D%E8%A6%81%E7%9A%84%E8%B5%84%E6%BA%90%E5%8D%8F%E8%B0%83-)
    - [WrokQueue 工作队列 - 后面了解](#wrokqueue-%E5%B7%A5%E4%BD%9C%E9%98%9F%E5%88%97---%E5%90%8E%E9%9D%A2%E4%BA%86%E8%A7%A3)
    - [Builder](#builder)
  - [Webhook](#webhook)
- [:point_right:operator-sdk](#point_rightoperator-sdk)
  - [安装](#%E5%AE%89%E8%A3%85-1)
  - [Ansible - 使用](#ansible---%E4%BD%BF%E7%94%A8)
    - [初始化](#%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [:point_right:go - 使用](#point_rightgo---%E4%BD%BF%E7%94%A8)
    - [:point_right:初始化项目](#point_right%E5%88%9D%E5%A7%8B%E5%8C%96%E9%A1%B9%E7%9B%AE)
    - [:point_right:创建API](#point_right%E5%88%9B%E5%BB%BAapi)
    - [:point_right:build镜像](#point_rightbuild%E9%95%9C%E5%83%8F)
    - [:point_right:部署OLM](#point_right%E9%83%A8%E7%BD%B2olm)
    - [:point_right:直接部署](#point_right%E7%9B%B4%E6%8E%A5%E9%83%A8%E7%BD%B2)
    - [:point_right:卸载](#point_right%E5%8D%B8%E8%BD%BD)
  - [:point_right:helm使用](#point_righthelm%E4%BD%BF%E7%94%A8)
    - [初始化](#%E5%88%9D%E5%A7%8B%E5%8C%96-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# client-go

Client-Go 是负责与 Kubernetes APIServer 服务进行交互的客户镇库，利用 Client-Go与 KubernetesAPISeIver 进行的交互访问，以此来对 Kubernetes 中的各类资源对象进行管理操作，包括内量的资源对象及CRD。

## :point_right:GVR/GVK

Kubernetes API 是通过 HTTP 协议以RESTful 的形式提供的，同时支持JSON 和 Protobuf 的数据格式，Protobuf 是为方便集群内部调用而支持的。我们自己平时调用 Kubernetes 接口，一般都是使用JSON 数据格式的。

在Kubernetes AP 中，我们一般使用GVR 或 GVK 来区分特定的资源，即根据不同的分组、版本及资源，进行 URL的定义，有了分组和多版本的支持，即便是后续的版本中，需要去掉资源对象的某些字段或者重构 API 资源，也可以保证版本之间的兼容性。

同时，因为历史原因，因此 Kubernetes 的 API 分组，是分为 **无组名资源组** 和 **有组名资源组** 的，无组名资源组 也被称之为核心资源组，Core Group。

GVR/GVK 含义介绍

- G (Group):资源组，包含一组资源操作的集合。
- V (Version):资源版本，用于区分不同 API 的稳定程度及兼容性
- R(Resource) :资源信息，用于区分不同的资源 API。
- K (Kind):资源对象的类型，每个资源对象都需要 Kind 来区分它自身代表的资源类型

[api文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)

### :point_right:有组名资源组

对应[api文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)中的有分组的部分，绝大多数，/apis（复数）

```bash
# apps: group
# v1 : version
# deployments: Resource
/apis/apps/v1/deployments
```

### :point_right:无组名资源组

对应[api文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)中的无组的部分。比如core，/api（单数）

```bash
# v1 : version
# pods: Resource
/api/v1/pods
```

### 请求路径

通过 GVR 组合成 RESTful API 请求路径。例如，针对apps/v1下面 Deployment 的 RESTful APl请求路径

```bash
GET /apis/apps/v1/namespaces/{namespace}/deployments/{name}
```

## Client-Go目录讲解

```bash
.client-go
├── discovery                   # 定义DsicoveryClient客户端。作用是用于发现k8s所支持GVR(Group, Version, Resources)。
├── dynamic                     # 定义DynamicClient客户端。可以用于访问k8s Resources(如: Pod, Deploy...)，也可以访问用户自定义资源(即: CRD)。
├── informers                   # k8s中各种Resources的Informer机制的实现。
├── kubernetes                  # 定义ClientSet客户端。它只能用于访问k8s Resources。每一种资源(如: Pod等)都可以看成是一个客端，而ClientSet是多个客户端的集合，它对RestClient进行了封装，引入了对Resources和Version的管
|                               # 理。通常来说ClientSet是client-gen来自动生成的。
├── listers                     # 提供对Resources的获取功能。对于Get()和List()而言，listers提供给二者的数据都是从缓存中读取的。
├── pkg
├── plugin                      # 提供第三方插件。如：GCP, OpenStack等。
├── rest                        # 定义RestClient，实现了Restful的API。同时会支持Protobuf和Json格式数据。
├── scale                       # 定义ScalClient。用于Deploy, RS, RC等的扩/缩容。
├── tools                       # 定义诸如SharedInformer、Reflector、DealtFIFO和Indexer等常用工具。实现client查询和缓存机制，减少client与api-server请求次数，减少api-server的压力。
├── transport
└── util                        # 提供诸如WorkQueue、Certificate等常用方法。
```

## Client-Go客户端

Client-Go 共提供了4种与 Kubernetes APIServer 交互的客户端对象。

- `RESTClient`：最基础的客户端，提供最基本的封装
- `Clientset`： 是一个Client的集合，在Clientset中包含了所有K8S内置资源的Client，通过Clientset便可以很方便的操作如Pod、Service这些资源
- `dynamicClient`：动态客户端，可以操作任意K8S的资源，包括CRD定义的资源
- `DiscoveryClient`: 用于发现K8S提供的资源组、资源版本和资源信息，比如:kubectl api-resources

```bash
Clientset、dynamicClient、DiscoveryClient  -> RESTClient -> net/http -> k8sAPISever
```

## :point_right:RESTClient 

RESTClient 是一个用于与 Kubernetes API 交互的高级 REST 客户端。它提供了一种直接与 Kubernetes API 进行交互的方式，可以用于执行 HTTP 请求，并处理请求和响应。RESTClient 可以用于与 Kubernetes 集群中的任何资源进行交互，包括内置资源和自定义资源。

> RESTClient  是所有Client 的父类。

```go
package main

import (
	"fmt"

	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/rest"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// 构建 Kubernetes 配置
	config, err := clientcmd.BuildConfigFromFlags("", "/path/to/kubeconfig")
	if err != nil {
		panic(err)
	}

	// 创建 Kubernetes 客户端
	client, err := kubernetes.NewForConfig(config)
	if err != nil {
		panic(err)
	}

	// 创建 RESTClient
	restClient := client.CoreV1().RESTClient()

	// 创建 HTTP 请求
	req := restClient.Get().
		Namespace("default").
		Resource("pods").
		Param("labelSelector", "app=myapp")

	// 发送请求并获取响应
	result := make(map[string]interface{})
	err = req.Do().Into(&result)
	if err != nil {
		panic(err)
	}

	// 处理响应
	items := result["items"].([]interface{})
	fmt.Printf("Pods:\n")
	for _, item := range items {
		pod := item.(map[string]interface{})
		fmt.Printf("- Name: %s, Status: %s\n", pod["metadata"].(map[string]interface{})["name"].(string), pod["status"].(map[string]interface{})["phase"].(string))
	}
}
```

## :point_right:ClientSet

ClientSet 是基于 RESTClient 的封装，同时 ClientSet 是使用预生成的 API 对象与APIServer 进行交互的，这样做更方便我们进行二次开发。

ClientSet 是一组资源对象客户端的集合，例如负责操作 Pods、Services 等资源的 CoreV1Client，负责操作 Deployments、DaemonSets 等资源的 AppsV1Client 等。通过这些资源对象客户端提供的操作方法，即可对 Kubernetes 内置的资源对象进行 Create、Update、Get、List、Delete 等操作。

```go
package main

import (
    "fmt"
    "log"
    "os"
    "time"

    v1 "k8s.io/api/core/v1"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/clientcmd"
)

func main() {
    // 从Kubeconfig获取集群配置信息
    kubeconfig := os.Getenv("HOME") + "/.kube/config"
    config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)
    if err != nil {
        log.Fatal(err)
    }

    // 创建ClientSet
    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        log.Fatal(err)
    }

    // 获取Pod列表
    pods, err := clientset.CoreV1().Pods("").List(metav1.ListOptions{})
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("There are %d pods in the cluster\n", len(pods.Items))

    // 创建Pod
    newPod := &v1.Pod{
        ObjectMeta: metav1.ObjectMeta{
            Name:      "test-pod",
            Namespace: "default",
        },
        Spec: v1.PodSpec{
            Containers: []v1.Container{
                {
                    Name:  "nginx",
                    Image: "nginx",
                },
            },
        },
    }
    createdPod, err := clientset.CoreV1().Pods("default").Create(newPod)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Created pod %q.\n", createdPod.GetObjectMeta().GetName())

    // 删除Pod
    err = clientset.CoreV1().Pods("default").Delete("test-pod", &metav1.DeleteOptions{})
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Pod deleted.")

    // 打印Node列表
    nodes, err := clientset.CoreV1().Nodes().List(metav1.ListOptions{})
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("There are %d nodes in the cluster\n", len(nodes.Items))

    // 持续监控Pod
    watch, err := clientset.CoreV1().Pods("").Watch(metav1.ListOptions{})
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Start watching pods...")
    for event := range watch.ResultChan() {
        pod, ok := event.Object.(*v1.Pod)
        if !ok {
            log.Println("Error: Unexpected type")
            continue
        }
        fmt.Printf("Event: %s, Pod: %s/%s\n", event.Type, pod.Namespace, pod.Name)
        // 停止监控
        if pod.Name == "test-pod" && event.Type == "MODIFIED" {
            fmt.Println("Stop watching.")
            watch.Stop()
        }
    }

    // 等待10秒钟
    time.Sleep(10 * time.Second)
}

```

##  :point_right:DynamicClient 

DynamicClient 是一种动态客户端，通过动态指定资源组、资源版本和资源等信息，来操作任意的Kubernetes 资源对象的一种客户端。即不仅仅是操作 Kubernetes 内置的资源对象，还可以操作CRD。这也是与 ClientSet 最明显的一个区别。
使用 ClientSet 的时候，程序会将所用的版本与类型紧密耦合。而 DynamicClient 使用嵌套的`map[string]interface{}` 结构存储 Kubernetes APIServer 的返回值，使用反射机制，在运行的时候，进行数据绑定，这种方式更加灵活，但是却无法获取强数据类型的检查和验证。
此外，我们在介绍 DynamicClient 之前，还需要了解另外两个重要的知识点，`Object.runtime` 接口和Unstructured 结构体

- **Object.runtime**: Kuberetes 中的所有资源对象，都实现了这个接口，其中包含 `DeepCopyObject`和`GetObjectKind` 的方法，分别用于对象深拷贝和获取对象的具体资源类型。
- **Unstructured**: 包含`map[string]interface{} `类型字段，在处理无法预知结构的数据时，将数据值存入`interface{}`中，待运行时利用反射判断。该结构体提供了大量的工具方法，便于处理非结构化的数据。

```go
package main

import (
	"context"
	"fmt"
	"os"

	"k8s.io/apimachinery/pkg/runtime"
	"k8s.io/apimachinery/pkg/runtime/schema"
	"k8s.io/client-go/dynamic"
	"k8s.io/client-go/dynamic/dynamicclient"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// 加载kubeconfig文件
	kubeconfig := os.Getenv("KUBECONFIG")
	config, err := clientcmd.BuildConfigFromFlags("", kubeconfig)
	if err != nil {
		panic(err)
	}

	// 创建Dynamic Client
	client, err := dynamic.NewForConfig(config)
	if err != nil {
		panic(err)
	}

	// 构建资源信息 设置GVR
	gvr := schema.GroupVersionResource{Group: "apps", Version: "v1", Resource: "deployments"}

	// 使用Dynamic Client读取Deployment资源
	unstructuredObj, err := client.Resource(gvr).Namespace("default").Get(context.Background(), "nginx-deployment", runtime.GetDefaultSerializerOptions())
	if err != nil {
		panic(err)
	}
	fmt.Printf("Deployment：%s\n", unstructuredObj)

	// 创建Deployment资源
	deployment := &unstructured.Unstructured{
		Object: map[string]interface{}{
			"apiVersion": "apps/v1",
			"kind":       "Deployment",
			"metadata": map[string]interface{}{
				"name": "test-deployment",
			},
			"spec": map[string]interface{}{
				"replicas": int64(3),
				"selector": map[string]interface{}{
					"matchLabels": map[string]interface{}{
						"app": "test",
					},
				},
				"template": map[string]interface{}{
					"metadata": map[string]interface{}{
						"labels": map[string]interface{}{
							"app": "test",
						},
					},
					"spec": map[string]interface{}{
						"containers": []map[string]interface{}{
							{
								"name":  "test-container",
								"image": "nginx",
							},
						},
					},
				},
			},
		},
	}
	newDeployment, err := client.Resource(gvr).Namespace("default").Create(context.Background(), deployment, dynamicclient.CreateOptions{})
	if err != nil {
		panic(err)
	}
	fmt.Printf("Created Deployment：%s\n", newDeployment)
}
```

## DiscoveryClient

前面3 种客户端对象，都是针对与资源对象管理的。而 DiscoveryClient 则是针对于 GVR的。

用于发现和获取 Kubernetes 中的 API 资源和 API 组信息。它可以帮助用户在运行时动态地查询和发现 Kubernetes 集群中可用的 API 资源和 API 组信息。

```bash
package main

import (
	"context"
	"fmt"
	"k8s.io/client-go/discovery"
	"k8s.io/client-go/rest"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// 1. 加载 kubeconfig 文件
	config, err := clientcmd.BuildConfigFromFlags("", "kubeconfig.yaml")
	if err != nil {
		panic(err)
	}

	// 2. 创建 DiscoveryClient
	discoveryClient, err := discovery.NewDiscoveryClientForConfig(config)
	if err != nil {
		panic(err)
	}

	// 3. 使用 DiscoveryClient 获取 API 资源
	apiResources, err := discoveryClient.ServerResources()
	if err != nil {
		panic(err)
	}

	// 4. 打印 API 资源列表
	fmt.Printf("Server API Resources:\n")
	for _, apiResource := range apiResources {
		fmt.Printf("  %s:\n", apiResource.GroupVersion)
		for _, resource := range apiResource.APIResources {
			fmt.Printf("    %s\n", resource.Name)
		}
	}
}
```

## 将 GVR 缓存到本地

前面简单介绍了一下 DiscoveryClient 的使用。通过 DiscoveryClient 与 APIServer 的交互，获得了 GVR的数据。

同时，我们可以发现，GVR 数据其实是很少变动的，因此我们可以将 GVR 的数据，缓存在本地，来减少Client 与 APIServer 交互，毕竟他们的交互还是存在网络损耗的。

在 discovery/cached 中，有另外两个客户端是来实现将 GVR 数据缓存到本地文件中和内存中的，分别是 `CachedDiscoveryClient` 和 `memCacheClient`。

其实，我们平时管理 k8s 的集群的 `kubectl` 命令也是使用的这种方式来使用我们的 GVR 与APIServer交互的。它的缓存文件默认是在 `~/.kube/cache` 中的。

以**CachedDiscoveryClient的为例**

```go
package main

import (
	"fmt"
	"time"

	"k8s.io/apimachinery/pkg/runtime/schema"
	"k8s.io/client-go/dynamic"
	"k8s.io/client-go/dynamic/discovery"
	"k8s.io/client-go/dynamic/fake"
	"k8s.io/client-go/tools/cache"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// 1. 加载 kubeconfig 配置文件
	config, err := clientcmd.BuildConfigFromFlags("", "/path/to/kubeconfig")
	if err != nil {
		panic(err)
	}

	// 2. 创建 dynamic client
	client, err := dynamic.NewForConfig(config)
	if err != nil {
		panic(err)
	}

	// 3. 创建 CachedDiscoveryClient
	dc := discovery.NewCachedDiscoveryClientForConfig(config, time.Second*10)

	// 4. 获取 GVR
	gvr, err := schema.ParseResourceArg("pods")
	if err != nil {
		panic(err)
	}

	// 5. 缓存 GVR
	if _, err := dc.ServerResourcesForGroupVersion(gvr.GroupVersion().String()); err != nil {
		panic(err)
	}

	// 6. 获取 fake clientset
	fakeClient := fake.NewSimpleDynamicClient(schema.GroupVersionResource{
		Group:    gvr.Group,
		Version:  gvr.Version,
		Resource: gvr.Resource,
	})

	// 7. 创建 Informer 并打印资源对象
	informer := cache.NewSharedIndexInformer(
		&cache.ListWatch{
			ListFunc: func(opts metav1.ListOptions) (runtime.Object, error) {
				return fakeClient.Resource(gvr).Namespace("").List(context.Background(), metav1.ListOptions{})
			},
			WatchFunc: func(opts metav1.ListOptions) (watch.Interface, error) {
				return fakeClient.Resource(gvr).Namespace("").Watch(context.Background(), metav1.ListOptions{})
			},
		},
		&unstructured.Unstructured{},
		time.Second*10,
		cache.Indexers{},
	)

	go informer.Run(context.Background().Done())

	for _, obj := range informer.GetStore().List() {
		fmt.Println(obj.(*unstructured.Unstructured).GetName())
	}
}
```

## Informer 组件

informer 是一种高级别的 API 对象观察者，用于监视 Kubernetes 中的对象（Pod、Service、Deployment 等）的创建、更新和删除事件，以及将这些事件转化为可被应用程序处理的通知。

informer 负责与 Kubernetes APServer 进行 Watch 操作，Watch 的资源，可以是 Kubernetes 内置资源对象，也可以是CRD。

Informer 是 Kubernetes 应用程序开发中常用的一种技术，它可以方便地实现 Kubernetes 中的自动扩展、自动伸缩、自动故障恢复等功能。

### Reflector

Reflector:使用 List-watch 来保证本地缓存数据的准确性、顺序性、一致性 。
List 负责调用资源对应的 Kubernetes APIServer的 RESTful API获取全局数据列表，并同步到本地缓存中。
Watch 负责监听资源的变化，并调用相应事件的处理函数进行处理，同时更新本地缓存，使本地缓存与Etcd 中数据，保持一致。
还有就是，List 是基于 HTTP 中的短链接实现。

Watch 则是基于 HTTP 长链接实现，Watch 使用长链接的方式极大的减轻了 Kubernetes APIServer 的访问压力。

```go
package main

import (
    "fmt"
    "log"
    "time"

    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/apimachinery/pkg/watch"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/rest"
    "k8s.io/client-go/tools/cache"
    "k8s.io/client-go/tools/clientcmd"
)

func main() {
    // 使用 kubeconfig 获取 kubeconfig 的 config
    config, err := clientcmd.BuildConfigFromFlags("", "kubeconfig.yaml")
    if err != nil {
        log.Fatal(err)
    }

    // 创建 kubernetes 的 clientset
    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        log.Fatal(err)
    }

    // 创建 listwatcher 用于监听 Pod 资源的变化
    lw := &cache.ListWatch{
        ListFunc: func(options metav1.ListOptions) (result metav1.Object, err error) {
            return clientset.CoreV1().Pods("default").List(options)
        },
        WatchFunc: func(options metav1.ListOptions) (watch.Interface, error) {
            return clientset.CoreV1().Pods("default").Watch(options)
        },
    }

    // 创建 pod informer
    resyncPeriod := 30 * time.Second
    podInformer := cache.NewSharedInformer(lw, &v1.Pod{}, resyncPeriod, cache.Indexers{})

    // 定义处理添加、更新、删除事件的函数
    addFunc := func(obj interface{}) {
        pod, ok := obj.(*v1.Pod)
        if !ok {
            log.Printf("object is not a Pod\n")
            return
        }
        log.Printf("Pod %s/%s added\n", pod.Namespace, pod.Name)
    }

    updateFunc := func(oldObj, newObj interface{}) {
        oldPod, ok := oldObj.(*v1.Pod)
        if !ok {
            log.Printf("old object is not a Pod\n")
            return
        }

        newPod, ok := newObj.(*v1.Pod)
        if !ok {
            log.Printf("new object is not a Pod\n")
            return
        }

        if oldPod.ResourceVersion == newPod.ResourceVersion {
            return
        }

        log.Printf("Pod %s/%s updated\n", newPod.Namespace, newPod.Name)
    }

    deleteFunc := func(obj interface{}) {
        pod, ok := obj.(*v1.Pod)
        if !ok {
            log.Printf("object is not a Pod\n")
            return
        }
        log.Printf("Pod %s/%s deleted\n", pod.Namespace, pod.Name)
    }

    // 注册事件处理函数
    podInformer.AddEventHandler(cache.ResourceEventHandlerFuncs{
        AddFunc:    addFunc,
        UpdateFunc: updateFunc,
        DeleteFunc: deleteFunc,
    })

    // 启动 informer
    stopCh := make(chan struct{})
    defer close(stopCh)
    podInformer.Run(stopCh)

    // 无限循环，等待结束
    select {}
}
```

### DeltaFIFO 

DeltaFIFO是Kubernetes informer库中的一个关键组件，它负责在内存中保存对象的FIFO队列，提供对象的增删改查操作，并支持对队列中所有对象的同步处理。

```go
package main

import (
	"fmt"
	"time"

	"k8s.io/apimachinery/pkg/apis/meta/v1/unstructured"
	"k8s.io/apimachinery/pkg/runtime/schema"
	"k8s.io/apimachinery/pkg/util/wait"
	"k8s.io/client-go/dynamic"
	"k8s.io/client-go/dynamic/fake"
	"k8s.io/client-go/tools/cache"
)

func main() {
	// Create a fake dynamic client for testing
	client := fake.NewSimpleDynamicClient(schema.GroupVersionResource{
		Group:    "example.com",
		Version:  "v1",
		Resource: "foos",
	})

	// Create a DeltaFIFO object with a key function that extracts the "metadata.name" field
	fifo := cache.NewDeltaFIFO(cache.MetaNamespaceKeyFunc, nil)
	defer fifo.ShutDown()

	// Define the informer function that will be used to process the deltas
	informerFunc := func(obj interface{}) {
		if d, ok := obj.(cache.Delta); ok {
			if d.Type == cache.Sync || d.Type == cache.Replaced {
				// This is a sync or replace delta, print the new object
				if fo, ok := d.Object.(*unstructured.Unstructured); ok {
					fmt.Printf("Sync/Replace: %v\n", fo.GetName())
				}
			} else if d.Type == cache.Added {
				// This is an add delta, print the new object
				if fo, ok := d.Object.(*unstructured.Unstructured); ok {
					fmt.Printf("Added: %v\n", fo.GetName())
				}
			} else if d.Type == cache.Deleted {
				// This is a delete delta, print the old object
				if fo, ok := d.Object.(*unstructured.Unstructured); ok {
					fmt.Printf("Deleted: %v\n", fo.GetName())
				}
			}
		}
	}

	// Create a DeltaFIFO controller with the fake client and the informer function
	controller := cache.NewDeltaFIFOController(cache.DeltaFIFOControllerConfig{
		Name:          "example",
		Client:        client,
		ResyncPeriod:  0,
		ListerWatcher: cache.NewListWatchFromClient(client, "foos", "default", nil),
		DeltaFIFO:     fifo,
		Sync:          informerFunc,
	})

	// Start the controller
	stopCh := make(chan struct{})
	defer close(stopCh)
	go controller.Run(stopCh)

	// Add an object to the client
	obj := &unstructured.Unstructured{
		Object: map[string]interface{}{
			"kind":       "Foo",
			"apiVersion": "example.com/v1",
			"metadata": map[string]interface{}{
				"name":      "foo1",
				"namespace": "default",
			},
		},
	}
	client.Resource(schema.GroupVersionResource{Group: "example.com", Version: "v1", Resource: "foos"}).Create(obj, metav1.CreateOptions{})

	// Wait for 5 seconds to allow the controller to process the delta
	time.Sleep(5 * time.Second)
}
```

### indexer

Indexer是一个接口，用于维护对象的缓存和索引。Indexer通常是以内存哈希表的形式实现的，支持通过索引快速检索和过滤对象，提高检索效率。

Indexer的主要方法包括：

- Get：根据key获取缓存的对象
- List：获取所有缓存的对象
- Add：添加对象到缓存中
- Update：更新缓存中的对象
- Delete：从缓存中删除对象

```go
package main

import (
	"fmt"
	"time"

	corev1 "k8s.io/api/core/v1"
	"k8s.io/client-go/informers"
	"k8s.io/client-go/kubernetes"
	"k8s.io/client-go/tools/cache"
	"k8s.io/client-go/tools/clientcmd"
)

func main() {
	// 1. 初始化一个 k8s client
	config, err := clientcmd.BuildConfigFromFlags("", "/path/to/kubeconfig")
	if err != nil {
		panic(err)
	}

	clientset, err := kubernetes.NewForConfig(config)
	if err != nil {
		panic(err)
	}

	// 2. 初始化一个 informerFactory
	informerFactory := informers.NewSharedInformerFactory(clientset, 0)

	// 3. 获取 pod informer 对象
	podInformer := informerFactory.Core().V1().Pods()

	// 4. 创建一个 indexer
	indexer := cache.NewIndexer(
		cache.MetaNamespaceKeyFunc,
		cache.Indexers{
			"phase": func(obj interface{}) ([]string, error) {
				pod := obj.(*corev1.Pod)
				return []string{string(pod.Status.Phase)}, nil
			},
		},
	)

	// 5. 将 indexer 绑定到 pod informer 上
	podInformer.Informer().AddEventHandler(cache.ResourceEventHandlerFuncs{
		AddFunc: func(obj interface{}) {
			indexer.Add(obj)
		},
		UpdateFunc: func(oldObj, newObj interface{}) {
			indexer.Update(newObj)
		},
		DeleteFunc: func(obj interface{}) {
			indexer.Delete(obj)
		},
	})

	// 6. 启动 pod informer
	go podInformer.Informer().Run(nil)

	// 7. 等待缓存同步完成
	if !cache.WaitForCacheSync(nil, podInformer.Informer().HasSynced) {
		panic("failed to sync informer cache")
	}

	// 8. 从 indexer 中获取 pod 列表
	pods, err := indexer.ByIndex("phase", string(corev1.PodRunning))
	if err != nil {
		panic(err)
	}

	// 9. 输出结果
	fmt.Println("Running pods:")
	for _, pod := range pods {
		p := pod.(*corev1.Pod)
		fmt.Printf("  %s/%s\n", p.Namespace, p.Name)
	}

	// 10. 等待一段时间后退出程序
	time.Sleep(time.Second * 5)
}
```

## ShardInformer

它是Informer的一种扩展

ShardInformer的解决方案是将完整的对象列表分成多个分片，并为每个分片创建一个Informer。这样，每个Informer只需要处理部分对象，就可以避免内存问题和响应时间问题。

另外，ShardInformer还提供了其他的特性，如自动重试、自动扩缩容等.

```go
import (
    "fmt"
    "k8s.io/client-go/informers"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/tools/cache"
)

func main() {
    // 创建 Kubernetes 客户端
    clientset := kubernetes.NewForConfigOrDie(config)

    // 创建 informer 工厂
    informerFactory := informers.NewSharedInformerFactory(clientset, 0)

    // 创建 ShardInformer
    shardInformer := informerFactory.Core().V1().Pods().Informer().(*cache.Controller)

    // 添加事件处理函数
    shardInformer.AddEventHandler(cache.ResourceEventHandlerFuncs{
        AddFunc: func(obj interface{}) {
            fmt.Printf("New Pod Added: %v\n", obj)
        },
        UpdateFunc: func(oldObj, newObj interface{}) {
            fmt.Printf("Pod Updated: %v -> %v\n", oldObj, newObj)
        },
        DeleteFunc: func(obj interface{}) {
            fmt.Printf("Pod Deleted: %v\n", obj)
        },
    })

    // 启动 ShardInformer
    informerFactory.Start(stopCh)
    shardInformer.Run(stopCh)
}
```

## workQueue 

WorkQueue 称为工作队列，特性有

- **有序**：按照添加顺序处理元素（item）。
- **去重**：相同元素在同一时间不会被重复处理，例如一个元素在处理之前被添加了多次，它只会被处理一次。
- **并发性**：多生产者和多消费者。
- **标记机制**：支持标记功能，标记一个元素是否被处理，也允许元素在处理时重新排队。
- **通知机制**：ShutDown 方法通过信号量通知队列不再接收新的元素，并通知 metric goroutine 退出。
- **延迟**：支持延迟队列，延迟一段时间后再将元素存入队列。
- **限速**：支持限速队列，元素存入队列时进行速率限制。限制一个元素被重新排队（Reenqueued）的次数。
- **Metric**：支持 metric 监控指标，可用于 Prometheus 监控。

# crd - 自定义资源

CRD 是 Kubernetes 中的一种扩展机制，它可以让用户自定义资源并将其注册到 Kubernetes API 中。CRD 的全称是 Custom Resource Definition，即自定义资源定义。

CRD 的使用需要以下步骤：

1. 编写 CRD 定义文件，定义自定义资源的 API 对象。
2. 使用 kubectl apply 命令将 CRD 定义文件提交到 Kubernetes 集群中。
3. 在代码中使用客户端库访问自定义资源的 API 对象

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: mycustomresource.example.com
spec:
  group: example.com
  versions:
    - name: v1alpha1
      served: true
      storage: true
  scope: Namespaced
  names:
    plural: mycustomresources
    singular: mycustomresource
    kind: MyCustomResource
    shortNames:
      - mcr
```

##  code-generator - 了解即可

是一种代码生成工具，用于为 Kubernetes API 扩展生成代码，例如自定义资源定义（CRD）。

code-generator 提供了以下工具用于为k8s中的资源生成相关代码，可以更加方便的操作自定义资源：

- `deepcopy-gen`: 生成深度拷贝方法,避免性能开销
  - 在文件中添加注释`// +k8s:deepcopy-gen=package`
  - 为单个类型添加自动生成`// +k8s:deepcopy-gen=true`
  - 为单个类型关闭自动生成`// +k8s:deepcopy-gen=false`
- `client-gen`:为资源生成标准的操作方法(get,list,create,update,patch,delete,deleteCollection,watch)
  - // +genclient - 生成默认的客户端动作函数（create, update, delete, get, list, update, patch, watch以及 是否生成updateStatus取决于.Status字段是否存在）。
  - // +genclient:nonNamespaced - 所有动作函数都是在没有名称空间的情况下生成
  - // +genclient:onlyVerbs=create,get - 指定的动作函数被生成.
  - // +genclient:skipVerbs=watch - 生成watch以外所有的动作函数.
  - // +genclient:noStatus - 即使.Status字段存在也不生成updateStatus动作函数
- `informer-gen`: 生成informer,提供事件机制来相应kubernetes的event
- `lister-gen`: 为get和list方法提供只读缓存层
- `conversion-gen`是用于自动生成在内部和外部类型之间转换的函数的工具
  - 标记转换内部软件包 `// +k8s:conversion-gen=<import-path-of-internal-package>`
  - 标记转换外部软件包`// +k8s:conversion-gen-external-types=<import-path-of-external-package>`
  - 标记不转换对应注释或结构 `// +k8s:conversion-gen=false`

- `defaulter-gen` 用于生产Defaulter函数
- `go-to-protobuf` 通过go struct生成pb idl
- `import-boss` 在给定存储库中强制执行导入限制
- `register-gen` 生成register
- `set-gen`

### 安装工具

```bash
go get k8s.io/code-generator
```

### 项目目录

```bash
$ tree
sample-controller                                   #  modulename
    ├── go.mod                                      #  go mod init 出来的文件
    ├── go.sum                                      #  go mod init 出来的检测文件
    ├── hack                                   
    │   ├── boilerplate.go.txt                      #  operator代码框架版权信息头内容
    │   ├── tools.go                                #  用于vendor自动下载code-generator文件的配置信息
    │   ├── update-codegen.sh                       #  官方sample-controller项目的模板文件，用于填写code-generator中的相关变量
    │   └── verify-codegen.sh                       #  调用update-codegen.sh，并检测代码变化的脚本，CI过程中非常重要
    ├── pkg                                         #  标准目录
    │   ├── apis                                    #  标准目录
    │   │   └── samplecontroller                    #  group,API组
    │   │       └── v1alpha1                        #  version,API版本
    │   │           ├── doc.go                      #  代码生成器模板，package相关内容，全局标签填写文件
    │   │           ├── types.go                    #  代码生成器模板，API类型相关内容，局部标签填写文件
    │   │           └── zz_generated.deepcopy.go    #  代码生成器运行后生成的深拷贝go文件
    │   └── generated
    │       ├── clientset                           #  代码生成器生成的clientset相关代码
    │       ├── informers                           #  代码生成器生成的informers相关代码
    │       └── listers                             #  代码生成器生成的listers相关代码
    └── vendor                                      #  vendor包管理，项目依赖包会放此处，包括code-generator
        ├── github.com
        ├── golang.org
        ├── gopkg.in
        └── k8s.io
            ├── apimachinery
            └── code-generator
                └── generate-groups.sh              # 代码生成器主要脚本
```

内容自行查找

## controller-tools - 了解即可

`controller-gen` 会根据我们定义的 CRD Spec，生成相应的 go 代码，可以自动生成的代码包括：

- CRD 的 kubernetes 的 YAML 配置文件
- Go 语言的 API 对象
- Go 语言的 kubernetes client
- Go 语言的 lister
- Go 语言的 informer
- DeepCopy 相关的代码
- OpenAPIValidation 相关的代码

主要工具

- `controller-gen` :
  - `controller-gen`可以生成`zz_xxx.deepcopy.go`文件以及`crd.yaml`文件。
  - 也可以通过`crd.yaml`生成go代码
- `helpgen` :
- `type-scaffold` :
  - `type-scaffold`可以用来生成我们需要的types.go文件

### :point_right:controller-gen - 命令行工作cli

```bash
controller-gen paths=./... crd:trivialVersions=true rbac:roleName=controller-perms output:crd:artifacts:config=config/crd/bases
```

https://book.kubebuilder.io/reference/controller-gen.html

### 安装工具

```bash
go get sigs.k8s.io/controller-tools/cmd/controller-gen
```

> 使用方法自行搜索

## Apimachinery、Api、Client-go库之间的关系

- apimachinery 是最基础的库，包括核心的数据结构，比如 Scheme、Group、Version、Kind、Resource，以及排列组合出来的 常用的GVK、GV、GK、GVR等等，再就是编码、解码等操作
- api 库，这个库依赖 apimachinery，提供了k8s的内置资源，以及注册到 Scheme 的接口，这些资源比如：Pod、Service、Deployment、Namespace
- client-go 库，这个库依赖前两个库，提供了访问k8s 内置资源的sdk，最常用的就是 clientSet。底层通过 http 请求访问k8s 的 api-server，从etcd获取资源信息

# kubebuilder

Kubebuilder 可以用于编写可扩展性更好的 Kubernetes 应用程序，同时可以帮助开发人员编写基于 K8s 原生 API 资源的代码。它提供了诸如代码生成、CRD（自定义资源定义）定义、控制器开发等功能。包括：

- Kubernetes API 和资源的结构
- API 版本语义
- 自愈
- 垃圾收集器和终结器
- 声明式与命令式 API
- 基于级别与基于边缘的 API
- 资源与子资源

## 安装

仅支持 darwin、linux系统

```bash
# download kubebuilder and install locally.
curl -L -o kubebuilder https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)
chmod +x kubebuilder && mv kubebuilder /usr/local/bin/
```

`kubebuilder --help`

```bash
completion  Load completions for the specified shell
create      创建 Kubernetes API 或 webhook
edit        更新项目配置
help        Help about any command
init        初始化一个新项目
version     Print the kubebuilder version
```

## :point_right:流程

### :point_right:init - 初始化一个项目

```bash
mkdir -p ~/projects/guestbook
cd ~/projects/guestbook
kubebuilder init --domain my.domain --repo my.domain/guestbook
```

### :point_right:create - 创建一个API

```bash
kubebuilder create api --group webapp --version v1 --kind Guestbook
```

如果您按Create Resource [y/n] 和 Create Controller [y/n]，那么这将创建定义 API 的文件以及为该 Kind (CRD) 实现协调业务逻辑的`y`文件。

```
api/v1/guestbook_types.go 
controllers/guestbook_controller.go
```

### :point_right:生成crd.yaml

使用 `make manifests` 命令生成crd,Makefile 内容格式如下

```makefile
.PHONY: manifests
manifests: controller-gen ## Generate WebhookConfiguration, ClusterRole and CustomResourceDefinition objects.
    $(CONTROLLER_GEN) rbac:roleName=manager-role crd webhook paths="./..." output:crd:artifacts:config=config/crd/bases
```

### :point_right:将 CRD 安装到集群中

`make install`

```makefile
.PHONY: install
install: manifests kustomize ## Install CRDs into the K8s cluster specified in ~/.kube/config.
    $(KUSTOMIZE) build config/crd | kubectl apply -f -
```

### :point_right:运行controller

`make run`

```makefile
.PHONY: run
run: manifests generate fmt vet ## Run a controller from your host.
    go run ./main.go
```

## :point_right:代码生成注释

https://book.kubebuilder.io/reference/markers.html

##  :point_right:controller-runtime

Controller-runtime 是 Kubebuilder 中的一个核心组件，是一个 Kubernetes 控制器库，提供了实现控制器的许多功能，例如对资源对象的缓存、高级事件处理、控制器恢复和 leader election。

controller-runtime 的核心是Manager 驱动 Controller 进而驱动 Reconciler。kubebuiler 用Manager.start 作为驱动入口， Reconciler 作为自定义入口（变的部分），Controller 是不变的部分。

 [controller-runtime](https://pkg.go.dev/sigs.k8s.io/controller-runtime?tab=doc)

### Scheme 和 GVK

```bash
var (
    scheme   = runtime.NewScheme()
    setupLog = ctrl.Log.WithName("setup")
)

func init() {
    utilruntime.Must(clientgoscheme.AddToScheme(scheme))

    //+kubebuilder:scaffold:scheme
}
```

### Client 与 Cache

- client: 指的是 clien-go 中的 reset-client 模式,封装后名称叫 typedClient ;

- cache: 指的是 client-go 中的 informer 模式, 封装后名称叫 informerCache;

```go
package main

import (
        "context"
        "fmt"
        corev1 "k8s.io/api/core/v1"
        "k8s.io/apimachinery/pkg/types"
        logf "sigs.k8s.io/controller-runtime/pkg/log"
        "sigs.k8s.io/controller-runtime/pkg/manager"
        "sigs.k8s.io/controller-runtime/pkg/studycr"
        "time"
)

func main() {
        mgr, err := manager.New(studycr.K8sRestConfig(), manager.Options{
                Logger: logf.Log.WithName("test"),
                // 设置client 并设置不缓存对象；打开注释后 pod 资源便不走缓存.
                //NewClient: func(cache cache.Cache, config *rest.Config, options client.Options, uncachedObjects ...client.Object) (client.Client, error) {
                //	return cluster.DefaultNewClient(cache, config, options, &corev1.Pod{})
                //},
        })
        studycr.Check(err)

        go func() {
                time.Sleep(time.Second * 2)
                pod := &corev1.Pod{}
                // 获取 client
                // 从这段代码进行调试,便可以看看到 /pkg/client/split.go 
                // delegatingClient.Get 对其进行拆分的方法就是判断是否加入了 uncacheGVKs 队列
                studycr.Check(mgr.GetClient().Get(context.TODO(), types.NamespacedName{
                        Namespace: "default",
                        Name:      "test-nginx",
                }, pod))
                fmt.Println(pod.Name, pod.Kind)
                
                // 获取 informer
                pod := &corev1.Pod{}
                // GetInformer 其实就是在一个 InformerMap 内去获取已经注册过的资源的Informer，如没有注册则没有该Informer
                informer, _ := mgr.GetCache().GetInformer(context.TODO(), pod)
                // 转换成client-go 的informer
                fmt.Println(informer.(cache.SharedIndexInformer).GetIndexer().ListKeys())
                

        }()

        studycr.Check(mgr.Start(context.TODO()))
}
```

### Manager

`Manager`可以通过以下方式限制所有控制器将监视资源的名称空间

```go
  mgr, err := ctrl.NewManager(ctrl.GetConfigOrDie(), ctrl.Options{
        Scheme:                 scheme,
        Namespace:              namespace,
        MetricsBindAddress:     metricsAddr,
        Port:                   9443,
        HealthProbeBindAddress: probeAddr,
        LeaderElection:         enableLeaderElection,
        LeaderElectionID:       "80807133.tutorial.kubebuilder.io",
        // 还可以使用MultiNamespacedCacheBuilder 来监视一组特定的命名空间
        // NewCache:               cache.MultiNamespacedCacheBuilder(namespaces),
    }
```

###  Controller 与 Reconciler (重要的资源协调 )

```go
package main

import (
        "context"
        "fmt"
        "github.com/gin-gonic/gin"
        corev1 "k8s.io/api/core/v1"
        "sigs.k8s.io/controller-runtime/pkg/controller"
        "sigs.k8s.io/controller-runtime/pkg/event"
        "sigs.k8s.io/controller-runtime/pkg/handler"
        cc "sigs.k8s.io/controller-runtime/pkg/internal/controller"
        "sigs.k8s.io/controller-runtime/pkg/manager"
        "sigs.k8s.io/controller-runtime/pkg/reconcile"
        "sigs.k8s.io/controller-runtime/pkg/source"
        "sigs.k8s.io/controller-runtime/pkg/studycr"
)

// 构建我们自己的 Controller
type TestController struct {
}

// Reconcile 函数正是我们接收变化的资源
func (this TestController) Reconcile(ctx context.Context, req reconcile.Request) (reconcile.Result, error) {
        fmt.Println(req.NamespacedName)
        return reconcile.Result{}, nil
}

// 创建一个 web 并实现了start方法, 这是为了等下加入到manager 进行统一的管理;
type MyWeb struct {
        h   handler.EventHandler
        ctl *cc.Controller
}

func NewMyWeb(h handler.EventHandler, ctl *cc.Controller) *MyWeb {
        return &MyWeb{h: h, ctl: ctl}
}

func (m *MyWeb) Start(ctx context.Context) error {
        r := gin.New()
        r.GET("/add", func(c *gin.Context) {
                p := &corev1.Pod{}
                p.Name = "hello-world"
                p.Namespace = "testNamespace"
                m.h.Create(event.CreateEvent{Object: p}, m.ctl.Queue)
        })
        return r.Run(":8081")
}

func main() {
        // new manager 
        mgr, err := manager.New(studycr.K8sRestConfig(), manager.Options{
                Namespace: "default",
        })

        studycr.Check(err)

        // 新建一个 controller
        c, err := controller.New("App", mgr, controller.Options{
                // 真正执行的reconsiler pkg.internal.controller.Controller.processNextWorkItem 从这往下看
                Reconciler: &TestController{},
                // 开启多个 Reconcile, 如果是多个同样的资源仅会触发一次，直到该资源done
                MaxConcurrentReconciles: 1,
        })

        studycr.Check(err)
        // EnqueueRequestForObject 这是一个默认提供 eventHandler 类似于 Informer 的 onCreate, onUpdate 等;
        h := &handler.nqueueRequestForObject{}
        
        // watch 资源 , handler.EnqueueRequestForObject 事件队列从informer中获取到信息后向Queue中发送对象，让 Reconciler 执行
        err = c.Watch(&source.Kind{Type: &corev1.Pod{}}, h)
        studycr.Check(err)

        // 将handler 和 controller 传递给 myweb 让其主动触发Reconciler
        // 强制转换 controller 接口至 internal.controller.Controller 
        // 把我们自己写的 web 加入启动管理
        mgr.Add(NewMyWeb(h, c.(*cc.Controller)))
        err = mgr.Start(context.TODO())
        studycr.Check(err)

}
```

### WrokQueue 工作队列 - 后面了解

### Builder

Builder 是一个串联所有核心功能的工具, 让开发者轻松上手

```go
func main () {
    // 创建 manager
    mgr, err := manager.New(studycr.K8sRestConfig(), manager.Options{ Namespace: "default", })
    // 创建 controller
    c := NewTestController()
    // ControllerManagedBy 生成buidler
    err := builder.ControllerManagedBy(mgr).
            // v1.DbConfig 是我们通过CRD的自定义资源, 
            // For 设定 Reconcile 资源
            For(&v1.DbConfig{}).
            // watch k8s资源并配置事件的变更监听
            Watches(
                    &source.Kind{Type: &appv1.Deployment{}},
                    handler.Funcs{
                            UpdateFunc: dbconfigController.OnUpdate,
                            DeleteFunc: dbconfigController.OnDelete,
                    },
            ).
            // 最后执行 builder, 生成controller 和 watch 对应的的资源
            Complete(c)
    ...
    // 最终启动管理器
    _ = mgr.Start(contenxt.TODO())
}
```

## Webhook

在 kubernetes 的世界里，有 3 种 webhook： [admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#admission-webhooks)， [authorization webhook](https://kubernetes.io/docs/reference/access-authn-authz/webhook/)和 [CRD conversion webhook](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/#webhook-conversion)。

# :point_right:operator-sdk

是 Operator Framework 的一个组件，Operator Framework 是一个开源工具包，用于以有效、自动化和可扩展的方式管理 Kubernetes 原生应用程序，称为 Operators。

## 安装

https://github.com/operator-framework/operator-sdk/releases

以linux为例

```bash
wget https://github.com/operator-framework/operator-sdk/releases/download/v1.28.0/operator-sdk_linux_amd64

chmod +x operator-sdk_linux_amd64 && sudo mv operator-sdk_linux_amd64 /usr/local/bin/operator-sdk
```

## Ansible - 使用

### 初始化

```bash
operator-sdk init --domain example.com --plugins ansible
```

## :point_right:go - 使用

### :point_right:初始化项目

```bash
mkdir memcached-operator
cd memcached-operator
operator-sdk init --domain example.com --repo github.com/example/memcached-operator
```

### :point_right:创建API

```bash
operator-sdk create api --group cache --version v1alpha1 --kind Memcached --resource --controller
```

### :point_right:build镜像

```sh
make docker-build docker-push IMG="example.com/memcached-operator:v0.0.1"
```

### :point_right:部署OLM

Operator Lifecycle Manager 

``` bash
operator-sdk olm install

make bundle IMG="example.com/memcached-operator:v0.0.1"
make bundle-build bundle-push BUNDLE_IMG="example.com/memcached-operator-bundle:v0.0.1"

operator-sdk run bundle <some-registry>/memcached-operator-bundle:v0.0.1

kubectl apply -f config/samples/cache_v1alpha1_memcached.yaml
memcached.cache.example.com/memcached-sample created

operator-sdk cleanup memcached-operator
```

### :point_right:直接部署

```bash
make deploy IMG="example.com/memcached-operator:v0.0.1"

kubectl apply -f config/samples/cache_v1alpha1_memcached.yaml
memcached.cache.example.com/memcached-sample created
```

### :point_right:卸载

```b
make undeploy
```

## :point_right:helm使用

### 初始化

```bash
operator-sdk init --domain example.com --plugins helm
```

> 其余参考go的实现。
