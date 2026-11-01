# 08. Jobs and CronJobs

## Concepts

A Job runs a Pod until a task completes. `backoffLimit` controls retries. A
CronJob creates Jobs according to a cron expression; in production, consider
duration, concurrency, and Job retention.

## Commands

```bash
kubectl apply -f ../../../manifests/jobs/job.yaml
kubectl get jobs,pods
kubectl logs job/fecha-job
kubectl apply -f ../../../manifests/jobs/cronjob.yaml
kubectl get cronjobs,jobs
```

## Exercise

Define a Job in `exercise.yaml` that prints output and exits successfully.
Verify the `Complete` status and inspect its logs.
