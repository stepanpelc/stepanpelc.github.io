+++
title = "Kubernetes v1.25"
tags = []
date = "2022-08-23"
toc = true
+++

## Kubernetes v1.25.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - PodSecurity Admission (Stable)

The **PodSecurity Admission controller** reached **general availability**, replacing the deprecated PodSecurityPolicy mechanism.

* Enforces standardized security levels: `restricted`, `baseline`, `privileged`
* Uses namespace labels to enforce policy

**Example namespace labels:**

```yaml
metadata:
  labels:
    pod-security.kubernetes.io/enforce: "restricted"
    pod-security.kubernetes.io/audit: "baseline"
    pod-security.kubernetes.io/warn: "privileged"
```

---

#### - Ephemeral Containers (Beta → Stable)

**Ephemeral containers** for live troubleshooting reached **general availability**.

* Used for debugging running pods
* Can be injected without restarting the pod

**Usage Example:**

```bash
kubectl debug -it mypod --image=busybox --target=app-container
```

---

#### - StatefulSet Start Ordinals (Stable)

StatefulSets now support **`podManagementPolicy: OrderedReady`** with control over the starting ordinal.

**Feature Benefits:**

* Restart pods from a given index instead of 0
* Useful for controlled rollout and recovery

---

#### - CRI v1 API (Stable)

The **Container Runtime Interface (CRI)** graduated to **v1**, standardizing the runtime communication between kubelet and container runtimes like `containerd` and `CRI-O`.

---

#### - CSI Migration (Stable for More Plugins)

CSI migration was declared stable for additional in-tree plugins:

* AWS EBS
* Azure Disk
* GCE PD
* OpenStack Cinder

> All new clusters should use **CSI drivers** instead of in-tree storage plugins.

---

#### - kubelet Credential Provider API (Stable)

Credential providers used by the kubelet for pulling private images now follow a **stable API**, improving extensibility and security.

---

## Major Removals in v1.25

#### - PodSecurityPolicy (Removed)

The long-deprecated **PodSecurityPolicy (PSP)** feature was **removed**.

> Users must migrate to **PodSecurity Admission** or external policy engines like OPA/Gatekeeper.

---

## Notable Limitations in v1.25

Still **not yet GA** or widespread:

* **Volume snapshot backup/restore** still maturing in ecosystem
* **RuntimeClass** support remains niche
* **Multi-runtime (per-pod)** scheduling still limited

---

## Deprecations or Removals in v1.25

* **PodSecurityPolicy** (`policy/v1beta1`) was fully removed
* **In-tree CSI plugins** for EBS, GCE, Azure, etc. marked deprecated in favor of CSI equivalents
* Removed alpha and legacy metrics and CLI flags
* Removed deprecated `events.k8s.io/v1beta1`, use `events.k8s.io/v1`
