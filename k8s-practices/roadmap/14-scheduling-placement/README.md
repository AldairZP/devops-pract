# 14. Scheduling and node placement

## Concepts

The scheduler chooses a node using resource requests and placement rules.
`nodeSelector` is a simple label requirement. Node affinity expresses
required or preferred rules. A taint repels Pods; a toleration allows a Pod to
run on a tainted node, but does not force placement. Topology spread
constraints help distribute replicas across failure domains.

Use placement rules only when they represent a real operational requirement.
Over-constraining a workload can leave it `Pending`.

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

Define a Pod or Deployment in `exercise.yaml` with a node selector or affinity
rule. Observe its assigned node, then create an impossible rule and diagnose
the resulting `Pending` state from events.
