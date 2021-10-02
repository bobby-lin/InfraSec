# Docker Storage
Understanding Docker storage will make it easier to understand the storage in Kubernetes

Two types of storage:
- Storage Drivers
- Volume Drivers

In container layer, the temp files are stored but will be destroyed if the container is removed.

# Storage Drivers
Based on your OS:
- AUFS
- ZFS
- BTRFS
- Device Mapper
- Overlay
- Overlay2

# Volume drivers
Examples:
- Local
- Azure File Storage
- gce-docker
- rexray

## Volume mounting
To persist the data, we need to create volume and mount it:
```
docker volume create data_volume
docker run -v data_volume:/var/lib/mysql mysql
```

## Bind mounting
If we want to mount an existing directory to the container
```
docker run -v /data/mysql:/var/lib/mysql mysql
```
New style
```
docker run \
--mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql
```
