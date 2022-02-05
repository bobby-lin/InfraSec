# RBAC
The dev-user is trying to get details about the dark-blue-app pod in the blue namespace. Investigate and fix the issue. We have created the required roles and rolebindings, but something seems to be wrong.

```
kubectl edit roles developer -n blue
kubectl auth can-i get pod/blue-app --as dev-user -n blue
```


apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: dep-role-binding
  namespace: blue
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: deployment-role
  apiGroup: rbac.authorization.k8s.io