+++
title = "Kubernetes v1.3"
tags = []
date = "2016-07-06"
toc = true
+++

## Kubernetes v1.3.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Cross-Cluster Federation (Beta)

Enabled management of multiple Kubernetes clusters as a single logical cluster using the federation control plane.

> Included federated services, deployments, and secrets (limited functionality).

**Example:**

```bash
kubefed join cluster2 --host-cluster-context=cluster1
```

> Note: `kubefed` CLI was introduced separately and matured slowly. Federation v1 was later replaced by Federation v2 (which itself faded in favor of multi-cluster via other tools).

---

#### - In-Cluster Load Balancing with ClusterIP Services (Stable)

By default, Kubernetes provided load-balanced services via the internal virtual IP mechanism, now better integrated with iptables-mode `kube-proxy`.

**Usage Example (YAML):**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 9376
  type: ClusterIP
```

---

#### - Initial Support for Network Policies (Alpha)

Introduced fine-grained network control between pods. Required compatible CNI plugins (e.g., Calico).

**Example (Alpha Syntax):**

```yaml
apiVersion: extensions/v1beta1
kind: NetworkPolicy
metadata:
  name: allow-frontend
spec:
  podSelector:
    matchLabels:
      role: db
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
```

> Note: NetworkPolicy moved to beta in 1.6 and became stable in 1.8.

---

#### - StatefulSet (Still called PetSet, Alpha)

The evolution of PetSet continued, laying groundwork for persistent identity and storage in pods.

---

#### - RBAC (Alpha)

Initial implementation of Role-Based Access Control (RBAC) API via `rbac.authorization.k8s.io`.

> Not enabled by default in v1.3 but began shaping cluster security models.

**Example:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1alpha1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
```

---

#### - `initContainers` Support (Alpha)

Introduced a lifecycle model where init containers run before app containers.

**Usage Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  initContainers:
    - name: init-myservice
      image: busybox
      command: ['sh', '-c', 'echo Initializing...']
  containers:
    - name: myservice
      image: nginx
```

---

## Notable Limitations in v1.3

Still **missing or underdeveloped** in v1.3:

* No **StatefulSets (stable)** (still PetSet alpha)
* No **RBAC (stable)**
* No **PodSecurityPolicies**
* No **CustomResourceDefinitions (CRDs)**
* No **Volume expansion** or **dynamic resizing**

---

## Deprecations or Removals in v1.3

* PetSet began transitioning toward **StatefulSet**.

  > Users were warned that renaming would occur in future versions.
* Federation v1 was marked as experimental and had limited support for workloads and service discovery.
