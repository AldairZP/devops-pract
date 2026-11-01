# 06. Requests, limits, and probes

## Concepts

The scheduler uses `resources.requests` to choose a node. `limits` restrict
consumption. `readinessProbe` controls whether the Pod receives traffic;
`livenessProbe` decides whether the container should restart; `startupProbe`
protects slow-starting applications.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-probe.yaml
kubectl describe pod nginx-con-probes
kubectl get pod nginx-con-probes -o wide
kubectl get events --sort-by=.lastTimestamp
```

## Exercise

Define a container with requests, limits, readiness, and liveness in
`exercise.yaml`. Change a probe path and observe the difference between being
not ready and restarting.
