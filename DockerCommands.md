# Baisc Commands

## Images

* View images
```
docker images
```
* Pull image
```
docker pull image_name
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
docker run -d --name "myContainerName" -p 80:80 -v c:/temp/db.json:/data/db.json -t image_name_or_id
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
-p {outside_port}:{inside_port}
```
* Specify image to use
```
-t image_name_or_id
```

## Containers
* View running containers
```
docker ps
```
* View all containers
```
docker ps -a
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
* Remove all containers (prune)
```
docker container prune --force
```
* Inspect container (view properties)
```
docker inspect container_name_or_id
```
* Start container (run), attach volume (-v) and attach terminal (-w)
```
docker run --rm -it -v local_folder:container_folder -w container_folder container_name bash
```
