---
layout: post
title:  "Hands-on Activity 6: Install, configure, and manage enterprise services via Ansible"
date:   2021-01-11 12:45:00 +0300
image:  hands-on-act-6-banner.png
tags:   [Midterm, Hands-on Activity]
---
The activity showcases the use of ansible roles to optimize an Ansible playbook.

***

## Objectives

* Create a workflow that installs, configure, and manages enterprise services via Ansible being Infrastructure as code tool.

***

## Tasks

1. Fork this repository https://github.com/ajcanlas-tip/sysad2-12021.git
2. Clone your newly forked repository. 
3. Make a new branch named "activity6" from master branch using git branch activity6 and git checkout activity6
4. Make a new new remote upstream with git remote add upstream https://github.com/ajcanlas-tip/sysad2-12021.git
5. Create a playbook that installs dhcpd, bind9, vsftpd, samba, httpd, mariadb in both Ubuntu and Centos (use Roles to optimize the playbook)
6. Create different plays in installing per service and identify it as a group in Inventory file.
7. add,commit and push it to your activity6 branch
8. Request a pull request for the master branch in https://github.com/ajcanlas-tip/sysad2-12021.git  and activity6 branch of your forked repository.

***

## Output

> jpcabral-tip/activity6/ansible.cfg
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

> jpcabral-tip/activity6/inventory
{% highlight cfg %}
[webserver]
192.168.254.123
192.168.254.124

[ftpserver]
192.168.254.123
192.168.254.124


[dhcpserver]
192.168.254.123
192.168.254.124


[dbserver]
192.168.254.123
192.168.254.124


[dnsserver]
192.168.254.123
192.168.254.124


[sambashare]
192.168.254.123
192.168.254.124
{% endhighlight %}
<br>

> jpcabral-tip/activity6/config.yaml
{% highlight cfg %}
#APACHE/HTTPD
domain: jpcabral-tip
owner: jpcabral-tip
group: jpcabral-tip
ubuntu_src: jpcabral-tip-ubuntu.conf
centos_src: jpcabral-tip-centos.conf

#VSFTPD
name: ftpuser
password: $5$Xz2k7xh2IwRAOtda$X/yQSNPOlQ0Y7t4OdlLCPvrf6vb0DAZEaJfE5tBux.D
user_comment: "FTP User"
ubuntu_vsftpd: u-vsftpd.conf
centos_vsftpd: c-vsftpd.conf
#Note: The password is "password". Generated using mkpasswd with sha-512 as method.
{% endhighlight %}
<br>

> jpcabral-tip/activity6/playbook.yaml
{% highlight yaml+jinja %}
---
  - name: Install and configyre Apache/HTTPD
    hosts: webserver

    tasks:
    - name: Include config.yaml variables
      include_vars:
        file: config.yaml

    - name: Install Apache using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: apache2
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install HTTPD using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package: httpd
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Configure Apache2 using configureapacheubuntu role
      include_role:
        name: configureapacheubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Configure HTTPD using configurehttpdcentos role
      include_role:
        name: configurehttpdcentos
      when: ansible_facts['os_family'] == 'RedHat'


  - name: Install and configure VSFTPD
    hosts: ftpserver

    tasks:
    - name: Include config.yaml variables
      include_vars:
        file: config.yaml

    - name: Install VSFTPD using using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: vsftpd
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install HTTPD using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package: vsftpd
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Stop UFW on ubuntu using stopfirewallubuntu role
      include_role:
        name: stopfirewallubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Stop FirewallD on CentOS using stopfirewallcentos role
      include_role:
        name: stopfirewallcentos
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Configure VSFTPD on Ubuntu using configurevsftpdubuntu
      include_role:
        name: configurevsftpdubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Configure VSFTPD on CentOS uisng configurevsftpcentos
      include_role:
        name: configurevsftpcentos
      when: ansible_facts['os_family'] == 'RedHat'


  - name: Install and configure DHCPD
    hosts: dhcpserver

    tasks:
    - name: Install isc-dhcp-server using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: isc-dhcp-server
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install dhcp-server using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package: dhcp-server
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Stop Firewall on Ubuntu
      include_role:
        name: stopfirewallubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Stop Firewall on CentOS
      include_role:
        name: stopfirewallcentos
      when: ansible_facts['os_family'] == 'RedHat'


  - name: Install MariaDB
    hosts: dbserver

    tasks:
    - name: Install MariaDB using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: mariadb-server
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install MariaDB using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package: mariadb-server
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Stop Firewall on Ubuntu
      include_role:
        name: stopfirewallubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Stop Firewall on CentOS
      include_role:
        name: stopfirewallcentos
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Start MariaDB on Ubuntu using mariadbubuntu role
      include_role:
        name: mariadbubuntu
      when: ansible_facts['os_family'] == 'Debian'
    
    - name: Start MariaDB on CentOS using mariadbcentos role
      include_role:
        name: mariadbcentos
      when: ansible_facts['os_family'] == 'RedHat'


  - name: Install Bind9
    hosts: dnsserver

    tasks:
    - name: Install Bind9 using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: bind9
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install Bind9 using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package:
          - bind
          - bind-utils
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Stop Firewall on Ubuntu
      include_role:
        name: stopfirewallubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Stop Firewall on CentOS
      include_role:
        name: stopfirewallcentos
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Start Bind9 on Ubuntu using bind9ubuntu role
      include_role:
        name: bind9ubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Start Bind on CentOS using bindcentos role
      include_role:
        name: bindcentos
      when: ansible_facts['os_family'] == 'RedHat'

      
  - name: Install Samba
    hosts: sambashare

    tasks:
    - name: Install Samba using installpackageubuntu role
      include_role:
        name: installpackageubuntu
      vars:
        package: samba
      when: ansible_facts['os_family'] == 'Debian'

    - name: Install Samba using installpackagecentos role
      include_role:
        name: installpackagecentos
      vars:
        package:
          - samba
          - samba-common
          - samba-client
      when: ansible_facts['os_family'] == 'RedHat'

    - name: Stop Firewall on Ubuntu
      include_role:
        name: stopfirewallubuntu
      when: ansible_facts['os_family'] == 'Debian'

    - name: Stop Firewall on CentOS
      include_role:
        name: stopfirewallcentos
      when: ansible_facts['os_family'] == 'RedHat'
    
    - name: Start Samba on Ubuntu using sambaubuntu role
      include_role:
        name: sambaubuntu
      when: ansible_facts['os_family'] == 'Debian'
    
    - name: Start Samba on CentOS using sambacentos role
      include_role:
        name: sambacentos
      when: ansible_facts['os_family'] == 'RedHat'
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/bind9ubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/bind9ubuntu
- name: Start Bind9
  service:
    name: bind9
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/bindcentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/bindcentos
- name: Start named service
  service:
    name: named
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/configureapacheubutu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/configureapacheubuntu
- name: Stop Firewall
  service:
    name: ufw
    state: stopped
    enabled: no

