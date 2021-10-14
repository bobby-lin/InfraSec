# Debugging Networking issues
https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/

https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/

## Network plugin
Kubernetes use CNI plugins for setting up network.

`kubelet` will execute plugins based on the parameters in `kubelet` config file:
- cni-bin-dir (the directory that contains the plugins)
- network-plugin (the name of the plugin - must corresponds to the directory)

### Examples of plugin
#### Installing Weave Net (available in official doc)
```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
source: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/

#### Installing Flannel
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml
```
Note: Flannel currently does not support kubernetes network policies.

#### Installing Calico
```
curl https://docs.projectcalico.org/manifests/calico.yaml -O
kubectl apply -f calico.yaml
```

## DNS
Kubernetes uses CoreDNS.

### Troubleshoot
Troubleshooting issues related to coreDNS
1. If you find CoreDNS pods in pending state first check network plugin is installed.

2. coredns pods have CrashLoopBackOff or Error state

If you have nodes that are running SELinux with an older version of Docker you might experience a scenario where the coredns pods are not starting. To solve that you can try one of the following options:

a)Upgrade to a newer version of Docker.

b)Disable SELinux.

c)Modify the coredns deployment to set allowPrivilegeEscalation to true:



kubectl -n kube-system get deployment coredns -o yaml | \
  sed 's/allowPrivilegeEscalation: false/allowPrivilegeEscalation: true/g' | \
  kubectl apply -f -

d)Another cause for CoreDNS to have CrashLoopBackOff is when a CoreDNS Pod deployed in Kubernetes detects a loop.



  There are many ways to work around this issue, some are listed here:



Add the following to your kubelet config yaml: resolvConf: <path-to-your-real-resolv-conf-file> This flag tells kubelet to pass an alternate resolv.conf to Pods. For systems using systemd-resolved, /run/systemd/resolve/resolv.conf is typically the location of the "real" resolv.conf, although this can be different depending on your distribution.

Disable the local DNS cache on host nodes, and restore /etc/resolv.conf to the original.

A quick fix is to edit your Corefile, replacing forward . /etc/resolv.conf with the IP address of your upstream DNS, for example forward . 8.8.8.8. But this only fixes the issue for CoreDNS, kubelet will continue to forward the invalid resolv.conf to all default dnsPolicy Pods, leaving them unable to resolve DNS.
3. If CoreDNS pods and the kube-dns service is working fine, check the kube-dns service has valid endpoints.

              kubectl -n kube-system get ep kube-dns

If there are no endpoints for the service, inspect the service and make sure it uses the correct selectors and ports.

## Kube-proxy
If you run a kubectl describe ds kube-proxy -n kube-system you can see that the kube-proxy binary runs with following command inside the kube-proxy container.
```
 Command:
      /usr/local/bin/kube-proxy
      --config=/var/lib/kube-proxy/config.conf
      --hostname-override=$(NODE_NAME)
```

In kube-proxy config file, we can define these info:
- clusterCIDR
- kubeproxy mode
- ipvs
- iptables
- bindaddress
- kube-config
- more...

### Troubleshoot
1. Check kube-proxy pod in the kube-system namespace is running.

2. Check kube-proxy logs.

3. Check configmap is correctly defined and the config file for running kube-proxy binary is correct.

4. kube-config is defined in the config map.

5. check kube-proxy is running inside the container

## Examples

```
Failed to create pod sandbox: rpc error: code = Unknown desc = [failed to set up sandbox container "f5581b75ba9eaa61b45e3a84df4d3293a6bbfe01f1fd42ccdcdb68e85d080f41" network for pod "mysql": networkPlugin cni failed to set up pod "mysql_triton" network: unable to allocate IP address: Post "http://127.0.0.1:6784/ip/f5581b75ba9eaa61b45e3a84df4d3293a6bbfe01f1fd42ccdcdb68e85d080f41": dial tcp 127.0.0.1:6784: connect: connection refused, failed to clean up sandbox container "f5581b75ba9eaa61b45e3a84df4d3293a6bbfe01f1fd42ccdcdb68e85d080f41" network for pod "mysql": networkPlugin cni failed to teardown pod "mysql_triton" network: Delete "http://127.0.0.1:6784/ip/f5581b75ba9eaa61b45e3a84df4d3293a6bbfe01f1fd42ccdcdb68e85d080f41": dial tcp 127.0.0.1:6784: connect: connection refused]
```
Do the services in triton namespace have a valid endpoint? If they do, check the kube-proxy and the weave logs.

Does the cluster have a Network Addon installed?
```
kubectl get pods -A
# To check if the network plugin is installed, identify if any network plugin pods are available
```

```
root@controlplane:/# kubectl logs -n kube-system kube-proxy-9nd8w 
F1012 00:49:04.976556       1 server.go:490] failed complete: open /var/lib/kube-proxy/configuration.conf: no such file or directory
```

kubctl describe -n kube-system configmaps kube-proxy
```
a.crt                     1      29m
root@controlplane:/# kubectl describe configmaps -n kube-system kube-proxy
Name:         kube-proxy
Namespace:    kube-system
Labels:       app=kube-proxy
Annotations:  kubeadm.kubernetes.io/component-config.hash: sha256:3c1c57a3d54be93d94daaa656d9428e3fbaf8d0cbb4d2221339c2d450391ee35

Data
====
config.conf:
----
apiVersion: kubeproxy.config.k8s.io/v1alpha1
bindAddress: 0.0.0.0
bindAddressHardFail: false
clientConnection:
  acceptContentTypes: ""
```
Edit the daemonset of kube-proxy
```
kubectl edit -n kube-system ds kube-proxy
```