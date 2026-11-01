# Kubernetes Guide: From Zero to Junior Level

This directory contains a progressive Kubernetes lab using
[`kind`](https://kind.sigs.k8s.io/). The manifests explain what each resource
does, how resources relate to one another, and how to troubleshoot an
application.

> **Junior goal:** deploy a stateless application, expose it, configure it,
> add storage when needed, apply basic health and access controls, and find the
> cause of failures with `kubectl`.

## Topic roadmap

Use the [topic roadmap](./roadmap/README.md) for the complete learning path.
Every topic directory contains three files:

- `README.md`: concepts, commands, and the exercise description.
- `exercise.yaml`: an empty file for your own solution.
- `solution.yaml`: a completed reference solution to consult after trying.

## Prerequisites

- Basic Linux, HTTP/TCP networking, and YAML knowledge.
- Docker running locally.
- [`kubectl`](https://kubernetes.io/docs/tasks/tools/) installed.
- [`kind`](https://kind.sigs.k8s.io/docs/user/quick-start/) installed.
- Git and a Bash terminal.

Kubernetes does not replace container fundamentals. If images, processes,
ports, environment variables, and volumes are new to you, start with the
[`docker-practices`](../docker-practices/) directory.

## Architecture

Kubernetes is a distributed system that maintains a **desired state**. You
declare that state with YAML or `kubectl`; the control plane observes the
current state and acts until it matches the desired state.

```text
                         kubectl / CI / users
                                  |
                                  v
                         +------------------+
                         | kube-apiserver   |
                         | auth + RBAC      |
                         +--------+---------+
                                  |
                +-----------------+-----------------+
                |                                   |
                v                                   v
       +------------------+               +------------------+
       | etcd             |               | scheduler        |
       | cluster state    |               | assigns Pods     |
       +------------------+               +------------------+
                ^                                   |
                |                                   v
       +------------------+               +------------------+
       | controller       |               | kubelet          |
       | manager          |               | runs Pods        |
       +------------------+               +--------+---------+
                                                     |
                                                     v
                                          container runtime
                                  CNI / kube-proxy / CSI
                                  Ingress Controller
```

### Control plane components

- **API server:** entry point to the cluster; validates requests and exposes
  the resource API.
- **etcd:** distributed database that stores cluster state.
- **scheduler:** selects a node for each pending Pod based on resources and
  constraints.
- **controller manager:** runs reconciliation loops. For example, a Deployment
  creates a ReplicaSet and replaces failed Pods.

### Node components

- **kubelet:** starts containers for assigned Pods and reports their status.
- **Container runtime:** runs containers, commonly containerd.
- **kube-proxy:** implements part of Service connectivity.
- **CNI:** provides Pod networking and IP addresses.
- **CSI:** connects Kubernetes to storage providers.

### Request flow

1. `kubectl apply` sends a manifest to the API server.
2. The API server validates it and stores the object in etcd.
3. Controllers create dependent resources and the scheduler assigns Pods.
4. The node kubelet downloads the image and starts the container.
5. A Service selects Pods by labels and provides a stable address.
6. An Ingress Controller can receive HTTP/HTTPS and route it to a Service.
7. Kubelet and controllers update `status`; you observe it with `get`,
   `describe`, and `logs`.

**Key idea:** a Pod is not a VM or a durable deployment unit. It is the
smallest schedulable unit and can contain one or more containers that share
network (`localhost`) and volumes. Production Pods are usually managed by
Deployments, StatefulSets, Jobs, or DaemonSets.

## Create the lab

From this directory:

```bash
kind create cluster --name practicas-k8s --config kind-config.yaml
kubectl cluster-info --context kind-practicas-k8s
kubectl get nodes -o wide
```

[`kind-config.yaml`](./kind-config.yaml) creates a local control-plane node
and maps:

| Host | kind node | Purpose |
|---:|---:|---|
| `localhost:8080` | `30080` | `NodePort` Service |
| `localhost:80` | `80` | HTTP Ingress |
| `localhost:443` | `443` | HTTPS Ingress |

This is a local learning cluster, not a highly available production cluster.

```bash
kind delete cluster --name practicas-k8s
```

## Learning sequence

Follow the [roadmap by topic](./roadmap/README.md). It contains the complete
learning material and one empty exercise file for every topic, including Pods,
Deployments, Services, configuration, probes, storage, Jobs, networking,
RBAC, autoscaling, scheduling, observability, packaging, security, and
production delivery.

## Troubleshooting workflow

Use this sequence before changing YAML randomly:

```bash
kubectl get pods -A
kubectl describe pod <pod>
kubectl logs <pod> --all-containers
kubectl get events --sort-by=.lastTimestamp
kubectl get deploy,rs,svc,endpoints,pvc
kubectl explain deployment.spec.template.spec.containers
```

| Symptom | First checks |
|---|---|
| `Pending` | Pod events, requests, scheduler, PVC, and nodes. |
| `CrashLoopBackOff` | `logs --previous`, command, variables, and probes. |
| Service has no traffic | Labels, selectors, and endpoints. |
| Ingress does not respond | Controller, IngressClass, host, Service, and ports. |
| PVC is `Pending` | StorageClass, available PV, access mode, and size. |
| HPA has no metrics | `kubectl top pods`, metrics-server, and requests. |
| `Forbidden` | Identity, namespace, Role/ClusterRole, and Binding. |

## Junior-level checklist

- [ ] Explain the control plane, nodes, kubelet, scheduler, etcd, and API server.
- [ ] Distinguish Pod, ReplicaSet, Deployment, StatefulSet, Job, and DaemonSet.
- [ ] Create a Deployment with labels, requests, limits, and probes.
- [ ] Expose an application with `ClusterIP`, `NodePort`, and Ingress.
- [ ] Diagnose `Pending`, `CrashLoopBackOff`, and a Service without endpoints.
- [ ] Use ConfigMaps and Secrets without confusing configuration and credentials.
- [ ] Explain why `emptyDir` is not persistent and how a PVC works.
- [ ] Apply least-privilege RBAC and a NetworkPolicy.
- [ ] Explain why HPA needs metrics and resource requests.
- [ ] Clean resources and reproduce the lab from scratch.

After completing this checklist, continue with Helm, Kustomize, observability,
CI/CD, image security, admission policies, and a cloud provider.
