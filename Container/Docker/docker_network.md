#Docker Networks
The default bridge network driver allow containers to communicate with each other when running on the same docker host.

List networks
```
docker network ls
```
Inspecting specific network
```
docker network inspect <network name or id>
docker network inspect bridge
```
Creating a custom virtual network
```
docker network create <name of network>
docker network create my_app_network
```
Running a container in a specific network
```
docker run --name nginx_server --network <name of network> -d nginx
docker run --name nginx_server --network my_app_network -d nginx
```
Attach or detach a container to virtual network
```
docker network connect <network> <container name>
docker network disconnect <network> <container name>
```
Creating network alias
```
docker network connect --alias <name> <name of network> <container name>
```

###Security Tips
- Manually expose the port via `-p`
- All external exposed port must be closed by default
- Create your frontend and backend to be in the same Docker network.

NIC == Network Interface Controller

###DNS
- Container should not rely on IP addresses for inter-communication
- Using custom networks, you can have built-in DNS (reference by Container name)

###Exercise
Create two containers in the same custom network. Run a ping or curl command from one container to test connection to the other container.
```
docker exec -it nginx curl nginx_server
```
Round-robin DNS using `--net-alias`
>"Round-robin DNS is a technique of load distribution, load balancing, or fault-tolerance provisioning multiple, redundant Internet Protocol service hosts, e.g., Web server, FTP servers, by managing the Domain Name System's (DNS) responses to address requests from client computers according to an appropriate statistical model." ~ Wikipedia
```
docker run -d --net vpn --net-alias search elasticsearch:2
docker run --rm --net vpn alpine:3.10 nslookup search
docker run --rm --net vpn centos curl -s search:9200
```