# Service and Networking
https://github.com/bmuschko/ckad-crash-course/blob/master/exercises/14-exposing-service/instructions.md

https://github.com/bmuschko/ckad-crash-course/blob/master/exercises/15-creating-networkpolicy/instructions.md

> Note: For docker-desktop user, you need to use localhost instead of the node IP address for testing

## Routing Traffic to Pods from Inside and Outside of a Cluster

1. Create a Service named `myapp` of type `ClusterIP` that exposes port 80 and maps to the target port 80.
```
kubectl create service clusterip myapp --tcp=80:80

$ kubectl get svc
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE  
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        9h   
myapp        ClusterIP   10.97.131.176    <none>        80/TCP         2m42s
```
2. Create a Deployment named `myapp` that creates 1 replica running the image `nginx`. Expose the container port 80.
```
$ kubectl expose deployment myapp --port=80 --target-port=80
```
3. Scale the Deployment to 2 replicas.
```
$ k scale deployment myapp --replicas=2
```
4. Create a temporary Pods using the image `busybox` and run a `wget` command against the IP of the service.
```
kubectl run busybox --image=busybox --rm -it -- //bin//sh

/ # wget --spider 10.97.131.176
Connecting to 10.97.131.176 (10.97.131.176:80)
remote file exists
```
5. Change the service type so that the Pods can be reached from outside of the cluster.
```
Change the Type to NodePort
Add nodePort: 3XXXX
```
6. Run a `wget` command against the service from outside of the cluster.

Get Node IP address
```
kubectl get nodes
kubectl describe [node name] | grep IP
```
```
$ curl localhost:31811 # localhost is used for docker-desktop but you should get the node ip
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>

[...]
```
7. (Optional) Discuss: Can you expose the Pods as a service without a deployment?
8. (Optional) Discuss: Under what condition would you use the service type `LoadBalancer`?

## Restricting Access to and from a Pod

Let's assume we are working on an application stack that defines three different layers: a frontend, a backend and a database. Each of the layers runs in a Pod. You can find the definition in the YAML file `app-stack.yaml`. The application needs to run in the namespace `app-stack`.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: frontend
  namespace: app-stack
  labels:
    app: todo
    tier: frontend
spec:
  containers:
    - name: frontend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: backend
  namespace: app-stack
  labels:
    app: todo
    tier: backend
spec:
  containers:
    - name: backend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: database
  namespace: app-stack
  labels:
    app: todo
    tier: database
spec:
  containers:
    - name: database
      image: mysql
      env:
      - name: MYSQL_ROOT_PASSWORD
        value: example
```

1. Create the required namespace.
2. Copy the Pod definition to the file `app-stack.yaml` and create all three Pods. Notice that the namespace has already been defined in the YAML definition.
3. Create a network policy in the YAML file `app-stack-network-policy.yaml`.
4. The network policy should allow incoming traffic from the backend to the database but disallow incoming traffic from the frontend.
5. Incoming traffic to the database should only be allowed on TCP port 3306 and no other port.
