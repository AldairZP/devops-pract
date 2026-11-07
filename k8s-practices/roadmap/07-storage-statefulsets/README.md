# 07. Storage and StatefulSets

## Concepts

`emptyDir` is temporary and lives while the Pod exists. A PV represents
available storage; a PVC is a user's storage request. A StorageClass enables
dynamic provisioning, and CSI connects Kubernetes to the storage provider.
StatefulSet adds stable identity and one PVC per replica.

## Structure of a PVC and its Pod

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: storage
spec:
  accessModes:
    - ReadWriteOnce       # Mounted by one node at a time
  resources:
    requests:
      storage: 100Mi      # Size requested from the storage class
---
apiVersion: v1
kind: Pod
metadata:
  name: storage-pod
spec:
  containers:
    - name: writer
      image: busybox:1.36
      volumeMounts:
        - name: data
          mountPath: /data # Where the volume appears in the container
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: storage # Links the Pod to the PVC
```

- The PVC is the request; the StorageClass or an administrator satisfies it
  with a PV. In this lab, the default StorageClass creates the PV dynamically.
- `volumeMounts.mountPath` is the directory inside the container.
  `volumes[].persistentVolumeClaim.claimName` connects the Pod to the PVC.
- Data survives Pod recreation because the PVC outlives the Pod.

## Commands

```bash
kubectl apply -f ../../../manifests/storage/volumenes.yaml
kubectl get pv,pvc
kubectl apply -f ../../../manifests/storage/statefulset.yaml
kubectl get statefulset,pods,pvc
```

## Exercise

1. Define a PVC and a Pod that mounts it at `/data` in `exercise.yaml`.
2. Apply them and write a file inside `/data` with `kubectl exec`.
3. Delete the Pod, recreate it, and verify the file is still there.
