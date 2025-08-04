+++
title = "Kubernetes v1.20"
tags = []
date = "2020-12-08"
toc = true
+++


## Kubernetes v1.20.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Volume Snapshot APIs (Stable)

CSI **VolumeSnapshot**, **VolumeSnapshotContent**, and **VolumeSnapshotClass** graduated to **GA** under the `snapshot.storage.k8s.io/v1` API.

**Example VolumeSnapshot:**

```yaml
apiVersion: snapshot.storage.k8s.io/v1
kind: VolumeSnapshot
metadata:
  name: my-snapshot
spec:
  volumeSnapshotClassName: csi-snapclass
  source:
    persistentVolumeClaimName: my-pvc
```

---

#### - Process PID Limits (Stable)

Kubernetes now supports setting **per-Pod PID limits** to prevent fork bombs or noisy neighbors.

**Example Pod with PID limit:**

```yaml
spec:
  containers:
    - name: nginx
      image: nginx
  securityContext:
    runAsUser: 1000
  overhead:
    pid: 100
```

Enabled via:

```bash
--enable-controller-attach-detach=true
```

---

#### - Immutable Secrets and ConfigMaps (Stable)

Improved performance and predictability by allowing `Secrets` and `ConfigMaps` to be **marked as immutable**.

**Example:**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
immutable: true
data:
  key: value
```

---

#### - API Priority and Fairness (Beta)

Introduced **fair request queuing** to the Kubernetes API server, improving control plane responsiveness under high load.

> Enabled via `--enable-priority-and-fairness=true` on API server.

---

#### - kubectl Debug (Beta)

New `kubectl debug` command for ephemeral containers and troubleshooting pods live.

```bash
kubectl debug pod/nginx --image=busybox --target=nginx
```

---

#### - CronJobs (v2 Controller, Beta)

The **CronJob controller** was rewritten for better performance and reliability.

> Continued under `batch/v1beta1` but `batch/v1` was introduced in v1.21.

---

## Notable Limitations in v1.20

Still **not yet GA** or widely supported:

* **Ephemeral containers** still alpha
* **PodSecurityPolicies** deprecated (GA replacement TBD)
* **RuntimeClass** adoption limited
* **Seccomp default policy** still alpha
* **CRI v1 API** still beta

---

## Deprecations or Removals in v1.20

* **Dockershim deprecated**
  Kubernetes announced it would remove support for Docker as a container runtime after v1.23.

> Affects container runtime, not Docker images or Dockerfiles.

* `extensions/v1beta1` API for `Ingress` and related objects fully removed (moved to `networking.k8s.io/v1`)
* `batch/v1beta1` CronJobs deprecated in favor of `batch/v1` (to become GA in 1.21)
