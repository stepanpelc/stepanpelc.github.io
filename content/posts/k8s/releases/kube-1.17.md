+++
title = "Kubernetes v1.17"
tags = []
date = "2019-12-09"
toc = true
+++

## Kubernetes v1.17.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Volume Snapshot APIs (Beta)

Introduced **beta support** for Kubernetes-native **volume snapshot**, restore, and management workflows via CRDs:

* `VolumeSnapshot`
* `VolumeSnapshotContent`
* `VolumeSnapshotClass`

> CSI driver required for snapshot capability.

**Example VolumeSnapshot:**

```yaml
apiVersion: snapshot.storage.k8s.io/v1beta1
kind: VolumeSnapshot
metadata:
  name: my-snapshot
spec:
  volumeSnapshotClassName: csi-snapclass
  source:
    persistentVolumeClaimName: my-pvc
```

---

#### - CustomResource Subresources and Defaulting (Stable)

CustomResourceDefinitions (`apiextensions.k8s.io/v1`) now fully support:

* `default` values via OpenAPI schema
* Subresources: `status`, `scale`
* Declarative defaulting and pruning

---

#### - Metrics Stability Framework (Alpha → Beta)

Introduced **graduation criteria** and standardization for Kubernetes metrics. This helped signal which metrics are stable and safe to rely on.

> Prometheus users now had clearer guidance on which metrics are stable.

---

#### - kubectl Events Sorting (Stable)

`kubectl get events` now sorts by timestamp **by default**, making it easier to interpret real-time events without needing custom scripts.

---

#### - Improvements to Windows Support

* Stable support for CSI proxy on Windows
* Expanded CSI support (e.g., inline ephemeral volumes)
* Improved process handling in kubelet and logging

---

#### - API Server Identity via BoundServiceAccountToken (Beta)

Added support for short-lived, **auditable, and revocable tokens** via the `TokenRequest` API and projected service account tokens.

**Projected Volume Token Example:**

```yaml
volumes:
  - name: sa-token
    projected:
      sources:
        - serviceAccountToken:
            audience: api
            expirationSeconds: 3600
            path: token
```

---

## Notable Limitations in v1.17

Still **not fully available or GA**:

* **PodSecurityPolicies** remained beta
* **Volume snapshots** only beta and CSI-dependent
* **Windows DaemonSets** still not supported
* No hierarchical namespaces or multi-tenant controls
* **RuntimeClass** adoption limited to advanced runtimes

---

## Deprecations or Removals in v1.17

* Deprecated old metrics in favor of versioned, documented stable metrics
* More removal of `extensions/v1beta1` APIs (e.g., `Ingress`) began in favor of `networking.k8s.io/v1beta1`
* Early warning that **PodSecurityPolicy** would eventually be deprecated (actual deprecation came in 1.21)
