# TLS in Kubernetes
All interaction between master and worker nodes must be encrypted.

## Server components
These components require a public-private keys:
- kube-apiserver
- etcd-server
- kubelet-server

## Client components
These components require their own public-private keys:
- user: admin
- kube-scheduler
- kube-controller manager
- kube-proxy
- kube-apiserver (talking to) -> etcd-server

## Certificate Generation


## View Certificates
### kubeadm way
Get the paths of all the certificates
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml
```
Decode the cert
```
openssl x509 -in /etc/kubernetes/pki/[cert name].crt -text -noout
```
Look for these fields
```
CN Name
ALT Names
Otganization
Issuer
Expiration
```
### manual way
Inspect the service logs
```
journalctl -u etcd.service -l
```
View logs
```
kubectl logs etcd-master
```
If kube-apiserver is down, look in docker level
```
docker ps -a # look for the etcd container
docker logs xxxx
```