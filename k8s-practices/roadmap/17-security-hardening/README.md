# 17. Security hardening

## Concepts

Secure workloads use least-privilege RBAC, non-root containers, read-only
filesystems where possible, dropped Linux capabilities, resource limits, and
trusted image sources. Use NetworkPolicies to restrict communication and
Secrets or an external secret manager for sensitive values.

Image tags such as `latest` are mutable. Prefer reviewed, scanned, immutable
digests in controlled environments. A Secret encoded with Base64 is not
encrypted by itself.

## Commands

```bash
kubectl auth can-i --list
kubectl get role,rolebinding,clusterrole,clusterrolebinding -A
kubectl get secrets
kubectl get pod <pod> -o yaml
kubectl explain pod.spec.securityContext
kubectl explain pod.spec.containers.securityContext
```

## Exercise

Define a restricted container in `exercise.yaml`: run as non-root, drop
capabilities, use a read-only root filesystem, set resource limits, and grant
only the permissions it needs. Explain each security choice.
