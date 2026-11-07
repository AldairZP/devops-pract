# 11. HPA and metrics

## Concepts

The HorizontalPodAutoscaler changes Deployment replicas according to metrics.
For CPU, requests are required because utilization is calculated against them.
Metrics Server provides usage metrics; it is not a complete observability
system.

## Structure of an HPA

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment       # The workload the HPA scales
    name: web
  minReplicas: 2           # Never scale below this
  maxReplicas: 5           # Never scale above this
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70  # Percentage of the request
```

- `scaleTargetRef` points to the Deployment (or other scaleable workload) to
  scale; it does not create one.
- `averageUtilization` is a percentage of `resources.requests.cpu`. Without a
  request the HPA cannot compute utilization and reports missing metrics.
- The HPA needs the Metrics Server installed (`kubectl top pods` must work).

## Commands

```bash
kubectl apply -f ../../../metrics-server.yaml
kubectl top nodes
kubectl top pods
kubectl apply -f ../../../manifests/autoscaling/stress-deployment.yaml
kubectl apply -f ../../../manifests/autoscaling/hpa.yaml
kubectl get hpa -w
```

## Exercise

1. Define an HPA in `exercise.yaml` for an existing Deployment, with explicit
   minimum, maximum, and CPU target values.
2. Apply it and check its status with `kubectl get hpa`.
3. Explain any missing-metrics message and which prerequisite is missing.
