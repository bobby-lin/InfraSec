## Container Image
>When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container’s filesystem, it must contain everything needed to run an application - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata. ~ https://docs.docker.com/get-started/

### Image Layer | Union File System Concept
Each image consists of layers. The layers are build on top of one another. In case there is change in the layer, Docker will rebuild this layer and all the layers build from this layer. Hence changes in the top layer will take the least computational work to rebuild the image. In practice, you should organize the layers that require regular changes to be higher up in the stack as possible.

### Image Tags
We can use image tag to label an image. In cases where there are two tags referring to the same image.
```
docker image ls
```
You will see two references pointing to the same image id.
You can retag an image to a new repository name and tag.
```
docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```
After you retag, you can login to Dockerhub and push the image.
NAME-> `dockerhub_name/image_name:TAG`
```
docker login
docker image push [OPTIONS] NAME[:TAG]
```
If you want to remove a tag
```
docker rmi REPOSITORY:TAG
```
### Build Images
```
docker image build [OPTIONS] PATH | URL | -
```
Reference to a specific docker file
```
docker image build -f URL
```

### Useful Commands
Clean up unused images
```
docker image prune
```
Clean up unused things
```
docker system prune
```
Clean up all images that you are not using
```
docker image prune -a
```
See Space usage
```
docker system df
```