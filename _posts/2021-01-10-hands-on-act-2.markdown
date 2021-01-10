---
layout: post
title:  "Hands-on Activity 2: Install and configure your repository in remote Git in GitHub"
date:   2021-01-10 22:45:00 +0300
image:  hands-on-act-2-banner.jpg
tags:   [Prelim, Hands-on Activity]
---
## Objectives

Create a local directory and link it in a centralized repository in Github.

## Output

1) Create a github account and proceed on creating a profile repository (named as 'username').

![]({{site.baseurl}}/img/hands-on-2-1.png)

2) Clone repository.

{% highlight bash %}
localhost:~# git clone https://github.com/jpcabral-tip/jpcabral-tip.git
localhost:~# cd jpcabral-tip
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-2.png)

3) Create README.md inside repository.

{% highlight bash %}
localhost:~/jpcabral-tip# touch README.md
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-3.png)

4) Modified README.md.

{% highlight bash %}
localhost:~/jpcabral-tip# vim README.md
{% endhighlight %}
{% highlight html %}
<b>Year Level: </b>Third Year

<b>Interests: </b>Penetration Testing

<b>Email Address: </b>qjpccabral@tip.edu.ph

#### Computer Specs
*<b>CPU: </b>Ryzen 7 4800H (8 Cores 16 Threads)
*<b>RAM: </b>16 GB DDR4 3200 MHz
*<b>Disk: </b>SSD 256 GB, HDD 500 GB
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-4.png)

5) Commit and push to remote repository.

{% highlight bash %}
localhost:~/jpcabral-tip# git add README.md
localhost:~/jpcabral-tip# git commit -m "First commit"
localhost:~/jpcabral-tip# git push origin master
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-5.png)

6) Forked https://github.com/ajcanlas-tip/sysad2-12021.git repository.

![]({{site.baseurl}}/img/hands-on-2-6.png)

7) Clone forked repository.

{% highlight bash %}
localhost:~/jpcabral-tip# cd ~
localhost:~# git clone https://github.com/jpcabral-tip/sysad2-12021.git
localhost:~# cd sysad2-12021
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-3.png)

8) Clone forked repository.

{% highlight bash %}
localhost:~/jpcabral-tip# cd ~
localhost:~# git clone https://github.com/jpcabral-tip/sysad2-12021.git
localhost:~# cd sysad2-12021
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-7.png)

9) Created activity2 branch

{% highlight bash %}
localhost:~/sysad2-12021# git checkout master
localhost:~/sysad2-12021# git branch activity2
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-8.png)

10) Switched to activity2 branch

{% highlight bash %}
localhost:~/sysad2-12021# git checkout activity2
localhost:~/sysad2-12021# git status
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-9.png)

11) Added an upstream repository.

{% highlight bash %}
localhost:~/sysad2-12021# git remote add upstream https://github.com/ajcanlas-tip/sysad2-12021.git
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-10.png)

12) Copy previous README.md as HA2.md.

{% highlight bash %}
localhost:~/sysad2-12021# mkdir jpcabral-tip/
localhost:~/sysad2-12021# cd jpcabral-tip
localhost:~/sysad2-12021# mkdir activity2
localhost:~/sysad2-12021/activity2# cd activity2/
localhost:~/sysad2-12021/activity2# cp ~/jpcabral-tip/README.md HA2.md
localhost:~/sysad2-12021/activity2# ls
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-11.png)

13) Push to activity2 branch.

{% highlight bash %}
localhost:~/sysad2-12021/activity2# git add .
localhost:~/sysad2-12021/activity2# git commit -m "First commit for activity2"
localhost:~/sysad2-12021/activity2# git push origin activity2
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-2-12.png)

14) Open a pull request for the master branch of the upstream with the activity2 branch.

![]({{site.baseurl}}/img/hands-on-2-13.png)

15) The request can be viewed under the 'Pull requests' tab of the upstream repository.

![]({{site.baseurl}}/img/hands-on-2-14.png)