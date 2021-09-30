# Certificates
The CA is usually stored at the master node. And it is required for signing the certificates of new admin.

Controller manager is carrying out the task of signing certificates.

## Certificates API
How to sign a certificate of a new admin using the Cert API?
https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/

Generate a new key-pair
```
# private key
openssl genrsa -out newguy.key 2048 
# public key
openssl req -new -key newguy.key -subj "/CN=newguy" -out newguy.csr
cat newguy.csr | base64
```
Create this request with kubectl
```yaml
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: newguy
spec:
  groups:
  - system:authenticated
  usages:
  - digital signature
  - key encipherment
  - server auth
  request: # use the base64 encoded value of xxx.csr
    xxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxx
    xxxxxxxxxxxxxxxxxxxxxxx
```
View the request
```
kubectl get csr
```
Approve the request
```
kubectl certificate approve newguy
```
Kubernetes sign the certificate with the CA key pairs and generate a new cert for the user.
```
kubectl get csr newguy -o yaml
echo "xxxxxxxxxxxxxxxx" | base64 --decode
```

## Other useful commands
Reject a cert request
```
kubectl certificate deny agent-smith
```
Delete a csr
```
kubectl delete csr agent-smith
```