+++
title = "Kubernetes v1.33"
tags = []
date = "2025-04-23"
toc = true
+++


## Kubernetes v1.33.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - In‑place Pod Resource Resize (Beta)

Users can now adjust CPU and memory of running pods **in place** without pod restart. Supports vertical scaling on long-running workloads.
**Usage Example (patch):**

```bash
kubectl patch pod mypod -p '{"spec":{"containers":[{"name":"mystuff","resources":{"limits":{"cpu":"2","memory":"2Gi"}}}]}}'
```

Monitored via new Pod conditions like `PodResizePending` and `PodResizeInProgress`. ([Loft][1])

#### - User Namespaces Enabled by Default (Stable)

Linux user namespaces are enabled by default to isolate container UIDs/GIDs from the host. Use `hostUsers: false` to opt out.
**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-userns
spec:
  hostUsers: false
  containers:
  - name: shell
    image: debian
    command: ["sleep", "infinity"]
```

Enhances host isolation and reduces container privilege risk. ([Loft][1], [PerfectScale][2])

#### - Sidecar Containers Lifecycle Control (Stable)

Native sidecar containers graduated to GA. They can now run with `restartPolicy: Always`, and complete independently from main containers.
Improves support for logging, mesh, and monitoring sidecars. ([Loft][1])

#### - Job Success Policy for Indexed Jobs (Stable)

The Job API now supports more flexible success criteria for indexed jobs—e.g. succeed when a designated leader index finishes or a minimum succeeded count is reached. ([Loft][1])

---

### Key Beta & Alpha Features

#### - ResourceClaim Device Status in DRA (Beta)

Drivers can now report detailed device status (like IP addresses, MACs) in `ResourceClaim` status. Offers richer observability for network and hardware. ([Loft][1])

#### - Ordered Namespace Deletion (Alpha)

Introduces deterministic namespace deletion order: pods are removed before dependent resources like NetworkPolicies, enhancing cleanup reliability. ([Loft][1])

#### - Pod Metadata.Generations (Alpha)

Adds `metadata.generation` to `Pod` objects to track spec changes over time, improving observability for custom controllers and GitOps workflows. ([Sysdig][3])

#### - OCI Image Volume Source (Alpha)

Allows mounting OCI images as volumes in pods—useful for distributing CLI tools or artifacts without baking into container images. ([Sysdig][3])

#### - HPA Configurable Tolerances (Alpha)

HorizontalPodAutoscaler now supports configuring scale-up/down tolerance per workload, improving autoscaling tuning. ([Loft][1])

#### - Kubectl RC Config File (`.kuberc`) (Alpha)

Users can store default flags, aliases, and preferences for `kubectl` in a standalone `~/.kube/kuberc` file instead of embedding them in `kubeconfig`. ([Loft][1])

---

## Notable Limitations in v1.33

Still **not GA or default** for:

* **Async preemption** and more advanced scheduler features
* **OpenTelemetry tracing** and observability APIs
* **PodSecurity Admission** RBAC exceptions
* **NodeLogQuery API** remains alpha
* Some resource scheduling and multi-network enhancements remain under development ([Medium][4], [anantacloud.com][5])

---

## Deprecations or Removals in v1.33

* **Endpoints API deprecated**: users should migrate to `EndpointSlice`; usage of the Endpoints API now emits deprecation warnings. ([Kubernetes][6])
* **Field `status.nodeInfo.kubeProxyVersion` removed** from Node status. ([Kubernetes][7])
* **Host networking on Windows Pods removed** due to containerd limitations. ([Kubernetes][7])
* Deprecated in-tree cloud provider integrations and legacy scheduler plugins removed. ([PerfectScale][8], [anantacloud.com][5])

---

[1]: https://www.loft.sh/blog/kubernetes-v-1-33-key-features-updates-and-what-you-need-to-know?utm_source=chatgpt.com "Kubernetes v1.33: Key Features, Updates, and What You ..."
[2]: https://www.perfectscale.io/blog/kubernetes-v1-33-octarine?utm_source=chatgpt.com "Kubernetes v1.33: What's New and Improved?"
[3]: https://sysdig.com/blog/kubernetes-1-33-whats-new/?utm_source=chatgpt.com "Kubernetes 1.33 – What's new?"
[4]: https://medium.com/google-cloud/upgrades-everything-new-with-kubernetes-1-33-2daead6b9fb4?utm_source=chatgpt.com "Upgrades!!! — Everything New With Kubernetes 1.33"
[5]: https://www.anantacloud.com/post/kubernetes-v1-33-key-features-that-redefine-your-container-platform?utm_source=chatgpt.com "Kubernetes v1.33: Key Features That Redefine Your ..."
[6]: https://kubernetes.io/blog/2025/04/24/endpoints-deprecation/?utm_source=chatgpt.com "Continuing the transition from Endpoints to EndpointSlices"
[7]: https://kubernetes.io/blog/2025/03/26/kubernetes-v1-33-upcoming-changes/?utm_source=chatgpt.com "Kubernetes v1.33 sneak peek"
[8]: https://www.perfectscale.io/blog/kubernetes-v1-33?utm_source=chatgpt.com "Kubernetes v1.33: Sneak Peek"
