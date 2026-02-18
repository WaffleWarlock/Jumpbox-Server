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