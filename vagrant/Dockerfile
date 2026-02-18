FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install systemd and essentials
RUN apt-get update && apt-get install -y \
    systemd \
    systemd-sysv \
    openssh-server \
    sudo \
    python3 \
    python3-apt \
    ca-certificates \
    python-is-python3 \
    curl \
    vim \
    jq \
    iproute2 \
    iputils-ping \
    net-tools \
    ufw \
    && rm -rf /var/lib/apt/lists/*

# Remove unnecessary systemd services that cause issues in containers
RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/sysinit.target.wants/systemd-tmpfiles-setup* \
    /lib/systemd/system/systemd-update-utmp*

# Create SSH directory and enable SSH service
RUN mkdir -p /var/run/sshd && \
    systemctl enable ssh

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

# Vagrant expects its default insecure key to be present for key-based SSH
RUN mkdir -p /home/vagrant/.ssh \
 && chmod 700 /home/vagrant/.ssh \
 && curl -fsSL https://raw.githubusercontent.com/hashicorp/vagrant/main/keys/vagrant.pub \
    -o /home/vagrant/.ssh/authorized_keys \
 && chmod 600 /home/vagrant/.ssh/authorized_keys \
 && chown -R vagrant:vagrant /home/vagrant/.ssh

CMD ["/usr/sbin/sshd", "-D", "-e"]
