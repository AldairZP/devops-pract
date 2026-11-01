# 09. Ingress and NetworkPolicy

## Concepts

Ingress declares HTTP/HTTPS rules, but requires an Ingress Controller to
implement them. NetworkPolicy controls traffic between Pods using selectors
and depends on CNI support. An ingress policy can isolate a Pod from every
source that is not explicitly allowed.

## Commands

```bash
kubectl apply -f ../../../manifests/networking/network-policy.yaml
kubectl get networkpolicy
kubectl describe networkpolicy nginx-allow-from-busybox
kubectl apply -f ../../../manifests/networking/ingress.yaml
kubectl get ingress
```

## Exercise

Define a NetworkPolicy in `exercise.yaml` that only allows traffic from Pods
with a label you choose. Test one allowed client and one blocked client.
