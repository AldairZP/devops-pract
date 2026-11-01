# 02. Pods and containers

## Concepts

A Pod is the smallest unit Kubernetes schedules. Its containers share network
and volumes, but a Pod can be replaced at any time. Do not use standalone Pods
for an application that needs replicas or updates; use a Deployment.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-pod.yaml
kubectl get pod nginx -o wide
kubectl describe pod nginx
kubectl logs nginx
kubectl exec nginx -- nginx -t
```

## Exercise

In `exercise.yaml`, define a Pod with a web image, an `app` label, a container
port, and an environment variable. Verify its logs and status.
