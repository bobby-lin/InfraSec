# Multiple Container Pods
There are cases where you need two different containers to work together (e.g. a web server and logging agent).

We use multiple containers in a pod to achieve this goal.

## Example of multiple container pod definition
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-pod
spec:
  containers:
    - name: nginx
      image: nginx
    - name: logger
      image: myorg/logger
```

## Common Multi-container design
- Sidecar
- Adapter
- Ambassador

## Useful commands
View logs of a pod in a namespace
```
kubectl -n elastic-stack logs kibana
```

## Init Container
https://kubernetes.io/docs/concepts/workloads/pods/init-containers/
They are "specialized containers that run before app containers in a Pod. Init containers can contain utilities or setup scripts not present in an app image."

Each init container will run sequentially one at a time.

For example, we can see the init:x/[#initContainers] under STATUS
```
root@controlplane:~# kubectl get pods
NAME    READY   STATUS     RESTARTS   AGE
blue    0/1     Init:0/1   0          21s
green   2/2     Running    0          21s
red     1/1     Running    0          21s
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]
```