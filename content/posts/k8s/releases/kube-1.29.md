+++
title = "Kubernetes v1.29"
tags = []
date = "2023-12-13"
toc = true
+++

## Kubernetes v1.29.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - ValidatingAdmissionPolicy (Stable)

**ValidatingAdmissionPolicy** graduated to **general availability**, enabling users to define admission rules using **Common Expression Language (CEL)** without external webhooks.

**Example:**

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingAdmissionPolicy
metadata:
  name: restrict-replicas
spec:
  matchConstraints:
    resourceRules:
      - apiGroups: ["apps"]
        apiVersions: ["v1"]
        operations: ["CREATE", "UPDATE"]
        resources: ["deployments"]
  validations:
    - expression: "object.spec.replicas <= 10"
      message: "Replicas must not exceed 10"
```

---

#### - Node System Swap Support (Beta)

Introduced support for limited **swap memory usage** on Linux nodes.

* Allows improved stability and pod performance in memory-constrained environments
* Requires explicit enablement in kubelet config

```yaml
memorySwap:
  swapBehavior: LimitedSwap
```

---

#### - Scheduler Framework Enhancements (Stable)

The scheduling framework continues to mature with **extensible scoring plugins**, improved support for:

* Volume binding
* Pod topology spread constraints
* Multi-zone clusters

---

#### - `kubectl get --show-kind` (Stable)

`kubectl get` now supports showing **resource kinds explicitly** using the `--show-kind` flag for improved clarity in scripts and dashboards.

```bash
kubectl get all --show-kind
```

---

#### - PersistentVolumeClaim Expansion for CSI Inline Volumes (Beta)

Improved support for **PVC resizing** when used with **ephemeral CSI volumes**, aligning with long-lived volume behaviors.

---

#### - Feature Gates Reorganization

Feature gates have been reorganized and documented by API level and component to enhance consistency and debugging.

---

## Notable Limitations in v1.29

Still **not GA or production-ready**:

* **NodeLogQuery API** remains alpha
* **Sidecar lifecycle improvements** remain beta
* **Per-container startup ordering** not complete
* **PodSecurity Admission** lacks namespace-level RBAC exception rules
* **Tracing integration** (OpenTelemetry) is still alpha

---

## Deprecations or Removals in v1.29

* **PodSecurityPolicy** fully removed since v1.25
* Deprecated APIs removed:

  * `discovery.k8s.io/v1beta1` (EndpointSlice)
  * `flowcontrol.apiserver.k8s.io/v1beta2`
* More **in-tree cloud providers** removed
* Removed deprecated metrics and flags in controller-manager, scheduler, kubelet
