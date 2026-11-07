# 13. Namespaces and YAML design

## Concepts

A namespace is a logical boundary for names, access, quotas, and organization.
Resources such as Pods, Deployments, Services, ConfigMaps, and Secrets are
usually namespaced. Nodes, PersistentVolumes, and ClusterRoles are
cluster-scoped.

Reliable manifests use `apiVersion`, `kind`, `metadata`, and `spec`. Keep
labels consistent, use meaningful names, and avoid putting generated status
fields in source-controlled YAML. Prefer declarative `kubectl apply` over
imperative commands for repeatable environments.

## Structure of a namespace and a labeled resource

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: learning           # Cluster-scoped: a Namespace has no namespace field
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: learning      # Namespaced resources declare their namespace
  labels:
    app: demo
    environment: learning  # Recommended labels for organization
    version: v1
data:
  APP_MODE: learning
```

- Every object has `apiVersion`, `kind`, and `metadata`; only namespaced kinds
  carry `metadata.namespace`.
- Consistent labels (`app`, `environment`, `version`) make selectors and
  troubleshooting predictable across manifests.
- Check scope with `kubectl api-resources --namespaced=true`.

## Commands

```bash
kubectl create namespace learning
kubectl get namespaces
kubectl config set-context --current --namespace=learning
kubectl api-resources --namespaced=true
kubectl diff -f exercise.yaml
kubectl apply --dry-run=client -f exercise.yaml
```

## Exercise

1. Create a namespace and a namespaced workload in `exercise.yaml`.
2. Add application, environment, and version labels.
3. Apply and explain which resources in the manifest are namespaced and which
   are cluster-scoped.
