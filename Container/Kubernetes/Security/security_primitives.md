# Security Primitives

## kube-apiserver

### Authentication: 
Who can access?

Methods:
- Files - Username and Passwords
- Files - Username and Tokens
- Certificates
- External Authentication providers - LDAP
- Service Accounts (machine)

### Authorization:
What can they do?

RBAC Authorization
ABAC Authorization
Node Authorization
Webhook Mode

## Secure communications between components
TLS Certificates

## Network Policies
By default, pods within the same node can have unrestricted communications between each other.

Restrict communications between pods in the node.