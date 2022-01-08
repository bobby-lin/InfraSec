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

## Liveness Probe
Simulates a scenario where the liveness probe fails...
```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
  - image: busybox
    name: app
    args:
    - /bin/sh
    - -c
    - 'while true; do touch /tmp/heartbeat.txt; sleep 70; done;'
    livenessProbe:
      exec:
        command:
        - test `find /tmp/heartbeat123.txt -mmin -1`
      initialDelaySeconds: 5
      periodSeconds: 30
 ```
We can see the pod has restarted many times due to liveness probe failure (which we can see in the events):
 ```
> kubectl get pods
$ kgp
NAME            READY   STATUS             RESTARTS         AGE
liveness-pod    0/1     CrashLoopBackOff   48 (2m27s ago)   25h
readiness-pod   0/1     Running            0                27h

> kubectl describe pod liveness-pod
Events:
  Type     Reason     Age                  From     Message
  ----     ------     ----                 ----     -------
  Warning  BackOff    13h (x473 over 24h)  kubelet  Back-off restarting failed container
  Warning  Unhealthy  12h (x138 over 25h)  kubelet  Liveness probe failed: OCI runtime exec failed: exec failed: container_linux.go:380: starting container process caused: exec: "test `find /tmp/heartbeat123.txt -mmin -1`": stat test `find /tmp/heartbeat123.txt -mmin -1`: no such file or directory: unknown
  Normal   Pulling    102s (x49 over 25h)  kubelet  Pulling image "busybox"
```

## Startup Probe
Simulates failure scenario for a startup probe (we can see that the wrong port is used)
```
apiVersion: v1
kind: Pod
metadata:
  name: startup-pod
spec:
  containers:
  - image: httpd:2.4.46
    name: http-server
    startupProbe:
      tcpSocket:
        port: 8999
      initialDelaySeconds: 3
      periodSeconds: 15
 ```
 From events of the pod, we can see that a startup probe has failed.
 ```
 > kubectl describe pods startup-pod
 Events:
  Type     Reason     Age   From               Message
  ----     ------     ----  ----               -------
  Normal   Scheduled  25s   default-scheduler  Successfully assigned default/startup-pod to docker-desktop
  Normal   Pulling    25s   kubelet            Pulling image "httpd:2.4.46"
  Normal   Pulled     14s   kubelet            Successfully pulled image "httpd:2.4.46" in 10.9422697s
  Normal   Created    14s   kubelet            Created container http-server
  Normal   Started    14s   kubelet            Started container http-server
  Warning  Unhealthy  11s   kubelet            Startup probe failed: dial tcp 10.1.0.93:8999: connect: connection refused
  ```
