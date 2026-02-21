# Download base image ubuntu 18.04
FROM ubuntu:24.04 AS base
ARG DEBIAN_FRONTEND=noninteractive
LABEL org.opencontainers.image.authors="hernan.mella@pucv.cl"

# # Set environment variable to allow matplotlib to run in a docker container
# ENV MPLCONFIGDIR="./tmp/matplotlib"

# Create new user
ARG UID=1000
ARG GID=1000
RUN groupadd --gid 1001 -r simvascular \
  && useradd --uid 1001 -g simvascular --no-log-init --shell /bin/bash simvascular
# RUN groupadd --gid ${GID} -r simvascular \
#   && useradd --uid ${UID} -g simvascular --no-log-init --shell /bin/bash simvascular
RUN mkdir -p /home/simvascular
WORKDIR /home/simvascular

# Change to root user for installation
USER root

# Update Ubuntu Software repository
# RUN apt -qq update && \
#     apt -y --with-new-pkgs \
#         -o Dpkg::Options::="--force-confold" upgrade
RUN apt-get update && apt-get install -y apt-transport-https && apt-get install -y sudo && apt-get -y upgrade

# Install necessary dependencies and the specific GUI application (replace 'x11-apps' with your app)
# 'x11-apps' provides simple apps like 'xeyes' for testing
RUN apt-get update && \
    apt-get install -y x11-apps && \
    rm -rf /var/lib/apt/lists/*

# Set the display environment variable to point to the host's X server
ENV DISPLAY=:0

# Set an environment variable to avoid GUI dialog prompts during unattended installation (optional)
ENV DEBIAN_FRONTEND=noninteractive

# Install simvascular externals dependencies
RUN apt-get update
RUN apt-get install -y ubuntu-desktop-minimal
RUN apt-get install -y libharfbuzz-dev libxcb-cursor0

# Copy the .deb file from the parent directory into the image
COPY ../SimVascular-Ubuntu-24-2025.12.21.deb /home/simvascular/

# Install Simvascular
RUN dpkg -i /home/simvascular/SimVascular-Ubuntu-24-2025.12.21.deb
RUN rm /home/simvascular/SimVascular-Ubuntu-24-2025.12.21.deb
# RUN /usr/local/sv/simvascular/./setup-symlinks.sh

#
ENV QTWEBENGINE_DISABLE_SANDBOX=1
ENV QTWEBENGINE_CHROMIUM_FLAGS="--no-sandbox"

# ENV LIBGL_ALWAYS_SOFTWARE=1
# ENV MESA_LOADER_DRIVER_OVERRIDE=llvmpipe

# Fix openmpi error messages
ENV OMPI_MCA_btl_vader_single_copy_mechanism=none

# Change default user
# USER simvascular
WORKDIR /home/simvascular

# Run install script to set up simvascular
RUN /usr/local/sv/simvascular/2025-12-21/./setup-symlinks.sh 

# # To allow matplotlib to run in a docker container
# RUN export

# In the host:
#   xhost +local:docker
#   docker run --rm -it --name test --device /dev/dri -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v $(pwd):/home/simvascular simvascular:latest

# Inside the container:
#   ../../usr/local/sv/simvascular/2025-12-21/./simvascular