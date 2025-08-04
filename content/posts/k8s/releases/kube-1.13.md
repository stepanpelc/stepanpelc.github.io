+++
title = "Kubernetes v1.13"
tags = []
date = "2018-12-03"
toc = true
+++


## Kubernetes v1.13.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - CoreDNS (Stable, Default in kubeadm)

**CoreDNS** officially became the **default DNS addon** in kubeadm, replacing `kube-dns`.

> CoreDNS is more memory efficient, supports plugin chains, and has a cleaner config.

---

#### - Kubelet TLS Bootstrap & Certificate Rotation (Fully Stable)

Certificate bootstrapping and rotation, including renewal for client and server certificates used by the kubelet, are now fully stable and widely adopted.

---

#### - Container Storage Interface (CSI) APIs (Beta)

The **CSI volume plugin system** became **beta**, making it possible to install and manage storage drivers independent of Kubernetes releases.

**Required enabling the feature gate:**

```bash
--feature-gates=CSINodeInfo=true,CSIDriverRegistry=true
```

---

#### - kubeadm GA

**kubeadm** graduated to **General Availability**, offering stable tooling for:

* Cluster bootstrap (single or multi-master)
* Join/leave workflows
* TLS cert management
* CoreDNS installation
* Configuration via `ClusterConfiguration` objects

```bash
kubeadm init
```

---

#### - Kubelet Plugin Registration (Beta)

Enabled **dynamic plugin discovery** for device and volume plugins, a precursor to full CSI support.

Plugins register themselves via UNIX domain sockets under `/var/lib/kubelet/plugins_registry`.

---

#### - RuntimeClass (Still Alpha)

Provided an interface to select a container runtime (e.g., gVisor, Kata) for individual pods.

---

## Notable Limitations in v1.13

Still **not available or experimental**:

* **PodSecurityPolicies** not enabled by default
* **Volume snapshot/restore** still alpha
* **Windows** container support still in beta (no DaemonSets, no HostPath)
* **CRD validation schemas** not yet GA
* **RuntimeClass** alpha only; adoption was low due to CRI constraints

---

## Deprecations or Removals in v1.13

* **kube-dns** deprecated in kubeadm-managed clusters (replaced by CoreDNS)
* `--insecure-bind-address` for the kubelet officially deprecated
* Deprecated kubelet command-line flags started being removed in favor of configuration files
* Old `cloud-provider` integrations began migration to external cloud-controller-managers
