# Ansible on docker ubuntu image

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

docker run --name ubuntu-ssh -d --privileged ubuntu-ssh /usr/sbin/init

--privileged and /usr/sbin/init will make systemctl working.
-> https://stackoverflow.com/questions/59466250/docker-system-has-not-been-booted-with-systemd-as-init-system

### Retrieve ip of container

```bash
hostname -I
```

### Try ssh

```bash
ssh test@172.17.0.2
```

password: 'test' (set in Dockerfile)

## Add python interpreter (optional)

Now do an "exit" to get on your local machine.

```bash
sudo nano /etc/ansible/hosts
```

Add this :

```
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

### Install docker, add user in docker group and install docker compose on the target

```ansible
ansible-playbook -i inventory Docker/Docker.yml Docker/DockerGroup.yml Docker/DockerGroup.yml Docker/Dockercompos_.yml -u test -kK
```

-kK will make root working
-> https://stackoverflow.com/questions/25582740/missing-sudo-password-in-ansible

password: 'test' (set in Dockerfile)

