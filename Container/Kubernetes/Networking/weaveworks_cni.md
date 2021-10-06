# Weaveworks (CNI example)
Weaveworks is an example of a CNI plugin.

It uses agent that are installed in the Nodes.

Weave creates their own bridge.

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-changing-configuration-options

## Deploy Weave
Methods:
- Setup as daemon or services
- Deploy as Pods

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

## View logs
```
kubectl logs weave-net-xxxx weave -n kube-system
```
Changing the default IP address and subnet of Weave
```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')&env.IPALLOC_RANGE=10.50.0.0/16"
```
