# Container Network Interface (CNI) in Kubernetes
 Standards:
- Container Runtime must create network namespace
- Identify network that the container must attach to
- Container Runtime to invoke Network Plugin (bridge):
    - When container is ADDed.
    - When container is DELeted.
- JSON format of network configuration

## Configuring CNI
Configure in kubelet service:

In `kubelet.service` file:
```
ExecStart=/usr/local/bin/kubelet \\
...
--network-plugin=cni \\
--cni-bin-dir=/opt/cni/bin \\
--cni-conf-dir=/etc/cni/net.d \\
...
```

View kubelet options:
```
ps -aux | grep kubelet
```
```
ls /opt/cni/bin
```
```
ls /etc/cni/net.d
```
What is the path configured with all binaries of CNI supported plugins?
```
ls /opt/cni/bin/
```
What binary exe file will be executed by kubelet after container and associated namespace are created?
```
cat /etc/cni/net.d/xxxxx
# Look at the type
```