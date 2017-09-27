#FROM ubuntu
#RUN apt-get update \
#  && apt-get install -y openssh-server \
#  && rm -rf /var/lib/apt/lists/*

FROM kinogmt/centos-ssh

# empty root password
RUN passwd -d root

# allow root to ssh without password
COPY sshd_config /etc/ssh/sshd_config
