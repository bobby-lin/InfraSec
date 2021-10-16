# Storage (10%)

## Understand storage classes, persistent volumes
Storage Classes: https://kubernetes.io/docs/concepts/storage/storage-classes/

PV: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

## Understand volume mode, access modes and reclaim policies for volumes
Volume: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#volume-mode

Access Modes: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes 

Reclaiming: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming 

## Understand persistent volume claims primitive
https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims

### Create Persistent Claim
Doc: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims

Scenario: If you notice an error message in a Pod showing that "pvc" does not exist, then you need to create it.

<details><summary>Solution</summary>
<p>

Use this manifest:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-claim
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1GB
```

Specify pv claims in the pod manifest

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

<p>
</details>

PV: https://kubernetes.io/docs/tasks/administer-cluster/change-pv-reclaim-policy/

## Know how to configure applications with persistent storage
Doc: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
