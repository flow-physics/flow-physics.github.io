---
author: Rajesh Venkatesan
comments: true
date: 2014-06-19 13:25:57+00:00
layout: post
link: http://flowphysics.com/file-system-structure-serc/
slug: file-system-structure-serc
title: File system structure in SERC
wordpress_id: 83
categories:
- SERC Series
tags:
- IISc
- Parallel Computing
- SERC
---

In this article, we shall see the basic file system structure used in SERC.   You would have known by now that when it comes to scientific computing, the only operating system used is Linux (or some UNIX based variants).  SERC systems generally have some Linux OS (e.g. Tyrone cluster has CentOS) installed in them.   First you should log into one of the clusters using the following command:

{% highlight shell %}
ssh -X username@ip_address_of_cluster
{% endhighlight %}

Note that the flag -X is optional.  If you leave it out, you will only be able to access the cluster using the command line.  The flag -X allows x-windows forwarding, which means, literally being able to see the desktop of that cluster.  We shall see more about this option later.  For now, even leaving out the flag is okay.

Once you are logged in to the cluster, you will see a prompt like the following, in case of Tyrone cluster,

{% highlight shell %}
username@tyrone-cluster:~$
{% endhighlight %}

A cluster like Tyrone generally consists of many nodes which you can imagine to be separate computer units. And these nodes will be connected using some high-speed data transfer device. Within each node, there will be many processors. To give an example situation, imagine four computers with quad-core processors connected using some high-speed device. In this case, there are four nodes and each node has 4 cores or processors. Note that the processors within each node share the same physical resources of that node, like physical memory. But a processor within a given node does _not_ have direct access to the memory of another node. So we need to manually enable communication between different nodes by data transfer using the high-speed connectivity device.

When you log into the Tyrone cluster, you are actually logging into the so-called _head node_ of that cluster. This is where all other students will also log in and submit jobs for their computation in the cluster. SERC has many clusters and supercomputers. But there is a main file system (or Hard Disk Drive) connected to all clusters. If you type the command pwd in the terminal, you will see the following:

{% highlight shell %}
username@tyrone-cluster:~$ pwd
/home/course/year/username
username@tyrone-cluster:~$
{% endhighlight %}

You will see msc/phd instead of _course_ and your year of joining instead of _year_ in the above code listing.  No matter which cluster you log into, this will be the working directory you get in first.  This is your HOME folder.  Here you will have your Documents, Downloads, etc. You have all permissions in this directory  and other students can only see your files.  You can create a directory for your code here and submit it for computation in Tyrone cluster. The files you create here will be accessible even when you log into other clusters at SERC.  But for running in a particular cluster, you have to submit the job after logging into that cluster only.

This $HOME folder gives you enough space to keep your codes and other files. But for storing the results from your large computations, SERC has created a separate file storage mounted at _/localscratch/username_. You may need to call/email SERC and ask them to enable this storage space for your username. Note that this local scratch space is private to each cluster. So, data saved in the local  scratch of Tyrone cluster will not be accessible to computations in Dell cluster.  Also SERC routinely deletes files in the local scratch space after 10 days or so. So you need to transfer your result files to your computer once they are obtained.

Small serial programs (Text editing, debugging,etc.) which do not use much resources can be run directly from the head node. But any serious parallel computation must be submitted into the queue. We shall see how to do that in another post.

P.S:

If you have logged into the cluster with the -X flag, then typing the _gedit_ command in the terminal will open the gedit text editor. Also you can type the command _nautilus _in the terminal and get the nautilus file browser GUI.

----->    [Previous Article in this series](http://flowphysics.com/introduction-to-computing-serc-iisc/)

----->    [Next Article in this series](http://flowphysics.com/environment-variables-and-path/)
