+++
title = "Kubernetes v1.31"
tags = []
date = "2024-08-13"
toc = true
+++

## Kubernetes v1.31.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - AppArmor Support (Stable)

AppArmor support reached **GA**, enabling administrators to enforce Linux security profiles via the `securityContext` field instead of annotations.
**Usage Example:**

```yaml
securityContext:
  appArmorProfile:
    type: RuntimeDefault
    name: my-custom-profile
```

([Kubernetes][1])

#### - Improved Ingress Connectivity via kube-proxy (Stable)

Kube‑proxy now handles connection draining on terminating nodes more reliably for services with `externalTrafficPolicy=Cluster`, reducing traffic drops during scale-down or maintenance. No config changes required; it's enabled by default.
**Example:**

```yaml
spec:
  type: LoadBalancer
  externalTrafficPolicy: Cluster
```

([Kubernetes][1])

#### - PodHealthyPolicy for PodDisruptionBudget (Stable)

Introduced more granular PDB behavior: allows control over evictions even when pods are healthy but not yet ready.
**Example:**

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: myapp-pdb
spec:
  minAvailable: 2
  unhealthyPodEvictionPolicy: Delay jobs-until-ready
  selector:
    matchLabels:
      app: myapp
```

([perfectscale.io][2])

#### - VolumeAttributesClass (Beta)

Feature to define reusable CSI volume parameter configurations separate from PVCs.
**Example:**

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: VolumeAttributesClass
metadata:
  name: fast-ssd
parameters:
  type: pd-ssd
```

([Kubernetes][3])

#### - ValidatingAdmissionPolicy (Beta)

Enabling CEL-based in-cluster validation policies without external webhooks.
**Example:**

```yaml
apiVersion: admissionregistration.k8s.io/v1beta1
kind: ValidatingAdmissionPolicy
metadata:
  name: restrict-replicas
spec:
  matchConstraints:
    resourceRules:
      - apiGroups: ["apps"]
        apiVersions: ["v1"]
        resources: ["deployments"]
        operations: ["CREATE", "UPDATE"]
  validations:
    - expression: "object.spec.replicas <= 10"
      message: "Replicas must not exceed 10"
```

([Kubernetes][3], [Sysdig][4])

---

### Key Beta and Alpha Features

* **Kubelet parallel image pull limits (Beta):** Configure `maxParallelImagePulls` to reduce cold start delays.
  ([devtron.ai][5])
* **Static CPU manager policy (Beta):** Distribute CPUs across cores for high-performance workloads.
  ([Kubernetes][3])
* **Pod-level resource limits (Alpha):** Enforcement of resource caps at individual pod scope.
  ([Sysdig][4])

---

## Notable Limitations in v1.31

Still **not stable or fully enabled by default**:

* Some enterprise features like **Pod-level resource limits** remain **alpha**
* **NodeLogQuery**, **tracing**, and other observability features are still nascent
* **PodSecurity Admission**, **RuntimeClass** and multi-networking remain evolving

---

## Deprecations or Removals in v1.31

#### - Removed In-Tree Cloud Provider Code

All remaining cloud provider integrations (e.g., AWS, GCE, Azure in-tree code) were removed—external CCMs are now required.
([Kubernetes][1], [Kubernetes][3])

#### - `--status.nodeInfo.kubeProxyVersion` Field (Deprecated)

This field on Node status is deprecated and now disabled by default via feature gate.
([Kubernetes][3])

#### - CephFS and Ceph RBD Volume Plugins Removed

The legacy non‑CSI CephFS and RBD volume plugins were removed; users must migrate to third-party CSI drivers.
([Kubernetes][3])

#### - Scheduler Plugins Deprecated

Non‑CSI volume limit scheduler plugins (`AzureDiskLimits`, `EBSLimits`, etc.) deprecated in favor of unified `NodeVolumeLimits`.
([Kubernetes][3])

#### - Other v1beta1 API Versions Removed

As part of the release cleanup, older v1beta1 versions of APIs such as Admission Webhooks were removed in favor of v1 stable counterparts.
([Kubernetes][3])

---
[1]: https://kubernetes.io/blog/2024/08/13/kubernetes-v1-31-release/?utm_source=chatgpt.com "Kubernetes v1.31: Elli"
[2]: https://www.perfectscale.io/blog/kubernetes-v1-31?utm_source=chatgpt.com "Kubernetes v1.31: What's New and Improved?"
[3]: https://kubernetes.io/blog/2024/07/19/kubernetes-1-31-upcoming-changes/?utm_source=chatgpt.com "Kubernetes Removals and Major Changes In v1.31"
[4]: https://sysdig.com/blog/whats-new-kubernetes-1-31/?utm_source=chatgpt.com "Kubernetes 1.31 – What's new?"
[5]: https://devtron.ai/blog/kubernetes-1-31-whats-new/?utm_source=chatgpt.com "Kubernetes 1.31: Here's what you should know about!"
