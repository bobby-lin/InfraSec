# DNS in Kubernetes
Whare names are assigned to what objects?
Service DNS records
Pod DNS records

## Name resolution in Kubernetes

### Service
<hostname>.<namespace>.<type>.<root> -> Link to IP address of a service

Example:
```
curl https://web-service.apps.svc.cluster.local
```
### Pods
Records for pods are not created by default.

We need enable it for pod.

Format:
<hostname>.<namespace>.<type>.<root> -> Link to IP address of a pod

Example: pod's IP addr is 10.244.2.5

Hostname: 10-244-2-5
Namespace: apps
Type: pod
Root: cluster.local

```
curl http://10-244-2-5.apps.pod.cluster.local
```

## Current Recommended DNS in k8s: CoreDNS
- CoreDNS is deployed as a Pod in kube-system namespace.
- Use a config file: `/etc/coredns/Corefile`
- `kube-dns` service is avaible for other pods
```
kubectl get service -n kube-system
```
- kubelet will add the nameserver in the pod by default
```
cat /var/lib/kubelet/config.yaml
...
clusterDNS:
- 10.96.0.10
clusterDomain: cluster.local
```

## Useful commands
Check the DNS pods to see what DNS solution is implemented
```
kubectl get pods -n kube-system
```
View the name of service for DNS
```
kubectl get services -n kube-system
```
Execute commands in pod
```
kubectl exec -it hr -- nslookup mysql.payroll > /root/CKA/nslookup.out
```