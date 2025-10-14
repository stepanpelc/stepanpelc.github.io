+++
title = "Kubernetes v1.0"
tags = ["kubernetes", "release", "v1.0", "history"]
date = "2015-07-21"
toc = true
+++

## Kubernetes v1.0.x – Summary of Major Stable Features

Released on **July 21, 2015**, Kubernetes v1.0 marked the **first stable release** of the Kubernetes container orchestration system. Backed by Google and the Cloud Native Computing Foundation (CNCF), this milestone set the foundation for modern cloud-native infrastructure.

> Official announcement: [Kubernetes 1.0 Launch](https://kubernetes.io/blog/2015/07/introducing-kubernetes-v10/)

---

## Major Stable Features Introduced

### - Pods (Stable)

A **Pod** is the smallest and simplest Kubernetes object. It represents a group of one or more containers that share storage and network resources.

**Use Cases:**

* Deploying a single-container app (e.g. nginx)
* Running sidecar containers (e.g. logging agents, proxies)

**Usage Example (YAML):**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

> Pods in v1.0 were scheduled directly without higher-level controllers like Deployments or ReplicaSets.

---

### - ReplicationController (Stable)

Ensures that a specified number of pod replicas are maintained across the cluster.

**Use Cases:**

* Achieving high availability
* Auto-recovering crashed pods

**Usage Example:**

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
```

> ⚠️ **Note:** Superseded by **ReplicaSets** and **Deployments** in later releases, but core to Kubernetes v1.0.

---

### - Services (Stable)

A **Service** provides a stable endpoint (ClusterIP, DNS) to expose a group of pods.

**Types Available in v1.0:**

* `ClusterIP` (default)
* `NodePort`

**Use Case:**

* Exposing internal or external access to a replicated nginx app

**Usage Example:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

> External LoadBalancers were cloud-provider-specific and not yet standard in v1.0.

---

### - Labels and Selectors (Stable)

Labels allow flexible grouping and querying of resources.

**Use Case:**

* Select pods for a service
* Group resources by environment or app version

**Label Example:**

```yaml
metadata:
  labels:
    app: nginx
    env: production
```

**Selector Example:**

```yaml
spec:
  selector:
    matchLabels:
      app: nginx
```

> Selectors were primitive and limited to exact matches in v1.0. `matchExpressions` came later.

---

### - Volumes (Stable)

Volumes in v1.0 allowed basic data persistence and sharing between containers in the same Pod.

**Supported Volume Types:**

* `emptyDir`
* `hostPath`
* `nfs`
* `gitRepo` (now deprecated)

**Usage Example:**

```yaml
spec:
  volumes:
    - name: html
      emptyDir: {}
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: html
```

> Storage was not yet abstracted via **PersistentVolumes** (introduced later in v1.2).

---

### - Namespaces (Stable)

Namespaces provide a mechanism for isolating resources within a cluster.

**Use Case:**

* Multi-tenant environments
* Organizing dev/staging/prod workloads

**Usage Example:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

Then deploy using:

```sh
kubectl create -f pod.yaml --namespace=dev
```

> Resource quotas and limits were primitive or non-existent at v1.0.

---

### - kubelet (Stable)

The kubelet is the agent that runs on each node and ensures that containers are running in Pods as defined by the control plane.

**Functions:**

* Watches API server for PodSpecs
* Manages container runtime (Docker in v1.0)
* Reports node and pod status

> Configuration was done via flags or manifest files (`/etc/kubernetes/manifests`).

---

### - kubectl CLI (Stable)

The primary CLI tool for Kubernetes, introduced with basic capabilities.

**Example Commands:**

```sh
kubectl get pods
kubectl create -f pod.yaml
kubectl delete pod nginx-pod
kubectl get services --namespace=dev
```

> `kubectl` was not yet extensible (no plugins, no kustomize integration).

---

### - Minions (Now called Nodes)

In v1.0, Kubernetes nodes were referred to as **minions**, a term carried over from the internal Borg system at Google.

Renamed to **nodes** in v1.1 for clarity and neutrality.

> Still referred to as `spec.nodeName` or `kubectl get nodes`.

---

## Notable Limitations in v1.0

These important features were **not available** in Kubernetes 1.0:

* No **Deployments**, **ReplicaSets**, **DaemonSets**, or **StatefulSets**
* No **Role-Based Access Control (RBAC)** – only basic auth/token auth
* No **Ingress** resources (came in v1.1 alpha)
* No **CustomResourceDefinitions (CRDs)** or Operator pattern
* No **Horizontal Pod Autoscaler** (introduced in v1.1)
* Networking was **primitive**, no standardized CNI support
* No native **Secrets encryption** at rest
* Lacked ecosystem tools like **Helm**, **Kustomize**, or **kubectl plugins**
* Cluster upgrades were manual; no declarative or rolling update process

---

## Deprecations or Removals in v1.0

Being the **initial stable release**, there were **no official deprecations**. However, some features were later replaced:

* **ReplicationController** → replaced by **ReplicaSet** and **Deployment**
* **Minions** → renamed to **Nodes**
* **Static volume types** (like `gitRepo`) → deprecated in favor of Persistent Volumes
* Legacy auth methods (basic auth files) → deprecated in favor of RBAC

> Backward compatibility was a major priority through the 1.x lifecycle.

---

## References

* [Kubernetes v1.0 Release Notes (GitHub)](https://github.com/kubernetes/kubernetes/releases/tag/v1.0.0)
* [Kubernetes Blog – Introducing Kubernetes v1.0](https://kubernetes.io/blog/2015/07/introducing-kubernetes-v10/)
* [CNCF Kubernetes Landscape](https://landscape.cncf.io/category=platform&format=card-mode&grouping=category)
* [Pod API Reference (v1)](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.30/#pod-v1-core)
