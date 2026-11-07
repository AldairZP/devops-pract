# 08. Jobs and CronJobs

## Concepts

A Job runs a Pod until a task completes. `backoffLimit` controls retries. A
CronJob creates Jobs according to a cron expression; in production, consider
duration, concurrency, and Job retention.

## Structure of a Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: completion
spec:
  backoffLimit: 2          # Retries before marking it failed
  template:
    spec:
      restartPolicy: Never # Jobs use Never or OnFailure, never Always
      containers:
        - name: task
          image: busybox:1.36
          command: ["sh", "-c", "echo job completed"]
```

- A Job wraps a Pod template like a Deployment, but the Pod runs a finite task.
- `restartPolicy` must be `Never` or `OnFailure`; `Always` is invalid for Jobs.
- The Job is `Complete` when the container exits successfully and
  `backoffLimit` has not been exceeded.

## Commands

```bash
kubectl apply -f ../../../manifests/jobs/job.yaml
kubectl get jobs,pods
kubectl logs job/fecha-job
kubectl apply -f ../../../manifests/jobs/cronjob.yaml
kubectl get cronjobs,jobs
```

## Exercise

1. Define a Job in `exercise.yaml` that prints output and exits successfully.
2. Apply it and verify the `Complete` status with `kubectl get job`.
3. Inspect its logs with `kubectl logs job/<name>`.
