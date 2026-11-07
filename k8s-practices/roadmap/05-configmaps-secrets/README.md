# 05. ConfigMaps and Secrets

## Concepts

ConfigMap separates non-sensitive configuration from code. Secret carries
credentials or keys, but Base64 is not encryption. Both can be consumed as
environment variables or mounted files. Do not commit real secrets to the
repository.

## Structure of a ConfigMap and a Secret

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:                       # Plain-text key/value configuration
  APP_MODE: learning
---
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque                # Default type for arbitrary key/value data
stringData:                 # Write plain text; Kubernetes encodes it
  API_KEY: change-me
```

```yaml
# How a Pod consumes one key from each
env:
  - name: APP_MODE
    valueFrom:
      configMapKeyRef:      # Reads a single ConfigMap key
        name: app-config
        key: APP_MODE
  - name: API_KEY
    valueFrom:
      secretKeyRef:         # Reads a single Secret key
        name: app-secret
        key: API_KEY
```

- `data` stores literal values. `stringData` is a convenience that lets you
  write plain text; Kubernetes encodes it in etcd.
- A Pod references keys with `configMapKeyRef` or `secretKeyRef`, naming both
  the object and the key.
- Creating the ConfigMap/Secret is not enough: the Pod must reference it.

## Commands

```bash
kubectl create configmap mensaje-config \
  --from-file=../../../manifests/config/mensaje.txt
kubectl get configmap mensaje-config -o yaml
kubectl create secret generic demo-secret --from-literal=API_KEY=change-me
kubectl get secret demo-secret
```

## Exercise

1. Define a ConfigMap, a Secret, and a Pod in `exercise.yaml`.
2. Make the Pod consume one ConfigMap key and one Secret key as environment
   variables.
3. Apply everything and verify the values reach the container, for example
   with `kubectl exec` and `env`.
