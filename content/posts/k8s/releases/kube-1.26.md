+++
title = "Kubernetes v1.26"
tags = []
date = "2022-12-09"
toc = true
+++

## Kubernetes v1.26.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - ValidatingAdmissionPolicy (Alpha → Beta)

Introduced the **ValidatingAdmissionPolicy** feature, a powerful alternative to `ValidatingAdmissionWebhook`, enabling **in-cluster policy expression without webhooks**.

**Benefits:**

* Declarative validation policies
* CEL (Common Expression Language) support
* Lower latency than webhooks

**Example (simplified):**

```yaml
apiVersion: admissionregistration.k8s.io/v1beta1
kind: ValidatingAdmissionPolicy
metadata:
  name: check-label
spec:
  matchConstraints:
    resourceRules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
  validations:
    - expression: "object.metadata.labels['team'] != ''"
      message: "team label is required"
```

> Feature gate: `ValidatingAdmissionPolicy=true`

---

#### - CRI API v1 Required by Default

**CRI v1 API** became **mandatory**, finalizing the shift away from alpha/beta CRI implementations.

> Only runtimes implementing `v1` are now supported (e.g., containerd, CRI-O).

---

#### - Storage Capacity Tracking (Stable)

The `CSIStorageCapacity` object, used to expose available space in topology zones for scheduling, reached **stable** status.

> Enables smarter scheduling of PVCs in multi-zone environments.

---

#### - Node System Swap Support (Alpha)

Added optional **support for swap memory** on Linux nodes. This long-standing limitation was partially lifted for use cases requiring swap.

> Feature gated and off by default: `NodeSwap=true`

---

#### - Kubelet Graduation to Structured Logging (Stable)

The **structured logging framework** continued to roll out across components, including full support in the kubelet.

---

#### - New APIs Promoted to Stable

* `autoscaling/v2` (for HPA with multiple metric types)
* `discovery.k8s.io/v1` (`EndpointSlice`)
* `flowcontrol.apiserver.k8s.io/v1beta3` (APF enhancements)

---

## Notable Limitations in v1.26

Still **not yet stable or finalized**:

* **ValidatingAdmissionPolicy** still beta (not GA)
* **PodSecurity Admission** lacks per-pod granularity
* **Swap support** still alpha and off by default
* **RuntimeClass** adoption limited
* **Multi-networking** not native (CNI dependent)

---

## Deprecations or Removals in v1.26

* Removed deprecated APIs:

  * `flowcontrol.apiserver.k8s.io/v1beta1`
  * `admissionregistration.k8s.io/v1beta1` (for webhooks)
* **kubectl run --generator** flags fully removed
* Deprecated cloud provider flags removed from in-tree kubelets

