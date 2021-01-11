---
layout: post
title:  "Hands-on Activity 5: Implement Ansible roles in playbooks"
date:   2021-01-11 01:00:00 +0300
image:  hands-on-act-5-banner.png
tags:   [Prelim, Hands-on Activity]
---
The activity showcases the use of ansible roles to optimize an Ansible playbook.

***

## Objectives

* Create an Ansible Playbook that uses Ansible roles as optimization.

***

## Tasks

1. Fork this repository https://github.com/ajcanlas-tip/sysad2-12021.git
2. Clone your newly forked repository. 
3. Make a new branch named "activity5" from master branch using git branch activity5 and git checkout activity5
    * Note: To Prevent Conflicts Create a directory with your username as its name and go inside of it, and create a directory called "activity5" and go inside it.
4. Make a new new remote upstream with git remote add upstream https://github.com/ajcanlas-tip/sysad2-12021.git
5. Optimize the playbook in Hands-on Activity 4: Ansible Playbooks
7. add,commit and push it to your activity5 branch
8. Request a pull request for the master branch in https://github.com/ajcanlas-tip/sysad2-12021.git  and activity5 branch of your forked repository.

***

## Output

> jpcabral-tip/activity5/ansible.cfg
{% highlight cfg %}
[defaults]

# Basic Configuration
inventory = ./inventory
remote_user = jpcabral-tip
private_key_file = ./private.key

# Priviege Escalation
[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
{% endhighlight %}
<br>

> jpcabral-tip/activity5/inventory
{% highlight cfg %}
[ubuntu]
192.168.254.115

[centos]
192.168.254.116
{% endhighlight %}
<br>

> jpcabral-tip/activity5/playbook.yaml
{% highlight yaml+jinja %}
---
  - name: Install Packages for Ubuntu
    hosts: ubuntu
    
    tasks:
    - name: Install Java via installpackages-ubuntu role
      include_role:
        name: installpackages-ubuntu
      vars:
        package: "default-jre"
  

  - name: Install Packages for Centos
    hosts: centos

    tasks:
    - name: Install Java via installpackages-centos role
      include_role:
        name: installpackages-centos
      vars:
        package: "java-1.8.0-openjdk"
       

  - name: Pip Install for All
    hosts: all

    tasks:
    - name: Install ansible, boto, and openstack via pipinstall role
      include_role:
        name: pipinstall
      vars:
        pippackages:
          - ansible
          - boto
          - python-openstackclient
{% endhighlight %}
<br>

> jpcabral-tip/activity5/roles/installpackages-centos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/installpackages-centos
- name: Install Package
  yum:
    name: "{{ package }}"
    state: present
    update_cache: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity5/roles/installpackages-ubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/installpackages-ubuntu
- name: Install Package
  apt:
    name: "{{ package }}"
    state: present
    update_cache: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity5/roles/pipinstall/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/pipinstall
- name: Pip Install Packages
  pip:
    name: "{{ pippackages }}"
{% endhighlight %}
<br>

Execute using the following command to run the playbook:
{% highlight bash %}
localhost:~/jpcabral-tip/activity5# ansible-playbook playbook.yaml
{% endhighlight %}
<br>

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/activity5">Github</a>.</p>