# Ansible

## Install Ansible

```bash
sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt update
sudo apt install -y ansible
```

## Docker

You need a server to run the ansible part, instead of using a vm with hypervisor, use a docker image of ubuntu

### Build

docker build -t ubuntu-ssh .

### Run

docker run --name ubuntu-ssh -d ubuntu-ssh

### Retrieve ip of container

```bash
hostname -I
```

### Try ssh

```bash
ssh test@172.17.0.2
```

password: 'test' (set in Dockerfile)

## Create ansible host

Now do an "exit" to get on your local machine.

```bash
sudo nano /etc/ansible/hosts
```

Add this :

```
[servers]
dockerUbuntu ansible_host=172.17.0.2

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

Replace 172.17.0.2 by the ip of your container. (Normally it's the same)

## Add publickey (to avoid finger print)

```bash
ssh-keyscan 172.17.0.2 >> ~/.ssh/known_hosts
```

Replace 172.17.0.2 by the ip of your container. (Normally it's the same)

## Ping ssh

```ansible
ansible all -m ping -u test -k
```

password: 'test' (set in Dockerfile)

## Use playbook

```ansible
ansible-playbook -i inventory HelloWorld.yml -u test -k
```

password: 'test' (set in Dockerfile)
