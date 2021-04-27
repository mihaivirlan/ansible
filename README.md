# Ansible Ad Hoc Commands
- Use `ansible-doc -l` for a list of modules currently available
- Find more info about certain module type: ansible-doc `user`
- ansible `linux` -m `command` -a 'cat /etc/hosts'
- ansible `linux` -m `user` -a 'name=lisa'
- ansible `linux` -m `command` -a 'id lisa'
- ansible `linux` -m `user` -a 'name=lisa state=absent'
- ansible `linux` -m `command` -a 'id lisa'
- ansible `linux` -m `copy` -a 'content="hello world" dest=/tmp/motd'
- ansible `linux` -m `shell` -a 'cat /tmp/motd'



# Ansible Playbooks
### Understanding Plays
- A `play` is a series of tasks that are executed against selected hosts from the inventory, using specific credentials.
- Using multiple `plays` allows running tasks on different hosts, using different credentials from the same playbook.
- Within a `play` definition, escalation parameters can be defined:<br/>
`remote_user`: the name of the remote user<br/>
`become`: to enable or disable privilege escalation<br/>
`become_method`: to allow using and alternative escalation solution<br/>
`become_user`: the target user used for privilege escalation

### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the `ip address` and `hostname` for `ansible-root` machine

- open terminal/shell and connecting on `ansible-root` vm/host through ssh: `ssh ansible-root`
- after connected, from `ansible-root` machine,<br/>
  edit and add your hostname and ip address for `ansible-client` machine in `/etc/hosts`,<br/>
  generate the ssh key,<br/>
  and transfer the ssh public key to `ansible-client` machine/vm: `ssh-copy-id username@ansible-client` / `ssh-copy-id root@ansible-client`

- after transfer successfully ssh public key,<br/>
  try to connect from `ansible-root` machine to `ansible-client` machine,<br/>
  you should connect/login successfully through ssh, if you successfully transferred the public key

