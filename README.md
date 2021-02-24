# Ansible modules
### Contents
- `ansible-root` and `ansible-client`: two up hosts/vm's, mandatory for this demo!

### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the hostname and ip address for `ansible-root` machine

- open terminal/shell and connecting on `ansible-root` vm/host through ssh: `ssh ansible-root`
- after connected, from `ansible-root` machine,<br/>
  edit and add your hostname and ip address for `ansible-client` machine in `/etc/hosts`,<br/>
  generate the ssh key,<br/>
  and transfer the ssh public key to `ansible-client` machine/vm: `ssh-copy-id ansible-client`

- after transfer successfully ssh public key,<br/>
  try to connect from `ansible-root` machine to `ansible-client` machine,<br/>
  you should connect/login successfully through ssh, if you successfully transferred the public key

### Install/setting up the git/ansible on your `ansible-root` machine:
- sudo su -
- apt update; apt install ansible
- apt install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[servers]<br/>
ansible-client ansible_host=192.168.8.108<br/>
[all:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- ansible servers -m ping

# Working with ansible modules
### From `ansible-root` machine:
- sudo su -
- git clone https://github.com/mihaivirlan/ansible.git
- cd path_to_new_cloned_repository
- cd ansible-modules/
- pwd (should be as: `/root/ansible/ansible-modules`)
- ansible-playbook copy_file_module.yml
- and so... for each module from the `/root/ansible/ansible-modules` folder


# Create EC2 instance using Ansible
### From your aws console `https://us-east-2.console.aws.amazon.com/`:
- Acces from `Services` menu, the `IAM` create a `user` and generate a `key` for your `new user created`,<br/> 
  or it's generated automatically,<br/> 
  and needed only copy+paste these keys into one new `notepad++` file and save this file into secure location<br/>
  later you will need to use this `key` into `.boto` file

- check if you have a `key pair` in your aws console, if not have, create one,<br/>
  to check for `key pair`, go in `Sevices` -> `EC2` -> `Resources` -> `Key pairs`<br/>
  later you will need to use this `key pair` in the `task.yml` file

### From `ansible-root` machine:
- sudo su - 
- apt install python-pip
- pip install boto
- nano `.boto`
[Credentials]<br/>
aws_access_key_id = `paste_your_generated_key_for_user_created_above_account`<br/>
aws_secret_access_key = `paste_your_generated_key_for_user_created_above_account`

- cd /root/ansible/create-ec2-instance-using-ansible
- ansible-playbook task.yml
- after task running successfully, check in your aws console `https://us-east-2.console.aws.amazon.com/`,<br/> 
  if the instance was successfully created.


# Ansible Inventories
### Creating a Custom Inventory File (by default, after install and configuration `ansible`, the `/etc/ansible/hosts` file, has a inventory role)
### From `ansible-root` machine:
- sudo su -
- edit your `/etc/hosts` file, and add the ip_addresses with hostnames for your client/servers group instances
- comment out all earlier `servers` entries from `/etc/ansible/hosts` file
- cd /root/ansible/ansible-inventories
- ansible-inventory -i inventory --list
- ansible servers -i inventory -m ping
- ansible servers -i inventory -a "df -h"
- ansible-playbook -i inventory updates_packages.yml