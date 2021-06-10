# Docker SKO 2020 Demos & Resources

## Lessons
[Running a Container](#run)

[Running a MATLAB Container](#runmatlab)

[Running in Batch Mode](#runbatch)

[Accessing files](#files)

[Image snapshots](#snapshots)

[Customizing a MATLAB Container Image](#buildmatlab)

[Building from scratch](#buildscratch)

## Resources
- MATLAB-Deps container image 
https://hub.docker.com/r/mathworks/matlab-deps
- MathWorks Dockerfile for DIY on GitHub
https://github.com/mathworks-ref-arch/matlab-dockerfile
- MATLAB Deep Learning Image on Nvidia GPU Cloud
https://ngc.nvidia.com/catalog/containers/partners:matlab
https://insidelabs-git.mathworks.com/jlmartin/docker/tree/master/nvidia-gpu-cloud/matlab-r2019b
- This repo 
https://insidelabs-git.mathworks.com/jfluet/docker-sko2019

Note that you can put anything in a container.  It's not a matter of whether you can but whether you should. Match the solution to the use case.
e.g. you can create a container with MATLAB Runtime and a compiled application and deploy it to multiple servers but Production Server would be a better match for this use case.

# Working with containers

## Installing docker

### Windows and Mac
https://www.docker.com/products/docker-desktop

### Linux
```bash
sudo apt-get update
sudo apt install docker.io
sudo systemctl start docker
```

## Docker Components

There are two primary components to docker, the Docker Engine and Docker Client.   The Engine is a background process that manages the containers. The client is the command line tool used to build and run containers.  Once installed you don't generally need to worry about the engine.  You'll interact with docker using the client. 

<a name="run"></a>
## Running a container

In this tutorial, you'll learn how to use the docker client to pull and run containers from existing images, list images and containers, and delete images and containers.

We'll get started by using the basic MATLAB docker image published by MathWorks.

The run a container we'll first need to locate one.  The primary place to find container images is DockerHub, a centralized docker container image registry.  There you'll find thousands of containers for all sorts of uses.  Since we're interested in MATLAB we'll find that at https://hub.docker.com/r/mathworks/matlab.

### Pull a container
Grabbing the container is a simple as telling docker to retrieve it from the DockerHub image registry.  A registry is where images are stored and distributed.  Images are stored in individual repositories which hold all the versions of an image.

To pull the latest matlab image we use the pull command.

```bash
docker pull mathworks/matlab
```
Docker will contact the registry and download the requested container image.  We've just pulled the latest image but it is possible to pull a specific version by using a tag.  For instance, we could have run ```docker pull matlab:r2021a```

This will take some time depending on your bandwidth since the image is pretty big.  Once the download completes we can view the image.

```bash
docker images
```
This will list the images on this machine
![](images/docker-images.png)

Now that we have the image we'll start a container from it using the run command*.  

### Run a container
```bash
docker -it mathworks/matlab
```

This will launch MATLAB.  By default is is configured to use Online Licensing so if you have an individual license or your organization is setup to use Online  Licensing enter your MathWorks Account username and password.  

![](images/run-matlab.png)

Running the ver command will list the products installed and we can see that MATLAB is the only product included in the image. 

Type ```exit``` and press RETURN.  This will exit the container. 

Docker's process status command (ps) will show us the containers on our system.

```bash
docker ps -a
```
Passing the -a flag tells docker to show all containers on this machine regardless of status.  Here we can see the one container we ran with the status of Exited.

![](images/docker-ps.png)

The container still exists.  We could relaunch it or snapshot it at this point.  However, it's just taking up disk space so lets delete it.

```bash
docker rm 224650169f24
```
