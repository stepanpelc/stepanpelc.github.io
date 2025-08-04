+++
title = "Kubernetes v1.1"
tags = []
date = "2015-11-09"
toc = true
+++

## Kubernetes v1.1.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Horizontal Pod Autoscaler (Beta)

Automatically scales the number of pods in a deployment or replication controller based on observed CPU utilization.

**Usage Example (YAML):**

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

---

#### - Cluster Add-ons (DNS, UI, Monitoring)

Bundled essential services like Kubernetes Dashboard, DNS (SkyDNS), and Heapster as cluster add-ons.

> These add-ons could be enabled using kube-up scripts or deployed as manifests.

---

#### - Enhanced `kubectl run` Command

Simplified launching pods or deployments from the CLI. Became a common entry point for quick development workflows.

**Example:**

```bash
kubectl run nginx --image=nginx --replicas=3
```

---

#### - Node Affinity via `nodeSelector`

Enabled basic pod scheduling rules using node labels. Introduced a predecessor to full node affinity rules.

**Example:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  nodeSelector:
    disktype: ssd
  containers:
    - name: frontend
      image: nginx
```

---

#### - kube-proxy iptables Mode (Experimental)

Introduced iptables-based service proxying for improved performance over the earlier userspace proxy.

> Controlled via the `--proxy-mode=iptables` flag on kube-proxy.

---

## Notable Limitations in v1.1

These features were still **not available**:

* No **Deployments** (introduced in 1.2)
* No **DaemonSets**
* No **StatefulSets**
* No **Ingress**
* No **RBAC**
* No **CRDs**
* No built-in **PodDisruptionBudgets**

---

## Deprecations or Removals in v1.1

There were **no official deprecations** in v1.1.
However, early prototypes of certain features like scheduling constraints were evolving and not yet standardized.

