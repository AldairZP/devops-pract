# 10. RBAC and ServiceAccounts

## Concepts

A ServiceAccount is an identity for processes. `Role` defines permissions
within a namespace; `ClusterRole` can cover the cluster. A Binding assigns
those permissions. RBAC should follow least privilege: only the required verbs
and resources.

## Commands

```bash
kubectl apply -f ../../../manifests/rbac/rbac.yaml
kubectl auth can-i list pods --as=system:serviceaccount:default:lector-pods
kubectl auth can-i delete pods --as=system:serviceaccount:default:lector-pods
```

## Exercise

Define a ServiceAccount, Role, and RoleBinding in `exercise.yaml` that allow
listing Pods but not deleting them. Demonstrate both results with
`kubectl auth can-i`.
