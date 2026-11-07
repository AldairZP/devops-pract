# 15. Observability and debugging

## Concepts

Observability combines logs, metrics, and traces. Kubernetes provides basic
object state, events, and container logs, but a production platform normally
adds a metrics system, centralized log collection, and distributed tracing.

Debug from the outside in: namespace, workload, Pod, container, process, and
network. Capture the exact error and event before changing the manifest.

## Structure of an inspectable Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: observable
  labels:
    app: observable     # Labels help filter and select during debugging
spec:
  containers:
    - name: app
      image: busybox:1.36
      command: ["sh", "-c", "echo application-started; sleep 3600"]
```

- The container logs everything written to stdout/stderr; `kubectl logs` reads
  them.
- `kubectl describe` and `kubectl get events` show the Pod lifecycle, images,
  probes, and recent problems.
- A failing workload often only shows a status like `CrashLoopBackOff`; the
  actual error is in the logs, events, or the exit reason.

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

1. Create a workload in `exercise.yaml` that can be inspected with logs and
   events.
2. Intentionally make it fail, collect the evidence (logs and events), and
   repair it.
3. Write the diagnosis as comments in the exercise file.
