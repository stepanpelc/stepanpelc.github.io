+++
title = "Kubernetes v1.8"
tags = []
date = "2017-09-29"
toc = true
+++


## Kubernetes v1.8.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - RBAC (Stable)

**Role-Based Access Control (RBAC)** graduated to **stable**. It became the default authorization mechanism in many Kubernetes distributions.

**Usage Example:**

```yaml
apiVersion: rbac.authorization.k8s.io/v1
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

#### - NetworkPolicy (Stable, Extended)

NetworkPolicy remained stable and now supported **egress rules**, allowing users to restrict both incoming and outgoing traffic to pods.

**Example with egress rule:**

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
  egress:
    - to:
        - ipBlock:
            cidr: 10.0.0.0/24
```

---

#### - Audit Logging (Beta)

Audit logging reached **beta**, with structured, policy-driven configuration to control what gets logged.

**API Server Flags:**

```bash
--audit-policy-file=/etc/kubernetes/audit-policy.yaml
--audit-log-path=/var/log/k8s-audit.log
```

**Sample Policy:**

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
  - level: Metadata
    resources:
      - group: ""
        resources: ["pods"]
```

---

#### - Mount Propagation (Beta)

Supported sharing mounted volumes between containers and the host, useful for storage and plugins.

**Example Mount with Propagation:**

```yaml
volumeMounts:
  - name: my-volume
    mountPath: /mnt/data
    mountPropagation: Bidirectional
```

---

#### - Taints and Tolerations (Beta → Extended)

Expanded scheduling control mechanisms using node taints and pod tolerations.

---

#### - kubeadm (Continued Beta)

Added support for:

* Multi-master HA clusters
* Configurable `kubelet` and `apiserver` parameters
* Upgrades via `kubeadm upgrade`

---

#### - Storage Enhancements (Alpha/Beta)

* Local Persistent Volumes (Alpha)
* Volume Snapshots API (Alpha)
* Dynamic provisioning now supported across more volume plugins

---

## Notable Limitations in v1.8

Still **not yet GA** or incomplete:

* **StatefulSets** still beta
* **CRI** not default
* **CRDs** remained `v1beta1`
* No built-in **volume expansion/resizing**
* **PodSecurityPolicies** not enforced by default
* Native **multi-tenancy** still missing

---

## Deprecations or Removals in v1.8

* `ThirdPartyResource` officially removed (fully replaced by CRDs)
* Legacy scheduler policy file format deprecated in favor of improved plugin architecture
* Initial steps to deprecate `kube-dns` in favor of **CoreDNS** (introduced in later releases)
