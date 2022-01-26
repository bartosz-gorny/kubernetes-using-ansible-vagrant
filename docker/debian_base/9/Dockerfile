FROM debian:9


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

# RUN echo 'root:haslo123' | chpasswd

RUN service ssh start
RUN sed -i '/^PermitRootLogin/s/without-password/yes/' /etc/ssh/sshd_config

EXPOSE 22/tcp
EXPOSE 22/udp



ENTRYPOINT service ssh restart && sleep infinity