# 01. Architecture and kubectl

## Concepts

Kubernetes maintains a desired state. The `kube-apiserver` receives requests,
`etcd` stores the state, the scheduler assigns Pods to nodes, and controllers
reconcile the current state. On each node, kubelet runs Pods through the
container runtime.

`kubectl` is an API client. `apply` declares state; `get`, `describe`, and
`events` let you observe it.

## Structure of a manifest

Every object you write in YAML has the same four top-level fields. The example
uses a Pod, but the same fields apply to Deployments, Services, and every other
resource.

```yaml
apiVersion: v1          # API group and version this object belongs to
kind: Pod               # Resource type: Pod, Service, Deployment, ...
metadata:               # Identity: names and labels that identify the object
  name: architecture-check
  namespace: architecture-lab
  labels:
    app: architecture-check
spec:                   # Desired state: what Kubernetes must keep running
  containers:           # A Pod spec requires at least one container
    - name: app         # Container name, used in logs and exec commands
      image: nginx:1.27 # Container image and tag
      ports:
        - containerPort: 80
```

- `apiVersion` selects the API group and version. `v1` is the core group;
  Deployments use `apps/v1`, Jobs use `batch/v1`.
- `kind` is the resource type. Together, `apiVersion` + `kind` tell Kubernetes
  which resource you are declaring.
- `metadata` carries the name, namespace, and labels. `name` must be unique
  inside the namespace; `labels` are key/value pairs used later for selectors.
- `spec` is the desired state; the fields inside it depend on the `kind`.
  `status` is the opposite: Kubernetes writes the observed state there, so you
  never put it in your manifest.

Use `kubectl explain` to discover fields, for example
`kubectl explain pod.spec.containers`.

## Commands

```bash
kubectl cluster-info
kubectl get nodes -o wide
kubectl api-resources
kubectl get events --sort-by=.lastTimestamp
kubectl explain pod.spec.containers
```

## Exercise

1. Create a `Namespace` and a `Pod` in `exercise.yaml` using the structure
   above. Give the Pod a name, an `app` label, one container with the
   `nginx:1.27` image, and container port `80`.
2. Apply it with `kubectl apply -f exercise.yaml`.
3. Confirm both objects exist and the Pod reaches `Running` with
   `kubectl get namespace,pod -o wide`.
4. Identify which cluster components participate when the Pod changes from
   `Pending` to `Running`: who stores the manifest, who chooses the node, and
   who starts the container.
