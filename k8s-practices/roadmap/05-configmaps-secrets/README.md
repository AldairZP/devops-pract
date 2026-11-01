# 05. ConfigMaps and Secrets

## Concepts

ConfigMap separates non-sensitive configuration from code. Secret carries
credentials or keys, but Base64 is not encryption. Both can be consumed as
environment variables or mounted files. Do not commit real secrets to the
repository.

## Commands

```bash
kubectl create configmap mensaje-config \
  --from-file=../../../manifests/config/mensaje.txt
kubectl get configmap mensaje-config -o yaml
kubectl create secret generic demo-secret --from-literal=API_KEY=change-me
kubectl get secret demo-secret
```

## Exercise

Define a Pod in `exercise.yaml` that consumes one ConfigMap key and one Secret
key. Verify that both reach the container through the correct mechanism.
