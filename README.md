# simvascular-with-docker
This repository contains a Dockerfile for building a Docker image of SimVascular, an open-source software suite for cardiovascular modeling and simulation. The Docker image allows users to easily run SimVascular in a containerized environment, ensuring consistency and ease of deployment across different systems.

The docker image allows running the GUI and the command line interface of SimVascular. It also includes the necessary dependencies and configurations to ensure that SimVascular runs smoothly within the Docker container.

## Building the Docker Image
To build the Docker image, navigate to the directory containing the Dockerfile and run the following command:
```bash
docker build . -f docker/Dockerfile -t simvascular
```
This command will build the Docker image and tag it as "simvascular". The build process may take some time as it installs all the necessary dependencies and sets up the environment for SimVascular.

Important: the Dockerfile requires a file named `SimVascular-Ubuntu-24-2025.12.21.deb` to be present in the same directory. This file is the Debian package for SimVascular and is necessary for the installation process within the Docker image.

## Running the Docker Container
Once the Docker image is built, you can run a container using the following commands:
```bash
xhost +local:docker # Allow Docker containers to access the X server
```
and then
```bash
docker run --rm -it --name test --device /dev/dri -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $(pwd):/home/simvascular simvascular