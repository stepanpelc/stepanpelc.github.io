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

## Get all images from all contexts

```bash
 for CONTEXT in $(kubectl config view -o jsonpath='{.contexts[*].name}'); do kubectl config use-context $CONTEXT; kubectl get pods -A -o jsonpath='{range .items[*]}{.spec.containers[*].image}{"\n"}' | sed 's/\s\+/\n/g' | sort | uniq; done;
```

## Pod anti-affinity

Pod has to be deployed on all nodes

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

## Pod on all servers

Configuration for ignoring master taint

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
...
  spec:
...
    spec:
...
      tolerations:
        - key: "node-role.kubernetes.io/master"
          effect: "NoSchedule"
          operator: "Exists"
```
