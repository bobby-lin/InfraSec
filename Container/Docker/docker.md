# Docker Basics
References
- https://docs.docker.com/

Image == The application to run
Container == An instance of the image running as a process
Container != Mini-VM
Registries == Repository of Images

Finding version of docker:
```
docker version
```
Finding local images:
```
docker images
```
Basic: Start different containers (nginx and apache) with detached
Explanation: Open port 80 on host and forward to port 80 in container
```
docker run --publish 80:80 -d --name nginx nginx 
docker run --publish 80:80 -d --name nginx nginx:alpine
docker run --publish 8080:80 -d --name httpd httpd
```
Passing environment variables when starting container
```
docker run --publish 3306:3306 -e MYSQL_RANDOM_ROOT_PASSWORD=yes -d --name mysql mysql 
```
List all the containers
```
docker ps -a
```
Giving a name to a container
```
docker run --publish 80:80 -d --name nginx_server nginx
```
Viewing logs of a container
```
docker logs <container name>
docker logs <container id>
```
Find the process running in a container
```
docker top <container name>
docker top <container id>
```
Stop and Remove a container
```
docker stop <container name or id>
docker rm <container name or id>
```
View performance of specific container
```
docker stats <container name or id>
```
View the configuration of container
```
docker inspect <container name or id>
```
Run a container interactively in bash
i: interactive
t: tty -> terminal
```
docker run -it <container> bash
```
Attaching to container bash
```
docker start -ai nginx
```
Execute command on an existing running container
```
docker exec -it <container> sh' (or bash)
docker exec -it <container name or id> <command>
docker exec -it nginx_server ls -al
```
Check which port is opened for a running container
```
docker port <container name or id>
```
### OS
Running Ubuntu / Centos (no docker entry point)
```
docker run -d -t -i ubuntu:14.04 /bin/bash
docker run -d -t -i centos /bin/bash
```
Using Nginx with ping
```
docker run nginx:alpine
```
Remove a container when exit (using --rm)
```
docker run --rm ubuntu
```