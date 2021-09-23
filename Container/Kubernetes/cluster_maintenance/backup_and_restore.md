# Backup and Restore Methods

## Backup - Resource Configs

```
kubectl get all --all-namespaces -o yaml > all-deployed-services.yaml
```

## Backup - ETCD
Create backup
```
etcdctl snapshot save snapshot.db
```
View backup status
```
etcdctl snapshot status snapshot.db
```
Help
```
etcdctl --h
```
What is the version of ETCD running on the cluster?
```
kubectl describe pod etcd-controlplane --namespace=kube-system
```

### Steps
- Ensure etcdctl api3 is used (on master node): `export ETCDCTL_API=3`
- For each `etcdctl` commands, these fields are mandatory:
```
--cacert xxxx
--cert xxxx
--endpoints=[127.0.0.1:2379]
--key xxxxx
```
- Stop the kube-apiserver: `service kube-apiserver stop`
- Restore the backup db from a directory.
```
etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup
```
- Configure etcd file to use the `data-dir`
 /etc/kubernetes/manifests/etcd.yaml
```
# In etcd.service
ExecStart=...
[...]
--data-dir=/var/lib/etcd-from-backup
```
- Reload the service" `systemctl daemon-reload`
- Start the kube-apiserver: `service kube-apiserver start`

Example:
```
etcdctl --endpoints=https://[127.0.0.1]:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/snapshot-pre-boot.db
```

```
ETCDCTL_API=3 etcdctl --data-dir /var/lib/etcd-from-backup \
snapshot restore /opt/snapshot-pre-boot.db
```