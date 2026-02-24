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


## Using Vagrant for development (This will emulate an ansible playbook being ran on an EC2 or VM)

- You must have `docker` and `vagrant` installed.
   1. Docker Engine
      ```
      sudo apt install -y docker.io
      ```
   2. Vagrant 
      ```
      wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
      echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
      sudo apt update && sudo apt install vagrant
      ```
1. Install a python virtual environment with the below commands

   NOTE: This step can be ommited if your `./ansible/install/.venv` directory already exists
   ```
   $ cd ./ansible/install
   
   $ python3 -m venv .venv
   ```

2. Source your python virtual environment with these commands (make sure that you are in the `./ansible/install` directory)
   ```
   $ source .venv/bin/activate
   ```

3. Install updated python modules for ansible (make sure that you are in the `./ansible/install` drectory)

   NOTE: In order for this pinned version of ansible to install your system python or python virtual environment must be >= python 3.11.x
   ```
   $ pip install -r requirements.txt
   ```

   NOTE: While installing `ansible` can be done via apt manager, the debian based repo's will often have outdated versions of what you are trying to install

4. Install ansible roles and pinned collections for the Jumpbox server
   ```
   $ cd ./ansible

   $ ansible-galaxy role install -r install/requirements.yaml 

   $ ansible-galaxy collection install -r install/requirements.yaml
   ```

5. Start up your vagrant container
   ```
   $ cd ./vagrant

   $ vagrant up --provider=docker
   ```

   Verify ssh works
   ```
   $ ssh -p 2222 vagrant@127.0.0.1
   ```
   password: vagrant

6. Run your ansible playbook against your "dev VM" vagrant container
   ```
   $ cd ./ansible

   $ ansible-playbook -i inventory/vagrant.ini playbooks/jumpbox.yaml

7. If your ansible playbook runs without error, you will have your Gitlab Runner server deployed as configured.

8. OPTIONAL: Cleanup vagrant container and prune old docker containers
   ```
   $ cd ./vagrant

   $ vagrant destroy -f

   $ docker system prune -a (enter yes)

   $ docker volume prune -a (enter yes)