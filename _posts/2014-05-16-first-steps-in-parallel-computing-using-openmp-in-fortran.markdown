---
author: Rajesh Venkatesan
comments: true
date: 2014-05-16 18:07:23+00:00
layout: post
link: http://flowphysics.com/first-steps-in-parallel-computing-using-openmp-in-fortran/
slug: first-steps-in-parallel-computing-using-openmp-in-fortran
title: 'First steps in Parallel Computing : Using OpenMP in Fortran'
wordpress_id: 35
categories:
- HPC
tags:
- Fortran
- OpenMP
- Parallel Computing
---

For a long time, I had this idea that parallel computing is a difficult task and kept away from it. Also my computations were not that demanding in those days. Recently when I had to solve a large system of ordinary differential equations numerically, I was forced to learn how to do this in parallel. There are two primary ways of doing parallel computing,

1. Shared-memory architecture (OpenMP)
2. Distributed-memory architecture (MPI)

As the names suggest, in the first approach there are many processors doing the tasks for you, but all of them have access to a shared physical memory. In the second approach, there are many processors with their own physical memory and you have to do the data communication between them yourself.

I used the simpler first approach because I had a nice Quad-core computer with a large enough RAM. And also my numerical problem was not very memory expensive. The MPI (Message Passing Interface) approach is more complex and needs a lot more work from the programmer. Some people may even say that MPI is _the real_ parallel computing. But as far as I know, the first step would be to learn OpenMP and then to go for MPI. Also modern computing environments use a hybrid OpenMP/MPI appraoch.

So, let us begin! OpenMP (Open Multi-Processing) is a standard that specifies how parallel computing directives are handled by the Fortran (or C/C++) compiler. All one needs to do is to learn a small number of important commands in OpenMP and use them (wisely!) inside the Fortran program. An example parallel 'Hello world' program (_hello.f_) would be:

{% highlight fortran %}
PROGRAM hello
!$OMP PARALLEL
  write(*,*) 'Hello World!'
!$OMP END PARALLEL
END PROGRAM hello
{% endhighlight %}

You must compile the above program using the following command:

1. If you are using gfortran, then,

{% highlight shell %}
$ gfortran -fopenmp hello.f
$ ./a.out
{% endhighlight %}

which will result in the output, (say, I am using a dual core machine)

{% highlight shell %}
Hello World!
Hello World!
{% endhighlight %}

_-fopenmp_ is the option to be included to tell the gfortran compiler that you are using OpenMP parallel computing inside the program.

1. If you are using Intel Fortran compiler (ifort), then

{% highlight shell %}
$ ifort -openmp hello.f
$ ./a.out
{% endhighlight %}

which will give the same result. Note that the corresponding OpenMP handle for _-fopenmp_ in ifort is _-openmp_.

Now let us see what is in the Fortran program hello.f. The string !$OMP is called the OpenMP sentinel. It is placed to indicate that the statements in the present line are to be treated by OpenMP standard. !$OMP PARALLEL/!$OMP END PARALLEL loop is identifying the region of the code that has to be run in parallel using the maximum of number of processors ('_threads_' is the proper usage) available. Since I have two cores in my computer, we are seeing the _write(*,*)_ line executed twice and in parallel by the two cores.

The primary application of this approach is to identify DO/ENDDO loops which can be run in parallel. Not all loops can be parallelized. But all those things are for another day.

Finally, a great source for learning OpenMP is the [report](http://www.openmp.org/presentations/miguel/F95_OpenMPv1_v2.pdf) by Miguel Hermanns. I like his simple and to-the-point approach.
