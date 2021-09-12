# Editing Pods and Deployment

## Pods
Only these Pod fields are allowed to be edited:
- spec.containers[*].image
- spec.initContainers[*].image
- spec.activeDeadlineSeconds
- spec.tolerations

Suppose you try to edit a pod
```
kubectl edit pod myapp
```
1) Create a temp def. yaml file, Delete the pod and Create with temp def yaml file.
If you edit some fields such as name or apiVersion, then you will see an error.
```
$ kubectl edit pod myapp
A copy of your changes has been stored to "C:\\Users\\[User]\\AppData\\Local\\Temp\\kubectl.exe-edit-pck3l.yaml"
error: At least one of apiVersion, kind and name was changed
```
Delete the pod
```
kubectl delete pod webapp
```
Recreate the pod
```
kubectl create -f C:\\Users\\[User]\\AppData\\Local\\Temp\\kubectl.exe-edit-pck3l.yaml
```

2) Generate and edit the yaml file, Delete the pod and Create the pod with the yaml file.
Generate a yaml file from the existing pod
```
kubectl get pod webapp -o yaml > my-new-pod.yaml
```
Edit the yaml file
```
nano my-new-pod.yaml
```
Delete the current pod
```
kubectl delete pod webapp
```
Create a new pod with the edited yaml file
```
kubectl create -f my-new-pod.yaml
```

## Deployment
Any fields in `deployment` can be edited.

For any changes, the deployment will terminate the existing pod and create the pod with the new changes.

```
kubectl edit deployment my-deployment
```