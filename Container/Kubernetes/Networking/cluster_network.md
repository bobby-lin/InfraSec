# Cluster network
https://kubernetes.io/docs/reference/ports-and-protocols/

Note these ports need to be opened in the Firewall.

Control plane
Protocol	Direction	Port Range	Purpose	Used By
TCP	Inbound	6443	Kubernetes API server	All
TCP	Inbound	2379-2380	etcd server client API	kube-apiserver, etcd
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	10259	kube-scheduler	Self
TCP	Inbound	10257	kube-controller-manager	Self

Worker node(s) 
Protocol	Direction	Port Range	Purpose	Used By
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	30000-32767	NodePort Services†	All

## Finding mac address of node

```
ssh node01
ifconfig
```
Find the status of interface
```
ip addr
```