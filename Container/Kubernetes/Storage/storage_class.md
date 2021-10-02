# Storage Class
https://kubernetes.io/docs/concepts/storage/storage-classes/

Without Storage Class, we have to provision for storage manually.

This manual process is called "Static Provisioning".

With storage class, we can dynamically provision for storage.

## Example
storage class def:
```yaml
git
``
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Retain
allowVolumeExpansion: true
mountOptions:
  - debug
volumeBindingMode: Immediate
```
In PVC def:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: google-storage
  resources:
    requests:
      storage: 500Mi
```
PV will be created automatically by the storage class.

## Common errors
This is waiting for the first pod to consume the PVC first.
FYI: The Storage Class which use VolumeBindingMode set to `WaitForFirstConsumer` will delay the binding and provisioning of a PersistentVolume until a Pod using the PersistentVolumeClaim is created.

```
Events:
  Type    Reason                Age               From                         Message
  ----    ------                ----              ----                         -------
  Normal  WaitForFirstConsumer  1s (x4 over 44s)  persistentvolume-controller  waiting for first consumer to be created before binding
```