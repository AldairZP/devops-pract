# 04. Labels, selectors, and Services

## Concepts

Labels identify objects and selectors connect them. A Service provides a stable
address even when Pods change. `ClusterIP` is internal, `NodePort` publishes a
node port, and `LoadBalancer` requires infrastructure integration.

## Structure of a Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service          # Stable DNS name inside the cluster
spec:
  selector:
    app: web                 # Matches the labels of the target Pods
  ports:
    - port: 80               # Port exposed by the Service
      targetPort: 80         # Port the container listens on
```

- The Service has no containers. It finds Pods by matching `spec.selector`
  against `metadata.labels`.
- `port` is the port clients connect to; `targetPort` is the Pod port the
  traffic is forwarded to.
- Kubernetes creates `Endpoints` with the IPs of the matching Pods. No
  endpoints means the selector matches no running Pod.

## Commands

```bash
kubectl apply -f ../../../manifests/base/nginx-service.yaml
kubectl get svc,endpoints
kubectl describe service nginx-service
curl http://localhost:8080
```

## Exercise

1. Create a Deployment and a Service in `exercise.yaml` that selects the
   application by label.
2. Apply them and verify the Service has endpoints with
   `kubectl get svc,endpoints`.
3. Explain what happens when you change the selector to a label that no Pod
   has.
