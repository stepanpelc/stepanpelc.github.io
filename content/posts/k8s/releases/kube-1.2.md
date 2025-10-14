+++
title = "Kubernetes v1.2"
tags = []
date = "2016-03-17"
toc = true
+++

## Kubernetes v1.2.x – Summary of Major Stable Features

### Major Stable Features Introduced

#### - Deployments (Beta)

Introduced as a higher-level abstraction to manage ReplicaSets and rollout strategies for stateless applications.

**Usage Example (YAML):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

---

#### - ConfigMaps (Beta)

Enabled storing non-confidential configuration data in key-value pairs, to decouple configuration from application images.

**Usage Example (YAML):**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  log_level: debug
```

Use in Pod:

```yaml
env:
  - name: LOG_LEVEL
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: log_level
```

---

#### - PetSets (Alpha, precursor to StatefulSets)

Introduced PetSets for managing stateful applications. Provided stable network identity and persistent storage.

> Later renamed to StatefulSets in v1.5+ and became stable in v1.9.

---

#### - Kubernetes Dashboard (UI) v1.0

The first version of the web-based dashboard for managing cluster resources visually.

> Replaced older UI prototype from v1.1 and bundled via add-ons.

---

#### - SecurityContext for Pods and Containers

Allowed defining privilege and access controls per container or Pod.

**Example:**

```yaml
securityContext:
  runAsUser: 1000
  runAsGroup: 3000
  fsGroup: 2000
```

---

## Notable Limitations in v1.2

These features were still **not available**:

* No **StatefulSets** (PetSets were alpha)
* No **Ingress** (beta in v1.3)
* No **RBAC**
* No **CRDs**
* No **PodDisruptionBudgets**
* Limited autoscaling metrics (CPU only)

---

## Deprecations or Removals in v1.2

There were **no official removals** in v1.2.
However, the introduction of Deployments began to shift usage away from **ReplicationControllers** and **`kubectl run`** in its original form.
