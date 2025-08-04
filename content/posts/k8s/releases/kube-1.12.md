+++
title = "Kubernetes v1.12"
tags = []
date = "2018-09-27"
toc = true
+++

## Kubernetes v1.12.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Kubelet TLS Bootstrap and Certificate Rotation (Stable)

The full lifecycle for **automatic certificate creation and rotation** on kubelets became **generally available**.

> This improved node security with minimal manual intervention.

**Enable via kubelet flags:**

```bash
--rotate-certificates=true
--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf
```

---

#### - Topology Aware Volume Scheduling (Stable)

Volumes and pods could now be co-scheduled with better **zone/region awareness**, minimizing data transfer latency.

**Example PV Label:**

```yaml
metadata:
  labels:
    topology.kubernetes.io/zone: us-east-1a
```

---

#### - Volume Plugin Expansion (Beta → Stable)

**Persistent Volume Claim (PVC) expansion** became **stable** for supported volume types like `gce-pd`, `aws-ebs`, `csi`, and `azure-disk`.

**PVC Resize Example:**

```yaml
spec:
  resources:
    requests:
      storage: 20Gi
```

> Note: Requires `allowVolumeExpansion: true` in the associated `StorageClass`.

---

#### - Snapshot and Restore (Still Alpha)

VolumeSnapshot and VolumeSnapshotContent APIs were introduced as **alpha**, enabling backup/restore workflows via external snapshot controllers.

---

#### - kubeadm Improvements

* Experimental support for **multi-master (HA) cluster bootstrapping**
* Better control over etcd configuration and certificates
* Official support for **CoreDNS** by default

---

#### - RuntimeClass (Alpha)

Introduced the **RuntimeClass** resource to support selecting container runtimes (e.g., gVisor, Kata Containers) per pod.

**Example:**

```yaml
apiVersion: node.k8s.io/v1beta1
kind: RuntimeClass
metadata:
  name: gvisor
handler: gvisor
```

---

## Notable Limitations in v1.12

Still **not yet available** or fully matured:

* **PodSecurityPolicies** remained beta
* **CRDs** were still at `v1beta1` (no structural schema validation yet)
* **CSI** was beta with limited driver coverage
* **Windows** container support still not GA
* **Volume snapshot/restore** was still alpha
* **RuntimeClass** experimental and dependent on specific CRI implementations

---

## Deprecations or Removals in v1.12

* `kube-dns` no longer used in kubeadm by default (fully replaced by **CoreDNS**)
* `--insecure-bind-address` and other insecure kubelet settings deprecated
* Deprecated alpha annotations for affinity, scheduling, and resource limits removed or replaced by GA fields
