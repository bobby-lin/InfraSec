# Multiple scheduler
https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/

## Deploy additional scheduler to the cluster
Search for the kube-scheduler manifest file used by kubeadm:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=false # set to false if you want multiple scheduler
    - --scheduler-name=my-custom-scheduler
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namesoace: kube-system
spec:
  containers:
  - image: nginx
    name: nginx
  schedulerName: my-custom-scheduler # add your custom scheduler name here
```

## Useful commands
View the pod creation event to see if the pod is created by the custom scheduler
```
kubectl get events
```
```
root@controlplane:~# kubectl get events -o wide
LAST SEEN   TYPE     REASON                    OBJECT              SUBOBJECT                SOURCE                                    MESSAGE                                                               FIRST SEEN   COUNT   NAME
28m         Normal   NodeHasSufficientMemory   node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasSufficientMemory              28m          8       controlplane.16a3b5d5a7cab473
28m         Normal   NodeHasNoDiskPressure     node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasNoDiskPressure                28m          8       controlplane.16a3b5d5a7cb18cb
28m         Normal   NodeHasSufficientPID      node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasSufficientPID                 28m          7       controlplane.16a3b5d5a7cb4276
27m         Normal   Starting                  node/controlplane                            kubelet, controlplane                     Starting kubelet.                                                     27m          1       controlplane.16a3b5ddda015de1
27m         Normal   NodeHasSufficientMemory   node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasSufficientMemory              27m          1       controlplane.16a3b5de09bf75c7
27m         Normal   NodeHasNoDiskPressure     node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasNoDiskPressure                27m          1       controlplane.16a3b5de09bf99b8
27m         Normal   NodeHasSufficientPID      node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeHasSufficientPID                 27m          1       controlplane.16a3b5de09bfafa3
27m         Normal   NodeAllocatableEnforced   node/controlplane                            kubelet, controlplane                     Updated Node Allocatable limit across pods                            27m          1       controlplane.16a3b5de489a63dc
27m         Normal   RegisteredNode            node/controlplane                            node-controller                           Node controlplane event: Registered Node controlplane in Controller   27m          1       controlplane.16a3b5e021671d06
27m         Normal   Starting                  node/controlplane                            kube-proxy, controlplane                  Starting kube-proxy.                                                  27m          1       controlplane.16a3b5e16e5e0b56
27m         Normal   NodeReady                 node/controlplane                            kubelet, controlplane                     Node controlplane status is now: NodeReady                            27m          1       controlplane.16a3b5e31ced9941
2m51s       Normal   Scheduled                 pod/nginx                                    my-scheduler, my-scheduler-controlplane   Successfully assigned default/nginx to controlplane                   2m51s        1       nginx.16a3b73c9bf08cc4
2m49s       Normal   Pulling                   pod/nginx           spec.containers{nginx}   kubelet, controlplane                     Pulling image "nginx"                                                 2m49s        1       nginx.16a3b73d26752fd5
2m22s       Normal   Pulled                    pod/nginx           spec.containers{nginx}   kubelet, controlplane                     Successfully pulled image "nginx" in 27.001031757s                    2m22s        1       nginx.16a3b7436fd88192
2m22s       Normal   Created                   pod/nginx           spec.containers{nginx}   kubelet, controlplane                     Created container nginx                                               2m22s        1       nginx.16a3b74380a7296e
2m21s       Normal   Started                   pod/nginx           spec.containers{nginx}   kubelet, controlplane                     Started container nginx                                               2m21s     
```
View the logs of a custom scheduler
```
kubectl logs my-custom-scheduler --namespace=kube-system
```

```
kubectl describe pods kube-scheduler-controlplane --namespace=kube-system | grep Image
```

```
  Type     Reason     Age                    From     Message
  ----     ------     ----                   ----     -------
  Warning  BackOff    2m12s (x5 over 2m52s)  kubelet  Back-off restarting failed container
  Normal   Pulled     119s (x4 over 2m59s)   kubelet  Container image "k8s.gcr.io/kube-scheduler:v1.20.0" already present on machine
  Normal   Created    119s (x4 over 2m59s)   kubelet  Created container kube-scheduler
  Normal   Started    119s (x4 over 2m58s)   kubelet  Started container kube-scheduler
  Warning  Unhealthy  37s (x8 over 107s)     kubelet  Startup probe failed: Get "https://127.0.0.1:10299/healthz": http: server gave HTTP response to HTTPS client
  ```