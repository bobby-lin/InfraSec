## Exposing K8s ports

### Service Types
- ClusterIP service
- NodePort service
- LoadBalancer service
- External DNS

## ClusterIP Services
### Creating a ClusterIP service
Create a deployment
```
kubectl create deployment httpenv --image=bretfisher/httpenv
```
Scale the deployment
```
kubectl scale deployment httpenv --replicas=5
```
Exposing Deployment on a port
```
kubectl expose deployment/httpenv --port 8888
```

### Inspecting ClusterIP service
Check the allocated IP
```
kubectl get service
```
##### How to test cluster IP (which is internal only)?
Note: If you are using Docker Desktop
```
kubectl run --generator=run-pod/v1 tmp-shell --rm -it --image bretfisher/netshoot -- bash
```
Then do a curl in bash:
```
curl 10.104.193.40:8888
{"HOME":"/root","HOSTNAME":"httpenv-6fdc8554fb-hnjxm","KUBERNETES_PORT":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP":"tcp://10.96.0.1:443","KUBERNETES_PORT_443_TCP_ADDR":"10.96.0.1","KUBERNETES_PORT_443_TCP_PORT":"443","KUBERNETES_PORT_443_TCP_PROTO":"tcp","KUBERNETES_SERVICE_HOST":"10.96.0.1","KUBERNETES_SERVICE_PORT":"443","KUBERNETES_SERVICE_PORT_HTTPS":"443","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"}
```
Note: if you are in linux
```
curl [ip of service]:8888
```

##### Create NodePort
NodePort is additive -> When you create NodePort, you will also create ClusterIP.

Expose a NodePort
```
kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort
```
Explanation: The left Port is exposed within cluster while right Port is exposed externally (default range is  30000-32767)
```
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/httpenv      ClusterIP   10.104.193.40    <none>        8888/TCP         16h
service/httpenv-np   NodePort    10.102.100.151   <none>        8888:31698/TCP   52s
service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          18h
```

##### Create LoadBalancer

```
kubectl expose deployment/httpenv --port 8888 --name httpenv-lb --type LoadBalancer
```

#### Delete Services
You can delete service or deployment in a single command:
```
kubectl delete service/httpenv service/httpenv-np
kubectl delete service/httpenv-lb deployment/httpenv
```

### Kubernetese Services DNS
This is DNS-Based Service Discovery

You can perform a curl on the hostname which works for services in the same namespaces
```
curl <hostname>
kubectl get namespaces
```
