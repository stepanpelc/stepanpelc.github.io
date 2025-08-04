+++
title = "Kubernetes v1.6"
tags = []
date = "2017-03-28"
toc = true
+++


---

## Kubernetes v1.6.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Role-Based Access Control (RBAC) (Beta, default enabled)

RBAC became the **default authorization mechanism** (via `--authorization-mode=RBAC`) and was significantly improved for managing access to Kubernetes resources.

> It was no longer experimental and became the recommended security model.

**Example:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
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

#### - ThirdPartyResource replaced by CustomResourceDefinition (CRD) (Beta)

Marked the introduction of **CustomResourceDefinitions**, which allowed users to extend the Kubernetes API with custom objects.

**Example CRD (shortened):**

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  version: v1
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
```

---

#### - NetworkPolicy (Beta)

The **NetworkPolicy** resource matured and became supported for CNI plugins implementing the spec (like Calico or Cilium).

**Example:**

```yaml
apiVersion: networking.k8s.io/v1
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

---

#### - Taints and Tolerations (Beta)

Enabled the scheduling of pods only on specific nodes or under defined conditions.

> Made it possible to reserve nodes for specific workloads.

**Taint Example (applied to node):**

```bash
kubectl taint nodes node1 dedicated=experimental:NoSchedule
```

**Toleration in Pod:**

```yaml
tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "experimental"
    effect: "NoSchedule"
```

---

#### - StatefulSet (Beta → Continued)

Now more widely used and maturing, though still technically beta.

---

#### - PodSecurityPolicies (Beta)

Allowed cluster admins to enforce security-related controls on pods, such as privileged mode, volume types, and allowed capabilities.

**Example:**

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  privileged: false
  runAsUser:
    rule: MustRunAsNonRoot
  seLinux:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  volumes:
    - 'configMap'
    - 'emptyDir'
```

---

#### - Etcd v3 and RBAC support

Introduced support for using **etcd v3** as the storage backend, enabling advanced features like improved performance and proper object versioning.

---

#### - kubeadm Improvements

Improved bootstrapping capabilities; supported more configuration flags and production cluster upgrades.

---

## Notable Limitations in v1.6

Still **not yet available** or unstable:

* No **native volume resizing**
* **Windows** support still alpha
* Some CRD features (validation schemas, subresources) were missing
* **PodSecurityPolicies** not enforced by default
* Federation still not stable or widely adopted

---

## Deprecations or Removals in v1.6

* **ThirdPartyResource** was officially deprecated in favor of **CRD**
* Alpha annotations for scheduling and affinity deprecated in favor of `affinity` API fields
* Legacy authorization modes like `ABAC` were discouraged in favor of RBAC
