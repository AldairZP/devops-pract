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

Create a namespace and a namespaced workload in `exercise.yaml`. Add
application, environment, and version labels. Explain which resources in the
manifest are namespaced and which are cluster-scoped.
