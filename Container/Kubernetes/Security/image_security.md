# Image security
How to use images from private repositories?

Creating credentials for Kubernetes to login to the private docker registry to pull the image
```
kubectl create secret docker-registry <name: regcred> \
  --docker-server=private-registry.com
  --docker-username=xxxxxxxxx
  --dockerpassword=xxxxxxxxxx
  --docker-email=xxxxxx@xxxx.com
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: k8s-dashboard
spec:
  containers:
    - name: custom-app
      image: private-registry.com/apps/custom-app
  imagePullSecrets:
  - name: regcred
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: k8s-dashboard
spec:
  containers:
    - name: nginx
      image: private-registry.com:5000/nginx:alpine
  imagePullSecrets:
  - name: regcred
```