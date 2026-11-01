# 11. HPA and metrics

## Concepts

The HorizontalPodAutoscaler changes Deployment replicas according to metrics.
For CPU, requests are required because utilization is calculated against them.
Metrics Server provides usage metrics; it is not a complete observability
system.

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

Define an HPA in `exercise.yaml` for an existing Deployment, with explicit
minimum, maximum, and CPU target values. Check its status and explain any
missing-metrics message.
