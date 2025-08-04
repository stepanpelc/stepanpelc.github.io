+++
title = "Kubernetes v1.21"
tags = []
date = "2021-04-08"
toc = true
+++


## Kubernetes v1.21.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - CronJobs (`batch/v1`) (Stable)

The **CronJob** resource graduated to **stable** in `batch/v1`, replacing `batch/v1beta1`.

**Example CronJob (v1):**

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: busybox
              args:
                - /bin/sh
                - -c
                - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

---

#### - Immutable Secrets & ConfigMaps (Stable)

Previously beta in v1.19, this enhancement became **fully stable**, allowing immutability to be declared on `Secret` and `ConfigMap` objects for performance and consistency.

---

#### - PodResource API (Stable)

The `kubelet`'s **PodResources API**, which provides information about resource allocation on a node (e.g. GPUs, CPUs), became stable.

> Used by observability and scheduling tools.

---

#### - Graceful Node Shutdown (Beta)

Introduced the **node shutdown handler**, enabling kubelet to gracefully evict pods during system shutdown.

> Required systemd integration and enabled via kubelet flags.

---

#### - PodSecurityPolicy (Deprecated)

**PodSecurityPolicy (PSP)** was **officially deprecated** in v1.21 and planned for **removal in v1.25**.

> Replacement tools (e.g., OPA Gatekeeper, Kyverno) were recommended.

---

#### - Seccomp Default (Alpha)

Default `seccomp` profiles were enabled by default for all containers (still **alpha**) to enhance sandboxing.

> Can be controlled via kubelet `--seccomp-default=true`.

---

#### - PersistentVolume Health Monitoring (Alpha)

Exposed volume health status from CSI drivers to Kubernetes for alerting and remediation.

---

#### - kubectl Debug Enhancements (Beta)

Support for **ephemeral containers** and pod-level debugging via `kubectl debug` was expanded.

---

## Notable Limitations in v1.21

Still **not GA** or fully supported:

* **Ephemeral containers** alpha
* **Seccomp default** alpha
* **PodSecurityPolicy** deprecated but still available
* **RuntimeClass** and multi-runtime support limited
* **CRI v1 API** still in progress

---

## Deprecations or Removals in v1.21

* **PodSecurityPolicy** deprecated (`policy/v1beta1`) – removal scheduled for v1.25
* Deprecated `batch/v1beta1` CronJob (replaced by `batch/v1`)
* `kubectl run` continued with restricted functionality (only Pods unless explicitly extended)
* `CSIInlineVolume`, `ServiceAccountIssuerDiscovery` moved toward GA with deprecation of older behaviors
