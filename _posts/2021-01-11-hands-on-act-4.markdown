---
layout: post
title:  "Hands-on Activity 4: Ansible Playbooks"
date:   2021-01-11 01:00:00 +0300
image:  hands-on-act-3-banner.png
tags:   [Prelim, Hands-on Activity]
---
This activity demonstrates the creation of a simple ansible playbook filled with various Ansible modules.

***

## Objectives

* Create Ansible Playbook to document the workflow.

***

## Tasks

1. Fork this repository https://github.com/ajcanlas-tip/sysad2-12021.git

2. Clone your newly forked repository. 

3. Make a new branch named "activity4" from master branch using git branch activity4 and git checkout activity4

    * Note: To Prevent Conflicts Create a directory with your username as its name and go inside of it, and create a directory called "activity4" and go inside it.

4. Make a new new remote upstream with git remote add upstream https://github.com/ajcanlas-tip/sysad2-12021.git

5. Create a playbook that install java via package manager , and install boto,ansible,and openstack py packages using pip in both Ubuntu and Centos.

7. add,commit and push it to your activity4 branch

8. Request a pull request for the master branch in https://github.com/ajcanlas-tip/sysad2-12021.git  and activity4 branch of your forked repository.

***

## Output

> jpcabral-tip/activity4/ansible.cfg
{% highlight cfg %}
[defaults]

# Basic Configuration
inventory = ./inventory
remote_user = jpcabral-tip
{% endhighlight %}

> jpcabral-tip/activity4/inventory.cfg
{% highlight cfg %}
[ubuntu]
192.168.254.115

[centos]
192.168.254.116
{% endhighlight %}

> jpcabral-tip/activity4/playbook.yaml
{% highlight yaml %}
---
  - name: Install Packages for Ubuntu
    hosts: ubuntu
    
    tasks:
    - name: Install Java
      apt:
        name: default-jre
        state: present
        update_cache: yes
  

  - name: Install Packages for Centos
    hosts: centos

    tasks:
    - name: Install Java
      yum:
        name: java-1.8.0-openjdk
        state: present
        update_cache: yes


  - name: Pip Install for All
    hosts: all

    tasks:
    - name: Install via ansible, boto, and openstack via pip
      pip:
        name:
          - ansible
          - boto
          - python-openstackclient
{% endhighlight %}


Execute using the following command to run the playbook:
{% highlight bash %}
localhost:~/jpcabral-tip/activity3# ansible-playbook playbook.yaml
{% endhighlight %}

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/activity4/jpcabral-tip/activity4">Github</a>.</p>