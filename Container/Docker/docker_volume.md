### Docker Volume
We separate the data from the container as we want the data to be persist even if the container is changed. In Docker, there are two ways to do this:
1) Volume (create special location outside of container UFS)
2) Bind Mounts (link a container path to host path)

Volume can outlive the container. And you need to delete the volume if you don't use it anymore

Different OS command for printing working directory (pwd)

Windows cmd:
```
echo %cd%
```
Mac / Linux
```
pwd
```

#### Basic Volume commands / info
```
docker volume ls
```
Remove unused volume
```
docker volume prune
```
Inspect container volume. You will see the mountpoint.
```
docker volume inspect <container name>
```
Creating volume before running container. Sometimes you need to specify the driver and label in production environment
```
docker volume create
```
When making a new volume for a mysql container, where could you look to see where the data path should be located in the container?
```
Look at Dockerhub readme
```

#### Named volume
Test with mysql image as it will use volume in dockerfile.
```
docker run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql_vol:/var/lib/mysql mysql
```
List the volume and you can see the volume created by this container
```
docker volume ls
```
Without named volume, it will be difficult to see what is the volume that was created by the container.

#### Bind Mounting
Maps a host file or directory to a container file or directory
Cannot be used in Dockerfile and must be specify at container run.

Mac / Linux:
```
... -v /Users/acc/folder:/var/lib/mysql ...
```
Windows:
```
... -v //c/Users/acc/folder:/var/lib/mysql ...
```
Example: Running nginx and map a local host directory to container

Windows:
```
docker run -d -p 80:80 -v %cd%:/usr/share/nginx/html --name nginx nginx
```
Remove the container
```
docker rm -f xxxxxx
```
Start the Nginx container with shell
```
docker run -it -p 80:80 -v %cd%:/usr/share/nginx/html --name nginx nginx sh
```
Navigate to the mount location
```
cd /usr/share/nginx/html
```
Add something to this location
```
echo 'new file' > test.txt
```
You will see that the file is created in the host directory. For dev, this means that you can map the container code directory to the host directory. And you can change your code and see an immediate change to the application.

##### Assignment: patch update of database container
Create a postgres container with named volume with version X. Then rm this container and create another higher version postgres container with version Y with the same named volume.

##### Assignment: change code in host file and see how the changes are reflected
Start a static site generator like Hugo or Jekyll. Use bind mounts to map the host directories to the container
```
https://github.com/klakegg/docker-hugo.git
```

```
docker run -p 80:4000 -v %cd%:/site bretfisher/jekyll
```