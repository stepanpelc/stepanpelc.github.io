+++
title = "Kubernetes v1.10"
tags = []
date = "2018-03-26"
toc = true
+++

## Kubernetes v1.10.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Dynamic Volume Provisioning and StorageClass (Stable)

Dynamic provisioning via **StorageClass** and support for block volumes became **generally available**.

**Example (Block Volume PVC):**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: block-volume
spec:
  accessModes: ["ReadWriteOnce"]
  volumeMode: Block
  storageClassName: fast
  resources:
    requests:
      storage: 1Gi
```

---

#### - Kubelet TLS Bootstrap (Stable)

The **TLS bootstrap mechanism** for automatically issuing kubelet certificates became stable.

> This improved security in kubeadm and custom bootstrapping workflows.

**Enabled by default:**

```yaml
--feature-gates=RotateKubeletServerCertificate=true
```

---

#### - Pod Priority and Preemption (Alpha)

Introduced the concept of assigning priorities to pods and preempting lower-priority ones when resources were constrained.

**PriorityClass Example:**

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 100000
globalDefault: false
description: "This priority class is for critical workloads."
```

---

#### - CSI (Container Storage Interface) Improvements (Still Alpha)

Expanded CSI support with volume lifecycle APIs and volume attachment logic.

> `CSIDriver` and `CSINode` resources were introduced in this version.

---

#### - CoreDNS (Beta)

**CoreDNS** reached **beta** status as a DNS provider and was integrated into `kubeadm`.

**To enable in kubeadm:**

```bash
kubeadm init --feature-gates=CoreDNS=true
```

---

#### - Topology-aware Persistent Volumes (Beta)

Improved volume scheduling by allowing Kubernetes to be aware of **zone/region constraints** in PVs.

**Example PV with zone constraint:**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-1
  labels:
    failure-domain.beta.kubernetes.io/zone: us-central1-a
```

---

#### - API Aggregation Layer (Stable)

Aggregation layer matured and reached stable status, solidifying support for extending the Kubernetes API server with additional APIs.

---

## Notable Limitations in v1.10

Still **not available or in progress**:

* **CustomResourceDefinitions (CRDs)** still `v1beta1`
* **PodSecurityPolicies** remained beta and disabled by default
* **Windows container support** remained beta with feature gaps (no DaemonSets, no HostPath)
* **Volume resizing** still not supported natively
* **CSI** still alpha

---

## Deprecations or Removals in v1.10

* Legacy `kube-dns` began being phased out in kubeadm configurations (replaced by **CoreDNS**).
* `extensions/v1beta1` for **DaemonSet**, **Deployment**, and **ReplicaSet** marked deprecated (use `apps/v1`)
* Kubelet flags for file-based TLS bootstrapping (like `--rotate-certificates`) became default in many environments
