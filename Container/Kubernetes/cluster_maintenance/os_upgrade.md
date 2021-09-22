# OS Upgrade
When?
- When applying patches
- Applying security updates

## Useful commands
Make the node unschedulable and move the pods to other clusters
```
kubectl drain node-1
```
Make the node unschedulable but does not move pods to the clusters.
```
kubectl cordon node-1
```
### After applying patches
Use `uncordon` to make the node schedulable again
```
kubectl uncordon node-1
```
## Common errors

```
root@controlplane:~# kubectl drain node01
node/node01 already cordoned
error: unable to drain node "node01", aborting command...

There are pending nodes to be drained:
 node01
error: cannot delete DaemonSet-managed Pods (use --ignore-daemonsets to ignore): kube-system/kube-flannel-ds-v5wlz, kube-system/kube-proxy-rw6m4
root@controlplane:~# kubectl drain node01 --ignore-daemonsets
```

Pods not in the replicaset
```
root@controlplane:~# kubectl drain node01 --ignore-daemonsets
node/node01 already cordoned
error: unable to drain node "node01", aborting command...

There are pending nodes to be drained:
 node01
error: cannot delete Pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet (use --force to override): default/hr-app
```
The pod that is forcefully drained will be removed
```
kubectl drain node01 --ignore-daemonsets --force
```
Mark a node as unschedulable to prevent new pods from being scheduled on this node. But do not move or forcefully destroy the pods
```
kubectl cordon node01
```