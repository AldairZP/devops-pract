# 09. Ingress and NetworkPolicy

## Concepts

Ingress declares HTTP/HTTPS rules, but requires an Ingress Controller to
implement them. NetworkPolicy controls traffic between Pods using selectors
and depends on CNI support. An ingress policy can isolate a Pod from every
source that is not explicitly allowed.

## Structure of a NetworkPolicy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-client
spec:
  podSelector:
    matchLabels:
      app: protected          # Pods the policy applies to
  policyTypes:
    - Ingress                 # Direction of traffic this policy controls
  ingress:
    - from:
        - podSelector:
            matchLabels:
              access: allowed # Only Pods with this label may connect
```

- `podSelector` selects the Pods being protected; an empty selector means all
  Pods in the namespace.
- `policyTypes` declares which directions are managed (`Ingress`, `Egress`).
- An `ingress` rule with `from` opens traffic only from the listed sources;
  traffic from any other source is denied.

## Commands

```bash
kubectl apply -f ../../../manifests/networking/network-policy.yaml
kubectl get networkpolicy
kubectl describe networkpolicy nginx-allow-from-busybox
kubectl apply -f ../../../manifests/networking/ingress.yaml
kubectl get ingress
```

## Exercise

1. Define a NetworkPolicy in `exercise.yaml` that only allows traffic from Pods
   with a label you choose.
2. Apply it and test one allowed client and one blocked client with
   `kubectl exec`.
