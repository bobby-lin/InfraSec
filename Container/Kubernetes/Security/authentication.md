# Authentication
Two types of account:
- User account (Admin and Developer)
- Service account (for bots)

## Authentication Mechanism

### Less secure way
#### Static Password File
Create a user-password csv file
```csv
password1, user1, u0001
password2, user2, u0002
```
Pass the basic auth file param in kube-apiserver config file
```
# In kube-apiserver.service
ExecStart=/usr/local/bin/kube-apiserver \\
[...]
--basic-auth-file=user-details.csv
```
If you use kubeadm tool, then modify the kube-apiserver.yaml file
```
apiVersion: v1
[...]
spec:
  containers:
  - commands:
    - kube-apiserver
    [...]
    - --basic-auth-file=user-details.csv
```
To access the api:
```
curl -v -k https://masternode-ip:6443/api/v1/pods -u "user1:password123"
```

#### Static Token File
Create a user-token csv file
```csv
xxxxxxxxxxxxxxxx, user1, u0001
xxxxxxxxxxxxxxxx, user2, u0002
```
Pass the token auth file param in kube-apiserver config file
```
--token-auth-file=user-token.csv
```
To access the api:
```
curl -v -k https://masternode-ip:6443/api/v1/pods --header "Authorization: Bearer xxxxxxxxxxxxxxxx"
```
### More secure way
#### Certificates

#### Identity Services
LDAP
Kerberos