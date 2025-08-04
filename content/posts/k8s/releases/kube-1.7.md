+++
title = "Kubernetes v1.7"
tags = []
date = "2017-06-30"
toc = true
+++

---

## Kubernetes v1.7.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - NetworkPolicy (Stable)

**NetworkPolicy** graduated to **stable** in `networking.k8s.io/v1`. This enabled production-ready pod-level traffic control, assuming a compatible CNI plugin was in use.

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
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              role: frontend
```

---

#### - RBAC (Still Beta, Improvements)

Although still technically in beta, **RBAC** became widely adopted with continued improvements and was now **enabled by default** in many distributions.

---

#### - PodSecurityPolicies (Beta, Extended Capabilities)

Expanded to allow more fine-grained control over volumes, SELinux, host paths, and capabilities.

> Became an important mechanism for hardened cluster setups.

---

#### - Aggregated API Server (Beta)

Introduced the **API Aggregation Layer**, allowing extensions to Kubernetes APIs via custom aggregated servers (a stepping stone for CRDs and service catalog).

> Example use: Service Catalog, Metrics Server

---

#### - Container Runtime Interface (CRI) Stabilization (Still Alpha)

Ongoing improvements to **CRI**, enabling drop-in replacements for Docker like `containerd` and `CRI-O`.

> Many distros began experimenting with Docker alternatives.

---

#### - kubeadm (Beta)

**kubeadm** entered **beta**, now capable of:

* Bootstrapping HA clusters
* Upgrading existing clusters
* Providing join tokens and CA pinning

```bash
kubeadm init
kubeadm join ...
```

---

#### - StatefulSets (Still Beta)

No API promotion, but gained stability and production maturity.

---

#### - Audit Logging (Alpha → Beta)

Introduced an extensible **audit logging system** to record cluster API access events.

**Example (kube-apiserver flag):**

```bash
--audit-log-path=/var/log/kubernetes/apiserver-audit.log
```

---

#### - Secrets and ConfigMaps in Downward API (Beta)

Allowed mounting `ConfigMap` and `Secret` values into environment variables and volumes more flexibly.

**Usage Example (Secret in env):**

```yaml
env:
  - name: SECRET_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

---

## Notable Limitations in v1.7

Still **not yet stable** or missing:

* **StatefulSets** not yet GA
* **RBAC** not GA
* **CRDs** still beta (`apiextensions.k8s.io/v1beta1`)
* No native support for **volume expansion**
* No **Pod Priority & Preemption**
* No built-in **multi-tenancy** support

---

## Deprecations or Removals in v1.7

* `ThirdPartyResource` fully removed in favor of `CustomResourceDefinition`
* Legacy scheduler annotations (like `scheduler.alpha.kubernetes.io/affinity`) were deprecated
* Some `kube-dns` behaviors deprecated in preparation for CoreDNS integration (coming later)
