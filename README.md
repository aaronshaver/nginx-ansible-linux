# nginx-ansible-linux

## Description

Automate the configuration and installation of Nginx on a Linux system, ideally RedHat, CentOS, or Fedora.

* Nginx should listen on port 8888.

## Usage

1. Install Ansible on the master node:
    * Ubuntu: `sudo apt-get install ansible`

2. Test the installation with `ansible --version`; should return something like `ansible 2.1.1.0` and some config info

3. Specify which child nodes you wish to deploy nginx to by editing the Ansible hosts file (`sudo vim /etc/ansible/hosts`) in your master node. Include lines similar to:

```
    [nginx]
    web1 ansible_ssh_host=52.91.71.123 ansible_ssh_user=ubuntu
```
    
4. Ensure the child nodes have the master node's public SSH key: on the child node, edit `~/.ssh/authorized_keys` to append your master node public keys. You can also use the ssh-copy-id command, though it may need to be installed.

5. Install Ansible on the child node: `sudo apt-get install ansible`

6. Ensure your nodes are accessible by Ansible: `ansible -m ping all`. If this fails, some things to check are:
    1. Make sure your child node is open to ping traffic, e.g. on AWS EC2, add "All ICMP - IPv4" to the inbound rule of an attached security group. While you're at it, add a Custom TCP Rule (assuming AWS) for port 8888.
    2. Make sure you can SSH manually into the child node without Ansible 
    3. Run the command with `-vvv` and manually execute the command Ansible is executing (e.g. it'll show the full log of SSH attempts)

7. Run the playbook on the master node: `ansible-playbook deploy.yml` (assumes you're in the project directory that has deploy.yml)

8. Verify nginx is running with `curl -Is 52.91.71.123 | grep HTTP`. This should return: `HTTP/1.1 200 OK` (alternatively, you could go to the IP in a browser, where you'll see the 'Welcome to nginx!' page)

9. If you have trouble with nginx or need to change key settings, this command (performed on the child node) to restart the server can help: `sudo /etc/init.d/nginx restart`
