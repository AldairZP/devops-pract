# 04. Labels, selectors, and Services

## Concepts

Labels identify objects and selectors connect them. A Service provides a stable
address even when Pods change. `ClusterIP` is internal, `NodePort` publishes a
node port, and `LoadBalancer` requires infrastructure integration.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-service.yaml
kubectl get svc,endpoints
kubectl describe service nginx-service
curl http://localhost:8080
```

## Exercise

Create a Service in `exercise.yaml` that selects an application by label.
Verify that it has endpoints and explain what happens when you change the
selector.
