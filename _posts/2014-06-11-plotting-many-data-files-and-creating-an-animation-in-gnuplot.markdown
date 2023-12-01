---
author: Rajesh Venkatesan
comments: true
date: 2014-06-11 13:31:03+00:00
layout: post
link: http://flowphysics.com/plotting-many-data-files-and-creating-an-animation-in-gnuplot/
slug: plotting-many-data-files-and-creating-an-animation-in-gnuplot
title: Plotting many data files and creating an animation in gnuplot
wordpress_id: 68
categories:
- Plotting
tags:
- GNUPlot
- Plotting
---

Say, you have many files with two columns of data.  If we want to plot the data in each file in a separate plot, it will take a lot of time do it manually one by one. But using a simple shell script, this can be done at once.

Let the files be named as data1, data2, data3, ..., dataN. An example shell script _plot_ for doing this looks like:

{% highlight shell %}
#!/bin/bash
for i in `seq 1 N`;
do
name="data${i}"
gnuplot << plotting_many_files
set xlabel "x"
set ylabel "y"
xy="./$name"
set terminal pngcairo
set output "$name.png"
plot xy using 1:2 with dots
plotting_many_files
done
{% endhighlight %}

This script is assuming that all the data files are in the same location as the script file. Also it creates _png_ figures with the same name as data files. Replace _N_ with the number of the last file. For running this script in terminal, you have to enter the command,

{% highlight shell %}
sh plot
{% endhighlight %}

For creating an animation from the plots created as above, we need the image editing software [GIMP](http://www.gimp.org/).

Steps:

  1. Open GIMP

  2. File > Open as layers

  3. Select all the plotted figures and open

  4. File > Save as

  5. Choose GIF and enter a name for the animation

  6. Select 'Save as animation' when prompted

  7. You can specify some parameter of the animation in this window. Like delay between frames, loop forever, etc. Default choices are okay for most purposes.

  8. Enjoy !!
