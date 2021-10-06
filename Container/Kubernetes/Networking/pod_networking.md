# Pod Networking

Networking model:
- Every pod should have an IP address
- Every pod should be able to:
 - communicate with other pods in the same node
 - coomunicate with other Pod on other nodes without NAT

General steps:
When adding Pods:
Create veth pair
Attach veth pair
Assign IP address
Bring up Interface

When deleting Pods:
Delete veth pair