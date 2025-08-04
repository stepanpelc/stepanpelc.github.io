+++
title = "Kubernetes v1.28"
tags = []
date = "2023-08-15"
toc = true
+++

## Kubernetes v1.28.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Sidecar Container Lifecycle (Alpha → Beta)

Kubernetes added **beta support** for sidecar lifecycle management, enabling:

* Independent container shutdown control
* Ordered startup/shutdown
* Container restart policies

**Example:**

```yaml
spec:
  containers:
    - name: app
      image: app-image
    - name: sidecar
      image: sidecar-image
      lifecycle:
        preStop:
          exec:
            command: ["/bin/sh", "-c", "sleep 5"]
```

---

#### - Recovery from Node Shutdown (Stable)

**Graceful node shutdown handling** became **stable**, allowing pods to be evicted predictably when a node is shutting down (e.g., via systemd signals).

> Improves availability and resilience during planned node reboots.

---

#### - PodDisruptionBudget `policy/v1` (Stable)

**PodDisruptionBudget (PDB)** API in `policy/v1` is now **stable**. Earlier `policy/v1beta1` version is removed.

**Example:**

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: myapp-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: myapp
```

---

#### - CRD Defaulting and CEL Validation (Stable)

CRDs now fully support:

* **Schema-based defaulting**
* **Custom validation** with CEL (Common Expression Language)

> Declarative validation and defaulting without needing a webhook.

---

#### - AppArmor Support (Stable)

**AppArmor profile selection** for Pods became generally available.

**Usage Example:**

```yaml
metadata:
  annotations:
    container.apparmor.security.beta.kubernetes.io/nginx: localhost/nginx-profile
```

> Profile must be preloaded on the node.

---

#### - `kubectl events` Command (Stable)

Introduced a dedicated `kubectl events` command for structured, timestamped viewing of Kubernetes events.

```bash
kubectl events
```

---

#### - NodeLogQuery (Alpha)

Early support for querying logs **across nodes** in a standardized, API-driven way (mostly for observability platforms).

---

## Notable Limitations in v1.28

Still **not GA or finalized**:

* **NodeLogQuery** still alpha
* **OpenTelemetry tracing** remains alpha
* **Per-container startup/shutdown ordering** still maturing
* **RuntimeClass** support minimal
* **PodSecurity Admission** lacks per-resource exceptions

---

## Deprecations or Removals in v1.28

* `policy/v1beta1` for **PodDisruptionBudget** removed
* More deprecated in-tree cloud provider code removed
* Old command-line flags and metrics removed for cleaned-up kubelet/kube-proxy/kube-controller-manager

