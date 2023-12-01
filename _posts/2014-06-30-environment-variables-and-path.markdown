---
author: Rajesh Venkatesan
comments: true
date: 2014-06-30 19:02:25+00:00
layout: post
link: http://flowphysics.com/environment-variables-and-path/
slug: environment-variables-and-path
title: Set Environment variables and Path in SERC
wordpress_id: 141
categories:
- SERC Series
tags:
- IISc
- linux
- SERC
---

There are many [environment variables](http://en.wikipedia.org/wiki/Environment_variable) used by programs in Linux. For example, the environment variable `HOME` points to the home folder of the user. To see the usage, try :

{% highlight shell %}
username@tyrone-cluster:~$ echo $HOME
/home/course/year/username
{% endhighlight %}

This shows your home directory. `PATH` is another important environment variable which sets the locations in which programs search for the required files/executables.

In SERC, many programs (like compilers, matlab, etc.) are installed in various folders. We need to actually go to these folders and then run the desired programs which is a messy task. Instead, we can specify the locations of these executables in a configuration file so that the system automatically looks into these places when we try to, say, compile a program. There are many other uses of setting environment variables. But which variables to set depends on the requirements of the user. Let us see how to do this in SERC clusters.

First you need to find out which shell you are using in the terminal by the command,

{% highlight shell %}
username@tyrone-cluster:~$ echo $0
csh
{% endhighlight %}

The above indicates that you are using the C shell. Some Linux systems may be using the bash shell as default. Whichever shell you are using, there is a configuration file which sets up the environmental variables and default paths for that shell. For C shell, this will located in `~/.cshrc` and, for bash shell, this will be `~/.bashrc`. Note that `~` indicates your home directory (/home/course/year/username).

Steps to modify the `.cshrc/.bashrc` file:

1. Open the file in a text editor: (you can use vi or gedit)

{% highlight shell %}
username@tyrone-cluster:~$ vi ~/.cshrc
{% endhighlight %}

  1. Insert the appropriate paths and environment variables for your required software. I have given the most used ones at the end of this post.

  2. Save the file and close.

  3. You have to source that file for the changes to take effect. Do:

{% highlight shell %}
username@tyrone-cluster:~$ source ~/.cshrc
{% endhighlight %}

Add the following lines to the ~/.cshrc file for the corresponding software mentioned.

**For Tyrone cluster:**
 Torque batch system:
You must add this to submit any job in Tyrone cluster.

{% highlight shell %}
set path=(/opt/toque-305/bin $path)
set path=(/opt/toque-305/sbin $path)
{% endhighlight %}

Intel Fortran compiler:

{% highlight shell %}
source /opt/intel/composerxe-2011.5.220/bin/compilervars.csh intel64
{% endhighlight %}

MPI support for Intel compilers: (by mvapich2)

{% highlight shell %}
set path=(/opt/mvapich2-1.8-r5423/intel/bin $path)
setenv LD_LIBRARY_PATH /opt/mvapich2-1.8-r5423/intel/lib
source /opt/mvapich2-1.8-r5423/intel/etc/mvapich_vars.csh
{% endhighlight %}

Ansys - Fluent:

{% highlight shell %}
set path=($path /home/pkg/lic/ansys/13/linux/64/v130/fluent/bin)
{% endhighlight %}

Matlab:

{% highlight shell %}
set path=(/home/pkg/lic/matlab-R2014a/bin $path)
{% endhighlight %}

Mathematica:

{% highlight shell %}
set path=(/home/pkg/lic/mathematica-9.0.1/bin $path)
{% endhighlight %}

**For IBM Blue Gene/L :**

{% highlight shell %}
set path=(/opt/ibmll/LoadL/full/bin $path)
set path=(/opt/ibmcmp/xlf/bg/10.1/bin $path)
set path=(/opt/blrts-gnu/bin/ $path)
{% endhighlight %}

where the first line is for the load leveler which schedules jobs, the second line is for the IBM XL Fortran compiler and the last line for the GNU compilers for Blue Gene/L.

If you want access to other softwares, you can have a look at the directories `/home/pkg/lic` and `/opt`. After adding these paths and sourcing the `.cshrc` file, you can start the desired program by simply entering the executable name in the terminal from anywhere. e.g. `matlab` will start the Matlab R2014a program.

----> [Previous article in this series](http://flowphysics.com/file-system-structure-serc/)

----> [Next article in this series](http://flowphysics.com/run-fluent-simulation-in-serc/)
