# SimVascular with Docker

This repository provides a Docker-based environment for running [SimVascular](https://simvascular.github.io/), an open-source software suite for cardiovascular modeling and simulation.

The Docker image is intended to simplify setup and improve reproducibility by packaging SimVascular together with the required dependencies and runtime configuration. It supports both:

- the **graphical user interface (GUI)**
- the **command-line interface (CLI)**

This makes it suitable for interactive use as well as command-line workflows.

## Requirements

Before building the image, ensure that the following file is available in the build context:

- `SimVascular-Ubuntu-24-2025.12.21.deb`

This Debian package is required during the image build process.

## Building the Docker Image

From the repository folder, run:

```bash
docker build . -f docker/Dockerfile -t simvascular
```

This command builds the Docker image and tags it as `simvascular`.

The build process may take several minutes, depending on your system and network speed.

## Running the Docker Container

To start the container with GUI support, run:

```bash
xhost +local:docker
docker run --rm -it \
  --name test \
  --device /dev/dri \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -e DISPLAY=$DISPLAY \
  -v "$(pwd)":/home/simvascular \
  simvascular
```

### Notes

* `xhost +local:docker` allows local Docker containers to access your X server for GUI rendering.
* `--device /dev/dri` enables GPU-related device access when available.
* `-v /tmp/.X11-unix:/tmp/.X11-unix` shares the X11 socket with the container.
* `-e DISPLAY=$DISPLAY` passes the current display to the container.
* `-v "$(pwd)":/home/simvascular` mounts the current working directory into the container.

## Testing

This setup has been tested with the following environment:

* **Host OS:** Fedora 42
* **Docker version:** 29.2.1
* **SimVascular version:** 2025-12-21
