# Ansible

## Create global config

sudo nano /etc/ansible/hosts

[servers]
dockerUbuntu ansible_host=172.17.0.2

[all:vars]
ansible_python_interpreter=/usr/bin/python3

## Ping ssh

ansible all -m ping -u root -k

## Use playbook

ansible-playbook -i inventory HelloWorld.yml -u root -k
