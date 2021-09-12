# Static Pods
https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/

It is possible to create Pod without using kube-apiserver.

These Pods are known as static Pods. And the static Pods are created by kubelet.

Simply place the pod definition file in a /etc/kubernetes/manifest directory.

Then the pods will be created.

## Useful commands
Find the static pods (look for those names with `-controlplane`)
```
kubectl get pods --all-namespaces
```
View the definition files of the static Pods
```
cd /etc/kubernetes/manifest
```

https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/
