# 16. Helm and Kustomize

## Concepts

Raw YAML is useful for learning and small deployments. Kustomize applies
overlays and patches without using templates. Helm packages Kubernetes
resources into charts with values and releases. Both tools help keep
environment differences explicit; neither replaces good manifest design.

Review rendered output before applying it. Never hide production differences
inside undocumented values or templates.

## Commands

```bash
kubectl kustomize .
helm create my-app
helm lint ./my-app
helm template my-app ./my-app
helm install my-app ./my-app --namespace learning --create-namespace
helm list --all-namespaces
helm uninstall my-app --namespace learning
```

## Exercise

Create a small Kustomize base or Helm chart in `exercise.yaml` as a written
design exercise: describe the base configuration, the development override,
the production override, and the values that must never contain secrets.
