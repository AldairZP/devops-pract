# 03. Deployments and ReplicaSets

## Concepts

A Deployment manages a ReplicaSet; the ReplicaSet maintains the replica count
and replaces failed Pods. A Deployment supports rolling updates and rollback.
Its selector must match the template labels.

## Structure of a Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 2                      # Desired number of identical Pods
  selector:
    matchLabels:
      app: web                     # Selects the Pods this Deployment owns
  template:                        # Pod template used to create the Pods
    metadata:
      labels:
        app: web                   # Must match the selector above
    spec:
      containers:
        - name: web
          image: nginx:1.27
```

- `replicas` is the desired count. The ReplicaSet creates or deletes Pods to
  reach it.
- `selector` tells the controller which Pods it manages; it must match
  `template.metadata.labels` or the apply is rejected.
- `template` is the Pod definition. You change the Deployment, never the Pods
  directly: Deployment → ReplicaSet → Pods.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-deployment.yaml
kubectl get deployment,replicaset,pods
kubectl scale deployment nginx-deploy --replicas=3
kubectl rollout status deployment/nginx-deploy
kubectl rollout history deployment/nginx-deploy
```

## Exercise

1. Define a two-replica Deployment in `exercise.yaml`.
2. Apply it and confirm with `kubectl get deployment,replicaset,pods`.
3. Delete one of its Pods and demonstrate that the controller creates a
   replacement.
