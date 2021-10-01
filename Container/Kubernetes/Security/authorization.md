# Authorization
Mechanisms:
- Node Authorizer (control the access within the cluster)
External access:
- Attribue-based AC (ABAC) - difficult to manage
- Role-based AC (RBAC) 
- Webhook (e.g. Open Policy Agent) - manage AC externally to another tool

Authz Modes:
- Node
- ABAC
- RBAC
- Webhook
- AlwaysAllow
- AlwaysDeny

Checking role in kube-apiserver:
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep authorization
kubectl describe role kube-proxy --namespace=kube-system
```

Setting the mode in kube-apiserver config:

By default, the mode is configured as AlwaysAllow
```
ExecStart=/usr/local/bin/kube-apiserver \\
[...]
--authorization-mode=AlwaysAllow \\           
```

You can use different modes in terms of priority queue. This means if one mode is denying access, then it will check with the next mode in the queue
```
ExecStart=/usr/local/bin/kube-apiserver \\
[...]
--authorization-mode=Node,RBAC,Webhook \\
```

## RBAC
https://kubernetes.io/docs/reference/access-authn-authz/rbac/

Example of creating a Role
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

Restricting a role to specific resources
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "create", "update"]
  resourcesNames: ["blue", "orange"]
```

Create deployment role in "blue" namespace
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: blue
  name: dev-deployment-role
rules:
- apiGroups: ["apps", "extensions"] # "" indicates the core API group
  resources: ["deployments"] # must be plural
  verbs: ["create"]
```
Role binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: create-deployment
  namespace: blue
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: dev-deployment-role
  apiGroup: rbac.authorization.k8s.io
```

Role-binding (giving a role to an account)
```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "jane" to read pods in the "default" namespace.
# You need to already have a Role named "pod-reader" in that namespace.
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: jane # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

### Useful commands
View RBAC
```
kubectl get roles
kubectl get rolebindings -o wide
kubectl describe role developer
kubectl describe role-binding devuser-developer-binding
```
Check Access
```
kubectl auth can-i create deployments
kubectl auth can-i delete nodes
# As a specific user
kubectl auth can-i create deployments --as dev-user
kubectl auth can-i create pods --as dev-user
# In namespace
kubectl auth can-i create deployments --as dev-user --namespace test
```
Edit a role
```
kubectl edit roles developer --namespace blue
```

## ClusterRole
There are resources that are shared in cluster.
- Nodes
- Storage
- clusterroles
- clusterrolebindings
- namespaces
- certificatesigningrequests

Example
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]
```
ClusterRoleBinding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: manager # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: monitoring
aggregationRule:
  clusterRoleSelectors:
  - matchLabels:
      rbac.example.com/aggregate-to-monitoring: "true"
rules: [] # The control plane automatically fills in the rules
```

Count number of clusterroles
```
kubectl get clusterroles --no-headers | wc -l
```
Get the API groups and resource names
```
kubectl api-resources
```