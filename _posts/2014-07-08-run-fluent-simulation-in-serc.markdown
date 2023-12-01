---
author: Rajesh Venkatesan
comments: true
date: 2014-07-08 10:46:28+00:00
layout: post
link: http://flowphysics.com/run-fluent-simulation-in-serc/
slug: run-fluent-simulation-in-serc
title: Run Fluent simulation in SERC
wordpress_id: 165
categories:
- SERC Series
tags:
- Fluent
- IISc
- Parallel Computing
- SERC
---

Ansys - Fluent 13.0 is installed in Delta and Tyrone clusters of  SERC.   Pre-processing and post-processing can be done using any of the network-connected terminals or RedHat/Suse linux HPC machines of SERC using 4 CPUs from Tesla cluster.  If you have a good machine in your lab, you can do pre-/post- processing there itself.

### **1. Creating the case file:**

If you use SERC Fluent to create your case file, you can specify in the beginning itself about the number of MPI processors you want to use to solve the problem.  This is advantageous because it will tell Fluent how to partition your mesh and so on. Then you can mention the same number processors in the SERC submit script so that your simulation is run most efficiently. If you create a case file using a serial version of Fluent in your laptop/desktop, then you can still submit in SERC to run with many processors in parallel. But the partitioning is done automatically by Fluent in this case. Hence the first approach is recommended. Once you specify the number of MPI processors in Fluent and create a case file, you can keep it in a directory in your home folder in SERC.

### **2. Creating the submission script:**

I provide below the submission file given in SERC website for Tyrone cluster (32 processors).

{% highlight shell %}
#!/bin/sh
#Sample mpi job script for 32-core fluent run
#PBS -N jobname
#PBS -l nodes=1:ppn=32:regular
#PBS -l walltime=24:00:00
#The localscratch directory on different execution nodes of the tyrone cluster is hosted on
#different storage servers. Hence, files required for execution need to be copied to
#/localscratch from the user home area in the script.
cd /localscratch/${USER}
cp $HOME/<program_directory>/* .
NPROCS=`wc -l < $PBS_NODEFILE`
/home/pkg/lic/ansys/13/linux/64/v130/fluent/bin/fluent -g 2ddp -t${NPROCS} -ssh -i /localscratch/${USER}/inputfile > /localscratch/${USER}/outputfile
mv ./* $HOME/<program_directory>
{% endhighlight %}

Let us say our case file `test.cas` is in the folder `$HOME/test_run`. For the above submission script, we need to create two files named `inputfile` and `outputfile` inside the `test_run` folder. And the `inputfile` will look like the following:

{% highlight shell %}
rc test.cas
/solve/init/init
it 50
wd test.dat
exit
yes
{% endhighlight %}

Note that `inputfile` is like a script file containing Fluent commands.  This file asks fluent to read the case file `test.cas`, initialize the solution, calculate the required number of iterations (here 50) , and write the final data to `test.dat`.

The `outputfile` just needs to be an empty file. All the outputs you will see in the Fluent window if you run the `inputfile` commands in Fluent will be stored in this file. This will include the usual convergence history and so on.

So, my submission script `my_fluent_run` will look like the following:

{% highlight shell %}
#!/bin/sh
#PBS -N test_run
#PBS -l nodes=1:ppn=32:regular
#PBS -l walltime=24:00:00
cd /localscratch/${USER}
cp $HOME/test_run/* .
NPROCS=`wc -l < $PBS_NODEFILE`
/home/pkg/lic/ansys/13/linux/64/v130/fluent/bin/fluent -g 2ddp -t${NPROCS} -ssh -i /localscratch/${USER}/inputfile > /localscratch/${USER}/outputfile
mv ./* $HOME/<program_directory>
{% endhighlight %}

You can change `2ddp` to any other option you want in Fluent. In the above script `my_fluent_run`, first line specifies the shell used. Next line specifies the job name. Line 3 mentions the number of processors to be used. Line 4 specifies the maximum number of hours your simulation can run. Line 5 changes into your `localscratch` directory. Line 6 copies the `test_run` folder in your `$HOME` to the `localscratch` directory. Line 8 starts the Fluent run with the `inputfile`. Line 9 finally moves all your results from the `localscratch` back to your `$HOME` directory.

### **3. Submitting your job:**

You can submit the script `my_fluent_run` using the following command.

{% highlight shell %}
username@tyrone-cluster:~$ qsub my_fluent_run
{% endhighlight %}

You will receive a confirmation with your job number.  You can check the status of your job using the command `qstat`.

--------> [Previous article in this series](http://flowphysics.com/environment-variables-and-path/)
