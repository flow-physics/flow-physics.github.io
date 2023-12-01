---
author: Rajesh Venkatesan
comments: true
date: 2019-07-21 10:29:56+00:00
layout: post
link: http://flowphysics.com/installing-hdf5-and-cgns-in-ubuntu-18-04-lts/
slug: installing-hdf5-and-cgns-in-ubuntu-18-04-lts
title: Installing HDF5 and CGNS in Ubuntu 18.04 LTS
wordpress_id: 398
categories:
- Installing Software
tags:
- CGNS
- HDF5
- hdfview
- install
- linux
- ubuntu
- ubuntu 18.04
---

If you are a computational fluid dynamics (CFD) practitioner, at some point, you would have to use Hierarchical Data Format 5 ([HDF5](https://www.hdfgroup.org/)) and CFD General Notation System ([CGNS](http://cgns.github.io/)) libraries in your programs. HDF5 format provides an advanced way of organizing any data and allows efficient access to the data. CGNS prescribes specific methods and practices to use when writing CFD simulation data into HDF5 format. You can find more info on everything about these libraries in their website. But here, I describe the way to install HDF5 & CGNS in a Ubuntu 18.04 system.

The installation procedure follows what is given in CGNS website, but with some important tweaks to get it to succeed installing in Ubuntu 18.04.

Download the CGNS source code into some directory.

{% highlight shell %}
git clone -b master https://github.com/CGNS/CGNS.git
{% endhighlight %}

Currently, this downloads CGNS 3.4.0 source code into a directory named `CGNS` in the present location. Although you can install HDF5 separately, and possibly a newer version from HDF5 website, I strongly advise against it. We want the CGNS library to work with our HDF5 installation. So, it is best to install the HDF5 version suggested by CGNS.

Change into the CGNS directory.

{% highlight shell %}
cd CGNS
{% endhighlight %}

There are install scripts provided in the `bin` folder. Three of them in fact, one to install HDF5 (`./bin/install-hdf.sh`), another to configure CGNS for our system (`./bin/config-cgns.sh`) and the last one to install the configured CGNS (`./bin/build-cgns.sh`). Of course, we need to do the installation in the order I have listed.

You can choose to use the `install-hdf.sh` script. Some options inside that script failed for me. So, instead, use the following commands:

{% highlight shell %}
git clone https://bitbucket.hdfgroup.org/scm/hdffv/hdf5.git --branch hdf5_1_8 --single-branch hdf5_1_8
{% endhighlight %}

This will download the HDF5 v1.8 into the newly created directory `hdf5_1_8`. Change into this directory and use the following command:

{% highlight shell %}
cd hdf5_1_8
./configure --enable-fortran --disable-hl --prefix=$HOME/hdf5 && make > result.txt 2>&1 && make install
{% endhighlight %}

Note that I am choosing to install HDF5 in the location `$HOME/hdf5`, you can choose any other convenient location as well. But remember, this is the location you need to link any program trying to compile with HDF5 libraries.
The installation of HDF5 will take some time, and finally it will output the HDF5 configuration installed in the system. I have configured HDF5 to include Fortran bindings as well by the flag `--enable-fortran`. If you only want C binidings, you can replace that by `--disable-fortran`.

After this, we need to configure CGNS using the `./bin/config-cgns.sh` script. The default script looks like this:

{% highlight shell %}
#!/bin/sh
#
# Configure CGNS for travis CI. 
#
set -e
cd src
if [ $TRAVIS_OS_NAME = "linux" ]; then
  export FLIBS="-Wl,--no-as-needed -ldl -lz"
  export LIBS="-Wl,--no-as-needed -ldl -lz"
fi

./configure \
--prefix=$PWD/cgns_build \
--with-hdf5=$HOME/hdf5 \
--with-fortran \
--enable-lfs \
--enable-64bit \
--disable-shared \
--enable-debug \
--with-zlib \
--disable-cgnstools \
--enable-64bit

{% endhighlight %}

Edit it to look like this:

{% highlight shell %}
#!/bin/sh
#
# Configure CGNS for travis CI. 
#
set -e
cd src
export FLIBS="-Wl,--no-as-needed -ldl -lz"
export LIBS="-Wl,--no-as-needed -ldl -lz"
./configure \
--prefix=$HOME/cgns \
--with-hdf5=$HOME/hdf5 \
--with-fortran \
--enable-lfs \
--enable-64bit \
--disable-shared \
--enable-debug \
--with-zlib \
--disable-cgnstools \
--enable-64bit
{% endhighlight %}

Apart from some specific modifications to make it work for Ubuntu, I have also specified the CGNS installation directory as `$HOME/cgns`. You can change this to any other convenient location. But again, this is where the built CGNS libraries and headers will be kept. You will be required to link to this location when compiling and linking programs with CGNS.
(If you have installed the HDF5 to some location other than `$HOME/hdf5`, you should specify that here in the flag `--with-hdf5=`)

Run this configure script using:

{% highlight shell %}
sh ./bin/config-cgns.sh
{% endhighlight %}

This will configure the CGNS for your system.

Finally, install CGNS using:

{% highlight shell %}
sh ./bin/build-cgns.sh
{% endhighlight %}

That's it. You have installed HDF5 & CGNS in your Ubuntu system at `$HOME/hdf5` and `$HOME/cgns` respectively.

There are test codes available inside the downloaded CGNS repo at: `./CGNS/src/Test_UserGuideCode`. They would have been tested during the build process as well.
If you have a program `test.c` which uses CGNS functions, then you would want to compile and link the program as follows:

{% highlight shell %}
gcc -I$HOME/cgns/include -g -g -O2 -c test.c
gcc -o test.out test.o $HOME/cgns/lib/libcgns.a $HOME/hdf5/lib/libhdf5.a -lz -lm -lm -lz -Wl,--no-as-needed -ldl -lz
{% endhighlight %}

`test.out` is the final executable created for the program.

Notice that we have chose not to install the utility tools for CGNS by choosing the flag `--disable-cgnstools` in out configure script. Enabling this fails for me. I tried to install cgnstools using various other methods as well, but it fails for me in Ubuntu 18.04.

**HDFVIEW**

If you want a GUI for viewing your CGNS/HDF5 files, you may want to use HDFVIEW software. Note that the `hdfview` available in ubuntu repositories does not work. Even the source code compilation process always fails in ubuntu. Just download the prebuilt binary for CentOS7 from the HDF [website](https://www.hdfgroup.org/downloads/hdfview/). And run the `HDFView-3.1.0-Linux.sh` script inside the downloaded archive to create the binary. It works properly in Ubuntu 18.04. I found this useful tip from Michael Hirsch's [blog](https://www.scivision.dev/view-hdf5-data-gui/).
