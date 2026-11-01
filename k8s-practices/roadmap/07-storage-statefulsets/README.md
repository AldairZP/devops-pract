# 07. Storage and StatefulSets

## Concepts

`emptyDir` is temporary and lives while the Pod exists. A PV represents
available storage; a PVC is a user's storage request. A StorageClass enables
dynamic provisioning, and CSI connects Kubernetes to the storage provider.
StatefulSet adds stable identity and one PVC per replica.

## Commands

```bash
kubectl apply -f ../../../manifests/storage/volumenes.yaml
kubectl get pv,pvc
kubectl apply -f ../../../manifests/storage/statefulset.yaml
kubectl get statefulset,pods,pvc
```

## Exercise

Define a PVC and a Pod that mounts it at `/data` in `exercise.yaml`. Write a
file, recreate the Pod, and verify persistence.
