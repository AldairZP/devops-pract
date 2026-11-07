# 12. DaemonSets and operations

## Concepts

A DaemonSet ensures one Pod per node, which is useful for logging, monitoring,
or network agents. Daily operations also include inspecting events, logs, node
status, and cleaning resources. In production, avoid leaving test workloads
consuming CPU or storage.

## Structure of a DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-agent
spec:
  selector:
    matchLabels:
      app: node-agent        # Selects the Pods the DaemonSet owns
  template:
    metadata:
      labels:
        app: node-agent      # Must match the selector
    spec:
      containers:
        - name: agent
          image: busybox:1.36
          command: ["sh", "-c", "echo node-agent; sleep 3600"]
```

- The DaemonSet has the same shape as a Deployment but without `replicas`:
  Kubernetes schedules exactly one Pod per matching node.
- Nodes can be excluded with node selectors, taints, or tolerations (topic 14).

## Commands

```bash
kubectl apply -f ../../../manifests/daemonset/daemonset.yaml
kubectl get daemonset,pods -o wide
kubectl get nodes
kubectl describe node <node>
kubectl delete daemonset logger
```

## Exercise

1. Define a DaemonSet in `exercise.yaml` with your own label.
2. Apply it and verify with `kubectl get pods -o wide` that one instance exists
   per node.
3. Delete the resource when finished.
