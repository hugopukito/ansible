# Ansible

## Docker

You need a server to run the ansible part, instead of using a vm with hypervisor, use a docker image of ubuntu

### Pull, run and exec the image into a container

```bash
docker pull ubuntu
```

```bash
docker run --name ubuntu -d ubuntu
```

```bash
docker exec -it ubuntu /bin/bash
```

### Enable ssh

```bash
apt-get update
apt-get install -y openssh-server
```

### Configure ssh

```bash
apt install nano
nano /etc/ssh/sshd_config
```

Set those options:

PermitRootLogin yes

```bash
service ssh restart
```

### Set password

```bash
passwd root
```

Now set the password you want, when you will launch a playbook, it will be ask.

### Retrieve ip of container

```bash
hostname -I
```

Now do an "exit" to get on your local machine.

```bash
sudo nano /etc/ansible/hosts
```

Add this :

```
[servers] dockerUbuntu ansible_host=172.17.0.2
[all:vars] ansible_python_interpreter=/usr/bin/python3
```

Replace 172.17.0.2 by the ip of your container. (Normally it's the same)

## Ping ssh

ansible all -m ping -u root -k

## Use playbook

ansible-playbook -i inventory HelloWorld.yml -u root -k
