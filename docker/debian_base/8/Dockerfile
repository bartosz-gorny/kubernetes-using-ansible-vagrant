FROM debian:8


RUN apt update && apt install -y \
    bzr \
    cvs \
    git \
    vim \
    iputils-ping \
    iproute2 \
    wget \
    zip \
    unzip \
    curl \
    openssh-server

# zabbix_agent_server

RUN service ssh start
RUN sed -i '/^PermitRootLogin/s/without-password/yes/' /etc/ssh/sshd_config

EXPOSE 22/tcp
EXPOSE 22/udp



ENTRYPOINT service ssh restart && sleep infinity
# CMD sleep 10000