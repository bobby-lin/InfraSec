# Service account
The Service account is used by machines
Example: Prometheus, Jenkins

## Service account example
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: k8s-dashboard
spec:
  containers:
    - name: k8s-dashboard
      image: k8s-dashboard
  serviceAccountName: dashboard-sa
  # Avoid automount of service acc -> automountServiceAccountToken: false
```


## Useful commands
Create service account
```
kubectl create serviceaccount dashboard-sa
```
View service accounts
```
kubectl get serviceaccount
```
Get info about a service account
```
kubectl describe serviceaccount dashboard-sa
```
Get the auth token of a service account
```
# Get the name of token
kubectl describe serviceaccount dashboard-sa | grep Tokens
# Look for the token
kubectl describe secret dashboard-sa-token-kbbm # Use the token as a Bearer token.
```
