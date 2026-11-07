# 06. Requests, limits, and probes

## Concepts

The scheduler uses `resources.requests` to choose a node. `limits` restrict
consumption. `readinessProbe` controls whether the Pod receives traffic;
`livenessProbe` decides whether the container should restart; `startupProbe`
protects slow-starting applications.

## Structure of requests, limits, and probes

```yaml
spec:
  containers:
    - name: web
      image: nginx:1.27
      resources:
        requests:            # Minimum reserved for scheduling
          cpu: 100m
          memory: 64Mi
        limits:              # Maximum the container may use
          cpu: 500m
          memory: 128Mi
      readinessProbe:        # Is the container ready for traffic?
        httpGet:
          path: /
          port: 80
        periodSeconds: 5
      livenessProbe:         # Should the container be restarted?
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 10
```

- `requests` are what the scheduler sums when choosing a node. `limits` are the
  hard ceiling; the container is throttled or killed if it exceeds them.
- A failed `readinessProbe` removes the Pod from Service endpoints but does not
  restart it. A failed `livenessProbe` restarts the container.
- `httpGet` checks an HTTP path; you can also use `tcpSocket` or `exec`.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-probe.yaml
kubectl describe pod nginx-con-probes
kubectl get pod nginx-con-probes -o wide
kubectl get events --sort-by=.lastTimestamp
```

## Exercise

1. Define a container with requests, limits, readiness, and liveness in
   `exercise.yaml`.
2. Apply it and watch `kubectl get pod` to confirm it becomes `Ready`.
3. Change a probe path to a non-existent one and observe the difference
   between being not ready and restarting.
