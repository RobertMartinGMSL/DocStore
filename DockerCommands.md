# Baisc Commands

## Images

* View images
```
docker images
```
* Pull image
```
docker pull <image name>
```
* Remove Image
```
docker rmi image_name_or_id
```

# Run Image in Container
* Run image with defaults
```
docker run image_name_or_id
```
* Example with additional options (see below)
```
docker run -d --name "myContainerName" --rm -p 1010:8080 -it my_image_or_id
```
## Additional Options
* Specify detached mode
  * Exits when container processes finishes
```
-d
```
* Specify remove when processing finished
  * Often used in combination with detached mode
```
--rm
```
* Specify container name
```
--name "container_name"
```
* Specify port
```
-p local_port:container_port
```
* Specify image to use
```
-t image_name_or_id
```

## Containers
* View running containers
```
docker container ls
```
* View all containers
```
docker container ls -a
```
* Start container
```
docker start container_name_or_id
```
* Stop container
```
docker stop container_name_or_id
```
* Remove container
```
docker rm container_name_or_id
```
* Inspect container (view properties)
```
docker inspect container_name_or_id
```
