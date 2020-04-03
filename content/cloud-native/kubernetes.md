+++
title = "Kubernetes"
tags = [
    "kubernetes",
    "cloud native",
    "development",
]
date = "2019-04-02"
toc = true
+++


## Patching commands

```bash
kubectl get deployment -n speed-ci-02-app | grep -v NAME | awk '{print $1}' | xargs -L1  kubectl patch deployment --patch '{\"spec\": {\"template\": {\"spec\": {\"imagePullSecrets\": [{\"name\": \"nexus3\"}]}}}}' -n speed-ci-02-app
```

## Pod anti-affinity

Pod has to be deployed on server

```yaml
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: k8s-app
                operator: In
                values:
                - kube-dns
            namespaces:
            - kube-system
```


