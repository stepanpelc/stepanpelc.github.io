+++
title = "Kubernetes v1.14"
tags = []
date = "2019-03-25"
toc = true
+++


## Kubernetes v1.14.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Windows Node Support (Stable)

**Windows Server containers** reached **general availability**, allowing scheduling of Windows workloads on Windows nodes in the same cluster as Linux.

> Supported `Deployment`, `Service`, and `Pod` objects on Windows, with some limitations.

**Windows Pod Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: win-pod
spec:
  nodeSelector:
    "kubernetes.io/os": windows
  containers:
    - name: powershell
      image: mcr.microsoft.com/windows/servercore:ltsc2019
      command: ["powershell", "-Command", "Start-Sleep -Seconds 3600"]
```

---

#### - PersistentVolume Expansion (Stable)

**PVC resizing** for more volume plugins became fully supported in stable releases.

> Resize required volume plugin support and proper `StorageClass` configuration.

**Example:**

```yaml
spec:
  resources:
    requests:
      storage: 50Gi
```

---

#### - kubectl Plugin System (Stable)

Users could now extend `kubectl` with custom subcommands by placing executables like `kubectl-foo` in their `PATH`.

```bash
kubectl myplugin
# Calls: kubectl-myplugin
```

---

#### - CustomResourceDefinition (CRD) v1 API (Beta)

A new `apiextensions.k8s.io/v1` version for CRDs was introduced, enabling:

* Structural schema validation
* Subresources (e.g. status and scale)
* Better openAPI generation

**CRD with validation:**

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com
spec:
  group: stable.example.com
  names:
    plural: crontabs
    singular: crontab
    kind: CronTab
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                schedule:
                  type: string
```

---

#### - Topology Aware Scheduling for Volumes (Stable)

Improved volume binding logic ensured pod scheduling considered volume constraints such as region/zone.

---

#### - Device Plugin Registration (Stable)

The **Device Plugin** framework reached stable status, enabling vendors to offer hardware accelerators (e.g., GPUs, FPGAs) as schedulable resources.

---

## Notable Limitations in v1.14

Still **not available or not GA**:

* **PodSecurityPolicies** remained beta
* **Volume snapshots** still alpha
* **RuntimeClass** remained alpha
* **CRI-O** and `containerd` support depended on environment
* **Mutating webhooks for CRDs** still in development

---

## Deprecations or Removals in v1.14

* `kube-dns` fully deprecated in kubeadm (replaced by CoreDNS)
* `kubectl run` changed to only create Pods by default (removed generator-based creation of deployments, etc.)
* Legacy flags for cloud providers deprecated, moving responsibility to **external cloud controller managers**

