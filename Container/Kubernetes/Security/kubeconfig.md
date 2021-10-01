# Kubeconfig
Typing the configurations in the `kubectl` command will be tedious.

We can move these configurations into a config file called `kubeconfig`

By default, `$HOME/.kube/config` file is used when running the `kubectl` command

## Format of Kubeconfig file
https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/

Three sections:
- clusters
- users (containing the cert configs etc.)
- contexts

```yaml
apiVersion: v1
kind: Config
current-context: dev-frontend # for specifying the default context if there are more than one context

clusters:
- cluster:
    certificate-authority: fake-ca-file
    server: https://1.2.3.4
  name: development
- cluster:
    insecure-skip-tls-verify: true
    server: https://5.6.7.8
  name: scratch

contexts:
- context:
    cluster: development
    namespace: frontend
    user: developer
  name: dev-frontend
- context:
    cluster: development
    namespace: storage
    user: developer
  name: dev-storage
- context:
    cluster: scratch
    namespace: default
    user: experimenter
  name: exp-scratch
current-context: ""

users:
- name: developer
  user:
    client-certificate: fake-cert-file
    client-key: fake-key-file
- name: experimenter
  user:
    password: some-password
    username: exp
```

```
kubectl config view
```

Choose a context
```
kubectl config use-context <name of context>
kubectl config use-context prod-user@production
```