# Ansible modules
### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the `ip address` and `hostname` for `ansible-root` machine

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
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `ansible-modules`
- pwd (should be as: `/root/ansible/ansible-modules`)
- ansible-playbook copy_file_module.yml
- and so... for each `ansible module` from the `/root/ansible/ansible-modules` folder

### Working with `task-control`
#### From `ansible-root` machine:
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `task-control`
- pwd (should be as: `/root/ansible/task-control`)

- ansible-playbook advanced_when_usage.yml
- ansible-playbook ifsize.yml
- ansible-playbook loopservices.yml

- ansible linux -a "sudo systemctl stop crond"
- ansible linux -a "sudo systemctl status crond"
- ansible-playbook restart_sshd_when_crond_is_running.yml
- ansible linux -a "sudo systemctl start crond"
- ansible linux -a "sudo systemctl status crond"
- ansible-playbook restart_sshd_when_crond_is_running.yml

- ansible-playbook when_multiple.yml
- ansible linux -m setup -a "filter=ansible_distribution"
- ansible linux -m setup -a "filter=ansible_memfree_mb"

- ansible linux -a "sudo systemctl status httpd"
- ansible-playbook when_multiple_complex.yml
- ansible linux -a "sudo systemctl status httpd"

- touch /tmp/index.html
- ansible-playbook handlers.yml
- ansible linux -a "ls -l /var/www/html"
- ansible linux -a "sudo rm /var/www/html/index.html"
- ansible linux -a "ls -l /var/www/html"
- ansible-playbook handlers.yml

- `Gathering Facts` are importants when we work with `templates` and not only:<br/>
   This module takes care of executing the configured facts modules, the default is to use the setup module.<br/>
   This module is automatically called by playbooks to gather useful variables about remote hosts that can be used in playbooks.<br/>
   It can also be executed directly by /usr/bin/ansible to check what variables are available to a host.<br/>
   Ansible provides many facts about the system, automatically.<br/>
   Ansible collects pretty much all the information about the remote hosts as it runs a playbook.<br/> 
   The task of collecting this remote system information is called as Gathering Facts by ansible,<br/>
   and the details collected are generally known as facts or variables.


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

- cd `/root/ansible/create-ec2-instance-using-ansible`
- ls -l
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
- cd `/root/ansible/ansible-inventories`
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
  `https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-centos-7`

#### For `centos7`: 
- yum install wget && yum install gcc && yum install python-devel && yum install python-pip && yum install python3-virtualenv
- yum install python2-winrm
- pip install python2-winrm
- pip3 install python2-winrm
- pip3 install pywinrm

#### For `centos8`: 
- dnf install wget && dnf install gcc && dnf install python3-devel && dnf install python3-pip && dnf install python3-virtualenv
- dnf install python2-winrm
- pip install python2-winrm
- pip3 install python2-winrm
- pip3 install pywinrm

#### For both `centos` versions:
- pip -V
- wget `https://bootstrap.pypa.io/get-pip.py`
- python3.6 get-pip.py / python get-pip.py
- pip install pywinrm
- edit and add in your `/etc/hosts` the `ip_address` and `hostname` for your `windows` machine
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
- cd `win_chocolately`
- ansible-playbook install_multiple_packages_sequentially.yml

#### From your `windows` machine:
- check if all tool mentioned in your `install_multiple_packages_sequentially.yml`,<br/> 
  on `ansible-root` machine, were installed successfully
  

# Ansible Tower steps for installation on RHEL server
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


# Ansible Roles
### Contents
- `ansible-root` one host/vm, mandatory for this demo!

#### Generate the ansible roles:
- cd `/etc/ansible`
- ls -la
- cd `roles`
- ansible-galaxy init apache --offline
- ls -la
- cd `apache`,<br/>
  and work with new generated `apache` role...

#### Describe the ansible role, for example `apache` role:
- `defaults` = Data about the role / application. Default variables.
- `files` = Put the static files here. Files will then be copied on `remote machines`
- `handlers` = Tasks which are based on some actions. Triggers.<br/>
   Example: in case my httpd.conf changes, it should trigger service restart.

- `meta` = Information about the role. Author, supported platforms, etc. Dependencies, if any.
- `tasks` = Core logic or code. Installing package, coping files, etc.
- `templates` = Similar to files except that templates support dynamic files. Jinja2 - template language.
- `vars` = Both vars and defaults stores variable.<br/>
   Variables stored under "vars" has got higher prioprity and difficult to override.

# Managing files
### Contents
- `ansible-root` machine and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Using Templates and Jinja2
#### From `ansible-root` machine:
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `managing-files`
- pwd (should be as: `/root/ansible/managing-files`)

- ansible-playbook vsftpd-template.yml
- ansible `ansible-client` -a "cat /etc/vsftpd/vsftpd.conf"

- ansible `ansible-client` -a "cat /etc/hosts"
- ansible-playbook hostsfile.yml

- ansible-playbook copy.yml
- ansible `linux` -a "cat /tmp/hosts"

#### Or connect through ssh on `ansible-client` machine and check if `vsftpd.j2` template was successfully copied:
- ls -l /etc/vsftpd
- cat /etc/vsftpd/vsftpd.conf