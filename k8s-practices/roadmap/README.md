# Kubernetes Roadmap

Practical path from the fundamentals to the knowledge expected from a junior
Kubernetes engineer. Complete the topics in order because each exercise builds
on the previous concepts.

|   # | Topic                                                                 | What you should learn                                         | Folder                       |
| --: | --------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------- |
|   1 | [Architecture and `kubectl`](./01-architecture-kubectl/README.md)     | Cluster, control plane, nodes, API, and declarative lifecycle | `01-architecture-kubectl`    |
|   2 | [Pods and containers](./02-pods-containers/README.md)                 | Smallest unit, lifecycle, logs, and execution                 | `02-pods-containers`         |
|   3 | [Deployments and ReplicaSets](./03-deployments-replicasets/README.md) | Replicas, self-healing, and updates                           | `03-deployments-replicasets` |
|   4 | [Labels, selectors, and Services](./04-services-networking/README.md) | Discovery and application exposure                            | `04-services-networking`     |
|   5 | [ConfigMaps and Secrets](./05-configmaps-secrets/README.md)           | External configuration and sensitive data                     | `05-configmaps-secrets`      |
|   6 | [Requests, limits, and probes](./06-resources-health/README.md)       | Scheduling, limits, and container health                      | `06-resources-health`        |
|   7 | [Persistence and StatefulSets](./07-storage-statefulsets/README.md)   | PV, PVC, StorageClass, and state                              | `07-storage-statefulsets`    |
|   8 | [Jobs and CronJobs](./08-jobs-cronjobs/README.md)                     | Finite and scheduled tasks                                    | `08-jobs-cronjobs`           |
|   9 | [Ingress and NetworkPolicy](./09-ingress-network-policy/README.md)    | HTTP entry points and network isolation                       | `09-ingress-network-policy`  |
|  10 | [RBAC and ServiceAccounts](./10-rbac-service-accounts/README.md)      | Identity, permissions, and least privilege                    | `10-rbac-service-accounts`   |
|  11 | [HPA and metrics](./11-hpa-autoscaling/README.md)                     | Usage-based horizontal scaling                                | `11-hpa-autoscaling`         |
|  12 | [DaemonSets and operations](./12-daemonsets-operations/README.md)     | Per-node agents, troubleshooting, and cleanup                 | `12-daemonsets-operations`   |
|  13 | [Namespaces and YAML design](./13-namespaces-yaml/README.md)          | Organization, scope, metadata, and reusable manifests         | `13-namespaces-yaml`         |
|  14 | [Scheduling and node placement](./14-scheduling-placement/README.md)  | Taints, tolerations, affinity, and topology                   | `14-scheduling-placement`    |
|  15 | [Observability and debugging](./15-observability-debugging/README.md) | Logs, events, metrics, and application visibility             | `15-observability-debugging` |
|  16 | [Helm and Kustomize](./16-helm-kustomize/README.md)                   | Packaging and environment-specific configuration              | `16-helm-kustomize`          |
|  17 | [Security hardening](./17-security-hardening/README.md)               | Pod security, image security, and safe defaults               | `17-security-hardening`      |
|  18 | [Production delivery](./18-production-delivery/README.md)             | CI/CD, rollouts, backups, and reliability                     | `18-production-delivery`     |

## How to use each topic

1. Read the topic `README.md`.
2. Run only the observation commands provided.
3. Complete `exercise.yaml` without copying a finished solution.
4. Apply your file with `kubectl apply -f exercise.yaml`.
5. Compare your result with `solution.yaml` only after attempting the exercise.
6. Verify the result and record what you learned.

Each topic `README.md` explains the structure of the resources involved
(`apiVersion`, `kind`, `metadata`, `spec`, and the relevant fields) before the
exercise, so you understand each part instead of copying a manifest.

Topics 1-12 cover the core junior curriculum. Topics 13-18 extend the path
with organization, scheduling, observability, packaging, security, and
production practices.

When finished, return to the
[junior checklist](../README.md#junior-level-checklist).
