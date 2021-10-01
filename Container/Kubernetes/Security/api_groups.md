# API Group
/metrics
/healthz
/version
/api
/apis
/logs

Core group
`/api`


Named group
`/apis` consists of API groups (/apps, /networking.k8s.io, /authentication.k8s.io etc)
Each groups have api version (e.g. /v1) and Resources with verbs

```
/apps/v1/deployments/list
```

Using `kubectl proxy` for accessing kube-apiserver
```
kubectl proxy
curl http://localhost:6443 0k
```