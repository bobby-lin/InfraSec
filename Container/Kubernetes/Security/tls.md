# TLS Certificate

## What are TLS Certs?
Used for encrypting communication between two parties to prevent eavesdropping AND ensure the integrity of the certificate
CA: Root certificate
Client: Client certs (to verify client)
Server: Server certs (public and private keys)

### Symmetric Encryption
Encrypt the data to be sent to the server. Note that the same key must be sent to server as well.

### Asymmetric Encryption
- Uses public and private keys
- Encrypting the symmetric key with public key provided by the server and only the server can decrypt the symmetric key with their private key.
- The symmetric key is then used for communication going forward

### Certificate Authority (CA)
- If a cert is self-signed, it is insecure as the attacker can sign the cert themselves.
- Acts as a verification of identity of the certificate owner.
- CA can verify that you are the actual owner of the domain.
- Example: Symantec, GlobalSign, digicert

### Files
Cert (Public key) - *.crt, *.pem
Cert (Private key) - *.key, *-key.pem

How does Kubernetes use Certs?
How to generate them?
How to configure them?
How to view them?
How to troubleshoot issues related to Certs?