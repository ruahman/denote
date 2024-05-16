---
title:      "Ansible"
date:       2023-09-14T15:32:13-04:00
tags:       ["devops", "lit", "tech"]
identifier: "20230914T153213"
---

IT automation tool

YAML / Playbook

/etc/ansible/ansible.cfg
- provides default values
- default configuration file for ansible
- you can also have a local copy for a playbook

``` shell
$ ANSIBLE_CONFIG=/opt/ansible-web.cfg ansible-playbook playbook.yml
```

- you can also overide these values through environment variables

`$ ansible-config list`
- list all config value for ansible

`$ ansible-config view`
- shows the current config file

`$ ansible-config dump`
- shows the current settins

Agentless
- don't need to install software on target machines

inventory file
==============

- information about your target systems
- default: /etc/ansible/hosts

inventory file
- you can also specify it in yaml formate

``` ini
server1.company.com
server2.company.com
server3.company.com

web ansible_host=server5.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=P@#
localhost ansible_connection=localhost

[mail]
server3.company.com
server4.company.com
```
- you can group targets in groups
- you can provide aliases
- you can specify how you connect to the machine
- you can also specify the user
- you can also specify localhost
- you can also specify password


Variables
=========

playbook.yml

``` yaml
- 
  name: Add DNS server to resolv.conf
  hosts: localhost
  vars:
    dns_server: 10.1.250.10
  tasks:
    - lineinfile:
	    path: /etc/resolv.conf
		line: 'nameserver {{ dns_server }}'
```

variables
``` yaml
variable1: value1
variable2: value2
```

``` yaml
---
- name: Check /ect/hosts file
  hosts: all
  tasks:
    - shell: cat /etc/hosts
	  register: result
	- debug:
	    var: result.stdout # or results.rc
```
- print result to debug output
- result store infromation from command


``` shell
ansible-playbook -i inventory playbook.yaml -v
```
- incase you don't want to use the debug module, just specify verbose

magic variable
- allow you to access variable defined in another host


Playbooks
=========

Playbook
- Plays :: defines a set of activities
  - Task :: an action to be performed on the host
    + Execut a command
    + Run a script
    + Install a package
    + Shutdown/Restart
	
playbook.yml

``` yaml
# this is all in one play you can split this up into multiple plays
-
  name: Play 1
  hosts: localhost
  tasks:
    - name: Execute command 'data'
	  command: data
	
	- name: Excute script
	  script: test_script.sh
	  
	- name: Install package
	  yum:
	    name: httpd
		state: present
		
	- name: Start web server
	  service:
	    name: httpd
		state: started
```

- host must match what is defined in inventory file

- the diffent actions run by tasks are called modules
  + there are hundreds of modules available out of the box

how you run your playbook
`ansible-playbook playbook.yml`

install_nginx.yml

``` yaml
---
- host: webservers
  tasks:
    - name: Ensure nginx is installed
	  apt:
	    name: nginx
		state: present
	  become: yes
```

`ansible-playbook install_nginx.yml --check --diff`
- to a test run but not install in host
- checks and diffs your playbook

`ansible-lint style_example.yml`
- this lints your playbook


Conditionals
============

``` yaml
---
- name: Install NGINX
  hosts: all
  tasks:
  - name: Install NGINX on Debian
    apt:
	  name: nginx
	  state: present
    when: ansible_os_family == "Debian"
  
  - name: Install NGINX on Redhat
    yum:
	  name: nginx
	  state: present
    when: ansible_os_family == "RedHat"
```


loops
-----

``` yaml
---
- name: Install Software
  hosts: all
  vars:
    packages:
	  - name: nginx
	    required: True
	  - name: mysql
	    required: True
	  - name: apache
	    required: False
		
  tasks:
    - name: Install "{{ item.name }}" on Debian
	  apt:
	    name: "{{ item.name }}"
		state: present
	  when: item.required == True
	  loop: "{{ packages }}"
```
- loop through packages

# Conditionsal and Register #

``` yaml
- name: Check status of a service and email if its down
  host: localhost
  tasks:
    - command: service httpd status
	  register: result
	- mail:
	    to: admin@company.com
		subject: Service Alert
		body: Httpd Service is down
		when: result.stdout.find('down') != -1
```

Modules
=======

# command #

``` yaml
- name: Play 1
  hosts: localhost
  tasks:
    - name: Execute command 'date'
	  command: data
	  
	- name: Disply resolve.conf 
	  command: cat resolve.conf chdir=/etc
	  
	- name: Make folder
	  command: mkdir /folder creates=/folder
```
- chdir :: moved to path
- creates :: create something so that we know not to run again

# script #

``` yaml
- name: Play 1
  hosts: localhost
  tasks:
    - name: Run a script on remote server
	  script: /some/local/script.sh -arg1 -arg2
```


Handlers
========

restarts server when task is run
- makes sure server or service is restated after a task


``` yaml
- name: Deploy Application
  host: application_server
  tasks:
    - name: Copy Application Code
	  copy:
	    src: app_code/
		dest: /opt/application/
	  notify: Restart Application Service  # notify handler to restart the service
	  
  handlers:
    - name: restart Application Service
	  service:
	    name: application_servie
		state: restarted
```

Roles
=====

- think of asigning a role to a machine????

reusable playbooks???

https://galaxy.ansible.com/home
- take a look a some roles you could use


MYSQL-Role

``` yaml
tasks:
  - name: Install Pre-Requisites
    yum: name=pre-req0packages state=present
  
  - name: Install MySQL Package
    yum: name=mysql state=present
	
  - name: Start MySQL Service
    service: name=mysql state=started
	
  - name: Configure Database
    mysql_db: name=db1 state=present
```
- this goes in the roles folder

playbook that uses role

``` yaml
- name: Install and Configure MySQL
  hosts: db-server
  roles:
    - mysql
```

you can initialize your own role

`ansible-galaxy init mysql`
- make sure that you definel a folder call roles in the directory where you run your playbook


you can search for other peoples roles

`ansible-galaxy search <text>`

to install a role

`ansible-galaxy install <name-of-role>`

## to test a command ##

`ansible <host> -m ping`
just see if you can ping it

`ansible-inventory -i inventory.ini --list`
- verify inventory file

you may need to install
`sudo apt-get install sshpass`

if you just want to run adhoc commands
`ansible -i inventory.ini -a "echo hello world"`

run your playbook
`ansible-playbook -i inventory.ini playbook.yaml`
- if somethign got changed the `changed=1` should show
- also in unreachable=0 that means we connected


timezone.yml

``` yaml
---
  - name: Set timezone and configure timesync
    hosts: "*"
	become: yes
	tasks:
	  - name: Set timezone
	    shell: timedatectl set-timezone America/Chicago
		
	  - name: Make sure timesyncd is stopped
	    systemd:
		  name: systemd-timesyncd.service
		  state: stopped
		  
      - name: Copy over the timesyncd config
	    template: src=./timesyncd.conf dest=/etc/systemd/...
		
	  - name: Make sure timesyncd is started
	    systemd:
		  name: systemd-timesyncd.service
		  state: started
```


modules are small programs that do the actual work

`ansible-playbook -i inventory.ini playbook.yaml -K`
- the -K asks for password when you do a command that uses sudo

pass in extra variable
`vansible-playbook -i inventory.ini playbook.yaml -K --extra-vars "upgrade=true"`

`ansible-playbook -i ./inventory.ini playbook.yaml -v -t systemd -K`
