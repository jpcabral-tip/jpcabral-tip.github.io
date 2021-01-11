---
layout: post
title:  "Hands-on Activity 1: Install and configure your repository in Local Git"
date:   2021-01-10 22:03:00 +0300
image:  hands-on-act-1-banner.png
tags:   [Prelim, Hands-on Activity]
---
This activity demonstrates how to create a local git repository. It also discusses basic tracking of files through commits.

***

## Objectives

* Create a git repository locally and emphasize the use of Git in your local project in a CI/CD environment.

***

## Output

1) Install bash and vim.

{% highlight bash %}
localhost:~# apk add bash
localhost:~# apk add vim
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-1.png)
<br>

2) Install git.

{% highlight bash %}
localhost:~# apk add git
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-2.png)
<br>

3) Create activity1 directory.

{% highlight bash %}
localhost:~# mkdir activity1
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-3.png)
<br>

4) Initialize activity1 as git repository.

{% highlight bash %}
localhost:~# cd activity1/
localhost:~/activity1# git init
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-4.png)
<br>

5) Created and added README.md.

{% highlight bash %}
localhost:~/activity1# git add README.md
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-5.png)
<br>

6) Commit README.md on the master branch.

{% highlight bash %}
localhost:~/activity1# git commit -m "First Commit"
{% endhighlight %}
![]({{site.baseurl}}/img/hands-on-1-6.png)
<br>