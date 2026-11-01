# 12. DaemonSets and operations

## Concepts

A DaemonSet ensures one Pod per node, which is useful for logging, monitoring,
or network agents. Daily operations also include inspecting events, logs, node
status, and cleaning resources. In production, avoid leaving test workloads
consuming CPU or storage.

## Commands

```bash
kubectl apply -f ../../../manifests/daemonset/daemonset.yaml
kubectl get daemonset,pods -o wide
kubectl get nodes
kubectl describe node <node>
kubectl delete daemonset logger
```

## Exercise

Define a DaemonSet in `exercise.yaml` with your own label. Verify that one
instance exists per node and delete the resource when finished.
