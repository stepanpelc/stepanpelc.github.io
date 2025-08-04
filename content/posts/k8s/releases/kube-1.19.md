+++
title = "Kubernetes v1.19"
tags = []
date = "2020-08-26"
toc = true
+++

## Kubernetes v1.19.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Ingress (`networking.k8s.io/v1`) (Stable)

**Ingress** finally graduated to **stable** in the `networking.k8s.io/v1` API group, replacing deprecated versions.

**Example Ingress (v1 API):**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

---

#### - Custom Resource Definitions (v1) (Fully GA)

CRDs in `apiextensions.k8s.io/v1` now fully supported:

* Schema validation
* Defaulting
* Subresources
* Pruning
* Webhooks

> Legacy `v1beta1` officially deprecated.

---

#### - CSI Volume Snapshots (Beta, Expanded)

The CSI **VolumeSnapshot APIs** became more widely supported across cloud providers and CSI plugins.

> Supports creating snapshots, restoring PVCs, and snapshot class management.

---

#### - Server-side Apply (Beta Improvements)

Improvements to SSA field ownership tracking and conflict detection; now suitable for production use in many workflows.

---

#### - Extended Support Window

Kubernetes **extended the support period from 9 months to 1 year**, allowing more stability for enterprise users.

---

#### - Pod Readiness Gates (Stable)

Enabled external systems (e.g. sidecars, services) to signal pod readiness using custom conditions.

**Example:**

```yaml
readinessGates:
  - conditionType: "example.com/ready"
```

---

#### - Seccomp Default Policy (Alpha)

Pods could now run with a **default seccomp profile** (in alpha), enhancing container isolation and security by default.

> Enabled via kubelet feature gates.

---

## Notable Limitations in v1.19

Still **not fully GA**:

* **PodSecurityPolicies** remained beta and deprecated soon
* **Windows DaemonSet** still not supported
* **Ephemeral Containers** only alpha
* **RuntimeClass** adoption limited
* **Volume snapshot APIs** still beta (v1 GA came in v1.20+)

---

## Deprecations or Removals in v1.19

* Deprecated `Ingress` in:

  * `extensions/v1beta1`
  * `networking.k8s.io/v1beta1`
* Deprecated `CustomResourceDefinition` in `v1beta1`
* Deprecated `PodDisruptionBudget` in `policy/v1beta1` (moved to `policy/v1`)
* Marked `PodSecurityPolicy` for future deprecation (officially removed in v1.25)
