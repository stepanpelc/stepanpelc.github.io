+++
title = "Kubernetes v1.16"
tags = []
date = "2019-09-18"
toc = true
+++

## Kubernetes v1.16.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - CustomResourceDefinition (CRD) `v1` API (Fully GA)

The `apiextensions.k8s.io/v1` version of **CRDs** officially became **fully GA**, solidifying:

* OpenAPI v3 schema validation
* Defaulting, pruning, versioning
* Subresources support (`status`, `scale`)

> Older `v1beta1` CRDs were now deprecated.

---

#### - Apps APIs `v1` (Stable)

The following workload controllers were **officially available in `apps/v1`** and **older API versions deprecated**:

* `Deployment`
* `DaemonSet`
* `StatefulSet`
* `ReplicaSet`

**All new manifests should now use `apps/v1`.**

**Example (Deployment):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 3
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

#### - EndpointSlices (Beta)

Introduced **EndpointSlice** resource to replace `Endpoints` for scalable, extensible service discovery.

**Example:**

```yaml
apiVersion: discovery.k8s.io/v1beta1
kind: EndpointSlice
metadata:
  name: example-slice
addressType: IPv4
endpoints:
  - addresses:
      - 10.1.2.3
ports:
  - name: http
    port: 80
```

> Automatically managed by kube-proxy and CoreDNS.

---

#### - CSI Inline Volume Support (Beta)

Added support for **CSI ephemeral volumes** in Pod specs, similar to `emptyDir` or `configMap`.

**Example:**

```yaml
volumes:
  - name: csi-inline
    csi:
      driver: example.csi.driver
      volumeAttributes:
        foo: "bar"
```

---

#### - Windows Improvements (Beta+)

* Beta support for **CSI on Windows**
* Support for `kubectl logs` and `exec` on Windows pods
* Improved `kube-proxy` in Windows mode

---

#### - kubeadm Enhancements

* Default CoreDNS and etcd versions upgraded
* Support for custom etcd certificates
* Better upgrade error handling and preflight checks

---

## Notable Limitations in v1.16

Still **not fully available**:

* **Volume Snapshots** remained alpha
* **PodSecurityPolicies** still beta and not enabled by default
* **Windows** DaemonSets still not supported
* **CRI-O**, **containerd** adoption varied across distros
* **RuntimeClass** still limited in adoption

---

## Deprecations or Removals in v1.16

#### - API Deprecations:

Removed or deprecated the following `v1beta1`/`v1alpha1` APIs:

* `Deployment`, `DaemonSet`, `StatefulSet` in `extensions/v1beta1` → use `apps/v1`
* `NetworkPolicy` in `extensions/v1beta1` → use `networking.k8s.io/v1`
* `PodSecurityPolicy` in `extensions/v1beta1` → use `policy/v1beta1`
* `CustomResourceDefinition` in `v1beta1` → use `v1`

> **All users were advised to update their manifests to `v1` APIs where available.**
