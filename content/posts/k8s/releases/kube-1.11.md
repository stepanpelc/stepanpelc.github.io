+++
title = "Kubernetes v1.11"
tags = []
date = "2018-06-27"
toc = true
+++


## Kubernetes v1.11.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - IPVS Mode in kube-proxy (Stable)

**IPVS-based service proxy** became **generally available**, offering significant performance improvements over `iptables` mode for high-scale clusters.

**Enable IPVS Mode:**

```bash
kube-proxy --proxy-mode=ipvs
```

> Required `conntrack` and `ipvsadm` on the node.

---

#### - CoreDNS (Stable, kubeadm default)

**CoreDNS** became the **default DNS provider** for kubeadm-based clusters, replacing `kube-dns`.

> Brought better configurability, faster startup times, and pluggable design.

---

#### - Custom Resource Definitions (CRDs) (Stable)

The **CRD API** under `apiextensions.k8s.io/v1beta1` reached **general availability**, empowering users to define and manage their own Kubernetes-style APIs.

**Example CRD:**

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  versions:
    - name: v1
      served: true
      storage: true
  scope: Namespaced
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
```

---

#### - Pod Priority and Preemption (Beta)

Allowed pods with higher priority to **preempt** lower-priority pods if needed for scheduling.

> This helped mission-critical workloads avoid starvation during resource pressure.

---

#### - CSI Plugins Support (Beta)

CSI volume plugins matured with better integration for attach/detach, mount, provisioning, and lifecycle handling.

> Enabled wider testing of CSI-compliant drivers for cloud and bare-metal storage.

---

#### - PersistentVolume Expansion (Beta)

Allowed users to **resize PVCs** without data loss when backed by a compatible volume plugin.

**Example:**

```yaml
spec:
  resources:
    requests:
      storage: 10Gi
```

> Requires volume plugin to support `AllowVolumeExpansion: true`.

---

## Notable Limitations in v1.11

Still **not fully available**:

* **PodSecurityPolicies** remained beta
* **Windows containers** still beta (no HostPath, limited scheduling support)
* **Volume snapshot/restore** still alpha
* **CRD versioning** was early-stage (no `v1` yet)
* CSI still required some manual provisioning

---

## Deprecations or Removals in v1.11

* **kube-dns** deprecated in kubeadm; **CoreDNS** used by default
* Older workload APIs (e.g., `extensions/v1beta1`) for Deployments, DaemonSets, and ReplicaSets were discouraged
* Deprecated support for legacy metrics and initial API aggregation behaviors as Metrics Server matured
