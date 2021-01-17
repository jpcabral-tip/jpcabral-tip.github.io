---
layout: post
title:  "Hands-on Activity 8: Containerization"
date:   2021-01-17 17:45:00 +0300
image:  hands-on-act-8-banner.png
tags:   [Finals, Hands-on Activity]
---
The activity demonstrates the use of Ansible to automate conteinerization through building a Dockerfile using ansible. The activity demonstrates this aspect with the installation and configuration of LAMP stack in a conteinerzied environment.

***

## Objectives

* Create a Dockerfile and form a workflow using Ansible as Infrastructure as Code (IaC) to enable Continuous Delivery process.

***

## Tasks

1. Fork this repository https://github.com/ajcanlas-tip/sysad2-12021.git
2. Clone your newly forked repository. 
3. Make a new branch named "activity8" from master branch using git branch activity8 and git checkout activity8
4. Make a new remote upstream with git remote add upstreamhttps://github.com/ajcanlas-tip/sysad2-12021.git
5. Install Docker and enable the docker socket.
6. Add to docker group to your current user.
7. Create a Dockerfile to install web and db server
8. Install and build the Dockerfile using Ansible.
9. add,commit and push it to your activity8 branch
10.  Request a pull request for the master branch in https://github.com/ajcanlas-tip/sysad2-12021.git  and activity8 branch of your forked repository

***

## Output

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/activity8">Github</a>.</p>

{% highlight cfg %}
activity8/
├── ansible.cfg
├── config.yaml
├── docker
│   ├── db
│   │   └── Dockerfile
│   └── web
│       ├── Dockerfile
│       ├── index.html
│       └── info.php
├── files
│   └── docker-ce.repo
├── inventory
├── playbook.yaml
└── roles
    ├── dockergrp
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── installpackagecentos
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── installpackageubuntu
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── lampstackdocker
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── pipinstall
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── stopfirewallcentos
    │   ├── README.md
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    └── stopfirewallubuntu
        ├── README.md
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml

68 directories, 65 files
{% endhighlight %}