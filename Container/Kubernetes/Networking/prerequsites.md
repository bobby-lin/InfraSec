# Prerequisites

## Linux Networking Basics

### Switching and Routing
- Switch: connects two devices for communication
- Router: connect two networks so that two devices in different networks can communicate
- Routing tables: use for adding ip routes in the machine to connect to another network
- Gateway: Like a door to external network

#### Useful commands
List and modify interfaces on host
```
ip link
```
See the assigned ip addresses to those interfaces (eg. `eth0`)
```
ip addr
```
Set ip addresses on the interfaces
```
ip addr add 192.168.1.10/24 dev eth0
```
To persist these changes in interfaces, make the changes in this file:
```
/etc/network/interfaces
```
View routing table
```
ip route
```
Add entries to routing table
```
ip route add 192.168.1.0/24 via 192.168.2.1
```
Configure IP forwarding
```
cat /proc/sys/net/ipv4/ip_forward # Change to 1
```

### DNS
A scalable way of managing domain name resolution instead of storing hostname-ip in `/etc/hosts` local file.

A famous name server is `8.8.8.8` hosted by Google that knows all the sites on Internet. Or `1.1.1.1` for Cloudflare DNS server.

#### Domain Names format
Example: `www.google.com`
- Root: `.`
- Top Level Domain Name: `.com`
- Domain Name: `google`
- SubDomain Name: `www` or `maps` or `mail`

#### Record Types
https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/#:~:text=What%20is%20a%20DNS%20A,210.9.

A: web-server (Ipv4)
AAAA: webserver (Ipv6)
CNAME: food.webserver (eats.webserver) -> name-mapping

#### Commands
```
nslookup
```
```
dig
```

### Network Namespaces


### Docker Networking
Networking Options:
- None: The container cannot connect to any other network
- Host: The container is attached to the host (No isolation between host and container)
- Bridge: Internal private network

Docker:
- Create Network Namespace
- Create Bridge Network / Interface
- Create Virtual eth pairs(Virtual Pipes are created to connect Container with the Bridge Network)
- Attach vEth to Namespace
- Attach other vEth to Bridge
- Assign IP addresses
- Bring interfaces up
- Enable NAT - IP Masquerade

# Container Network Interface (CNI)
Provide plugins:
- Bridge
- VLAN
- IPVLAN
- MACVLAN
- WINDOWS
- DHCP
- host-local

Docker does not follow CNI.