# Manifests

Reference manifests used by the commands in
[`k8s-practices/roadmap`](../k8s-practices/roadmap/README.md). They are applied
from each topic directory, so the README commands reference them as
`../../../manifests/...`.

| Topic | Manifest |
|---|---|
| 02 Pods and containers | [`base/nginx-pod.yaml`](./base/nginx-pod.yaml) |
| 03 Deployments and ReplicaSets | [`base/nginx-deployment.yaml`](./base/nginx-deployment.yaml) |
| 04 Labels, selectors, and Services | [`base/nginx-service.yaml`](./base/nginx-service.yaml) |
| 05 ConfigMaps and Secrets | [`config/mensaje.txt`](./config/mensaje.txt), [`config/pod-con-configmap.yaml`](./config/pod-con-configmap.yaml), [`config/pod-secreto.yaml`](./config/pod-secreto.yaml) |
| 06 Requests, limits, and probes | [`base/nginx-probe.yaml`](./base/nginx-probe.yaml) |
| 07 Persistence and StatefulSets | [`storage/volumenes.yaml`](./storage/volumenes.yaml), [`storage/statefulset.yaml`](./storage/statefulset.yaml) |
| 08 Jobs and CronJobs | [`jobs/job.yaml`](./jobs/job.yaml), [`jobs/cronjob.yaml`](./jobs/cronjob.yaml) |
| 09 Ingress and NetworkPolicy | [`networking/network-policy.yaml`](./networking/network-policy.yaml), [`networking/ingress.yaml`](./networking/ingress.yaml) |
| 10 RBAC and ServiceAccounts | [`rbac/rbac.yaml`](./rbac/rbac.yaml) |
| 11 HPA and metrics | [`autoscaling/stress-deployment.yaml`](./autoscaling/stress-deployment.yaml), [`autoscaling/hpa.yaml`](./autoscaling/hpa.yaml) |
| 12 DaemonSets and operations | [`daemonset/daemonset.yaml`](./daemonset/daemonset.yaml) |

Topic 09 needs an Ingress Controller. Install it once per cluster with:

```bash
kubectl apply -f manifests/networking/ingress-nginx-controller.yaml
```

Topic 11 needs Metrics Server. Install it from the repository root with
`kubectl apply -f metrics-server.yaml`.

The base manifests share the `app: nginx` label, so the Deployment created in
topic 03 and the Pod created in topic 02 are both selected by the Service in
topic 04. Delete the standalone Pod before exposing the Deployment if you want
the Service to reach only the Deployment replicas.
