# Persistent Volume
https://kubernetes.io/docs/concepts/storage/persistent-volumes/

PV is created by administrator.

## Example

Hostpath volume (not recommended)
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  persistentVolumeReclaimPolicy: Retain
  capacity:
    storage: 5Gi
  hostPath:
    path: /tmp/data
  accessModes:
    - ReadWriteOnce # ReadWriteOnce, ReadOnlyMany, ReadWriteMany, ReadWriteOncePod
```
AWS EBS
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  persistentVolumeReclaimPolicy: Recycle # Delete, Retain, Recycle (scrub the data)
  capacity:
    storage: 5Gi
  awsElasticBlockStore:
      volumeID: "<volume id>"
      fsType: ext4
  accessModes:
    - ReadWriteOnce # ReadWriteOnce, ReadOnlyMany, ReadWriteMany, ReadWriteOncePod
```

# Persistent Volume Claims
This is usually created by users for their pods

Example:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

## Adding PVCs to Pods
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes

Once the PVCs are created, you can specify the PVCs in the pod definition file.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```

## Commands
Delete PVCs
```
kubectl delete persistentvolumeclaims myclaim
```

# Common errors
Mismatch between access modes of PV and PVC
```
  Type    Reason         Age                 From                         Message
  ----    ------         ----                ----                         -------
  Normal  FailedBinding  10s (x9 over 118s)  persistentvolume-controller  no persistent volumes available for this claim and no storage class is set
```
If PVC is deleted but a pod is using the PVC, then the claim will be in terminating status.
```
root@controlplane:~# kubectl delete persistentvolumeclaims claim-log-1
root@controlplane:~# kubectl get persistentvolumeclaims 
NAME          STATUS        VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
claim-log-1   Terminating   pv-log   100Mi      RWX                           14m
```