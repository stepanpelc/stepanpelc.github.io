+++
title = "Kubernetes v1.22"
tags = []
date = "2021-08-04"
toc = true
+++

## Kubernetes v1.22.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Server-side Apply (Stable)

**Server-side Apply (SSA)** reached **general availability**, enabling:

* Declarative configuration management
* Field ownership tracking
* Improved conflict resolution

**Usage:**

```bash
kubectl apply --server-side -f deployment.yaml
```

---

#### - External Credential Providers (Stable)

The **exec plugin mechanism** for cloud credential providers became stable, allowing plugins (like `aws-iam-authenticator`) to provide credentials dynamically for `kubectl` and clients.

> This allows secure, pluggable credential acquisition without embedding secrets.

---

#### - CronJob `batch/v1` Only (Deprecated v1beta1)

`batch/v1` became the **only supported API** for CronJobs. The older `batch/v1beta1` was **removed**.

---

### Key Beta Features

#### - Ephemeral Containers (Beta)

Enabled **debug containers** to be added to a running Pod for troubleshooting, without restarting it.

```bash
kubectl debug -it nginx --image=busybox --target=nginx
```

> Requires `EphemeralContainers` feature gate enabled.

---

#### - PodSecurity Admission (Alpha)

Introduced a new **PodSecurity** admission controller to eventually replace PodSecurityPolicies.

> Based on predefined policy levels: `privileged`, `baseline`, `restricted`.

---

#### - CSIStorageCapacity (Beta)

Exposed available storage capacity information per storage class and topology to improve scheduling decisions.

---

## Notable Limitations in v1.22

Still **not yet GA**:

* **Ephemeral containers** still beta
* **PodSecurity admission** only alpha
* **PodSecurityPolicy** still present but deprecated
* **Volume health monitoring** alpha
* **RuntimeClass** support limited

---

## Deprecations or Removals in v1.22

### - Removed APIs

The following **v1beta1 APIs were removed**:

* `Ingress` (`extensions/v1beta1`, `networking.k8s.io/v1beta1`) → Use `networking.k8s.io/v1`
* `CustomResourceDefinition` (`apiextensions.k8s.io/v1beta1`) → Use `v1`
* `ValidatingWebhookConfiguration` / `MutatingWebhookConfiguration` (`admissionregistration.k8s.io/v1beta1`) → Use `v1`
* `CertificateSigningRequest` (`certificates.k8s.io/v1beta1`) → Use `v1`
* `Lease` (`coordination.k8s.io/v1beta1`) → Use `v1`
* `APIService`, `TokenReview`, `SubjectAccessReview`, `SelfSubjectAccessReview` (`authentication.k8s.io` and `authorization.k8s.io`) → Use `v1`
