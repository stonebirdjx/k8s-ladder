<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [client-go](#client-go)
  - [GVR/GVK](#gvrgvk)
    - [有组名资源组](#%E6%9C%89%E7%BB%84%E5%90%8D%E8%B5%84%E6%BA%90%E7%BB%84)
    - [无组名资源组](#%E6%97%A0%E7%BB%84%E5%90%8D%E8%B5%84%E6%BA%90%E7%BB%84)
    - [请求路径](#%E8%AF%B7%E6%B1%82%E8%B7%AF%E5%BE%84)
  - [Client-Go客户端](#client-go%E5%AE%A2%E6%88%B7%E7%AB%AF)
  - [RESTClient](#restclient)
  - [ClientSet](#clientset)
  - [DynamicClient](#dynamicclient)
  - [DiscoveryClient](#discoveryclient)
  - [将 GVR 缓存到本地](#%E5%B0%86-gvr-%E7%BC%93%E5%AD%98%E5%88%B0%E6%9C%AC%E5%9C%B0)
  - [Informer 介绍](#informer-%E4%BB%8B%E7%BB%8D)
- [kubebuilder](#kubebuilder)
- [operator-sdk](#operator-sdk)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# client-go

Client-Go 是负责与 Kubernetes APIServer 服务进行交互的客户镇库，利用 Client-Go与 KubernetesAPISeIver 进行的交互访问，以此来对 Kubernetes 中的各类资源对象进行管理操作，包括内量的资源对象及CRD。

## GVR/GVK

Kubernetes API 是通过 HTTP 协议以RESTful 的形式提供的，同时支持JSON 和 Protobuf 的数据格式，Protobuf 是为方便集群内部调用而支持的。我们自己平时调用 Kubernetes 接口，一般都是使用JSON 数据格式的。

在Kubernetes AP 中，我们一般使用GVR 或 GVK 来区分特定的资源，即根据不同的分组、版本及资源，进行 URL的定义，有了分组和多版本的支持，即便是后续的版本中，需要去掉资源对象的某些字段或者重构 API 资源，也可以保证版本之间的兼容性。

同时，因为历史原因，因此 Kubernetes 的 API 分组，是分为 **无组名资源组** 和 **有组名资源组** 的，无组名资源组 也被称之为核心资源组，Core Group。

GVR/GVK 含义介绍

- G (Group):资源组，包含一组资源操作的集合。
- V (Version):资源版本，用于区分不同 API 的稳定程度及兼容性
- R(Resource) :资源信息，用于区分不同的资源 API。
- K (Kind):资源对象的类型，每个资源对象都需要 Kind 来区分它自身代表的资源类型

[api文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)

### 有组名资源组

对应[api文档](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)中的有分组的部分，绝大多数，/apis（复数）

```bash
# apps: group
# v1 : version
# deployments: Resource
/apis/apps/v1/deployments
```

### 无组名资源组

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

## Client-Go客户端

Client-Go 共提供了4种与 Kubernetes APIServer 交互的客户端对象。

- `RESTClient`：最基础的客户端，提供最基本的封装
- `Clientset`： 是一个Client的集合，在Clientset中包含了所有K8S内置资源的Client，通过Clientset便可以很方便的操作如Pod、Service这些资源
- `dynamicClient`：动态客户端，可以操作任意K8S的资源，包括CRD定义的资源
- `DiscoveryClient`: 用于发现K8S提供的资源组、资源版本和资源信息，比如:kubectl api-resources

```bash
Clientset、dynamicClient、DiscoveryClient  -> RESTClient -> net/http -> k8sAPISever
```

## RESTClient 

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

## ClientSet

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

##  DynamicClient 

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

## Informer 介绍

# kubebuilder

# operator-sdk

