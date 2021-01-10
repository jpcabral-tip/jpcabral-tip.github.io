---
layout: post
title:  "Hands-on Prelim Exam"
date:   2021-01-11 1:50:00 +0300
image:  prelim-exam.png
tags:   [Prelim, Exam]
---
## Tasks

1. Fork this repository https://github.com/ajcanlas-tip/sysad2-12021.git (Links to an external site.)
2. Clone your new repository in your VM https://github.com/< your username >/sysad2-12021.git
3. Create a branch named "prelim-exam" and checkout in that branch. 
4. Create an Ansible playbook that does the following with an input of a config.yaml file
	* Role 1 (python):
		1. Installs the latest python3 and pip3
		2. use pip3 as default pip 
		3. use python3 as default python 
	* Role 2 (Java)
		1. Install Java open-jdk
	* Role 3 (Change motd)
		1. Create Motd containing the text defined by a variable defined in config.yaml file and if there is no variable input the default motd is "Ansible Managed node by (your user name)"
	* Role 4 (Create user)
		1. Create a user with a variable defined in config.yaml
5. push and commit your prelim-exam branch in the VM (no need for ansible.cfg and inventory upon pushing)
6. request a pull request from that branch in GitHub
7. For your prelim exam to be counted, please paste your repository link as an answer in this exam.

***

## Output

> 1811023/prelim-exam/config.yml
{% highlight yaml %}
motd: "Comment this line to change to default motd"
user: "sample"
{% endhighlight %}

> 1811023/prelim-exam/roles/changemotd/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/changemotd
# Note: restart is needed for the motd changes to take effect
- name: Change Message of the Day
  copy:
    content: "{{ motd }} \n"
    dest: /etc/motd

- name: Disable default motd
  file:
    dest: "/etc/update-motd.d/"
    mode: "u-x,g-x,o-x"
    state: directory
    recurse: yes

  #ALTERNATIVE
  #shell: "chmod -x /etc/update-motd.d/*"
{% endhighlight %}

> 1811023/prelim-exam/roles/createuser/tasks/main.yml
{% highlight yaml %}
---
# tasks file for roles/createuser
- name: Create user
  user:
    name: "{{ user }}"
    comment: "{{ user }}"
    shell: /bin/bash
{% endhighlight %}

> 1811023/prelim-exam/roles/java/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/java
- name: Install Java open-jdk
  apt:
    name: default-jre
    state: present
    update_cache: yes
{% endhighlight %}

> 1811023/prelim-exam/roles/python/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/python
- name: Install latest python3 and pip3
  apt:
    name:
      - python3
      - python3-pip
    state: present
    update_cache: yes

- name: Use pip3 as default pip
  shell: update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

- name: Use python3 as default python
  shell: update-alternatives --install /usr/bin/python python /usr/bin/python3 10
{% endhighlight %}

> 1811023/prelim-exam/playbook.yaml
{% highlight yaml+jinja %}
---
  - name: Prelim Exam
    hosts: ubuntu
    
    tasks:
    - name: Include config.yaml variables
      include_vars:
        file: config.yaml

    - name: Role 1 (python)
      include_role:
        name: python

    - name: Role 2 (java)
      include_role:
        name: java

    - name: Role 3 (change motd)
      include_role:
        name: changemotd

    - name: Role 4 (create user)
      include_role:
        name: createuser
{% endhighlight %}

> 1811023/prelim-exam/README.md
{% highlight html %}

# Directory Summary

**Author:** Jose Paulo Cabral

## Prequisites

* Ansible (installed on local machine)
* SSH (installed on both local and remote machine/s)
* Target Machine: Ubuntu 20.04 Server Edition LTS

## Requirements

* SSH private key file for authentication placed on working directory.
* Ansible configuration file.
* Inventory file.

## Directory Structure

```
prelim-exam
	roles/
		changemotd/
			defaults/
				main.yml
			files/
			handlers/
				main.yml
			meta/
				main.yml
			tasks/
				main.yml
			templates/
			tests/
				inventory
				test.yml
			vars/
				main.yml
			README.md
		createduser/
			defaults/
				main.yml
			files/
			handlers/
				main.yml
			meta/
				main.yml
			tasks/
				main.yml
			templates/
			tests/
				inventory
				test.yml
			vars/
				main.yml
			README.md
		java/
			defaults/
				main.yml
			files/
			handlers/
				main.yml
			meta/
				main.yml
			tasks/
				main.yml
			templates/
			tests/
				inventory
				test.yml
			vars/
				main.yml
			README.md
		python/
			defaults/
				main.yml
			files/
			handlers/
				main.yml
			meta/
				main.yml
			tasks/
				main.yml
			templates/
			tests/
				inventory
				test.yml
			vars/
				main.yml
			README.md
ansible.cfg*
config.yaml
inventory*
playbook.yaml
private.key*
.gitignore
README.md
```

Note: Files marked with asterisk (*) at the end are declared inside ``.gitignore``.

## Content Structure for Files Declared in .gitgnore
* ``private.key``
	The localmachine generated SSH private key (named ``id_rsa`` by default inside ``~/.ssh/``
* ``ansible.cfg``
	Format for the configuration file can be seen as follows:
	```
	[defaults]
	
	inventory = ./inventory
	remote_user = <REMOTE_USER>
	private_key_file = ./<PRIVATE_KEY_FILE_FOR_KEY_BASED_AUTHENTICATION>

	[privilege_escalation]
	become = True
	become_method = sudo
	become_user = root
	become_ask_pass = False
	```
* ``inventory``
	The file name itself is inventory. If changed, redeclare the file name or path as value on the ``ansible.cfg`` file inside the ``inventory`` variable. An example format is presented as follows:
	```
	[<GROUP_NAME (OPTIONAL)>]
	<REMOTE HOST/S IP>
	```
	Ex.
	```
	[ubuntu]
	192.168.254.69
	```
{% endhighlight %}

Execute using the following command to run the playbook:
{% highlight bash %}
localhost:~/1811023/prelim-exam# ansible-playbook playbook.yaml
{% endhighlight %}

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/prelim-exam/">Github</a>.</p>