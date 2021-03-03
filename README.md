# Ansible modules
### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
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

#### Install/setting up the git/ansible on your `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu):
- sudo su -
- apt update; apt install ansible
- apt install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[servers]<br/>
ansible-client ansible_host=192.168.8.108<br/>
[servers:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- nano `/etc/ansible/ansible.cfg` (edit and add uncomment the follow line)<br/>
#host_key_checking = False

- ansible servers -m ping

### Working with `ansible modules`
#### From `ansible-root` machine:
- sudo su -
- git clone https://github.com/mihaivirlan/ansible.git
- cd path_to_new_cloned_repository
- cd ansible-modules/
- pwd (should be as: `/root/ansible/ansible-modules`)
- ansible-playbook copy_file_module.yml
- and so... for each `ansible module` from the `/root/ansible/ansible-modules` folder


# Create EC2 instance using Ansible
### Contents
- `ansible-root` machine and `aws account`: two stuff mandatory for this demo!

#### From your aws console `https://us-east-2.console.aws.amazon.com/`:
- Acces from `Services` menu, the `IAM` create a `user` and generate a `key` for your `new user created`,<br/> 
  or it's generated automatically,<br/> 
  and needed only copy+paste these keys into one new `notepad++` file and save this file into secure location<br/>
  later you will need to use this `key` into `.boto` file

- check if you have a `key pair` in your aws console, if not have, create one,<br/>
  to check for `key pair`, go in `Sevices` -> `EC2` -> `Resources` -> `Key pairs`<br/>
  later you will need to use this `key pair` in the `task.yml` file

#### From `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu):
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
### Contents
- `ansible-root` machine and `ansible-client`: two up vm's/machines,, mandatory for this demo!

#### Creating a Custom Inventory File (by default, after install and configuration `ansible`, the `/etc/ansible/hosts` file, has a inventory role)
#### From `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu):
- sudo su -
- edit your `/etc/hosts` file, and add the `ip_addresses` with `hostnames` for your `client` group instances/machines
- comment out all earlier `servers` entries from `/etc/ansible/hosts` file
- cd /root/ansible/ansible-inventories
- ansible-inventory -i inventory --list
- ansible servers -i inventory -m ping
- ansible servers -i inventory -a "df -h"
- ansible-playbook -i inventory updates_packages.yml


# Ansible Manage Windows / Ansible Winrm
### Contents
- `ansible-root` and `windows`: two up hosts/vm's, mandatory for this demo!

#### From your `windows` machine:
- Create/add a new user on `windows` machine, and assign it to group Administrator:<br/>
	username: `your_username`<br/>
	password: `your_set_password`<br/>
	password hint: `your_set_hint`<br/>

- From Start search -> Windows defender firewall -> Advanced Settings -><br/>
  Windows defender Firewall Propeties -> Firewall State -><br/>
  Inbound connections set as "Allow" for all tabs (Domain Profile/Private Profile/etc...)

- Click to `Start` -> acces and open the `terminal` as a Administrator and set up/run follow:<br/>
  winrm quickconfig -q<br/>
  winrm set winrm/config/service @{AllowUnencrypted="true"}<br/>
  winrm set winrm/config/service/auth @{Basic="true"}<br/>

- `Mentioned:` On all your future `windows` machines, which we want to automate with ansible,<br/> 
  we will must to have a dedicated `ansible user` with same credentials,<br/> 
  therefore, the connection credentials from `/etc/ansible/group_vars/win.yml` file,<br/> 
  from `ansible-root` machine, will be only for one user!

#### From `ansible-root` machine (for this demo, I used the RedHat distribution - CentOS7):
- sudo su -
- should need to have installed the `ansible`:<br/>
  https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-centos-7

- for `centos7`: yum install wget && yum install gcc && yum install python-devel && yum install python-pip
- or, for `centos8`: dnf install wget && dnf install gcc && dnf install python3-devel && dnf install python3-pip
- pip -V
- wget https://bootstrap.pypa.io/get-pip.py
- python3.6 get-pip.py / python get-pip.py
- pip install pywinrm
- edit and add in your `/etc/hosts` the `hostname` and `ip_address` for your `windows` machine
- edit `/etc/ansible/hosts` and add to end of the file follow:<br/>
[windows]<br/>
windows_hostname<br/>
another_windows_hostname<br/>

  [windows:vars]<br/>
  ansible_user=`your_username_created_above`<br/>
  ansible_password=`your_set_password_above`<br/>
  ansible_connection=`winrm`<br/>
  ansible_winrm_transport=`basic`<br/>
  ansible_winrm_port=`5985`

- test connection between your `ansible-root` machine and `windows` machine:<br/>
  `ping windows_hostname`

- ansible win -m win_ping

### Working with `win_chocolatey` - ansible module
#### From `ansible-root` machine:
- ansible-doc win_chocolatey
- ansible win -m win_chocolatey -a 'name=notepadplusplus state=present'
- cd win_chocolately/
- ansible-playbook install_multiple_packages_sequentially.yml

#### From your `windows` machine:
- check if all tool mentioned in your `install_multiple_packages_sequentially.yml`,<br/> 
  on `ansible-root` machine, were installed successfully
  

# Ansible Tower steps for instillation on RHEL server
### Contents
- `ansible-root` one host/vm, mandatory for this demo!

#### From `ansible-root` machine (for this demo, I used the RedHat distribution - CentOS7):
- sudo su -
- yum -y update
- yum whatprovides netstat
- yum -y install net-tools
- yum -y install epel-release wget curl tree vim ansible -y
- `mkdir /tmp/tower && cd  /tmp/tower`
- curl -k -O https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz
- ls
- tar xvf `ansible-tower-setup-latest.tar.gz`
- ls
- cd `ansible-tower-setup-3.8.1-1/`
- nano `inventory` (in this file for a single standalone tower setup (non-cluster),<br/> 
  just update “admin_password”, “pg_password”)

- ls
- `./setup.sh`
- Once setup finish you will be able to access Tower on IP address on server: `https://IPAddress/`
- `Note:` Default user name to login will be `admin (admin_password)`<br/>
   and password will be value set in `inventory` file for `admin_password`

- netstat -tulpn
- Now you have to enter license detail before you can use Tower,<br/> 
  there are two ways to enter license detail:<br/>
  Browse license file (that you have acquired from RedHat)<br/>
  Login with your RedHat account (for a free trail license):<br/>
  `https://access.redhat.com/`<br/>

  Since we are evaluating we will go with second option.<br/>
  You must have a RedHat account, free Redhat account will do:<br/>
  `https://www.redhat.com/en/technologies/management/ansible/try-it/`<br/>
  `https://access.redhat.com/management/subscriptions`<br/>
  `https://access.redhat.com/management/subscription_allocations`

#### You may check service status that should be running now:
- supervisorctl status
- ansible-tower-service status
