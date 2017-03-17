# nginx-ansible-linux

## Description

Automates configuration and installation of nginx onto a Linux client

## Usage

1. Install Ansible on the master node:
    * Ubuntu: `sudo apt-get install ansible`

2. Test the installation with `ansible --version`; should return something like `ansible 2.1.1.0` and some config info

3. Specify which child nodes you wish to deploy nginx to by editing the Ansible hosts file (`sudo vim /etc/ansible/hosts`) in your master node. Include lines similar to:

    [nginx]
    web1 ansible_ssh_host=52.91.71.123 ansible_ssh_user=ubuntu
4. Ensure the child node have the master node's public SSH key: on the child node, edit `~/.ssh/authorized_keys` to append your master node public key 
5. Install Ansible on the child node: `sudo apt-get install ansible`
6. Ensure your nodes are accessible by Ansible: `ansible -m ping all`. If this fails, some things to check are:
  6a. Make sure your child node is open to ping traffic, e.g. on AWS EC2, add "All ICMP - IPv4" to the inbound rule of an attached security group
  6b.  

