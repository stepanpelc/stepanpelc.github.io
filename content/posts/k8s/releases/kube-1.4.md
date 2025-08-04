+++
title = "Kubernetes v1.4"
tags = []
date = "2016-09-26"
toc = true
+++


## Kubernetes v1.4.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - StatefulSets (Beta, renamed from PetSets)

Introduced stable, unique network identities and persistent storage for stateful applications.

**Usage Example (YAML):**

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
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
          image: nginx
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

---

#### - Dynamic Volume Provisioning (Stable)

Enabled PVCs to automatically trigger dynamic provisioning of storage via StorageClass.

**Example with StorageClass:**

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
```

**PVC Example:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes: ["ReadWriteOnce"]
  storageClassName: fast
  resources:
    requests:
      storage: 1Gi
```

---

#### - `kubectl apply` Enhancement (First-Class Declarative Management)

Solidified `kubectl apply` as the go-to method for managing declarative configuration using server-side diffing and patching.

```bash
kubectl apply -f deployment.yaml
```

---

#### - Cluster Federation (Federation v1) Improvements

Added support for federated **ReplicaSets**, **ConfigMaps**, and **Secrets**.

---

#### - Kubeadm (Alpha)

First appearance of `kubeadm`, a tool for bootstrapping Kubernetes clusters.

```bash
kubeadm init
```

> Became the default installation method in later versions.

---

#### - New Web UI Dashboard (Replaced Old UI)

Enhanced user experience with better metrics, logs, and namespace switching.

> Accessed via `kubectl proxy`.

---

#### - Initial FlexVolume Support

Allowed custom volume drivers without modifying core Kubernetes code.

---

## Notable Limitations in v1.4

Still **not yet stable** or missing:

* **RBAC** (still alpha)
* **PodSecurityPolicies**
* **CustomResourceDefinitions (CRDs)**
* **NetworkPolicy** (still alpha/beta and required CNI plugins)
* **Native volume expansion or resizing**
* No **CRI-based runtimes** (dockershim still default)

---

## Deprecations or Removals in v1.4

* **PetSet** officially renamed to **StatefulSet**; old API was removed in later versions.
* The original UI (kube-ui) was replaced by the new Kubernetes Dashboard.
* Federation v1 continued to be marked as experimental despite additional features.
