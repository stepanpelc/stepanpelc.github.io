+++
title = "Kubernetes v1.0"
tags = []
date = "2015-07-21"
toc = true
+++
---

## Kubernetes v1.0.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Pods (Stable)

The **Pod** is the basic unit of deployment in Kubernetes.

**Usage Example (YAML):**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
```

---

#### - ReplicationController (Stable)

Ensures a specified number of pod replicas are running at any given time.

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

> **Note:** ReplicationController is now largely replaced by **Deployments**, but was a core v1.0 feature.

---

#### - Services (Stable)

Abstracts access to a set of Pods using a stable IP and DNS name.

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

---

#### - Labels and Selectors (Stable)

Let you tag and group Kubernetes objects (e.g. pods, services).

**Example:**

```yaml
metadata:
  labels:
    app: nginx
```

Selector in service:

```yaml
spec:
  selector:
    app: nginx
```

---

#### - Volumes (Stable)

Provided a way to persist data beyond container lifecycle.

**Example:**

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

---

#### - Namespaces (Stable)

Introduced basic multi-tenancy via logical separation of resources.

**Example:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

You could then deploy resources into this namespace.

---

#### - kubelet (Stable)

Node agent responsible for maintaining pods, watching the API server.

> No YAML example; this is a system-level component.

---

#### - kubectl CLI (Stable)

The CLI tool to interact with the Kubernetes cluster.

**Example:**

```sh
kubectl get pods
kubectl create -f pod.yaml
kubectl delete pod nginx-pod
```

---

#### - Minions (Now called Nodes)

Originally called **minions**, renamed to **nodes** in later releases.

---

## Notable Limitations in v1.0

These were **not available yet** in v1.0:

* No **Deployments**, **DaemonSets**, or **StatefulSets**
* No **RBAC**
* No **Ingress**
* No **CRDs**
* Limited networking support (CNI not yet standardized)
* No **Helm** or plugin system

---

## Deprecations or Removals in v1.0

There were **no deprecations** in v1.0, since it was the initial stable release. Features like `ReplicationController` were **superseded** in later versions, but remained for backward compatibility for several releases.

