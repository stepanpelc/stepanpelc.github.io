+++
title = "Kubernetes v1.32"
tags = []
date = "2024-12-11"
toc = true
+++


## Kubernetes v1.32.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Custom Resource Field Selectors (Stable)

CRDs now support **selectableFields**, allowing users to query CRDs using field selectors for custom resource fields. This enables filtering using JSON paths defined in CRD specs.

**Example CRD:**

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: selector.stable.example.com
spec:
  group: stable.example.com
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
                color:
                  type: string
                size:
                  type: string
      selectableFields:
        - jsonPath: .spec.color
        - jsonPath: .spec.size
      additionalPrinterColumns:
        - name: Color
          jsonPath: .spec.color
          type: string
        - name: Size
          jsonPath: .spec.size
          type: string
```

You can then query:

```bash
kubectl get selector.stable.example.com --field-selector spec.color=blue
```

([Medium][1])

---

#### - Auto‑remove StatefulSet PVCs (Stable)

StatefulSets now support automatic cleanup of associated PVCs when no longer in use, reducing orphaned storage resources.

**Example behavior enabled via feature gate:**

* When a StatefulSet is scaled down or deleted, its PVCs are removed automatically if retention policy is default.

([Loft][2])

---

#### - Memory Manager (Stable)

The **Memory Manager** feature graduated to GA, ensuring memory allocation and pinning for Guaranteed QoS pods is handled consistently.

> Provides predictable behavior, especially for high-performance workloads.

([Kubernetes][3])

---

#### - Strict CPU Reservation (Stable)

A new `strict-cpu-reservation` option within the static CPU manager policy ensures reserved CPU cores are exclusively used for system daemons, improving performance isolation.

**Example kubelet config excerpt:**

```yaml
featureGates:
  CPUManagerPolicyOptions: true
cpuManagerPolicy: static
cpuManagerPolicyOptions:
  strict-cpu-reservation: "true"
reservedSystemCPUs: "0,32,1,33"
```

([Loft][2])

---

### Key Beta Features

#### - Asynchronous Preemption (Beta)

Scheduling performance is improved through **asynchronous preemption**, allowing the scheduler to continue scheduling while evicting lower-priority pods in parallel.

([Medium][1])

---

#### - KV pod termination sleep hook / Sleep Action for PreStop (Beta)

Adds support for a configurable sleep in `preStop` hook logic, enabling graceful pod termination workflows.

([Devtron][4])

---

#### - Kubelet OpenTelemetry Tracing (Beta)

`kubelet` now supports **OpenTelemetry traces**, exposing span data for reconcile loops and runtime interfaces to tabulate performance and latency behavior.

([Devtron][4])

---

### Notable Limitations in v1.32

Still **not fully stable or enabled by default**:

* **Asynchronous Preemption** remains beta
* **OpenTelemetry tracing** remains beta
* **Pod-level resource spec limits** still alpha
* **NodeLogQuery API** still alpha
* **PodSecurity Admission** lacks advanced RBAC exceptions

([Medium][1], [Sysdig][5])

---

### Deprecations or Removals in v1.32

* Removed **flowcontrol.apiserver.k8s.io/v1beta3**; migrate to `v1` FlowSchema & PriorityLevelConfiguration.
  ([Kubernetes][6])

* Operator-facing annotation `kubernetes.io/enforce-mountable-secrets` on ServiceAccount deprecated; use separate namespaces instead.
  ([AWS Documentation][7])

* The **DRAControlPlaneController** feature gate (alpha since v1.26) has been removed.
  ([AlibabaCloud][8])

---

[1]: https://medium.com/google-cloud/upgrades-everything-new-with-kubernetes-1-32-e59dab35a0bf?utm_source=chatgpt.com "Upgrades!!! — Everything New With Kubernetes 1.32 | by ..."
[2]: https://www.loft.sh/blog/kubernetes-132?utm_source=chatgpt.com "Kubernetes v1.32: Key Features, Updates, and What You ..."
[3]: https://kubernetes.io/blog/2024/12/11/kubernetes-v1-32-release/?utm_source=chatgpt.com "Kubernetes v1.32: Penelope"
[4]: https://devtron.ai/blog/kubernetes-v1-32-whats-new/?utm_source=chatgpt.com "Kubernetes v1.32: Here's what you should know about!"
[5]: https://sysdig.com/blog/kubernetes-1-32-whats-new?utm_source=chatgpt.com "Kubernetes 1.32 – What's new?"
[6]: https://kubernetes.io/blog/2024/11/08/kubernetes-1-32-upcoming-changes/?utm_source=chatgpt.com "Kubernetes v1.32 sneak peek"
[7]: https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions-standard.html?utm_source=chatgpt.com "Review release notes for Kubernetes versions on standard ..."
[8]: https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/kubernetes-1-32-release-notes?utm_source=chatgpt.com "Kubernetes 1.32 release notes - Container Service for ..."
