# 18. Production delivery

## Concepts

Production delivery combines version control, image builds, vulnerability
scanning, manifest validation, automated deployment, health checks, and
rollback. A rolling update should preserve availability; a canary or blue/green
strategy limits blast radius for higher-risk releases.

Plan for failure: define PodDisruptionBudgets, backups for persistent data,
retention policies for Jobs, and recovery procedures. Kubernetes is not a
backup system and does not guarantee application-level consistency.

## Structure of a PodDisruptionBudget

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-availability
spec:
  minAvailable: 1          # Keep at least one Pod during evictions
  selector:
    matchLabels:
      app: web             # Applies to the selected workload
```

- A PDB protects availability during voluntary disruptions such as node drains.
- `minAvailable` is a count or percentage; `maxUnavailable` is the alternative
  way to express the same budget.
- A PDB does nothing alone: it only constrains disruptions for Pods matching
  its selector.

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

1. Design a release procedure in `exercise.yaml` comments.
2. Include image validation, a deployment health check, rollback criteria, a
   data backup step, and the cleanup command.
3. State which steps belong in CI and which belong in CD.
