# 17. Security hardening

## Concepts

Secure workloads use least-privilege RBAC, non-root containers, read-only
filesystems where possible, dropped Linux capabilities, resource limits, and
trusted image sources. Use NetworkPolicies to restrict communication and
Secrets or an external secret manager for sensitive values.

Image tags such as `latest` are mutable. Prefer reviewed, scanned, immutable
digests in controlled environments. A Secret encoded with Base64 is not
encrypted by itself.

## Structure of a hardened Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hardened
spec:
  securityContext:
    runAsNonRoot: true        # Forbid running as UID 0
    seccompProfile:
      type: RuntimeDefault    # Default kernel syscall filter
  containers:
    - name: app
      image: nginxinc/nginx-unprivileged:1.27
      securityContext:
        allowPrivilegeEscalation: false  # No setuid/setgid gain
        readOnlyRootFilesystem: true     # Root filesystem cannot be written
        capabilities:
          drop: ["ALL"]                  # Drop Linux capabilities
      resources:
        requests:
          cpu: 100m
          memory: 64Mi
        limits:                          # Limits prevent resource abuse
          cpu: 500m
          memory: 128Mi
```

- `runAsNonRoot: true` fails the container if it would run as UID 0.
- `allowPrivilegeEscalation: false` and `drop: ["ALL"]` reduce what a
  compromised process can do.
- `readOnlyRootFilesystem: true` requires an image that tolerates a
  non-writable root; some images need an `emptyDir` for writable paths.

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

1. Define a restricted container in `exercise.yaml`: run as non-root, drop
   capabilities, use a read-only root filesystem, set resource limits, and
   grant only the permissions it needs.
2. Apply it and confirm the Pod becomes `Running`.
3. Explain each security choice as comments in the file.
