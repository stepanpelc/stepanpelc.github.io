+++
title = "Kubernetes v1.15"
tags = []
date = "2019-06-19"
toc = true
+++

## Kubernetes v1.15.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - CustomResourceDefinition (CRD) `v1` API (Stable)

The `apiextensions.k8s.io/v1` version of **CRDs** became **stable**, offering:

* Full OpenAPI v3 schema validation
* Defaulting
* Subresources like `status` and `scale`
* Support for multiple versions

**CRD with validation and subresources:**

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
  scope: Namespaced
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
      subresources:
        status: {}
        scale:
          specReplicasPath: ".spec.replicas"
          statusReplicasPath: ".status.replicas"
```

---

#### - NodeLocal DNSCache (Beta)

Introduced a **per-node DNS caching DaemonSet** that reduced latency and DNS traffic to the cluster DNS service.

> Especially useful in large clusters or under DNS load.

**Enable via kubelet:**

```bash
--cluster-dns=169.254.20.10
```

---

#### - Volume Cloning (Beta)

Added ability to **clone an existing PVC** when provisioning a new one, supported by CSI drivers.

**PVC Clone Example:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: cloned-pvc
spec:
  dataSource:
    name: source-pvc
    kind: PersistentVolumeClaim
    apiGroup: ""
  accessModes: ["ReadWriteOnce"]
  resources:
    requests:
      storage: 5Gi
```

---

#### - kubeadm Improvements

* Configurable component configs via versioned YAML
* Automatic etcd version detection
* Support for external CA and certificate management
* More predictable upgrade workflows

---

#### - kubectl Diff (Stable)

Introduced `kubectl diff`, a new built-in command to preview changes before applying manifests.

```bash
kubectl diff -f my-deployment.yaml
```

---

## Notable Limitations in v1.15

Still **not available or under development**:

* **PodSecurityPolicies** still beta
* **Volume snapshots** remained alpha
* **Windows** support improving but lacking in DaemonSets and hostPath
* **RuntimeClass** still alpha
* No built-in multi-tenancy or hierarchical namespace support

---

## Deprecations or Removals in v1.15

* Deprecated `kubectl run` behavior: it no longer created Deployments, only Pods
* Deprecated the `extensions/v1beta1` versions of `Deployment`, `DaemonSet`, and `ReplicaSet` (use `apps/v1`)
* `cloud-provider` flags deprecated for in-tree cloud integrations (e.g., `--cloud-provider`, `--cloud-config`)

