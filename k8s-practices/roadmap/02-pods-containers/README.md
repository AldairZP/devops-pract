# 02. Pods and containers

## Concepts

A Pod is the smallest unit Kubernetes schedules. Its containers share network
and volumes, but a Pod can be replaced at any time. Do not use standalone Pods
for an application that needs replicas or updates; use a Deployment.

## Structure of a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web             # Unique name inside the namespace
  labels:
    app: web            # Target for Services and controller selectors
spec:
  containers:
    - name: web         # Container name for logs and exec
      image: nginx:1.27
      ports:
        - containerPort: 80  # Documents the port; it does not publish a node port
      env:
        - name: APP_ENV      # Environment variable name
          value: learning    # Literal value
```

- `spec.containers` is a list: a Pod can run several containers that share
  `localhost` and the same volumes.
- `ports.containerPort` documents the listening port and helps Services; the
  container can listen without declaring it.
- `env` injects variables. `value` sets a literal; `valueFrom` reads from a
  ConfigMap or Secret (topic 05).

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-pod.yaml
kubectl get pod nginx -o wide
kubectl describe pod nginx
kubectl logs nginx
kubectl exec nginx -- nginx -t
```

## Exercise

1. In `exercise.yaml`, define a Pod with a web image, an `app` label, a
   container port, and an environment variable.
2. Apply it and verify its status with `kubectl get pod`.
3. Read its logs with `kubectl logs` and run a command inside it with
   `kubectl exec`.
