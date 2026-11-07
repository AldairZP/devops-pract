# 16. Helm and Kustomize

## Concepts

Raw YAML is useful for learning and small deployments. Kustomize applies
overlays and patches without using templates. Helm packages Kubernetes
resources into charts with values and releases. Both tools help keep
environment differences explicit; neither replaces good manifest design.

Review rendered output before applying it. Never hide production differences
inside undocumented values or templates.

## Structure of the design

The exercise uses a ConfigMap as the written design document:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: packaging-design
data:
  approach: "Kustomize base + dev and production overlays, or a Helm chart"
  secrets: "Managed outside Git by a secret manager"
```

- A Kustomize **base** holds the common manifests; **overlays** apply
  environment-specific patches (replicas, images, namespaces).
- A Helm **chart** groups templates plus a `values.yaml`; `helm template`
  renders the final YAML without installing it.
- Secrets belong in a secret manager or Helm/Kustomize secret handling, never
  as plain text in Git.

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

1. Create a small Kustomize base or Helm chart in `exercise.yaml` as a written
   design exercise.
2. Describe the base configuration, the development override, and the
   production override.
3. List the values that must never contain secrets.
