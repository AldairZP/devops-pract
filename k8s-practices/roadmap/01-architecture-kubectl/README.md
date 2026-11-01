# 01. Architecture and kubectl

## Concepts

Kubernetes maintains a desired state. The `kube-apiserver` receives requests,
`etcd` stores the state, the scheduler assigns Pods to nodes, and controllers
reconcile the current state. On each node, kubelet runs Pods through the
container runtime.

`kubectl` is an API client. `apply` declares state; `get`, `describe`, and
`events` let you observe it.

## Commands

```bash
kubectl cluster-info
kubectl get nodes -o wide
kubectl api-resources
kubectl get events --sort-by=.lastTimestamp
kubectl explain pod.spec.containers
```

## Exercise

Write a resource in `exercise.yaml` that you can apply to verify your context.
Then identify which cluster component participates when the resource changes
from `Pending` to `Running`.
