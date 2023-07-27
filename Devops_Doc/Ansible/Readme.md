# [Ansible](https://docs.ansible.com/ansible/latest/index.html)
## Overview
Ansible is an open source IT automation engine that automates provisioning configuration management, application deployment ,orchestration, and many other IT processes.  
Use Ansible  automation to install software, automate daily task, provision infrastructure,  improve security and compliance, patch systems, and share automation across your organization.  

## Ad hoc commands
Ad hoc commands are commands which can be run individually to perform quick functions.  
Ad hoc commands are not used for configuration management and deployment, because these commands are of one time usage. ansible-playbook is used for configuration management and deployment.

## Inventory
Ansible is to use an inventory file to organize your managed nodes into the groups with information like the ansible_network_os and the SSH user.running a playbook without an inventory requires several command_line flags.  

inventory syntax type and grouping syntax  

1. in YAML format

```yaml
---

leafs:
  hosts:
    leaf01:
      ansible_host: 10.16.10.11
    leaf02:
      ansible_host: 10.16.10.12

spines:
  hosts:
    spine01:
      ansible_host: 10.16.10.13
    spine02:
      ansible_host: 10.16.10.14

network:
  children:
    leafs:
    spines:
```
2. in INI format
```INI
[leafs]
leaf01
leaf02

[spines]
spine01
spine02

[network:children]
leafs
spines
```
[more information](https://docs.ansible.com/ansible/latest/network/getting_started/first_inventory.html)

### Dynamic inventory
If your Ansible inventory fluctuates over time, with hosts spinning up and shutting down in response to business demands, the static inventory solutions described in How to build your inventory will not serve your needs. You may need to track hosts from multiple sources: cloud providers, LDAP, Cobbler, and/or enterprise CMDB systems. 

[more information](https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.htmlS)

### Inventory variable syntax
The syntax for variable values is different in inventory, in playbooks, and in the group_vars files, which are covered below. Even though playbook and group_vars files are both written in YAML, you use variables differently in each.  

1. in YAML format
```yaml
network:
  children:
    leafs:
    spines:
  vars:
    ansible_connection: ansible.netcommon.network_cli
    ansible_network_os: vyos.vyos.vyos
    ansible_user: my_vyos_user
```
2. in INI format

```INI
[network:children]
leafs
spines

[network:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=vyos.vyos.vyos
ansible_user=my_vyos_user
```

1. In an ini-style inventory file you must use the syntax `key=value` for variable values: `ansible_network_os=vyos.vyos.vyos`.

2. In any file with the `.yml` or `.yaml` extension, including playbooks and `group_vars` files, you must use YAML syntax: `key: value`.

3. In `group_vars` files, use the full `key` name: `ansible_network_os: vyos.vyos.vyos`.

4. In playbooks, use the short-form `key` name, which drops the ansible prefix: `network_os: vyos.vyos.vyos`.


### Inventory structure
```yaml
inventories/
   production/
      hosts               # inventory file for production servers
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         hostname1.yml    # here we assign variables to particular systems
         hostname2.yml

   staging/
      hosts               # inventory file for staging environment
      group_vars/
         group1.yml       # here we assign variables to particular groups
         group2.yml
      host_vars/
         stagehost1.yml   # here we assign variables to particular systems
         stagehost2.yml
```

## Playbook
Playbook are one of the core features of Ansible and tell Ansible what to execute.they are like to do list for Ansible that contains a list of tasks.  
They contain Plays (which are the basic unit of Ansible execution).Playbooks are written in YAML and are easy to read, write, share and understand.  

[more information](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks.html)

### Playbook structure 
Each playbook is an aggregations of one or more plays in it.playbook are structures using plays. There can be more than one play inside a playbook.

```text
Play

Role

Block

Task (actions)
```
[more information](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)

### Play
The main context for Ansible execution, this playbook object maps managed nodes (hosts) to tasks. The Play contains variables, roles and an ordered lists of tasks and can be run repeatedly.

### Role
Roles provide a framework for fully independent , or interdependent collections of variables,task,templates and modules.  
in Ansible the role is a primary mechanism for breaking a playbook into multiple files.

#### Role structure
```yaml
roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```
1. `tasks/main.yml` - the main list of tasks that the role executes.

1. `handlers/main.yml` - handlers, which may be used within or outside this role.

2. `library/my_module.py` - modules, which may be used within this role.

3. `defaults/main.yml` - default variables for the role. These variables have the lowest priority of any variables available, and can be easily overridden by any other variable, including inventory variables.
 
4. `vars/main.yml` - other variables for the role.
 
5. `files/main.yml` - files that the role deploys.
 
6. `templates/main.yml` - templates that the role deploys.

1. `meta/main.yml` - metadata for the role, including role dependencies and optional Galaxy metadata such as platforms supported.

[more information](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

### Block
Blocks create logical groups of tasks. Blocks also offer ways to handle task errors, similar to exception handling in many programming languages.

[more information](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_blocks.html)

### Task
The definition of an ‘action’ to be applied to the managed host.  
By default, Ansible executes each task in order, one at a time, against all machines matched by the host pattern. Each task executes a module with specific arguments. When a task has executed on all target machines, Ansible moves on to the next task.  

## Ansible vault
Once you have a strategy for managing and storing vault passwords, you can start encrypting content. You can encrypt two types of content with Ansible Vault: variables and files. Encrypted content always includes the `!vault` tag, which tells Ansible and YAML that the content needs to be decrypted, and a `|` character, which allows multi-line strings. Encrypted content created with `--vault-id` also contains the vault ID label.  

Ansible Vault can encrypt any structured data file used by Ansible, including:

1. group variables files from inventory
1. host variables files from inventory
1. variables files passed to ansible-playbook with -e @file.yml or -e @file.json
1. variables files loaded by include_vars or vars_files
1. variables files in roles
1. defaults files in roles
1. tasks files
1. handlers files
1. binary files or other arbitrary files

## Install
install on ubuntu 20.04
```shell
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```
## CLI workflow

1. ansible
1. ansible-config
1. ansible-console
1. ansible-doc
1. ansible-galaxy
1. ansible-inventory
1. ansible-playbook
1. ansible-pull
1. ansible-vault

[more information](https://docs.ansible.com/ansible/latest/command_guide/command_line_tools.html)

### ansible
is an extra-simple tool/framework/API for doing ‘remote things’. this command allows you to define and run a single task ‘playbook’ against a set of hosts.

#### sample usage
-C, --check  
don’t make any changes; instead, try to predict some of the changes that may occur

-D, --diff  
when changing (small) files and templates, show the differences in those files; works great with –check

-K, --ask-become-pass  
ask for privilege escalation password

-M, --module-path  
prepend colon-separated path(s) to module library (default={{ ANSIBLE_HOME ~ “/plugins/modules:/usr/share/ansible/plugins/modules” }})

-a,  --args  
The action’s options in space separated k=v format: -a ‘opt1=val1 opt2=val2’ or a json string: -a ‘{“opt1”: “val1”, “opt2”: “val2”}’

-b, --become  
run operations with become (does not imply password prompting)

-i, --inventory, --inventory-file  
specify inventory host path or comma separated host list. –inventory-file is deprecated

--ask-vault-password, --ask-vault-pass  
ask for vault password

```shell
$ ansible inventory_group_name -m copy -a "src=/etc/hosts dest=/tmp/hosts"
$ ansible -i inventory/path/to/file inventory_group_name -m ping --ask-vault-password
$ ansible all -m ansible.builtin.setup
```

[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible.html)

### ansible-config
Config command line class

1. list
2. dump
3. view
4. init

#### sample usage

```shell
$ ansible-config list 
$ ansible-config dump
$ ansible-config view
$ ansible-config init
```

[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-config.html)


### ansible-console
A REPL that allows for running ad-hoc tasks against a chosen inventory from a nice shell with built-in tab completion (based on dominis’ ansible-shell).

1. It supports several commands, and you can modify its configuration at runtime:
2. cd [pattern]: change host/group (you can use host patterns eg.: app*.dc*:!app01*)
3. list: list available hosts in the current path
4. list groups: list groups included in the current path
5. become: toggle the become flag
6. !: forces shell module instead of the ansible module (!yum update -y)
7. verbosity [num]: set the verbosity level
8. forks [num]: set the number of forks
9. become_user [user]: set the become_user
10. remote_user [user]: set the remote_user
11. become_method [method]: set the privilege escalation method
12. check [bool]: toggle check mode
13. diff [bool]: toggle diff mode
14. timeout [integer]: set the timeout of tasks in seconds (0 to disable)
15. help [command/module]: display documentation for the command or module
16. exit: exit ansible-console

#### sample usage

```shell
$ ansible-console
```
[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-console.html)

### ansible-doc
displays information on modules installed in Ansible libraries. 

#### sample usage

-F, --list_files  
Show plugin names and their source files without summaries (implies –list). A supplied argument will be used for filtering, can be a namespace or full collection name.

-j, --json  
Change output into json format.

-l, --list  
List available plugins. A supplied argument will be used for filtering, can be a namespace or full collection name.

```shell
$ ansible-doc ping
$ ansible-doc -F
$ ansible-doc -j -F
$ ansible-doc -j -l
$ ansible-doc -l
```
[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-doc.html)

### ansible-galaxy
Command to manage Ansible roles and collections.

1. role
    1. init : Initialize new role with the base structure of a role.
    1. remove : Delete roles from roles_path.
    1. delete : Removes the role from Galaxy. It does not remove or alter the actual GitHub repository.
    1. list : Show the name and version of each role installed in the roles_path.
    1. search : Search the Galaxy database by tags, platforms, author and multiple keywords.
    1. import : Import a role into a galaxy server
    1. setup : Manage the integration between Galaxy and the given source.
    1. info : View more details about a specific role.
    1. install : Install role(s) from file(s), URL(s) or Ansible Galaxy
2. collection
    1. download : Download collections and their dependencies as a tarball for an offline install.
    2. init : Initialize new collection with the base structure of a collection.
    3. build : Build an Ansible collection artifact that can be published to Ansible Galaxy.
    4. publish : Publish a collection artifact to Ansible Galaxy.
    5. install : Install collection(s) from file(s), URL(s) or Ansible Galaxy
    6. list : Show the name and version of each collection installed in the collections_path.
    7. verify : Compare checksums with the collection(s) found on the server and the installed copy. This does not verify dependencies.

#### sample usage

```shell
$ ansible-galaxy role list -p roles/
$ ansible-galaxy role init /path/to/new_role_name
```
[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html)

### ansible-inventory
used to display or dump the configured inventory as Ansible sees it

#### sample usage
-y, --yaml   
Use YAML format instead of default JSON, ignored for –graph

-i, --inventory, --inventory-file   
specify inventory host path or comma separated host list. –inventory-file is deprecated  

--vault-password-file, --vault-pass-file   
vault password file  

--vars   
Add vars to graph display, ignored unless used with –graph    

--output    
When doing –list, send the inventory to a file instead of to the screen  

--export    
When doing an –list, represent in a way that is optimized for export,not as an accurate representation of how Ansible has processed it  

--graph   
create inventory graph, if supplying pattern it must be a valid group name. It will ignore limit  

--host    
Output specific host info, works as inventory script. It will ignore limit  

--list   
Output all hosts info, works as inventory script  

```shell
$ ansible-inventory --list
$ ansible-inventory --list --ask-vault-pass
$ ansible-inventory --host 192.168.10.10
$ ansible-inventory --graph
$ ansible-inventory --graph app
$ ansible-inventory  --list --output inventory_file.json
$ ansible-inventory --graph  --vars
$ ansible-inventory -i inventory/sample_inv_file --list
$ ansible-inventory -i inventory/sample_inv_file --list --yaml
```
[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-inventory.html)

### ansible-playbook
the tool to run Ansible playbooks, which are a configuration and multinode deployment system.  

#### sample usage
--ask-vault-password, --ask-vault-pass  
ask for vault password  

--become-method   
privilege escalation method to use (default=sudo), use ansible-doc -t become -l to list valid choices.  

--become-password-file , --become-pass-file   
Become password file  

--become-user   
run operations as this user (default=root)  

--list-hosts  
outputs a list of matching hosts; does not execute anything else  

--list-tags  
list all available tags  

--list-tasks  
list all tasks that would be executed  

--private-key , --key-file   
use this file to authenticate the connection  

--skip-tags  
only run plays and tasks whose tags do not match these values  

--start-at-task   
start the playbook at the task matching this name  

--step  
one-step-at-a-time: confirm each task before running  

--syntax-check  
perform a syntax check on the playbook, but do not execute it  

-C, --check  
don’t make any changes; instead, try to predict some of the changes that may occur  

-D, --diff  
when changing (small) files and templates, show the differences in those files; works great with –check  

-K, --ask-become-pass  
ask for privilege escalation password  

-i, --inventory, --inventory-file  
specify inventory host path or comma separated host list. –inventory-file is deprecated  

-k, --ask-pass  
ask for connection password  

-l , --limit   
further limit selected hosts to an additional pattern  

-t, --tags  
only run plays and tasks tagged with these values  

-u , --user   
connect as this user (default=None)  


```shell
$ ansible-playbook -i inventory/sample_inv_file playbook_getInfo.yml --step --ask-vault-pass
$ ansible-playbook  playbook_user_management.yml --list-tasks
$ ansible-inventory -i inventory/sample_inv_file --list --yaml --ask-vault-pass
$ ansible-playbook playbook_check.yml --list-tags
$ ansible-playbook -i inventory/sample_inv_file playbook_getInfo.yml --syntax-check
```

[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)

### ansible-pull
Used to pull a remote copy of ansible on each managed node, each set to run via cron and update playbook source via a source repository.  

[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-pull.html)

### ansible-vault
can encrypt any structured data file used by Ansible. This can include group_vars/ or host_vars/ inventory variables, variables loaded by include_vars or vars_files, or variable files passed on the ansible-playbook command line with -e @file.yml or -e @file.json. Role variables and defaults are also included!

Because Ansible tasks, handlers, and other objects are data, these can also be encrypted with vault. If you’d like to not expose what variables you are using, you can keep an individual task file entirely encrypted.


1. create : Create new vault encrypted file
1. decrypt : Decrypt vault encrypted file
1. edit : Edit vault encrypted file
1. view : View vault encrypted file
1. encrypt : Encrypt YAML file
1. encrypt_string : Encrypt a string
1. rekey : Re-key a vault encrypted file

#### sample usage

--ask-vault-password, --ask-vault-pass  
ask for vault password

--encrypt-vault-id    
the vault id used to encrypt (required if more than one vault-id is provided)

--vault-id  
the vault identity to use

--vault-password-file, --vault-pass-file  
vault password file

```shell
ansible-vault create inventory/group_vars/sample_group_vars_file.yml
ansible-vault encrypt inventory/group_vars/sample_group_vars_file.yml
ansible-vault decrypt inventory/group_vars/sample_group_vars_file.yml
ansible-vault edit inventory/group_vars/sample_group_vars_file.yml
ansible-vault view inventory/group_vars/sample_group_vars_file.yml
ansible-vault view inventory/group_vars/sample_group_vars_file.yml
```

[more information and options](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)

## source
related:  
https://www.tutorialspoint.com/ansible/index.htm  
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html

