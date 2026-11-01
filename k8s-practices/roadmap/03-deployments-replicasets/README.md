# 03. Deployments and ReplicaSets

## Concepts

A Deployment manages a ReplicaSet; the ReplicaSet maintains the replica count
and replaces failed Pods. A Deployment supports rolling updates and rollback.
Its selector must match the template labels.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-deployment.yaml
kubectl get deployment,replicaset,pods
kubectl scale deployment nginx-deploy --replicas=3
kubectl rollout status deployment/nginx-deploy
kubectl rollout history deployment/nginx-deploy
```

## Exercise

Define a two-replica Deployment in `exercise.yaml`. Delete one of its Pods and
demonstrate that the controller creates a replacement.
