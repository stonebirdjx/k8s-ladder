<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [资源概念与解释](#%E8%B5%84%E6%BA%90%E6%A6%82%E5%BF%B5%E4%B8%8E%E8%A7%A3%E9%87%8A)
  - [:point_right:Client](#point_rightclient)
  - [:point_right:Cache](#point_rightcache)
  - [:point_right:Manager](#point_rightmanager)
  - [:point_right:Controller](#point_rightcontroller)
  - [:point_right:Webhook](#point_rightwebhook)
  - [:point_right:Reconciler](#point_rightreconciler)
  - [:point_right:Source](#point_rightsource)
  - [:point_right:EventHandler](#point_righteventhandler)
  - [:point_right:Predicate](#point_rightpredicate)
  - [:point_right:PodController Diagram](#point_rightpodcontroller-diagram)
- [usage](#usage)
- [实现defaulting/validating webhooks](#%E5%AE%9E%E7%8E%B0defaultingvalidating-webhooks)
- [证书](#%E8%AF%81%E4%B9%A6)
- [部署](#%E9%83%A8%E7%BD%B2)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

controller-runtime是Kubernetes官方提供的一个用于编写自定义控制器的框架。它是基于Kubernetes的client-go库构建的，为编写控制器提供了一套强大的工具和抽象。

- **Leader Election（领导者选举）**：controller-runtime提供了一个简单的Leader Election实现，以确保在多个控制器实例运行时只有一个实例活跃，避免重复处理相同的任务。
- **事件管理**：controller-runtime为控制器提供了事件管理功能，可以监听和处理Kubernetes集群中发生的各种事件，例如资源的创建、更新和删除等。

- **资源缓存和索引**：controller-runtime内部维护了一个缓存，可以用于存储和索引Kubernetes集群中的资源对象，从而提供高效的资源查询和操作。

- **控制循环（Control Loop）**：controller-runtime实现了一个基本的控制循环机制，控制器可以通过该机制根据需要对资源进行调谐和协调，确保实际状态与期望状态的一致性。

- **Webhook支持**：controller-runtime支持通过自定义的Webhook来验证和处理资源的创建和更新请求，以便进行额外的验证和操作。

# 资源概念与解释

##  :point_right:Client

Client 提供 Read + Write 客户端，用于读写 Kubernetes 对象。

## :point_right:Cache

Cache 提供了一个 Read 客户端，用于从本地缓存中读取对象。 缓存可以注册处理程序以响应更新缓存的事件。

## :point_right:Manager 

Manager 是创建 Controller 所必需的，并提供 Controller 共享依赖项，例如客户端、缓存、方案等。Controller 应通过调用 Manager.Start 通过 Manager 启动。

## :point_right:Controller

Controller通过响应事件（对象创建、更新、删除）并确保对象规范中指定的状态与系统状态相匹配来实现 Kubernetes API。 这称为调和。 如果它们不匹配，Controller 将根据需要创建/更新/删除对象以使它们匹配。

控制器实现为处理 **reconcile.Requests**（协调特定对象状态的请求）的工作队列。

- Controllers require a Reconciler to be provided to perform the work pulled from the work queue.

* Controllers require Watches to be configured to enqueue reconcile.Requests in response to events.

## :point_right:Webhook 

Webhooks 是一种扩展 kubernetes API 的机制。 Webhooks 可以配置目标事件类型（对象创建、更新、删除），当某些事件发生时，API 服务器将向它们发送 AdmissionRequests。

admission webhook 有两种类型：变异和验证 admission webhook。 

- Mutating webhook 用于在 API 服务器承认之前改变核心 API 对象或 CRD 实例。 验证 webhook 用于验证对象是否满足特定要求。

* Admission Webhooks 需要提供处理程序来处理收到的 AdmissionReview 请求。

## :point_right:Reconciler

Reconciler 是提供给 Controller 的函数，可以随时使用对象的 Name 和 Namespace 调用。 调用时，Reconciler 将确保系统状态与调用 Reconciler 时对象中指定的状态相匹配。

协调器提供了要协调的对象的名称/命名空间。

## :point_right:Source 

resource.Source 是 Controller.Watch 的参数，它提供事件流。 事件通常来自观察 Kubernetes API（例如 Pod 创建、更新、删除）。

Source 通常通过 Watch API 为 Kubernetes 对象提供事件流（例如对象创建、更新、删除）。

## :point_right:EventHandler 

handler.EventHandler 是 Controller.Watch 的参数，它使 reconcile.Requests 排队以响应事件。

示例：将来自 Source 的 Pod Create 事件提供给 eventhandler.EnqueueHandler，它将包含 Pod 的名称/命名空间的 reconcile.Request 排入队列。

## :point_right:Predicate

predicate.Predicate 是 Controller.Watch 的可选参数，用于过滤事件。 这允许重复使用和组合公共过滤器。

## :point_right:PodController Diagram

```go
// Source provides event:
&source.KindSource{&v1.Pod{}} // (Pod foo/bar Create Event)

// EventHandler enqueues Request:
&handler.EnqueueRequestForObject{} // (reconcile.Request{types.NamespaceName{Name: "foo", Namespace: "bar"}})

// Reconciler is called with the Request:
Reconciler(reconcile.Request{types.NamespaceName{Name: "foo", Namespace: "bar"}})
```

# usage

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"os"

	corev1 "k8s.io/api/core/v1"
	"k8s.io/apimachinery/pkg/runtime"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/client"
	"sigs.k8s.io/controller-runtime/pkg/controller"
	"sigs.k8s.io/controller-runtime/pkg/event"
	"sigs.k8s.io/controller-runtime/pkg/healthz"
	"sigs.k8s.io/controller-runtime/pkg/log/zap"
	"sigs.k8s.io/controller-runtime/pkg/manager"
	"sigs.k8s.io/controller-runtime/pkg/predicate"
	"sigs.k8s.io/controller-runtime/pkg/reconcile"
)

const (
	registerLabelKey   = "lable_key"
	registerLabelValue = "register"
)

// ReconcileConfigMap cm reconcile.
type ReconcileConfigMap struct {
	client client.Client
	scheme *runtime.Scheme
}

func (r *ReconcileConfigMap) Reconcile(ctx context.Context, request reconcile.Request) (reconcile.Result, error) {
	cm := &corev1.ConfigMap{}
	r.client.Get(ctx, request.NamespacedName, cm)

	b, err := json.Marshal(cm)
	if err != nil {
		panic(err.Error())
	}

	fmt.Println(string(b))
	return reconcile.Result{}, nil
}

func main() {
	scheme := runtime.NewScheme()
	_ = corev1.AddToScheme(scheme)

	setupLog := ctrl.Log.WithName("setup")
	opts := zap.Options{
		Development: true,
	}
	ctrl.SetLogger(zap.New(zap.UseFlagOptions(&opts)))

	// init Manager
	mgr, err := ctrl.NewManager(ctrl.GetConfigOrDie(), manager.Options{
		Namespace:        "hjx",
		LeaderElection:   true,
		LeaderElectionID: "v1",
	})
	if err != nil {
		panic(err.Error())
	}

	if err := ctrl.NewControllerManagedBy(mgr).
		WithEventFilter(predicate.Funcs{
			CreateFunc: func(ce event.CreateEvent) bool {
				cm, ok := ce.Object.(*corev1.ConfigMap)
				if !ok {
					return false
				}

				// enqueue register if the register has register annotation
				v, ok := cm.Labels[registerLabelKey]
				if ok {
					return v == registerLabelValue
				}
				return false
			},
			DeleteFunc: func(de event.DeleteEvent) bool {
				return true
			},
		}).
		For(&corev1.ConfigMap{}).
		WithOptions(controller.Options{MaxConcurrentReconciles: 1}).
		Complete(&ReconcileConfigMap{
			client: mgr.GetClient(),
			scheme: scheme,
		}); err != nil {
		panic(err.Error())
	}

	if err := mgr.AddHealthzCheck("healthz", healthz.Ping); err != nil {
		setupLog.Error(err, "unable to set up health check")
		os.Exit(1)
	}
	if err := mgr.AddReadyzCheck("readyz", healthz.Ping); err != nil {
		setupLog.Error(err, "unable to set up ready check")
		os.Exit(1)
	}

	// 3. start Manager
	if err := mgr.Start(ctrl.SetupSignalHandler()); err != nil {
		setupLog.Error(err, "problem running manager")
		panic(err.Error())
	}
}
```

> 注意rabc权限

# 实现defaulting/validating webhooks

CRD (CronJob) 创建一个 webhooks 的支架。我们将需要运行下面的命令并带上 `--defaulting` 和 `--programmatic-validation` 标志（因为我们的测试项目会用到默认和验证 webhooks)：

```bash
kubebuilder create webhook --group batch --version v1 --kind CronJob --defaulting --programmatic-validation
```

用法

```go
func (r *CronJob) SetupWebhookWithManager(mgr ctrl.Manager) error {
    return ctrl.NewWebhookManagedBy(mgr).
        For(r).
        Complete()
}


var _ webhook.Defaulter = &CronJob{}

// Default 实现了 webhook.Defaulter ，因此将为该类型注册一个webhook。
func (r *CronJob) Default() {
    cronjoblog.Info("default", "name", r.Name)

    if r.Spec.ConcurrencyPolicy == "" {
        r.Spec.ConcurrencyPolicy = AllowConcurrent
    }
    if r.Spec.Suspend == nil {
        r.Spec.Suspend = new(bool)
    }
    if r.Spec.SuccessfulJobsHistoryLimit == nil {
        r.Spec.SuccessfulJobsHistoryLimit = new(int32)
        *r.Spec.SuccessfulJobsHistoryLimit = 3
    }
    if r.Spec.FailedJobsHistoryLimit == nil {
        r.Spec.FailedJobsHistoryLimit = new(int32)
        *r.Spec.FailedJobsHistoryLimit = 1
    }
}


var _ webhook.Validator = &CronJob{}

// ValidateCreate 实现了 webhook.Validator，因此将为该类型注册一个webhook。
func (r *CronJob) ValidateCreate() error {
    cronjoblog.Info("validate create", "name", r.Name)

    return r.validateCronJob()
}

// ValidateUpdate 实现了 webhook.Validator，因此将为该类型注册一个webhook。
func (r *CronJob) ValidateUpdate(old runtime.Object) error {
    cronjoblog.Info("validate update", "name", r.Name)

    return r.validateCronJob()
}

// ValidateDelete 实现了 webhook.Validator，因此将为该类型注册一个webhook。
func (r *CronJob) ValidateDelete() error {
    cronjoblog.Info("validate delete", "name", r.Name)

    // TODO(user): 填写删除对象时你的验证逻辑。
    return nil
}

func (r *CronJob) validateCronJob() error {
    var allErrs field.ErrorList
    if err := r.validateCronJobName(); err != nil {
        allErrs = append(allErrs, err)
    }
    if err := r.validateCronJobSpec(); err != nil {
        allErrs = append(allErrs, err)
    }
    if len(allErrs) == 0 {
        return nil
    }

    return apierrors.NewInvalid(
        schema.GroupKind{Group: "batch.tutorial.kubebuilder.io", Kind: "CronJob"},
        r.Name, allErrs)
}

func (r *CronJob) validateCronJobSpec() *field.Error {
    // kubernetes API machinery 的字段助手会帮助我们很好地返回结构化的验证错误。
    return validateScheduleFormat(
        r.Spec.Schedule,
        field.NewPath("spec").Child("schedule"))
}

func validateScheduleFormat(schedule string, fldPath *field.Path) *field.Error {
    if _, err := cron.ParseStandard(schedule); err != nil {
        return field.Invalid(fldPath, schedule, err.Error())
    }
    return nil
}
```

# 证书

cert-manger

https://cert-manager.io/docs/installation/supported-releases/#installing-with-regular-manifests



# 部署

```bash
make deploy IMG=<some-registry>/<project-name>:tag
```