#### Install/setting up the git/ansible on your `ansible-root` machine (for this demo, I used the RedHat distribution - Centos7)
- sudo su -
- yum update; yum install ansible
- yum install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[linux]<br/>
ansible-client<br/>
[linux:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- nano `/etc/ansible/ansible.cfg` (edit and add uncomment the follow line)<br/>
#host_key_checking = False

- ansible `linux` -m `ping`

#### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `ansible_modules_and_ansible_playbooks`
- pwd (should be as: `/root/ansible/ansible_modules_and_ansible_playbooks`)

- cat vsftpd.yml
- ansible-playbook --syntax-check vsftpd.yml
- ansible-playbook -vvvv vsftpd.yml / ansible-playbook -vv vsftpd.yml
- ansible-playbook vsftpd.yml
- ansible `linux` -a "ls -l /var/ftp/pub"
- ansible `linux ` -m `shell` -a "cat /var/ftp/pub/README"

- cat webserver_setup_and_test.yml
- ansible-playbook --syntax-check webserver_setup_and_test.yml
- ansible-playbook webserver_setup_and_test.yml / ansible-playbook -v webserver_setup_and_test.yml
- ansible `centos8-server` -m `shell` -a "cat /var/www/html/index.html"

- ansible-playbook webserver_uninstall.yml
- ansible centos8-server -m shell -a "rpm -qa | grep http"



# Ansible modules
### Essential Ansible Modules
- `ping`: ansible `linux` `ping`
- `service`: ansible `linux` -m `service` -a "name=httpd state=started"
- `command`: ansible `linux` -m `command` -a "/sbin/reboot -t now"
- `shell`: ansible `linux` -m `shell` -a set
- `raw`: runs a command on a remote host without a need for Python
- `copy`: ansible `linux` -m `copy` -a 'content="hello world" dest=/tmp/motd'

### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the `ip address` and `hostname` for `ansible-root` machine

- open terminal/shell and connecting on `ansible-root` vm/host through ssh: `ssh ansible-root`
- after connected, from `ansible-root` machine,<br/>
  edit and add your hostname and ip address for `ansible-client` machine in `/etc/hosts`,<br/>
  generate the ssh key,<br/>
  and transfer the ssh public key to `ansible-client` machine/vm: `ssh-copy-id username@ansible-client` / `ssh-copy-id root@ansible-client`

- after transfer successfully ssh public key,<br/>
  try to connect from `ansible-root` machine to `ansible-client` machine,<br/>
  you should connect/login successfully through ssh, if you successfully transferred the public key

#### Install/setting up the git/ansible on your `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu 19.10)
- sudo su -
- apt update; apt install ansible
- apt install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[linux]<br/>
ansible-client<br/>
[linux:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- nano `/etc/ansible/ansible.cfg` (edit and add uncomment the follow line)<br/>
#host_key_checking = False

- ansible `linux` -m `ping`

#### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `ansible_modules_and_ansible_playbooks`
- pwd (should be as: `/root/ansible/ansible_modules_and_ansible_playbooks`)

- ansible-playbook copy_file_module.yml
- and so... for each `ansible module` from the `/root/ansible/ansible_modules_and_ansible_playbooks` folder



# Ansible Variables and Facts
### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the `ip address` and `hostname` for `ansible-root` machine

- open terminal/shell and connecting on `ansible-root` vm/host through ssh: `ssh ansible-root`
- after connected, from `ansible-root` machine,<br/>
  edit and add your hostname and ip address for `ansible-client` machine in `/etc/hosts`,<br/>
  generate the ssh key,<br/>
  and transfer the ssh public key to `ansible-client` machine/vm: `ssh-copy-id username@ansible-client` / `ssh-copy-id root@ansible-client`

- after transfer successfully ssh public key,<br/>
  try to connect from `ansible-root` machine to `ansible-client` machine,<br/>
  you should connect/login successfully through ssh, if you successfully transferred the public key

#### Install/setting up the git/ansible on your `ansible-root` machine (for this demo, I used the RedHat distribution - Centos7)
- sudo su -
- yum update; yum install ansible
- yum install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[linux]<br/>
ansible-client<br/>
[linux:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- nano `/etc/ansible/ansible.cfg` (edit and add uncomment the follow line)<br/>
#host_key_checking = False

- ansible `linux` -m `ping`

#### Using Variables
- `Variables` can be set at the different levels:<br/>
`In a playbook`<br/>
`In inventory` (deprecated)<br/>
`In inclusion files`

- `Variables` names have some requirements:<br/>
`The name must start with a letter`<br/>
`Variable names can only contain letters, numbers, and underscores`

#### Defining Variables
- `Variables` can be defined in a vars block in the beginning of a playbook
- Alternatively, `variables` can be defined in a variable file, which will be included from the playbook

#### Variables precedence/scope
- `Variables` can be set with different types of scope:<br/>
`Global scope`: this is when a variable is set from inventory or the command line<br/>
`Play scope`: this is applied when it is set from a play<br/>
`Host scope`: this is applied when set in inventory or using a host variable inclusion file

### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `variables_and_facts`
- pwd (should be as: `/root/ansible/variables_and_facts`)

- ansible-playbook -v user.yml
- ansible `linux` -m `shell` -a "grep lisa /etc/passwd"

#### Managing Host Variables
- cd webservers
- pwd (should be as: `/root/ansible/variables_and_facts/webservers`)
- tree
- ansible-playbook site.yml<br/>
  and not a default - `/etc/ansible/hosts` or `/etc/asible/asible.cfg`)

#### Using Multi-valued Variables / Using Dictionary
- To address items in a dictionary, you can use two notations:<br/>
`variable_name['key']`, as in `users['linda']['shell']`
`variable_name.key` as in `users.linda.shell`
- You cannot use `loop` or `with_items` on dictionaries

- cd arrays
- pwd (should be as: `/root/ansible/variables_and_facts/arrays`)
- tree
- ansible-playbook multi-list.yml
- ansible-playbook multi-dictionary.yml

#### Using Ansible Vault / Creating an Encrypted file
- Some modules require sensitive data to be processed
- This may include webkeys, passwords, and more...
- To process sensitive data in a secure way, `Ansible Vault` can be used
- `Ansible Vault` is used to encrypt and decrypt files
- To manage this process, the `ansible-vault` command is used

- To create an encrypted file, use `ansible-vault create playbook.yml`
- `ansible-vault create playbook.yml` - will prompt for a new vault password, and opens the file in `vi`for further editing
- To view a vault encrypted file, use `ansible-vault view playbook.yml`
- To edit, use `ansible-vault edit playbook.yml`
- Use `ansible-vault encrypt playbook.yml` to encrypt an existing file,<br/>
  ans use `ansible-vault decrypt playbook.yml` to decrypt it

- To change a password on an existing file, use `ansible-vault rekey`

#### Using Playbooks with Vault
- To run playbook that accesses `Vault` encrypted files,<br/> 
  you need to use the `--vault-id @prompt` option to be prompted for a password

- Alternatively, you can store the password as a single-line string in a password file,<br/>
  and access that using the `--vault-password-file=vault-file` option

#### Managing Vault files
- When setting up projects with `Vault` encrypted files,<br/>
  it makes sense to use separate files to store encrypted and non-encrypted variables

- To store host or host-group related variable files, you can use the following structure:<br/>
`-group_vars`<br/>
  `--dbservers`<br/>
    `- vars`<br/>
    `- vault`

- cd vault
- pwd (should be as: `/root/ansible/variables_and_facts/vault`)
- tree
- ansible-vault create secret.yml<br/>
  and after run the above command, need to set the password, after that, should be oppened a new file in `vi` editor,<br/>
  in which need to set the username, like follow:<br/>
  `username: lisa`<br/>
  `pwhash: password`

- ls -l
- cat secret.yml
- ansible-playbook --ask-vault-pass create-user.yml

#### Working with Facts
- BY default, all `playbooks` perform `fact gathering` before running the actual plays
- You can run `fact gathering` in an `ad hoc` command using the `setup` module
- To show `facts`, use the debug module to print the value of the `ansible_facts` variable
- Notice that in `facts`, a hierarchical relationship is shown where you can use the dotted format to refer to a specific `fact`

- ansible -m `setup` `linux` / ansible -m `setup` `linux` | less
- cd facts
- pwd (should be as: `/root/ansible/variables_and_facts/facts`)
- ls -la
- ansible-playbook facts.yml / ansible-playbook facts.yml | less
- ansible-playbook ipfact.yml

#### Dispaying Fact Names
- In Ansible 2.4 and before, Ansible `facts` were stored as individual variables,<br/>
  such as `ansible_hostname` and `ansible_interfaces`.

- In Ansible 2.5 and later, all `facts` are stored in one variable with the name `ansible_facts`,<br/>
  and referring to specific `facts` happens in a different way:<br/> 
  `ansible_facts['hostname']` and `ansible_facts['interfaces']`

#### Turning off Fact Gathering
- Disabling `fact gathering` may seriously speed up playbooks
- Use `gather_facts: no` in the play header to disable
- Even if `fact gathering` is disabled, it can ne enabled again by running the `setup` module in a task
- `Gathering Facts` are importants when we work with `templates` and not only:<br/>
   This module takes care of executing the configured facts modules, the default is to use the setup module.<br/>
   This module is automatically called by playbooks to gather useful variables about remote hosts that can be used in playbooks.<br/>
   It can also be executed directly by /usr/bin/ansible to check what variables are available to a host.<br/>
   Ansible provides many facts about the system, automatically.<br/>
   Ansible collects pretty much all the information about the remote hosts as it runs a playbook.<br/> 
   The task of collecting this remote system information is called as Gathering Facts by ansible,<br/>
   and the details collected are generally known as facts or variables.

#### Creating Custom Facts
- `Custom facts` allow administrators to dynamically generate variables which are stored as `facts`
- `Custom facts` are stored in an `ini` or `json` file<br/> 
   in the `/etc/ansible/facts.d` directory on the managed host.<br/>
   The name of these files must end in `.fact`

- `Custom facts` are stored in the `ansible_facts.ansible_local` variable
- Use `ansible hostname -m setup -a "filter=ansible_local"` to display local `facts`
- Notice how fact filename and label are used in the `fact`

- cd facts
- pwd (should be as: `/root/ansible/variables_and_facts/facts`)
- ls -la
- ansible-playbook newlocalfacts.yml
- ansible all -m setup -a "filter=ansible_local"

#### Working with Variables and Facts
- cd facts
- pwd (should be as: `/root/ansible/variables_and_facts/facts`)
- ls -la
- ansible-playbook localfacts.yml
- ansible lamp -m shell -a "rpm -ql samba | grep smb"
- ansible lamp -a "ls -l /etc/ansible/facts.d"
- ansible lamp -m shell -a "cat /etc/ansible/facts.d/localfacts.fact"
- ansible lamp -a "systemctl status smb"



# Ansible tasks-control
### Contents
- `ansible-root` and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Get started
- edit your local `/etc/hosts` file and add inside,<br/> 
  the `ip address` and `hostname` for `ansible-root` machine

- open terminal/shell and connecting on `ansible-root` vm/host through ssh: `ssh ansible-root`
- after connected, from `ansible-root` machine,<br/>
  edit and add your hostname and ip address for `ansible-client` machine in `/etc/hosts`,<br/>
  generate the ssh key,<br/>
  and transfer the ssh public key to `ansible-client` machine/vm: `ssh-copy-id username@ansible-client` / `ssh-copy-id root@ansible-client`

- after transfer successfully ssh public key,<br/>
  try to connect from `ansible-root` machine to `ansible-client` machine,<br/>
  you should connect/login successfully through ssh, if you successfully transferred the public key

#### Install/setting up the git/ansible on your `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu 19.10)
- sudo su -
- apt update; apt install ansible
- apt install git
- ansible --version
- git --version
- nano `/etc/ansible/hosts` (edit and add follow entry at the end of the file)<br/>
[linux]<br/>
ansible-client<br/>
[linux:vars]<br/>
ansible_python_interpreter=/usr/bin/python3

- nano `/etc/ansible/ansible.cfg` (edit and add uncomment the follow line)<br/>
#host_key_checking = False

- ansible `linux` -m `ping`

#### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `task_control`
- pwd (should be as: `/root/ansible/task_control`)
- tree

#### Using Loops and Items
- The `loop` keyword allows you to iterate through a simple list of items
- Before Ansible 2.5, the `items` keyword was used instead
- The list that loop is using can be defined by a variable
- Each item in a loop can be a hash/dictionary with multiple keys in each hash/dictionary
- The `loop` keyword is the current keyword
- In previous versions of Ansible, the `with_*` keyword was used fo the same purpose

- ansible-playbook loopservices.yml
- ansible `linux` -a "sudo systemctl stop crond"
- ansible `linux` -a "sudo systemctl status crond"

- ansible-playbook loopusers.yml
- ansible `linux` -m shell -a "grep -r wheel /etc/group"
- ansible `linux` -m shell -a "ls -l /home"

#### Using Register Variables with Loops
- A `register` is used to store the output of a command and address it is a variable
- You can next use the result ot the command is a conditional or in a loop

- ansible-playbook register_loop.yml
- ansible-playbook register_command.yml

#### Using when to Run Tasks Conditionally / Defining Simple Conditions
- `when` statements are used to run a task conditionally
- A condition can be used to run a task only if specific conditions are true
- Playbook variables, registered variables,<br/>
  and facts can be used in conditions and make sure that tasks only run if specific conditions are true

- For instance, check if a task has run successfully, a certain amount of memory is available, a file exist, etc...

- The simplest example of a condition, is to check whether a Boolean variable is tru or false
- You can also check and see if a non-Boolean variable has a value and use that value in the conditional
- Or use a conditional in which you compare the value of a fact to the specific value of a variable
- Examples:<br/>
`ansible_machine == "x86_64"`<br/>
`ansible_distribution_major_version == "8"`<br/>
`ansible_memfree_mb == 1024`<br/>
`ansible_memfree_mb < 256`<br/>
`ansible_memfree_mb > 256`<br/>
`ansible_memfree_mb <= 256`<br/>
`ansible_memfree_mb != 256`<br/>
`ansible_memfree_mb != 512`<br/>
`my_variable is defined`<br/>
`my_variable is not defined`<br/>
`my_variable`<br/>
`ansible_distribution in supported_distros`

- ansible-playbook distro.yml

#### Testing Multiple Conditions
- `when` can be used to test multiple conditions as well
- Use `and` or `or` group the conditions with parantheses:<br/>
`when: ansible_distribution == "CentOS" or ansible_distribution == "RedHat"`<br/>
`when: ansible_machine == "x86_64" and ansible_distribution == "CentOS"`<br/>
- The `when` keyword also supports a list, and when using a list, all of the conditions must be true
- Complex conditional statements can group conditions using parantheses

- ansible-playbook restart_sshd_when_crond_is_running.yml
- ansible `linux` -a "sudo systemctl start crond"
- ansible `linux` -a "sudo systemctl status crond"
- ansible-playbook restart_sshd_when_crond_is_running.yml

- ansible-playbook when_multiple.yml
- ansible `linux` -m `setup` -a "filter=ansible_distribution"
- ansible `linux` -m `setup` -a "filter=ansible_memfree_mb"

- ansible `linux` -a "sudo systemctl status httpd"
- ansible-playbook when_multiple_complex.yml
- ansible `linux` -a "sudo systemctl status httpd"


#### Using Handlers
- Handlers allow you to configure playbooks in a way that one task<br/> 
  will only run if another task hs been running successfully

- In order to run the handler, a `notify` statement is used from the main task to trigger the handler
- Handlers tipically are used to restart services or reboot hosts
- Handlers are executed after running all tasks in a play
- Handlers will only run if a task has `changed` something,<br/>  
  so if an `ok` result instead of a `changed` result is reported, the handler will not run

- If one of the tasks fails, the handler will not run, but this may be overwritten using `force_handlers: True`
- One task may trigger more than one handler

- touch /tmp/index.html
- ansible-playbook handlers.yml
- ansible `linux` -a "ls -l /var/www/html"
- ansible `linux` -a "sudo rm /var/www/html/index.html"
- ansible `linux` -a "ls -l /var/www/html"
- ansible-playbook handlers.yml
- Remove the file from destination:<br/>
  ansible `linux` -m `file` -a "path=/var/www/html/index.html state=absent" /<br/> 
  ansible `linux` -a "sudo rm /var/www/html/index.html"
- ansible `linux` -a "ls -l /var/www/html"

- Notice:<br/>
  When you run the `add-hoc` command use the `=` notation, but in `playbooks (.yml files)`, use the `:` notation

#### Using Blocks
- A block is a logical group of tasks
- It can be used to control how tasks are executed
- One block can, for instance, be enabled using a single `when`
- Blocks can also be used in error condition handling
- Notice that items can be used on blocks

- ansible-playbook blocks.yml
- ansible-playbook blocks2.yml
- ansible `linux` -m `file` -a "path=/var/www/html/index.html state=touch" /<br/> 
  ansible `linux` -a "sudo touch /var/www/html/index.html"

- ansible `linux` -a "ls -l /var/www/html"
- ansible-playbook blocks2.yml

#### Dealing with Failures
- Ansible looks at the exit status of a task to determine whether it has failed
- When any task fails, Ansible aborts the rest of the play on that host and continues with the next host
- Different solutions can be used to change that behavior
- Use `ignore_errors` in a task to ignore failures
- Use `force_handlers` to force a handler that has been triggered to run, even if (another) task fails

- As Ansible only looks at the exit status of a failed task, it may think a task was successfully where that is not the case
- To be more specific, use `failed_when` to specify what to look for in command output to recognize a failure

- The `failed_when` keyword can be used in a task to identify when a task has failed
- The `fail` module can be used to print a message that informs why a task has failed
- To use `failed_when` or `fail`, the result of the command must be registered,<br/>
  and the registered variable output must be analyzed

- When using the `fail` module, the failing task must have `ignore_errors` set to yes

- ansible-playbook failure.yml
- ansible-playbook failure2.yml

#### Managing Changed Status
- Managing the changed status may be important, as handlers trigger on the changed status
- The result of a command can be registered, and the registered variable can be scanned<br/> 
  for specific text to determine that a change has occurred

- This alllows Ansible to report a changed status, where it normally wouldnot, thus allowing handlers to be triggered
- Using `changed_when` is usable in two cases:<br/>
to allow handlers to run when a change would not normally trigger<br/>
to disable commands that run successful to report a changed status

- ansible-playbook changed.yml

#### Using Task Control
- ansible-playbook lab.yml



# Ansible Deploying Files
### Contents
- `ansible-root` machine and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Using Modules to Manipulate Files
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `deploying_files`
- pwd (should be as: `/root/ansible/deploying_files`)
- ansible-doc files
- ansible-playbook file.yml
- ansible-doc stat
- ansible-doc fetch
- ansible-playbook copy.yml

#### Managing SELinux File Context
- ansible-playbook selinux.yml
- ansible linux -a "ls -lZ /tmp/removeme"


# Ansible Roles
### Contents
- `ansible-root` machine and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Generate the ansible `roles` with `Ansible Galaxy`:
###### You can find more information about galaxy roles, accesing the follow link: https://galaxy.ansible.com/
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `managing_files`
- pwd (should be as: `/root/ansible/managing_files`)
- ansible-galaxy search `nginx`
- ansible-galaxy search `nginx` --platform EL | grep `geerl`
- ansible-galaxy install g`eerlingguy.nginx`
- cd `/home/your_username/.ansible/roles/geerlingguy.nginx/roles/`
- ls -la
- ls -l vars/
- cat tasks/main.yml
- cat tasks/setup-RedHat.yml
- cd ~
- cd `/root/ansible/managing_files`
- cat nginx-role.yml
- ansible-playbook nginx-role.yml

- cd `/etc/ansible/roles`
- ls -la
- ansible-galaxy init `apache` --offline
- ls -la
- cd `apache`,<br/>
  and work with new generated `apache` role...

#### Writing Ansible `Custom Roles`
- cd ~
- mkdir roles
- cd roles/
- ansible-galaxy init `motd`
- ls -la
- tree motd
- cd motd
- you can edit and add more info about you in the file `meta/main.yml`
- nano `tasks/main.yml` (and complete like follow, and uncomment the lines)<br/>
---<br/>
#tasks file for motd<br/>
#- name:	copy motd file<br/>
  #template: <br/>
    #src: templates/motd.j2<br/>
    #dest: /etc/motd<br/>
    #owner: root<br/>
    #group: root<br/>
    #mode: 0444

- nano `templates/motd.j2` (and complete like follow)
Welcome	to {{ ansible_hostname }}<br/>

This file was created on {{ ansible_date_time.date }}<br/>
Go away	if you have no business	being here<br/>

Contact	{{ system_manager }} if	anything is wrong

- nano `defaults/main.yml` (and complete like follow)
---<br/>
#defaults file for motd<br/>
system_manager:	`your_system_manager_email_address`

- cd ../../managing-files
- ansible-playbook motd-role.yml
- ansible `linux` -a "cat /etc/motd"

#### Using `System Roles`
- sudo yum search `system-role`
- sudo yum install `rhel-system-roles`
- sudo rpm -ql `rhel-system-roles`
- sudo su -
- cd /usr/share/ansible/roles/
- ls -l
- tree rhel-system-roles.network/
- cd /usr/share/doc/rhel-system-roles-1.0 / cd /usr/share/doc/rhel-system-roles
- ls -l
-  cd network/
- ls -l

#### Describe the ansible role, for example `apache` role
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
#### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `managing-files`
- pwd (should be as: `/root/ansible/managing-files`)

- ansible-playbook vsftpd-template.yml
- ansible `linux` -a "cat /etc/vsftpd/vsftpd.conf"

#### Or connect through ssh on `ansible-client` machine and check if `vsftpd.j2` template was successfully copied
- ls -l /etc/vsftpd
- cat /etc/vsftpd/vsftpd.conf

#### From `ansible-root` machine:
- ansible `linux` -a "cat /etc/hosts"
- ansible-playbook hostsfile.yml

- ansible-playbook copy.yml
- ansible `linux` -a "cat /tmp/hosts"

- tree files
- tree vars
- cat vars/groups
- cat vars/users
- cat setup_users.yml
- ansible-playbook setup_users.yml
- ansible `linux` -a "sudo cat /etc/group"
- ansible `linux` -a "sudo ls -la /home"

#### Test the ssh connection with new created users, from your `ansible-root` machine to `ansible-client` machine
- ssh `linda@ansible-client` / ssh `lisa@ansible-client`



# Ansible Inventories
### Contents
- `ansible-root` machine and `ansible-client`: two up vm's/machines, mandatory for this demo!

#### Creating a Custom Inventory File (by default, after install and configuration `ansible`, the `/etc/ansible/hosts` file, has a inventory role)
#### From `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu 19.10)
- sudo su -
- edit your `/etc/hosts` file, and add the `ip_addresses` with `hostnames` for your `client` group instances/machines
- comment out all earlier `servers` entries from `/etc/ansible/hosts` file
- cd `/root/ansible/ansible-inventories`
- ansible-inventory -i inventory --list
- ansible `servers` -i inventory -m `ping`
- ansible `servers` -i inventory -a "df -h"
- ansible-playbook -i inventory updates_packages.yml

#### Setting up Static Inventory
#### From `ansible-root` machine
- sudo su -
- git clone `https://github.com/mihaivirlan/ansible.git`
- cd `path_to_new_cloned_repository`
- cd `cd ansible-inventories/install/`
- pwd (should be as: `/root/ansible/ansible-inventories/install/`)
- ansible -i inventory `all` --list-hosts



# Ansible Winrm (Windows machines management)
### Contents
- `ansible-root` and `windows`: two up hosts/vm's, mandatory for this demo!

#### From your `windows` machine
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

#### From `ansible-root` machine (for this demo, I used the RedHat distribution - CentOS7)
- sudo su -
- should need to have installed the `ansible`:<br/>
  `https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-centos-7`

#### For `centos7`
- yum install wget && yum install gcc && yum install python-devel && yum install python-pip && yum install python3-virtualenv
- yum install python2-winrm
- pip install python2-winrm
- pip3 install python2-winrm
- pip3 install pywinrm

#### For `centos8`
- dnf install wget && dnf install gcc && dnf install python3-devel && dnf install python3-pip && dnf install python3-virtualenv
- dnf install python2-winrm
- pip install python2-winrm
- pip3 install python2-winrm
- pip3 install pywinrm

#### For both `centos` versions
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

- ansible `windows` -m `win_ping`

### Working with `win_chocolatey` - ansible module for Windows machines management
#### From `ansible-root` machine
- ansible-doc `win_chocolatey`
- ansible `windows` -m `win_chocolatey` -a 'name=notepadplusplus state=present'
- cd `win_chocolately`
- ansible-playbook install_multiple_packages_sequentially.yml

#### From your `windows` machine:
- check if all tool mentioned in your `install_multiple_packages_sequentially.yml`,<br/> 
  on `ansible-root` machine, were installed successfully



# Create EC2 instance using Ansible
### Contents
- `ansible-root` machine and `aws account`: two stuff mandatory for this demo!

#### From your aws console `https://us-east-2.console.aws.amazon.com/`
- Acces from `Services` menu, the `IAM` create a `user` and generate a `key` for your `new user created`,<br/> 
  or it's generated automatically,<br/> 
  and needed only copy+paste these keys into one new `notepad++` file and save this file into secure location<br/>
  later you will need to use this `key` into `.boto` file

- check if you have a `key pair` in your aws console, if not have, create one,<br/>
  to check for `key pair`, go in `Sevices` -> `EC2` -> `Resources` -> `Key pairs`<br/>
  later you will need to use this `key pair` in the `task.yml` file

#### From `ansible-root` machine (for this demo, I used the Debian distribution - Ubuntu 19.10)
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



# Ansible Tower installation on RHEL server
### Contents
- `ansible-root` one host/vm, and one or more another `ansible-client(s)` mandatory for this demo!

#### Usually for installation of Ansible Tower, you need to have installed CentOS7.7 or a greather of CentOS7.7 version, for example - CentOS8, and a memory RAM >= 8192 MB
#### From `ansible-root` machine (for this demo, I used the RedHat distribution - CentOS8)

- sudo su -
- dnf -y update
- dnf whatprovides netstat
- dnf -y install net-tools
- dnf -y install epel-release wget curl tree ansible -y
- dnf reinstall python3-setuptools
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
- need to have a `httpd` server stopped, and `nginx` server started:<br/>
  systemctl stop httpd<br/>
  systemctl start nginx

- Once setup finish you will be able to access Tower on IP address from browser: `https://your_ip_address/:80`
- `Note:` Default user name to login will be `admin (admin_password)`<br/>
   and password will be value set in `inventory` file for `admin_password`

- After login successfully on your `ansible tower` from browser you must need to have a generated/created/downloaded `subscription`<br/>,
  from here: https://access.redhat.com/management/subscription_allocations

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

#### You may check service status that should be running now
- supervisorctl status
- ansible-tower-service status

#### Configuring a Sample Project in `Ansible Tower`
- Login under your installed `ansible tower`, click `Inventories` option, click `Create a new inventory`,<br/>
  named for example `project1`, click `Save` button, choose `HOSTS` tab from `Inventories`,<br/>
  click `Create a new host`, add the name of your `ansible-client` machine, click `Save` button.
  
- Choose the `Credentials` option from `Menu`, click `Create a new credential`, 
  named for example `project1-credentials`, the `ORGANIZATION` select as `Default`,<br/>
  as `CREDENTIAL TYPE` choose the `Machine` option, after, from `TYPE DETAILS` specify the `USERNAME` and the `PASSWORD`,<br/> 
  with which you will connect to the `ansible-client` machine,<br/>
  in `SSH PRIVATE KEY` input copy and paste your `id_pub`,<br/>
  and in `SIGNED SSH CERTIFICATE` input copy and paste your `id_pub.pub`,<br/>
  as `PRIVILEGE ESCALATION METHOD` choose `sudo` option,<br/>
  and in `PRIVILEGE ESCALATION USERNAME` input, type `root`, click `Save` button.

- Choose the `Projects` option from `Menu`, click `Create a new project`,<br/>
  named for example `project1-project`, from `SCM TYPE` choose the `Git` option,<br/>
  in `SCM URL` input, paste your github repository created,<br/>
  for example in my case it will be: `https://github.com/mihaivirlan/ansible`, click `Save` button.

- Choose the `Templates` option from `Menu`, click `Create a new Job Template`,<br/>
  named for example `project1-template`, from `JOB TYPE` choose `Run` option,<br/>
  from `INVENTORY` choose `project1`, from `PROJECT` choose `project1-project`,<br/>
  and from `PLAYBOOK` choose the certain `playbook` from your repository_url,<br/> 
  in my case can choose the `ansible-modules_and_ansible_playbooks/check_disk_space_usage.yml`,
  from `CREDENTIALS` choose `project1-credentials`, click `Save` button, and `LAUNCH` button.

  Note: First attempt can failed, I mean, when you try to create a `Project`,<br/>
  in this case, you need to go again in `Project` option from `Menu`,<br/> 
  and click to files icon of `project1-project`, after that, you will see that the project take a copy<br/> 
  (something like - `project1-project@7:18:48 PM`),<br/>
  and it should be ok, should connect to your github repository,<br/> 
  and after successfully connected you can delete first version for project - `project1-project`.

