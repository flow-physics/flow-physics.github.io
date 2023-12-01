---
author: Rajesh Venkatesan
comments: true
date: 2020-02-22 20:19:54+00:00
layout: post
link: http://flowphysics.com/fourier-transform-for-confused-engineers/
slug: fourier-transform-for-confused-engineers
title: Fourier transform for confused engineers
wordpress_id: 426
categories:
- Mathematical Physics
tags:
- DFT
- FFT
- Fourier transform
---

If you are an engineer, you most likely encountered one of the following situations:

* Plot the power spectral density of a signal you have

* Find the dominant frequency/mode in a signal

* A journal paper stating, "It is obvious(?!) that this operation is straightforward to carry out in wave space".

I have been in many such instances myself and referred several texts on Fourier analysis. There are some wonderful ones, don't mistake me, but I have always felt that it is not straightforward to see the implications of the theory in the output of a Fourier transform function spit by a program/library in Python/Matlab. There are many equivalent ways in which the Fourier transform can be formulated, computed and interpreted. Throw the dreaded complex numbers in the mix , it is quite normal to feel lost. The feeling of not fully comprehending the idea, and yet repeatedly using it like a black box is frustrating. Having once been lost myself and now with a good grip on the means and ends of this wonderful mathematical tool, I attempt to write about this often-explained but rarely-understood method. This is mainly for my satisfaction, and if it helps someone understand this tool with a bit more of clarity, I will take that too.

This is the start of a series of posts on how to numerically compute and interpret the Fourier transform of signals/images/fields. Let's dive right in!

_Next article:_ [2. What is the Fourier transform?](http://flowphysics.com/what-is-the-fourier-transform/)
