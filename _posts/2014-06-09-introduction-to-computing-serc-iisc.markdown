---
author: Rajesh Venkatesan
comments: true
date: 2014-06-09 09:11:12+00:00
layout: post
link: http://flowphysics.com/introduction-to-computing-serc-iisc/
slug: introduction-to-computing-serc-iisc
title: Introduction to computing @ SERC, IISc
wordpress_id: 48
categories:
- SERC Series
tags:
- Fortran
- IISc
- OpenMP
- Parallel Computing
- SERC
---

The Supercomputer Education and Research Centre (SERC) at Indian Institute of Science (IISc) is a large computing facility available to the students of the Institute. But many students face the problem of not knowing where to start and how to go about things to perform computations there. Also I found that the [SERC website](http://www.serc.iisc.ernet.in) is not clear enough to get a newbie started with performing computations there. So I provide below a beginner's guide to SERC.

**Administrative Steps:**

  1. First you have to obtain a username and password to access (all) systems at SERC. You can go to SERC library (Room No. 103) and obtain the application form. In that form, you have to get your advisor's signature and mention the clusters you want access to. In the beginning you will be given access to basic clusters (the best among them is Tyrone Cluster). Later on, depending on your needs, you can try applying for other IBM clusters or the supercomputer IBM Blue Gene/L.

  2. Once you receive an email confirming your access to the cluster, you can log onto that system in the following way. Assuming you are working on a linux system connected to LAN inside the campus, all you have to do is to open a Terminal (Ctrl+Alt+T) and enter the following command:

{% highlight shell %}
$ ssh username@10.**.**.**
{% endhighlight %}

where you have to replace the username by your username and 10.**.**.** by the IP address of the cluster (both provided to you by SERC in the acceptance e-mail). Then the system will prompt you for your password. After this you will be accessing the cluster from the command prompt. So, you dont have to go to SERC to do computations there. A great thing if you are in the other end of the campus.

(Note: The above command tries to connect to the cluster (with the given IP address) with your username using ssh protocol. By default, almost all linux systems will have ssh installed. Otherwise you can install it manually by, in ubuntu,

{% highlight shell %}
$ sudo apt-get install ssh
{% endhighlight %}

For windows, there are freely available SSH shells which can be downloaded from the Internet.)

—–>      [Next Article in this Series](http://flowphysics.com/file-system-structure-serc/)
