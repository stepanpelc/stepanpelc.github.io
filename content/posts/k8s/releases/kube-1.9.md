+++
title = "Kubernetes v1.9"
tags = []
date = "2017-12-15"
toc = true
+++


## Kubernetes v1.9.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Workloads API (`apps/v1`) Becomes Default

The **apps/v1** API group became the default for core workload resources:

* Deployments
* DaemonSets
* ReplicaSets
* StatefulSets

All future versions should use `apps/v1` instead of `extensions/v1beta1`.

**Example Deployment (Updated API):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
```

---

#### - Windows Container Support (Beta)

Introduced **beta support for Windows Server containers**. Windows nodes could now join a Kubernetes cluster and run Windows-based workloads.

> Required containerd or Docker EE for Windows, and Kubernetes control plane on Linux.

---

#### - CSI (Container Storage Interface) Alpha

Initial support for **CSI plugins**, decoupling volume plugin development from the Kubernetes release cycle.

**Example of enabling CSI:**

```bash
--feature-gates=CSIPersistentVolume=true
```

> CSI reached GA in later releases, but was usable experimentally via `PersistentVolume` resources.

---

#### - Volume Snapshots (Alpha)

Introduced alpha APIs for creating volume snapshots and restoring from them.

> Not yet available via standard PVCs, but enabled experimentation with backup/restore workflows.

---

#### - CoreDNS Support (Alpha)

Started offering **CoreDNS** as an alternative to `kube-dns`.

> CoreDNS brought configurability, better memory usage, and plugin-based DNS behavior.

---

#### - API Aggregation Layer Improvements

Improved performance and reliability of aggregated APIs (like Metrics Server, Service Catalog).

---

#### - kubeadm (Continued Beta)

Added support for:

* TLS bootstrapping by default
* More flexible configuration (via config files)
* Better upgrade support (`kubeadm upgrade` enhancements)

---

## Notable Limitations in v1.9

Still **not yet available** or stable:

* **CustomResourceDefinitions (CRDs)** still at `v1beta1`
* **Volume expansion** not available
* **Windows** support limited (e.g., no DaemonSets)
* **PodSecurityPolicies** not enabled by default
* **CSI** was alpha, with limited driver availability
* **Audit webhook** still in development

---

## Deprecations or Removals in v1.9

* Legacy workload APIs such as `extensions/v1beta1` for Deployments and DaemonSets were marked deprecated in favor of `apps/v1`
* `kube-dns` began its deprecation path as **CoreDNS** gained traction
* Older alpha features like PetSet were removed

