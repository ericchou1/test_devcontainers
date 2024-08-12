# FROM mcr.microsoft.com/devcontainers/python:3.11-bookworm
# FROM mcr.microsoft.com/devcontainers/base:bullseye 
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# containelab version will be set in devcontainer.json
ARG _CLAB_VERSION

# Set permissions for mounts in devcontainer.json
RUN mkdir -p /home/vscode/.vscode-server/bin
RUN chown -R vscode:vscode /home/vscode/.vscode-server

# install some basic tools inside the container
# adjust this list based on your demands
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    sshpass \
    vim \
    curl \
    iputils-ping \
    htop \
    yamllint \
    && rm -rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

# install preferred version of the containerlab
# RUN bash -c "$(curl -sL https://get.containerlab.dev)" -- -v ${_CLAB_VERSION}
RUN bash -c "$(curl -sL https://get.containerlab.dev)"
