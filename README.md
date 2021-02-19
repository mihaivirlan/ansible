# Ansible modules
### Contents
- working with more and different ansible modules
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
- and so for each module in the `/root/ansible/ansible-modules` folder...
