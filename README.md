# Install AWX

Source: https://github.com/ansible/awx/blob/devel/INSTALL.md#getting-started

- Centos OS 7.4
- 2 CPU / 4 GB RAM / at least 60GB storage

```bash
# Install Git
sudo yum -y install git

# Clone AWX from repo, use a tag
git clone -b 16.0.0 https://github.com/ansible/awx.git

# Install AWX branding (optional)
git clone https://github.com/ansible/awx-logos.git

# Ansible >= 2.8
sudo yum -y install ansible

# Docker 
## https://docs.docker.com/engine/install/centos/
##  SET UP THE REPOSITORY
## Install the yum-utils package (which provides the yum-config-manager utility) and set up the stable repository.
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

## INSTALL DOCKER ENGINE
sudo yum -y install docker-ce docker-ce-cli containerd.io

## Start Docker.
sudo systemctl enable docker
sudo systemctl start docker
sudo docker run hello-world

## Create group docker
sudo groupadd docker
## Add user vagrant to docker
sudo usermod -a -G docker vagrant

# Install Python (3.x) and pip
sudo yum install -y python3 pip3

# docker Python module
sudo pip3 install docker

# communiti.general.docker_image collection
# Required for Ansible >= 2.10
ansible-galaxy collection install community.general

# GNU Make
sudo yum -y install gcc

# Node >= 14.xs LTS version
# and NPM >= 6.x LTS
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
sudo yum install -y nodejs

# AWS Over Docker (docekr-compose), Openshift or Kubernetes

## With Docker (docker-compose)

### Install docker-compose Python module
sudo pip3 install docker-compose

# Install libselinux-python
sudo yum install -y libselinux-python3


### Install docker-compose
### Run this command to download the current stable release of Docker Compose:
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

#### Apply executable permissions to the binary:
sudo chmod +x /usr/local/bin/docker-compose
### Test the installation.
sudo docker-compose --version

#reboot

cd /home/vagrant/awx/installer
sed "s|create_preload_data=True|create_preload_data=False|g"
ansible-playbook -i inventory install.yml
# Post install 
sudo pip3 install awxkit

# Get the docker logs of awx_task
# docker logs -f -n 20 awx_task

# Para acceder a la web es con 
user: admin
password: password
url: http://localhost:8080/#/workflow_approvals

---
---

```bash
# https://docs.ansible.com/ansible-tower/latest/html/quickinstall/download_tower.html
wget https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-3.6.2-1.tar.gz
tar xzvf ansible-tower-setup-3.6.2-1.tar.gz
cd ansible-tower-setup-3.6.2-1
sed "s|admin_password=''|admin_password='adminpassword'|g" -i inventory
sed "s|pg_password=''|pg_password='adminpassword'|g" -i inventory
sed "s|rabbitmq_password=''|rabbitmq_password='adminpassword'|g" -i inventory
sudo ./setup.sh

```

## Install GUI
https://www.itzgeek.com/how-tos/linux/centos-how-tos/install-gnome-gui-on-centos-7-rhel-7.html
```bash
sudo yum update -y

# Let's start by enabling the EPEL repository
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# Install all prerequsites
sudo yum install -y perl gcc dkms kernel-devel kernel-headers make bzip2
# Confirm that kernel-headers installed are matching your currently running kernel
# The next command should produce a list of kernel header files.
ls -l /usr/src/kernels/$(uname -r)

sudo yum group list
# Enable GUI on system startup.
sudo yum groupinstall -y "GNOME Desktop" "Graphical Administration Tools"
# Enable GUI on system startup.
sudo ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
# Reboot the machine
sudo reboot
```