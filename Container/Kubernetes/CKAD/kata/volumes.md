# Persistent Storage

https://github.com/bmuschko/ckad-crash-course/blob/master/exercises/16-creating-persistentvolume/instructions.md

## Defining and Mounting a PersistentVolume

1. Create a Persistent Volume named `pv`, access mode `ReadWriteMany`, storage class name `shared`, 512MB of storage capacity and the host path `/data/config`.
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv
spec:
  capacity:
    storage: 512m
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /data/config
  storageClassName: shared
  ```
2. Create a Persistent Volume Claim named `pvc` that requests the Persistent Volume in step 1. The claim should request 256MB. Ensure that the Persistent Volume Claim is properly bound after its creation.
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 256m
  storageClassName: shared
```
3. Mount the Persistent Volume Claim from a new Pod named `app` with the path `/var/app/config`. The Pod uses the image `nginx`.
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: app
  name: app
spec:
  volumes:
    - name: app-volume
      persistentVolumeClaim:
        claimName: pvc
  containers:
  - image: nginx
    name: nginx
    resources: {}
    volumeMounts:
      - name: app-volume
        mountPath: /var/app/config
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
4. Check the events of the Pod after starting it to ensure that the Persistent Volume was mounted properly.
```
$ k describe pods app | grep -C 3 Mounts
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/app/config from app-volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xwj6h (ro)
```

## Volumes
1. Create a Pod YAML file with two containers that use the image `alpine:3.12.0`. Provide a command for both containers that keep them running forever.
2. Define a Volume of type `emptyDir` for the Pod. Container 1 should mount the Volume to path `/etc/a`, and container 2 should mount the Volume to path `/etc/b`.
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: alpine-pod
  name: alpine-pod
spec:
  volumes:
    - name: my-volume
      emptyDir: {}
  containers:
  - image: alpine:3.12.0
    name: alpine-a
    args:
      - /bin/sh
      - -c
      - while true; do sleep 60; done;
    volumeMounts:
    - name: my-volume
      mountPath: /etc/a
  - image: alpine:3.12.0
    name: alpine-b
    args:
      - /bin/sh
      - -c
      - while true; do sleep 60; done;
    volumeMounts:
    - name: my-volume
      mountPath: /etc/b
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
3. Open an interactive shell for container 1 and create the directory data in the mount path. Navigate to the directory and create the file `hello.txt` with the contents “Hello World.” Exit out of the container.
```
$ kubectl exec alpine-pod -c alpine-a -it -- //bin//sh
/ # cd /etc/a
/etc/a # echo Hello World > hello.txt
/etc/a # cat hello.txt
Hello World
/etc/a # exit
```
4. Open an interactive shell for container 2 and navigate to the directory `/etc/b`. Inspect the contents of file `hello.txt`. Exit out of the container.
```
$ k exec alpine-pod -c alpine-b -it -- //bin//sh
/ # cd /etc/b
/etc/b # ls
data       hello.txt
/etc/b # cat hello.txt
Hello World
/etc/b # exit
```
### PV and PVC
5. Create a PersistentVolume named `logs-pv` that maps to the hostPath `/var/logs`. The access mode should be ReadWriteOnce and ReadOnlyMany. Provision a storage capacity of 5Gi. Ensure that the status of the PersistentVolume shows Available.
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: logs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
    - ReadOnlyMany
  hostPath:
    path: /var/logs
```
```
$ k get pv logs-pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      [...]   AGE
logs-pv   5Gi        RWO,ROX        Retain          Available            51s
```
6. Create a PersistentVolumeClaim named `logs-pvc`. The access it uses is `ReadWriteOnce`. Request a capacity of 2Gi. Ensure that the status of the PersistentVolume shows Bound.
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: logs-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```
```
$ k get pvc
NAME       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
db-pvc     Bound    pvc-ca019e04-6f02-4120-b219-bc590d9ad603   512m       RWO            hostpath       3h17m
logs-pvc   Bound    pvc-4d74e716-6d8e-4ca3-8ec7-5f3680f53920   2Gi        RWO            hostpath       5s
```
7. Mount the PersistentVolumeClaim in a Pod running the image nginx at the mount path `/var/log/nginx`.
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  volumes:
    - name: logs-volume
      persistentVolumeClaim:
        claimName: logs-pvc
  containers:
  - image: nginx
    name: nginx
    resources: {}
    volumeMounts:
      - name: logs-volume
        mountPath: /var/log/nginx
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```
8. Open an interactive shell to the container and create a new file named `my-nginx.log` in `/var/log/nginx`. Exit out of the Pod.
9. Delete the Pod and re-create it with the same YAML manifest. Open an interactive shell to the Pod, navigate to the directory `/var/log/nginx`, and find the file you created before.
```
$ k exec nginx -it -- //bin//sh
# cat /var/log/nginx/log
cat: /var/log/nginx/log: No such file or directory
# cat /var/log/nginx/my-nginx.log
hello
# exit
```