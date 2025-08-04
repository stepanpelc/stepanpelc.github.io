+++
title = "Kubernetes v1.5"
tags = []
date = "2016-12-13"
toc = true
+++

## Kubernetes v1.5.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - StatefulSets (Beta, API `apps/v1beta1`)

Officially supported workloads requiring stable identities and persistent volumes.

> Now supported via the `apps/v1beta1` API group.

**Usage Example (same as v1.4):**

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

#### - Container Runtime Interface (CRI, Alpha)

Introduced a pluggable interface for container runtimes, enabling alternatives to Docker (like `containerd`, `CRI-O`, etc.).

> This marked the start of modular runtime support.

---

#### - RBAC (Beta)

Expanded and improved Role-Based Access Control for managing permissions across users and service accounts.

**Role Example (Cluster-wide):**

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: pod-reader
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]
```

**Binding Example:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: read-pods-global
subjects:
  - kind: User
    name: jane
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

---

#### - PodDisruptionBudgets (Alpha)

Allowed administrators to control the number of concurrent disruptions to pods during voluntary operations (like upgrades).

**Example:**

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
  name: pdb-example
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: myapp
```

---

#### - Node Affinity (Beta)

Improved pod placement control using `affinity` fields, replacing the basic `nodeSelector`.

**Example:**

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd
```

---

#### - taints and tolerations (Alpha)

Introduced a more flexible mechanism for node scheduling restrictions.

**Example Toleration:**

```yaml
tolerations:
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"
```

---

#### - Federation Improvements

Extended support to **ConfigMaps**, **Namespaces**, and **Events** in federated clusters.

---

#### - Windows Server Container (Alpha Support)

Initial alpha support for running Windows containers on Windows Server nodes.

---

## Notable Limitations in v1.5

Still **not yet available or unstable**:

* **CustomResourceDefinitions (CRDs)**
* **Volume expansion**
* **CRI not default yet**
* No **API aggregation layer**
* **RBAC still beta** and required enabling manually

---

## Deprecations or Removals in v1.5

* PetSet API was officially removed; **StatefulSet** continued under `apps/v1beta1`.
* Continued phasing out of alpha scheduling annotations (`scheduler.alpha.kubernetes.io/*`) in favor of new affinity APIs.
* `ThirdPartyResource` remained deprecated, as CRDs were in development.
