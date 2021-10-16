# Storage (10%)

## Understand storage classes, persistent volumes

## Understand volume mode, access modes and reclaim policies for volumes

## Understand persistent volume claims primitive

## Know how to configure applications with persistent storage

# Persistent Volume
Doc: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

## Create Persistent Claim
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
