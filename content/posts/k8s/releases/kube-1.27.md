+++
title = "Kubernetes v1.27"
tags = []
date = "2023-04-11"
toc = true
+++

## Kubernetes v1.27.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - CEL in Admission Control (Stable)

**Common Expression Language (CEL)** is now supported in **ValidatingAdmissionPolicy**, enabling declarative policies using standardized syntax.

**Example validation:**

```yaml
expression: "object.spec.replicas <= 5"
message: "replicas must be 5 or less"
```

---

#### - Sidecar Container Lifecycle (Alpha)

Introduced the concept of **sidecar lifecycle control**, allowing init-like behavior for sidecars with ordered startup/shutdown.

> Adds support for `restartPolicy: "Always"` containers that block pod completion.

---

#### - Pod Scheduling Readiness (Alpha)

Introduced readiness signaling between pods and the scheduler, improving orchestration in complex workloads (e.g., distributed systems like MPI).

---

#### - Mixed Protocol Services (Stable)

Services can now expose **both TCP and UDP ports** under the same Service definition.

**Example:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - name: http
      port: 80
      protocol: TCP
    - name: dns
      port: 53
      protocol: UDP
```

---

#### - CSI Migration Complete for Most Drivers

CSI migration is **complete and GA** for:

* AWS EBS
* GCE PD
* Azure Disk/File
* OpenStack
* Cinder
* VMware vSphere

> In-tree storage plugins for these are deprecated and can be disabled.

---

#### - ImagePullPolicy Defaults to `IfNotPresent` (Stable)

The default behavior for `imagePullPolicy` is now explicitly set to `IfNotPresent` unless `:latest` is used.

---

#### - API Server Tracing (Alpha)

Introduced **OpenTelemetry tracing** support in the Kubernetes API server, improving observability of API calls and component latency.

> Enabled via feature gate and environment configuration.

---

## Notable Limitations in v1.27

Still **not GA** or default:

* **Sidecar lifecycle** is alpha
* **API tracing** in alpha
* **Pod scheduling readiness** not yet GA
* **PodSecurity Admission** lacks fine-grained RBAC control
* **RuntimeClass** usage still minimal

---

## Deprecations or Removals in v1.27

* Deprecated APIs removed:

  * `admissionregistration.k8s.io/v1beta1`
  * `flowcontrol.apiserver.k8s.io/v1beta1`
* In-tree cloud providers further deprecated (use external CCM)
* More kubelet configuration fields removed for old dockershim dependencies

