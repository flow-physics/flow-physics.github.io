---
author: Rajesh Venkatesan
comments: true
date: 2022-10-17 10:16:20+00:00
layout: post
link: http://flowphysics.com/reading-data-in-parallel-using-mpi-i-o/
slug: reading-data-in-parallel-using-mpi-i-o
title: Reading data in parallel using MPI I/O
wordpress_id: 665
categories:
- HPC
tags:
- mpi
- MPI I/O
- Parallel Computing
- parallel file access
- parallel I/O
---




In the previous post "[First steps in parallel file access using MPI I/O](http://flowphysics.com/first-steps-in-parallel-file-access-using-mpi-i-o/)", we discussed how to write some simple data in parallel using MPI I/O. Here we shall read back that data in parallel. The entire code is given in my GitHub repo [MPI_Notes](https://github.com/rajesh-ae/MPI_Notes/blob/main/MPI_IO/read_char_parallel.c). Essentially, we have the following text data in a file:

{% highlight shell %}
abcdefghijklmnopqrstuvwxyz
{% endhighlight %}

We will try to read this using 6 MPI processes. And we want each process to read a part of this data as shown below: (this is the same partitioning used when we wrote the data)

![](/assets/img/2022/10/read_data-1.png)

This partitioning is arbitrary and we could have chosen different ways to divide up the data among processes to read. Similar to the writing process, there are three approaches in MPI I/O which can be used to read this data:

  1. Using explicit offsets
  2. Using individual file pointers
  3. Using shared file pointers

## Using explicit offsets

As the name suggests, we need to calculate where each process should start reading its data ('_offset_') and the length of the data to be read in by that process ('_count_'). So, _proc 0_ should read at the beginning of the file (3 characters to be read), _proc 1_ should start writing after 3 characters (5 characters to be read), _proc 2_ should start reading after 8 characters (3 characters to be read) and so on. This can be done by the following piece of code:

{% highlight fortran %}
MPI_File_open(MPI_COMM_WORLD, "file_exp_offset.dat", MPI_MODE_RDONLY, MPI_INFO_NULL, &file_handle);
MPI_File_read_at_all(file_handle, disp, test_txt,arr_len_local,MPI_CHAR,MPI_STATUS_IGNORE);
MPI_File_close(&file_handle);
{% endhighlight %}

'disp' denotes the offset calculated and 'arr_len_local' is the count (length) of the data to read in. Once the data is read in, the MPI processes will have the following data in their '_test_txt_' array:

![](/assets/img/2022/10/data-2.png)

While the explicit offset approach seems quite straightforward, it is preferable to use the individual file pointer method for more complex datasets and partitioning. This is discussed next.

## Using individual file pointers  

Since we have explained the individual file pointer method in the previous [post](http://flowphysics.com/first-steps-in-parallel-file-access-using-mpi-i-o/), we shall only outline the approach here. Instead of calculating individual file pointer locations manually, we define a new global datatype to represent the data partitioning. This is done by:

{% highlight fortran %}
MPI_Type_create_subarray(1, &total_len, &arr_len_local, &disp, MPI_ORDER_C, MPI_CHAR, &char_array_mpi);
MPI_Type_commit(&char_array_mpi);
{% endhighlight %}

Once the global datatype is created like this, reading the data in parallel is very simple as shown below:

{% highlight fortran %}
MPI_File_open(MPI_COMM_WORLD, "file_ind_ptr.dat", MPI_MODE_RDONLY, MPI_INFO_NULL, &file_handle);
MPI_File_set_view(file_handle, 0, MPI_CHAR, char_array_mpi,"native", MPI_INFO_NULL);
MPI_File_read_all(file_handle, test_txt, arr_len_local, MPI_CHAR, MPI_STATUS_IGNORE);
MPI_File_close(&file_handle);
{% endhighlight %}

## Using shared file pointers

In the shared file pointer approach, we specify simply the 'count' of the data to be read by each process and let MPI calculate the offsets for us. This can be achieved by the following code snippet:

{% highlight fortran %}
MPI_File_open(MPI_COMM_WORLD, "file_shr_ptr.dat", MPI_MODE_RDONLY, MPI_INFO_NULL, &file_handle);
MPI_File_read_ordered(file_handle, test_txt, arr_len_local, MPI_CHAR,MPI_STATUS_IGNORE);
MPI_File_close(&file_handle);
{% endhighlight %}

Of course, this will come with a performance penalty as the shared file pointer is synchronized internally by MPI. The approach to use for reading/writing should be tested and its performance evaluated before deploying for production use in a software.

For demonstration purposes, we have chosen to read/write some character array in the examples here. We shall look into writing/reading general numerical data in some complex data partitioning in the next articles in this series on MPI I/O.