- name: Start and enable Apache service
  service:
    name: apache2
    state: started
    enabled: yes

- name: Create /var/www/domain/ for domain config
  file:
    path: "/var/www/{{ domain }}"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Copy conf file to /etc/apache2/sites-available
  copy:
    src: "{{ ubuntu_src }}"
    dest: "/etc/apache2/sites-available/{{ domain }}.conf"
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'
  register: result

- name: Set new virtual host above default
  shell: "a2ensite {{ domain }}; a2dissite 000-default"
  when: result.changed

- name: Copy index.html
  copy:
    src: index.html
    dest: "/var/www/{{ domain }}/index.html"
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'
  register: index_html

- name: Restart Apache2
  service:
    name: apache2
    state: restarted
  when: index_html.changed
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/configurehttpdcentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/configurehttpdcentos
- name: Stop Firewall
  service:
    name: firewalld
    state: stopped
    enabled: no

- name: Start and enable HTTPD service
  service:
    name: httpd
    state: started
    enabled: yes

- name: Create /var/www/domain/ for domain config
  file:
    path: "/var/www/{{ domain }}"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Create /var/www/domain/html for domain config
  file:
    path: "/var/www/{{ domain }}/html"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Create /var/www/domain/log for domain config
  file:
    path: "/var/www/{{ domain }}/log"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Copy index.html
  copy:
    src: index.html
    dest: "/var/www/{{ domain }}/html/index.html"
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'
  register: index_html

- name: Create /etc/httpd/sites-available
  file:
    path: "/etc/httpd/sites-available"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Create /etc/httpd/sites-enabled
  file:
    path: "/etc/httpd/sites-enabled"
    state: directory
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'

- name: Append Optional files option on httpd.conf
  lineinfile:
    path: /etc/httpd/conf/httpd.conf
    line: "IncludeOptional sites-enabled/*.conf"
  register: httpd_conf

- name: Copy conf file to /etc/httpd/sites-available/
  copy:
    src: "{{ centos_src }}"
    dest: "/etc/httpd/sites-available/{{ domain }}.conf"
    owner: "{{ owner }}"
    group: "{{ group }}"
    mode: '0755'
  register: result

- name: Serve virtual host
  file:
    src: "/etc/httpd/sites-available/{{ domain }}.conf"
    dest: "/etc/httpd/sites-enabled/{{ domain }}.conf"
    state: link

- name: Install python-utils for semanage
  yum:
    name: policycoreutils-python-utils
    state: latest
    update_cache: yes

- name: Adjust apache policies
  seboolean:
    name: httpd_unified
    state: yes
    persistent: yes

- name: Restart httpd
  service:
    name: httpd
    state: restarted
  when: index_html.changed or result.changed
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/configurevsftpdcentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/configurevsftpcentos
- name: Start and Enable vsftpd
  service:
    name: vsftpd
    state: started
    enabled: yes

- name: Create FTP User
  user:
    name: "{{ name }}"
    password: "{{ password }}"
    comment: "{{ user_comment }}"

- name: Create FTP directory
  file:
    path: "/home/{{ name }}/ftp"
    state: directory
    owner: "{{ name }}"
    group: "{{ name }}"
    mode: '0755'

