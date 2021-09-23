# Cluster upgrade
https://kubernetes.io/docs/tasks/administer-cluster/cluster-upgrade/
Upgrade kubeadm: https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

Kubernetes version format: `X.X.X` (Major.Minor.Patch)

## Versions
kube-apiserver version should be higher or equals to other components versions (kube-scheduler and controller-manager).

kubelet and kube-proxy can be two versions behind the version of kube-apiserver

## Recommended upgrade approach
Kubernetes support the last three versions from the latest.

We should upgrade if our current version is unsupported now.

It is recommended to upgrade one minor version at a time.

## How to upgrade?
1) Cloud provider
2) Kubeadm
```
apt-get upgrade -y kubeadm=1.12.0-00
systemctl restart kubelet
kubeadm upgrade plan
kubeadm upgrade apply v1.12.0
```
3) Manual

## Sequences
Upgrade Master node first

### Worker nodes
Strategy 1: Kill all nodes
- Then the new nodes will be recreated with new version as the master node.
- But this requires downtime

Strategy 2: Kill one node, Upgrade the node, Repeat
Drain the Worker node
```
kubectl drain node-1
```
Update worker node
```
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kubeadm upgrade node config --kubelet-version v1.12.0
systemctl restart kubelet
```
Uncordon Worker node
```
kubectl uncordon node-1
```
Strategy 3: Add new nodes to cluster and move workload to new nodes

## Useful commands
Get cluster version
```
kubeadm upgrade plan
```
Update kubeadm
```
apt update
apt install kubeadm=1.20.0-00
```
Upgrade controlplane
```
kubeadm upgrade apply v1.20.0
```

## Upgrading Worker node

From master node, run `ssh nodeX` to go to `nodeX`
```
apt update
apt install kubeadm=<VERSION>-00
kubeadm upgrade node # Upgrade the node configuration.
apt install kubelet=1.20.0-00 # Update kubelet
systemctl restart kubelet
exit
```