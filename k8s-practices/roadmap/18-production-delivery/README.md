# 18. Production delivery

## Concepts

Production delivery combines version control, image builds, vulnerability
scanning, manifest validation, automated deployment, health checks, and
rollback. A rolling update should preserve availability; a canary or blue/green
strategy limits blast radius for higher-risk releases.

Plan for failure: define PodDisruptionBudgets, backups for persistent data,
retention policies for Jobs, and recovery procedures. Kubernetes is not a
backup system and does not guarantee application-level consistency.

## Commands

```bash
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl get pdb
kubectl get resourcequota,limitrange
kubectl delete -f exercise.yaml
```

## Exercise

Design a release procedure in `exercise.yaml` comments. Include image
validation, a deployment health check, rollback criteria, a data backup step,
and the cleanup command. State which steps belong in CI and which belong in CD.
