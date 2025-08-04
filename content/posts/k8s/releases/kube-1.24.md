+++
title = "Kubernetes v1.24"
tags = []
date = "2022-05-03"
toc = true
+++

## Kubernetes v1.24.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Structured Logging (Beta → Stable)

**Structured logging** format was promoted to **stable**, standardizing log output from control plane components for better log processing and observability.

**Example log (JSON):**

```json
{
  "ts": 1651570062.123456,
  "msg": "Successfully synced pod",
  "pod": "nginx-1234",
  "namespace": "default",
  "component": "kubelet"
}
```

> Enabled by default in newer components using klog.

---

#### - CSI Volume Expansion on Node (Stable)

Volume expansion (resize) on CSI volumes now **automatically updates the file system** in-place on the node, no longer requiring pod restarts.

---

#### - PodSecurity Admission (Beta)

Continued enhancements for **PodSecurity Admission**, which enforces predefined security levels (`restricted`, `baseline`, `privileged`) on namespaces.

**Namespace label example:**

```yaml
metadata:
  labels:
    pod-security.kubernetes.io/enforce: "restricted"
```

---

#### - CSI Ephemeral Volume Enhancements (Stable)

CSI inline ephemeral volumes matured, improving dynamic volume creation directly from pod specs without persistent resources.

**Pod with CSI ephemeral volume:**

```yaml
volumes:
  - name: ephemeral-vol
    csi:
      driver: csi.example.com
      volumeAttributes:
        foo: "bar"
```

---

#### - Seccomp Default Enabled by Default (Alpha → Beta)

Kubelets now default pods to a **Seccomp profile** (`RuntimeDefault`) unless overridden.

> Enhances sandboxing and security hardening by default.

---

## Major Breaking Change

#### - Dockershim Removed (Complete)

**Dockershim was officially removed** in v1.24.

* Kubernetes **no longer supports Docker as a container runtime**.
* Users must switch to **containerd**, **CRI-O**, or another **CRI-compliant runtime**.

> Docker images and Dockerfiles are still fully supported.

---

## Notable Limitations in v1.24

Still **not GA or enabled by default**:

* **Ephemeral containers** remained **beta**
* **PodSecurity Admission** still **beta**
* **Seccomp default** not enforced by all components
* **CRI v1 API** finalized, but not default across all tools

---

## Deprecations or Removals in v1.24

* **Dockershim removed** from kubelet
* Deprecated `PodSecurityPolicy` (previously deprecated in v1.21) – still removed in v1.25
* `kubeadm` dropped flags related to dockershim
* Deprecated in-tree credential providers in favor of external credential plugins
* Deprecated metrics for HPA and legacy APIs removed
