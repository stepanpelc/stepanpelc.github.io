+++
title = "Kubernetes v1.2"
tags = []
date = "2016-03-17"
toc = true
+++


## Kubernetes v1.2.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Deployments (Beta)

Provided a declarative way to manage ReplicaSets and perform rolling updates and rollbacks.

**Usage Example (YAML):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
```

---

#### - DaemonSets (Beta)

Ensured that a copy of a pod runs on all (or selected) nodes in the cluster.

**Usage Example:**

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      name: fluentd
  template:
    metadata:
      labels:
        name: fluentd
    spec:
      containers:
        - name: fluentd
          image: fluent/fluentd
```

---

#### - Secrets Volumes (Stable)

Allowed mounting Kubernetes Secrets into pods as files.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-test
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret
  volumes:
    - name: secret-volume
      secret:
        secretName: my-secret
```

---

#### - PetSet (Alpha, precursor to StatefulSet)

PetSet introduced identity and stable storage for stateful pods, a critical step toward full StatefulSets.

> **Note:** PetSet was renamed to StatefulSet and officially introduced in v1.5.

---

#### - Kubernetes Dashboard (Web UI)

A general-purpose, web-based UI for managing applications and the cluster.

**Access Example:**

```bash
kubectl proxy
# Then visit: http://localhost:8001/ui
```

---

#### - Federated Clusters (Alpha)

Laid the foundation for managing multiple Kubernetes clusters as one via federation.

> Feature was alpha and had many limitations.

---

#### - Resource Limits via `LimitRange` (Beta)

Allowed setting default CPU/memory requests and limits per namespace.

**Example:**

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
    - default:
        memory: 512Mi
      defaultRequest:
        memory: 256Mi
      type: Container
```

---

## Notable Limitations in v1.2

Still **missing or experimental** in v1.2:

* No **StatefulSets** (PetSets were alpha)
* No **RBAC**
* No **PodSecurityPolicies**
* No **NetworkPolicy (stable)**
* No **Pod disruption budgets**

---

## Deprecations or Removals in v1.2

* `kubectl resize` was deprecated in favor of `kubectl scale`
* Some legacy fields and annotations in early DaemonSet and PetSet APIs began to phase out as beta versions matured.
