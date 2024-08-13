# Dockerfile from https://github.com/nautobot/nautobot-lab/blob/main/Dockerfile
# Example from https://containerlab.dev/install/#__tabbed_3_2 
# FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04
FROM mcr.microsoft.com/devcontainers/base:1.0.9-ubuntu22.04 

ARG NAUTOBOT_VERSION="2.1.4"

ARG DB_NAME="nautobot"

ARG DB_USER="nautobot"

ARG DB_PASSWORD="E1x3oasg"

ARG DB_HOST="localhost"

ARG DB_PORT="5432"

ARG NAPALM_USERNAME

ARG NAPALM_PASSWORD

ARG NAUTOBOT_USERNAME="demo"

ARG NAUTOBOT_EMAIL="opensource@networktocode.com"

ARG NAUTOBOT_PASSWORD="nautobot"

ARG NAUTOBOT_TOKEN="aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"

# containelab version will be set in devcontainer.json
# ARG _CLAB_VERSION

ENV NAUTOBOT_VERSION=${NAUTOBOT_VERSION}

ENV NAUTOBOT_ROOT="/opt/nautobot"

ENV PATH="$NAUTOBOT_ROOT/bin:$PATH"

ENV DB_NAME=${DB_NAME}

ENV DB_USER=${DB_USER}

ENV DB_PASSWORD=${DB_PASSWORD}

ENV DB_HOST=${DB_HOST}

ENV DB_PORT=${DB_PORT}

ENV NAPALM_USERNAME=${NAPALM_USERNAME}

ENV NAPALM_PASSWORD=${NAPALM_PASSWORD}

ENV NAUTOBOT_USERNAME=${NAUTOBOT_USERNAME}

ENV NAUTOBOT_EMAIL=${NAUTOBOT_EMAIL}

ENV NAUTOBOT_PASSWORD=${NAUTOBOT_PASSWORD}

ENV NAUTOBOT_TOKEN=${NAUTOBOT_TOKEN}

ENV DEBIAN_FRONTEND=noninteractive

ENV DEBCONF_NONINTERACTIVE_SEEN=true

WORKDIR /opt/nautobot

COPY pb_nautobot_install.yml .

COPY templates templates

# required for postgresql-14
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
 
# hadolint ignore=DL3008,DL3013
RUN apt-get update -y && \
    apt-get install -y vim tzdata --no-install-recommends && \
    ln -fs /usr/share/zoneinfo/UTC /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    apt-get install -y python3 python3-psycopg2 python3-pip \
      python3-venv python3-dev python3-apt postgresql-14 \
      libpq-dev redis-server systemctl git --no-install-recommends && \
    pip3 install --no-cache-dir pip setuptools wheel --upgrade && \
    pip3 install --no-cache-dir --requirement ./templates/requirements.txt && \
    ansible-galaxy collection install community.postgresql && \
    ansible-playbook pb_nautobot_install.yml && \
    pip3 uninstall -y ansible && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set permissions for mounts in devcontainer.json (containerlab)
# RUN mkdir -p /home/vscode/.vscode-server/bin
# RUN chown -R vscode:vscode /home/vscode/.vscode-server
# install some basic tools inside the container
# adjust this list based on your demands
# RUN apt-get update && \
#     apt-get upgrade -y && \
#     apt-get install -y --no-install-recommends \
#     sshpass \
#     curl \
#     iputils-ping \
#     htop \
#     yamllint \
#     && rm -rf /var/lib/apt/lists/* \
#     && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
#     && apt-get clean
# install preferred version of the containerlab
# RUN bash -c "$(curl -sL https://get.containerlab.dev)" -- -v ${_CLAB_VERSION}


EXPOSE 8000/tcp

VOLUME /var/lib/redis

VOLUME /var/lib/postgresql/12/main

HEALTHCHECK CMD supervisorctl status || exit 1

CMD ["sh", "-c", "${NAUTOBOT_ROOT}/bin/supervisord", "-c", "/etc/supervisord.conf"]
