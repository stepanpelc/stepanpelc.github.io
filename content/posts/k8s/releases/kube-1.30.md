
+++
title = "Kubernetes v1.30"
tags = []
date = "2024-04-17"
toc = true
+++

## Kubernetes v1.30.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Sidecar Container Lifecycle (Stable)

The long-awaited **sidecar lifecycle** feature is now **generally available**.

* Allows certain containers (like sidecars) to **continue running after init containers finish** or to **terminate independently** of the main containers.
* Improves startup/shutdown ordering in service mesh, logging, and monitoring sidecars.

**Key feature:** `restartPolicy: Always` sidecars no longer block pod completion by default.

---

#### - ReadWriteOncePod Access Mode (Stable)

**`ReadWriteOncePod`** volume access mode is now stable.

* Guarantees a volume is **mounted by only one pod** at a time in the entire cluster
* Helps enforce stronger data safety in scenarios like databases

**Example PVC:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pvc
spec:
  accessModes:
    - ReadWriteOncePod
  resources:
    requests:
      storage: 10Gi
```

---

#### - VolumeAttributesClass (Beta)

Introduced **VolumeAttributesClass** for defining CSI volume parameters separately from PVCs.

* Enables storage team to define storage classes with consistent parameters
* Allows reusable configurations (e.g., performance tiers)

**Example:**

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: VolumeAttributesClass
metadata:
  name: fast-ssd
parameters:
  csi.storage.k8s.io/fstype: xfs
  type: pd-ssd
```

---

#### - Automatic Volume Resize on Node (Stable for CSI)

Kubelet can now **automatically resize** CSI volumes on the node without requiring pod restarts or manual intervention.

---

#### - KMS v2 API for Encryption Providers (Stable)

The **KMSv2 API** is now stable, providing:

* **Envelope encryption**
* Improved performance
* Key rotation without cluster restart

> Requires compatible KMS plugins (e.g., HashiCorp Vault, AWS KMS, Azure Key Vault)

---

#### - kubectl `--subresource` Flag (Stable)

`kubectl` now supports the `--subresource` flag for working with subresources like `status`, `scale`.

```bash
kubectl get deployment nginx --subresource=status
```

---

## Notable Limitations in v1.30

Still **not GA or experimental**:

* **NodeLogQuery API** still alpha
* **Tracing and OpenTelemetry integration** not GA
* **PodSecurity Admission** lacks fine-grained RBAC policies
* **Multi-networking** not supported natively (CNI-dependent)
* **RuntimeClass** still not widely adopted

---

## Deprecations or Removals in v1.30

* **CSI Migration** completed — all in-tree volume plugins fully deprecated
* Removed `storage.k8s.io/v1beta1` and `v1alpha1` APIs for storage classes
* Legacy `kubectl` generators and `--export` flag permanently removed
* Deprecated metrics and CLI flags removed across control plane components

