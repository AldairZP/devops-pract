# 15. Observability and debugging

## Concepts

Observability combines logs, metrics, and traces. Kubernetes provides basic
object state, events, and container logs, but a production platform normally
adds a metrics system, centralized log collection, and distributed tracing.

Debug from the outside in: namespace, workload, Pod, container, process, and
network. Capture the exact error and event before changing the manifest.

## Commands

```bash
kubectl get all -A
kubectl describe pod <pod>
kubectl logs <pod> --all-containers --timestamps
kubectl logs <pod> --previous
kubectl get events -A --sort-by=.lastTimestamp
kubectl top nodes
kubectl top pods
kubectl exec -it <pod> -- sh
kubectl port-forward service/<service> 8080:80
```

## Exercise

Create a workload in `exercise.yaml` that can be inspected with logs and
events. Intentionally make it fail, collect the evidence, repair it, and
write the diagnosis as comments in the exercise file.
