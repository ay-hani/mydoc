# Ansible 
## Overview
Ansible is an open source IT automation engine that automates provisioning configuration management, application deployment ,orchestration, and many other IT processes.  
Use Ansible  automation to install software, automate daily task, provision infrastructure,  improve security and compliance, patch systems, and share automation across your organization.  

## Ad hoc commands
Ad hoc commands are commands which can be run individually to perform quick functions.  
Ad hoc commands are not used for configuration management and deployment, because these commands are of one time usage. ansible-playbook is used for configuration management and deployment.

## [inventory](https://docs.ansible.com/ansible/latest/network/getting_started/first_inventory.html)
Ansible is to use an inventory file to organize your managed nodes into the groups with information like the ansible_network_os and the SSH user.running a playbook without an inventory requires several command_line flags.  

invnetory syntax type and grouping syntax  

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
### [dynamic inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.htmlS)
If your Ansible inventory fluctuates over time, with hosts spinning up and shutting down in response to business demands, the static inventory solutions described in How to build your inventory will not serve your needs. You may need to track hosts from multiple sources: cloud providers, LDAP, Cobbler, and/or enterprise CMDB systems. 

### inventory variable syntax
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


### inventory structure
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

## [Playbook](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks.html)
Playbook are one of the core features of Ansible and tell Ansible what to execute.they are like to do list for Ansible that contains a list of tasks.  
They contain Plays (which are the basic unit of Ansible execution).Playbooks are written in YAML and are easy to read, write, share and understand.

### [Playbook structure ](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)
Each playbook is an aggregations of one or more plays in it.playbook are structures using plays. There can be more than one play inside a playbook.

```text
Play

Role

Block

Task (actions)
```
### Play
The main context for Ansible execution, this playbook object maps managed nodes (hosts) to tasks. The Play contains variables, roles and an ordered lists of tasks and can be run repeatedly.

### Role
Roles provide a framework for fully independent , or interdependent collections of variables,task,templates and modules.  
in Ansible the role is a primary mechanism for breaking a playbook into multiple files.

#### [Role structure](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html  )
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

### [Block](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_blocks.html)
Blocks create logical groups of tasks. Blocks also offer ways to handle task errors, similar to exception handling in many programming languages.

### Task
The definition of an ‘action’ to be applied to the managed host.  
By default, Ansible executes each task in order, one at a time, against all machines matched by the host pattern. Each task executes a module with specific arguments. When a task has executed on all target machines, Ansible moves on to the next task.  

## Install
install on ubuntu 20.04
```bash
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```
## CLI workflow  








## source
related:  
https://www.tutorialspoint.com/ansible/index.htm  
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html

