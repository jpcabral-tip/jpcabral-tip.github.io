---
layout: post
title:  "Hands-on Activity 3: Ansible Basics"
date:   2021-01-11 00:15:00 +0300
image:  hands-on-act-3-banner.png
tags:   [Prelim, Hands-on Activity]
---
The following activity presents the use of Ansible as an automation tool via the execution of Adhoc commands. Usage of basic Ansible modules such as the 'shell' and 'copy' modules are also included.

***

## Objectives

* Create Ansible configuration, Inventory and execute and Adhoc command of Ansible.

***

## Tasks

Setup is Alpine and Ubuntu VMs are connected via a local network, in my case
Alpine VM, IP is 192.168.122.117 (Or Any VM that has Git installed)
Ubuntu VM,IP is 192.168.122.125 and hostname should be ubuntu-(your student number) (e.g. ubuntu-1110670)

1. Fork the instructor repository and clone the repository.
2. Create a new branch called "activity3" and checkout in that branch.
3. Create a directory with your username as its name and go inside of it, and create a directory called "activity3" and go inside it.
4. Install Ansible via apk in your Alpine VM.
5. To check if you installed Ansible correctly check the python version it in Ansible version command would reflect python 3.
6. First, create a configuration file named "ansible.cfg", Ansible checks per-directory thus you can have multiple configurations in a system depending on your configuration file in the current directory.
7. Next, populate the ansible.cfg file with basic information such as inventory, host verification, and your default remote user.
8. Next, create your first inventory file with Ubuntu IP on it.
9. Then try to connect using "ping" module via adhoc command
10. Now use "shell" module to check the user via bash commands. (check for the user, hostname, and id)
11. And we can use the "copy" module to create a file with a certain file with the content or copy from your local VM to your remote VM.
12. For more modules, you can check this link (Links to an external site.) of the module index.
13. You can also check the description of the module using ansible-doc 
14. Also, you can group your inventory via the grouping feature of Ansible.
15. Add commit push and create a PR.

***

## Output

> jpcabral-tip/activity3/ansible.cfg
{% highlight cfg %}
[defaults]

# Basic Configuration
inventory = ./inventory
remote_user = jpcabral-tip
{% endhighlight %}

> jpcabral-tip/activity3/inventory
{% highlight cfg %}
[ubuntu]
192.168.254.115
{% endhighlight %}

> jpcabral-tip/activity3/COPY_THIS_FILE
{% highlight cfg %}
This file is copied from jpcabral-tip's Alphine VM using ansible via adhoc command using the "copy" module.

--Below are the responses from executing the adhoc commands--
192.168.254.115 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
192.168.254.115 | CHANGED | rc=0 >>
jpcabral-tip
192.168.254.115 | CHANGED | rc=0 >>
ubuntu-1811023
192.168.254.115 | CHANGED | rc=0 >>
uid=1000(jpcabral-tip) gid=1000(jpcabral-tip) groups=1000(jpcabral-tip),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lxd)
192.168.254.115 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,
    "checksum": "f9abe53aa4580bef300f9a06fc0570721c96b4cf",
    "dest": "/home/jpcabral-tip/COPY_THIS_FILE",
    "gid": 1000,
    "group": "jpcabral-tip",
    "md5sum": "ed479ef0ed69c7aa35f4919434d001ef",
    "mode": "0664",
    "owner": "jpcabral-tip",
    "size": 598,
    "src": "/home/jpcabral-tip/.ansible/tmp/ansible-tmp-1607405195.6496644-3497-26228768185583/source",
    "state": "file",
    "uid": 1000
}
{% endhighlight %}

Execute via Ansible Adhoc using the following command:
{% highlight bash %}
localhost:~/jpcabral-tip/activity3# ansible ubuntu -m copy -a "src: COPY_THIS_FILE dest: ~"
{% endhighlight %}

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/activity3/">Github</a>.</p>