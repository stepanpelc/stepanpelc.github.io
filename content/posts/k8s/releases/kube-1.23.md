+++
title = "Kubernetes v1.23"
tags = []
date = "2021-12-07"
toc = true
+++


## Kubernetes v1.23.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Ephemeral Containers (Beta → GA)

**Ephemeral containers** reached **beta** (not GA yet), enabling live debugging of running pods **without restarting them**.

**Usage:**

```bash
kubectl debug -it mypod --image=busybox --target=main-container
```

> Use case: production pod debugging without lifecycle disruption.

---

#### - Generic Ephemeral Volumes (Stable)

**Generic ephemeral volumes** became **generally available**, allowing pods to use inline `PersistentVolumeClaim` templates.

**Example:**

```yaml
volumes:
  - name: scratch
    ephemeral:
      volumeClaimTemplate:
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi
```

---

#### - Kubernetes Event v1 API (Stable)

The `events.k8s.io/v1` API became **stable**, replacing the legacy `core/v1` Event objects.

> More structured events with better tooling support.

---

#### - HorizontalPodAutoscaler v2 API (Beta → Stable Candidate)

The **`autoscaling/v2` API** gained support for multiple metrics (CPU, memory, custom, external) in the same HPA.

**Example:**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Pods
      pods:
        metric:
          name: requests_per_second
        target:
          type: AverageValue
          averageValue: 100
```

---

#### - Kubelet Credential Providers (Beta → Stable)

Credential plugin support for kubelet container runtime registry auth became **stable**, improving private image pulling securely.

---

#### - PodSecurity Admission (Beta)

**PodSecurity Admission** controller replaced deprecated PSP and was promoted to **beta**.

**Example namespace label:**

```yaml
metadata:
  labels:
    pod-security.kubernetes.io/enforce: "baseline"
```

---

## Notable Limitations in v1.23

Still **not fully GA**:

* **PodSecurity Admission** still beta
* **Ephemeral containers** beta (GA in v1.25)
* **Seccomp default** still alpha
* **Volume snapshots** GA, but still maturing in ecosystem
* **CRI v1 API** not yet default

---

## Deprecations or Removals in v1.23

* **Dockershim Removed**:
  Kubernetes removed support for **dockershim**, requiring users to switch to container runtimes like `containerd` or `CRI-O`.

> Docker images still work — only Docker as the **runtime** was removed.

* Deprecated `batch/v1beta1` CronJob: fully removed (use `batch/v1`)
* Deprecated legacy metrics and alpha kubelet flags removed
* Deprecated `v1beta1` APIs for HPAs, leases, and more removed if stable APIs exist
