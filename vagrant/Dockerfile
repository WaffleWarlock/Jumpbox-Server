FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    openssh-server sudo \
    python3 python3-apt ca-certificates \
    curl vim jq iproute2 iputils-ping net-tools \
    ufw fail2ban \
 && rm -rf /var/lib/apt/lists/*

# sshd runtime dir
RUN mkdir -p /var/run/sshd

# Vagrant expects a user it can SSH as
RUN useradd -m -s /bin/bash vagrant \
 && echo "vagrant:vagrant" | chpasswd \
 && usermod -aG sudo vagrant \
 && echo "vagrant ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/vagrant \
 && chmod 440 /etc/sudoers.d/vagrant

# Enable ssh password auth so vagrant can connect initially (we'll harden via Ansible)
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config \
 && sed -i 's/#KbdInteractiveAuthentication yes/KbdInteractiveAuthentication no/' /etc/ssh/sshd_config \
 && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D", "-e"]
