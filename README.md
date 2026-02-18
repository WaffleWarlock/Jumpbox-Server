# Jumpbox-Server
Jumpbox server using ansible configurations

```
Jumpbox-Server/
├─ ansible/
│  ├─ ansible.cfg
│  ├─ install/
│  │  ├─ requirements.txt
│  │  └─ requirements.yaml
│  ├─ inventory/
│  │  ├─ hosts.ini              # AWS later
│  │  └─ vagrant.ini            # Docker now (127.0.0.1:2222)
│  ├─ group_vars/
│  │  ├─ all.yml
│  │  └─ jumpbox.yml
│  ├─ playbooks/
│  │  └─ jumpbox.yml
│  └─ roles/
│     └─ jumpbox/
│        ├─ tasks/
│        │  └─ main.yml
│        ├─ handlers/
│        │  └─ main.yml
│        ├─ templates/
│        │  └─ sshd_config.j2
│        └─ files/
│           └─ issue.net
└─ vagrant/
   ├─ Dockerfile
   └─ Vagrantfile
```

# Vagrant install
```
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```