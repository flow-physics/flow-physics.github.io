---
author: Rajesh Venkatesan
comments: true
date: 2014-06-24 06:19:00+00:00
layout: post
link: http://flowphysics.com/my-ubuntu-packages/
slug: my-ubuntu-packages
title: My Ubuntu Packages
wordpress_id: 98
categories:
- Installing Software
tags:
- linux
- ubuntu
---

In this article, I shall list down the useful software packages I have installed in my Ubuntu (18.04.2 LTS 64-bit) operating system.  I used the following commands to install them just after a fresh install of the OS.

Some General and Basic Packages:

{% highlight shell %}
sudo apt-get update
sudo apt-get install gnome-shell gftp gparted djview4 diffuse vim-gnome linuxdcpp vlc
{% endhighlight %}

The first command updates the package list of the system.  The command `sudo apt-get install` followed by the name of the packages (separated by space) installs those packages from Ubuntu repositories.

Some image editing softwares:

{% highlight shell %}
sudo apt-get install gimp inkscape
{% endhighlight %}

Version Control System:

{% highlight shell %}
sudo apt-get install git
{% endhighlight %}

Python:

{% highlight shell %}
sudo apt-get install python3 python3-pip python3-numpy python3-scipy python3-matplotlib python3-dev ipython3 ipython3-notebook mercurial python3-sphinx spyder3
{% endhighlight %}

Fortran:

{% highlight shell %}
sudo apt-get install gfortran libopenmpi-dev openmpi-bin liblapack-dev libfftw3-dev libfftw3-doc
{% endhighlight %}

`gfortran` installs the GNUFortran compiler.  The next two packages install OpenMPI libraries and executables respectively.  The last one installs the standard linear algebra package LAPACK.  Note that `gfortran` comes with OpenMP libraries pre-installed.

Plotting:

{% highlight shell %}
sudo apt-get install gnuplot
{% endhighlight %}

(_This post will be updated as and when I find other useful packages._)