- name: Add FTP user to user_list
  lineinfile:
    path: /etc/vsftpd/user_list
    line: "{{ name }}"
  register: user_list

- name: Copy vsftpd.conf to /etc/vsftpd/vsftpd.conf
  copy:
    src: "{{ centos_vsftpd }}"
    dest: /etc/vsftpd/vsftpd.conf
  register: vsftpd_conf

- name: Restart VSFTPD
  service:
    name: vsftpd
    state: restarted
  when: user_list.changed or vsftpd_conf.changed
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/configurevsftpdubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/configurevsftpdubuntu
- name: Start and enable VSFTPD
  service:
    name: vsftpd
    state: started
    enabled: yes

- name: Create FTP User
  user:
    name: "{{ name }}"
    password: "{{ password }}"
    comment: "{{ user_comment }}"

- name: Add Deny in /etc/ssh/sshd_config
  lineinfile:
    path: /etc/ssh/sshd_config
    line: "DenyUsers {{ name }}"
  register: sshd_config

- name: Restart SSHD service
  service:
    name: sshd
    state: restarted
  when: sshd_config.changed

- name: Create FTP directory
  file:
    path: "/home/{{ name }}/ftp"
    state: directory
    owner: nobody
    group: nogroup
    mode: '0555'

- name: Create files directory
  file:
    path: "/home/{{ name }}/ftp/files"
    state: directory
    owner: "{{ name }}"
    group: "{{ name }}"

- name: Copy vsftpd.conf to /etc/vsftpd.conf
  copy:
    src: "{{ ubuntu_vsftpd }}"
    dest: /etc/vsftpd.conf
  register: vsftpd_conf

- name: Restart VSFTPD
  service:
    name: vsftpd
    state: restarted
  when: vsftpd_conf.changed
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/installpackagecentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/installpackagecentos
- name: Install Package using dnf
  yum:
    name: "{{ package }}"
    state: latest
    update_cache: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/installpackageubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/installpackageubuntu
- name: Install package
  apt:
    name: "{{ package }}"
    state: latest
    update_cache: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/mariadbcentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/mariadbcentos
- name: Start MariaDB
  service:
    name: mariadb
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/mariadbubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/mariadbubuntu
- name: Start MariaDB
  service:
    name: mariadb
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/sambacentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/sambacentos
- name: Start samba
  service:
    name: smb
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/sambaubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/sambaubuntu
- name: Start Samba
  service:
    name: smbd
    state: started
    enabled: yes
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/stopfirewallcentos/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/stopfirewallcentos
- name: Stop FirewallD
  service:
    name: firewalld
    state: stopped
    enabled: no
{% endhighlight %}
<br>

> jpcabral-tip/activity6/roles/stopfirewallubuntu/tasks/main.yml
{% highlight yaml+jinja %}
---
# tasks file for roles/stopfirewallubuntu
- name: Stop Firewall
  service:
    name: ufw
    state: stopped
    enabled: no
{% endhighlight %}
<br>

> jpcabral-tip/activity6/files/c-vsftpd.conf
{% highlight aconf %}
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
chroot_local_user=YES
allow_writeable_chroot=YES
listen=NO
listen_ipv6=YES
pasv_min_port=30000
pasv_max_port=31000
userlist_file=/etc/vsftpd/user_list
userlist_deny=NO
pam_service_name=vsftpd
userlist_enable=YES
{% endhighlight %}
<br>

> jpcabral-tip/activity6/files/index.html
{% highlight html %}
<html>
	<head>
		<title>jpcabral-tip</title>
	</head>
	<body>
		<h1>Welcome!</h1>

		<p>This is the landing page of <strong>jpcabral-tip</strong>.</p>
	</body>
</html>
{% endhighlight %}
<br>

> jpcabral-tip/activity6/files/jpcabral-tip-centos.conf
{% highlight aconf %}
<VirtualHost *:80>
	ServerName www.jpcabral-tip
	ServerAlias jpcabral-tip
	DocumentRoot /var/www/jpcabral-tip/html
	ErrorLog /var/www/jpcabral-tip/log/error.log
	CustomLog /var/www/jpcabral-tip/log/requests.log combined
</VirtualHost>
{% endhighlight %}
<br>

> jpcabral-tip/activity6/files/jpcabral-tip-ubuntu.conf
{% highlight aconf %}
<VirtualHost *:80>
	ServerName jpcabral-tip
	ServerAlias www.jpcabral-tip
	DocumentRoot /var/www/jpcabral-tip
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
{% endhighlight %}
<br>

> jpcabral-tip/activity6/files/u-vsftpd.conf
{% highlight aconf %}
listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
force_dot_files=YES
pasv_min_port=40000
pasv_max_port=50000
user_sub_token=$USER
local_root=/home/$USER/ftp
{% endhighlight %}
<br>

Execute using the following command to run the playbook:
{% highlight bash %}
localhost:~/jpcabral-tip/activity6# ansible-playbook playbook.yaml
{% endhighlight %}
<br>

<p>As seen on <a href="https://github.com/jpcabral-tip/sysad2-12021/tree/activity6">Github</a>.</p>