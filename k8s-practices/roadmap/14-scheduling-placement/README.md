# 14. Scheduling and node placement

## Concepts

The scheduler chooses a node using resource requests and placement rules.
`nodeSelector` is a simple label requirement. Node affinity expresses
required or preferred rules. A taint repels Pods; a toleration allows a Pod to
run on a tainted node, but does not force placement. Topology spread
constraints help distribute replicas across failure domains.

Use placement rules only when they represent a real operational requirement.
Over-constraining a workload can leave it `Pending`.

## Structure of a nodeSelector

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: placement
spec:
  nodeSelector:
    kubernetes.io/os: linux  # Only nodes carrying this label
  containers:
    - name: app
      image: nginx:1.27
      resources:
        requests:
          cpu: 100m           # Also used by the scheduler to filter nodes
          memory: 64Mi
```

- `nodeSelector` is a hard requirement: the Pod only runs on nodes with every
  listed label.
- `nodeAffinity` expresses the same idea with `requiredDuringScheduling` and
  `preferredDuringScheduling` rules.
- A taint is set on the node; a toleration is set on the Pod. A toleration
  allows but never forces placement.

## Commands

```bash
kubectl get nodes --show-labels
kubectl describe node <node>
kubectl get pods -o wide
kubectl get events --sort-by=.lastTimestamp
kubectl taint nodes <node> dedicated=learning:NoSchedule
kubectl taint nodes <node> dedicated=learning:NoSchedule-
```

## Exercise

1. Define a Pod or Deployment in `exercise.yaml` with a node selector or
   affinity rule.
2. Apply it and observe its assigned node with `kubectl get pods -o wide`.
3. Create an impossible rule and diagnose the resulting `Pending` state from
   events.
