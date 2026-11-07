# 10. RBAC and ServiceAccounts

## Concepts

A ServiceAccount is an identity for processes. `Role` defines permissions
within a namespace; `ClusterRole` can cover the cluster. A Binding assigns
those permissions. RBAC should follow least privilege: only the required verbs
and resources.

## Structure of RBAC resources

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: pod-reader
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
rules:
  - apiGroups: [""]      # Core group for Pods
    resources: ["pods"]  # The resources the rule applies to
    verbs: ["get", "list", "watch"]  # The allowed operations
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
subjects:
  - kind: ServiceAccount  # Who receives the permissions
    name: pod-reader
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role             # What permissions are assigned
  name: pod-reader
```

- A `Role` only lists permissions; it grants nothing until a `RoleBinding`
  connects it to a `subject`.
- `subjects` is the identity (here a ServiceAccount); `roleRef` is the Role or
  ClusterRole being granted.
- The pattern is "identity + rules + binding". If any piece is missing, access
  is denied.

## Commands

```bash
kubectl apply -f ../../../manifests/rbac/rbac.yaml
kubectl auth can-i list pods --as=system:serviceaccount:default:lector-pods
kubectl auth can-i delete pods --as=system:serviceaccount:default:lector-pods
```

## Exercise

1. Define a ServiceAccount, Role, and RoleBinding in `exercise.yaml` that allow
   listing Pods but not deleting them.
2. Apply them and demonstrate both results with `kubectl auth can-i`.
