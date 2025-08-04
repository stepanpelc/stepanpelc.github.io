+++
title = "Kubernetes v1.18"
tags = []
date = "2020-03-25"
toc = true
+++


## Kubernetes v1.18.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Ingress in `networking.k8s.io/v1beta1` (Stable in v1.19)

Although not stable yet, v1.18 formalized support for **Ingress** in the `networking.k8s.io` API group with improvements preparing for stability.

> Deprecated `extensions/v1beta1` Ingress definitions in favor of `networking.k8s.io/v1beta1`.

---

#### - Server-side Apply (Beta)

**Server-side Apply** became **beta**, providing declarative configuration management with field ownership and conflict resolution.

**Example:**

```bash
kubectl apply --server-side -f deployment.yaml
```

---

#### - Topology-aware Hints for Services (Alpha)

Introduced **topology-aware routing hints**, helping traffic stay within the same zone/region when possible.

> Requires support in kube-proxy and controllers.

---

#### - CSI Volume Health Monitoring (Alpha)

CSI drivers could now surface **volume health status** via a gRPC interface, allowing Kubernetes to display volume errors in `PersistentVolumeClaim` status.

---

#### - kubectl diff (Stable)

`kubectl diff` reached GA, enabling dry-run-style review of manifest changes.

```bash
kubectl diff -f my-deployment.yaml
```

---

#### - Windows Enhancements

* HostProcess containers (Alpha): provided closer-to-host execution
* Improved logging and `kubectl exec` reliability
* Improved CSI proxy for Windows nodes

---

#### - Volume Snapshots (Beta → More Widespread Support)

Adoption of **VolumeSnapshot**, **VolumeSnapshotContent**, and **VolumeSnapshotClass** expanded across more CSI drivers.

---

#### - Graduated Metrics Stability Framework (Beta)

Enabled metric deprecation and removal policies, as well as documentation of metrics in Kubernetes code.

---

## Notable Limitations in v1.18

Still **not fully GA**:

* **Ingress** not yet in `networking.k8s.io/v1` (coming in 1.19)
* **PodSecurityPolicies** remained beta
* **VolumeSnapshot** still beta
* **Windows DaemonSet** support still pending
* **RuntimeClass** used only with advanced CRI setups

---

## Deprecations or Removals in v1.18

* `kubectl run` continued its modern behavior (only creating Pods unless `--generator` used)
* Deprecated use of `extensions/v1beta1` and `networking.k8s.io/v1beta1` for `Ingress`, with recommendation to move to `networking.k8s.io/v1`
* Several deprecated metrics and alpha APIs marked for future removal
