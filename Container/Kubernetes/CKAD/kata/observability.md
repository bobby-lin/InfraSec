## Readiness Probe
Creating a failed scenario where the ports in container and readiness probe are configured wrongly
```
apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
spec:
  containers:
  - image: bmuschko/nodejs-hello-world:1.0.0
    name: hello-world
    ports:
    - name: nodejs-port
      containerPort: 3000
    readinessProbe:
      httpGet:
        path: / 
        port: 111 # should fail :(
      initialDelaySeconds: 2
      periodSeconds: 8
```
What we expect is a "readiness probe" error message when we look at the event of the pod.
```
> kubectl describe pods readiness-pod

[...]
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  6m                    default-scheduler  Successfully assigned default/readiness-pod to docker-desktop
  Normal   Pulled     6m                    kubelet            Container image "bmuschko/nodejs-hello-world:1.0.0" already present on machine 
  Normal   Created    5m59s                 kubelet            Created container hello-world
  Normal   Started    5m59s                 kubelet            Started container hello-world
  Warning  Unhealthy  56s (x38 over 5m52s)  kubelet            Readiness probe failed: Get "http://10.1.0.90:111/": dial tcp 10.1.0.90:111: connect: connection refused
```
