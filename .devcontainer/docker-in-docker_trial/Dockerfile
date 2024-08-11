# Dockerfile from https://github.com/nautobot/nautobot-lab/blob/main/Dockerfile
# Example from https://containerlab.dev/install/#__tabbed_3_2 
# FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04
# FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04 
FROM mcr.microsoft.com/devcontainers/python:3.11-bookworm

ARG CLAB_VERSION

# RUN bash -c "$(curl -sL https://get.containerlab.dev)" 
RUN echo "deb [trusted=yes] https://netdevops.fury.site/apt/ /" | \
    tee -a /etc/apt/sources.list.d/netdevops.list

# install containerlab and tools
RUN apt update && apt install -y --no-install-recommends containerlab${CLAB_VERSION:+=$CLAB_VERSION} \
    direnv \
    btop \
    gh \
    iputils-ping \
    tcpdump \
    iproute2 \
    qemu-kvm \
    dnsutils \
    telnet

