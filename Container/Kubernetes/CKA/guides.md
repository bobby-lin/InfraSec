# Preparation Guide
- https://kubernetes.io/docs/reference/kubectl/conventions/
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands


## Tedious to create and edit YAML files manually
### Create objects without YAML

Pods
```
kubectl run nginx --image=nginx
```
Deployment
```
kubectl create deployment --image=nginx nginx
```

### Dry-run and output the YAML files
Create Pod yaml file
```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```
Expose a port in a Pod
```
kubectl run custom-nginx --image=nginx --port=8080 -o yaml > custom-nginx.yaml
```
Create Deployment yaml file
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```
Save Deployment yaml file
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```
Use the saved yaml file
```
kubectl create -f nginx-deployment.yaml
```
For k8s version 1.19+, use `--replicas` option to create deployment with 4 replicas
```
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```
Create a Service - Nodeport and expose the pod's port on nodeport of the nodes
A) This can use the pod label as selectors but cannot specify the node port
```
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```
B) If we use this command, then pods labels cannot be used as selectors
```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
Most of the time, using the pod label as selectors are useful. So try to use option A to create a service. If you want to change the node-port, then edit the service definition file to add the `nodePort`.kubec

# Tips
In the exam, there is no verification on whether you did something correctly or not.

#### Self-verification approach:
You need to verify things yourself. For example, if one task involves creating a pod with specific image, you need to run `kubectl describe pod xxxx` command to check if the name and image is correct.

# Networking
In the CKA exam, for a question that requires you to deploy a network addon, unless specifically directed, you may use any of the solutions described in the link above.

However, the documentation currently does not contain a direct reference to the exact command to be used to deploy a third party network addon.

The links above redirect to third party/ vendor sites or GitHub repositories which cannot be used in the exam. This has been intentionally done to keep the content in the Kubernetes documentation vendor-neutral.

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#steps-for-the-first-control-plane-node