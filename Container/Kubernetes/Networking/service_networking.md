# Service Networking

## Useful commands
Find network range of nodes in cluster
```
ip addr # find eth0
```
Find the Ip addresses allocated to pods
```
kubectl logs weave-net-j7pr9 weave -n kube-system | grep ipalloc
```
Get IP range of service cluster
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep cluster-ip-range
```

```
kubectl logs <kube-proxy-pod-name> -n kube-system
```