# Image VS Container
Image: All of our source code, env configurations, base image, packages, etc that are needed to run a container. It's like a tea bag, a spoon of sugar and hot water
Container: An instance of running image. when uyou run an image using docker run, a container is made. It's like when you start a process of making a cup of tea. A container is a cup. You can set the water temperature, decide how do you want to stir it, how much sugar you will add, etc.

# Docker Image Management
## build new custom docker image with Dockerfile
docker build --tag <imageName:version> <Dockerfile location>
ex: docker build -t rest-server:latest .
> Use the same command to rebuild the image after source code change, .

## remove Docker images
docker rmi <imageIDorName>
remove docker images

docker rmi $(docker images -f "dangling=true")
removes old unused images after rebuilding.

# Container Management
## run an image and expose/publish container's port to host's port
docker run -p <host port>:<container port> <imagename>
> ex: docker run -p 3000:8000 node-server
> buat supaya port 8000 di container terhubung ke port 3000 di host.
> Jadi, untuk bukanya, kita akses localhost:3000 pada host kita, bukan localhost:8000. Biar gampang, samain aja host port sama ccontainer port nya.

docker run -dit ...
detach (run in background), interactive, tty (so you can see input output).

## show running containers
docker ps

## show all containers that has been started or stopped
docker ps -a

## restart containers to run with previous flags/options
docker restart <containerIDName>
> the container will be run using the same options as before, e.g. with -p 8000:8000 etc.

## remove started/stopped containers
docker rm <imageIDName>
> it will only remove the containers, not the actual image. So you can still run the image and make a container from it.

# Docker Volumes
For storing persistent data and configs needed for development in our local host machine. You can bind mount on linux, but volumes is more performant and easier to migrate/send to cloud, and it's OS independent.
Volumes is used as if a symlink to the container's app folder. So, whatever you store in the volume inside host is going to be mirrored into the container.
To attach volume to app folder inside container, do this:
docker run -v myApp:/app
You can also attach multiple volumes to a container:
docker run -v userdata:/app -v mongodb:/data/db etc... 

to create volume:
docker volume create <volumeName>
docker volume inspect <volumename>

## Volume location on host machine
You can map volume directory on host machine using -v <hostDirLocation>/<containerDirLocation>
In linux, the host directory will be located on /var/lib/docker/volumes. On WSL2, \\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes\

## Removing volumes
docker volume prune
remove unused volumes
docker volume rm <volume...>

# Network
Is to use to connect between containers.
By default, when you start more than 1 containers without defining the network flag, they will be connected with default unnamed bridge network. But, it's better to make your own network for specific stacks for clarity. Plus, user created network support connecting using container's name rather than using its IP address. This ability is called 'automatic service discovery".

docker network create <networkName>
docker network ls
docker network inspect <networkName>

# Dockerfile
## CMD
Command to run the image
CMD ["/usr/bin/sleep", "10"]
Remember to type the full excutable path when using JSON format. This is the same with:
CMD sleep 10

## RUN
Used for running command to add layer to the image, e.g. npm install --production. The result of the RUN command will be committed as a new layer on the image.

## ENTRYPOINT
used to run command when starting the container. It can be used in conjunction with CMD. If there is ENTRYPOINT and CMD, the CMD will be appended to the ENTRYPOINT command, but only works with JSON form.
ENTRYPOINT ["/usr/bin/sleep"]
CMD ["10"]
When the container is run with docker run command,the container will sleep after 10 seconds. This is similar with running docker run imageName sleep 10
The CMD instruction above can be overwritten by adding args in docker run command.
the ENTRYPOINT can be overwritten by using --entrypoint flag in docker run command.

# detach and attach
To run container in detach mode, use -d flag in docker run command
to attach the detached container:
docker attach <container-name>
to detach from attached container (without stopping it), press Ctrl P Q.