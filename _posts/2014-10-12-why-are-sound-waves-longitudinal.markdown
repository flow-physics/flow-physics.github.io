---
author: Rajesh Venkatesan
comments: true
date: 2014-10-12 06:00:48+00:00
layout: post
link: http://flowphysics.com/why-are-sound-waves-longitudinal/
slug: why-are-sound-waves-longitudinal
title: Why are sound waves longitudinal?
wordpress_id: 321
categories:
- Mathematical Physics
tags:
- longitudinal waves
- sound waves
- transverse waves
- velocity potential
- waves
---

"Give me some example for waves in nature! "

"Sound is a wave.. Light is a wave.. Of course we see water waves.. "

Thats the usual answer we think about when asked that question.  Moreover, we are told that sound waves are longitudinal waves (compression waves) and light waves are transverse waves.  But we do not often ask the question 'Why?'.  Why are sound waves longitudinal?.  I give a short answer and a lengthy one.

## Short Answer

Both light propagation and sound propagation (in air or water) are governed by the same wave equation.  But in case of a light wave or traveling waves on a string, the variable governed by the wave equation is the disturbance itself.  Thus leading to a transverse wave.  In the case of a sound wave, the variable governed by the wave equation is the velocity potential $latex \phi &s=1$.  This is related to the disturbance velocity in the following way,

$latex \vec{V}=\nabla \phi &s=2$

And from vector analysis, it is easy to show that the gradient vector $latex \nabla \phi$ is perpendicular to constant lines of the original field $latex \phi$.  Hence, even though $latex \phi$ propagates like a transverse wave, the disturbance velocity $latex \vec{V}$ propagates like a longitudinal wave.

## Long Answer

Waves are everywhere around us.  In simple terms, a wave is a disturbance propagating through a medium (say,  air or water).  Most wave phenomenon we see in nature are governed by the so-called wave equation,

$latex \frac{\partial^2 \psi}{\partial t^2}=c^2\frac{\partial ^2 \psi}{\partial x^2}&s=2$

The solution to the above equation is any (good!) function traveling in the _x_-direction with speed _c_. Formally written as $latex \psi=f(x\pm ct)$.  For example, in the case of a ripple in a pond, a change in the height of the water plays the role of the disturbance.  This change propagates through the pond from the source that created the disturbance.  In this case, $latex \psi &s=1$ is the displacement of water from the undisturbed level.

Similarly, for a light wave, the thing that changes is the electromagnetic field.  And this change is propagated through space.  There are two types of waves.  Longitudinal waves and transverse waves.  In the example given above, the change in the water level is propagating outwards throughout the pond.  But the change itself makes the water go either up/down at a given location.  Thus we say, the wave propagates in one direction and the displacement of the medium is at right angles to that direction.  This kind of wave is called a transverse wave.  An illustration of a transverse wave is given below: (taken from Wikipedia)

![](/assets/img/2014/10/onde_cisaillement_impulsion_1d_30_petit.gif)

On the other hand, we have longitudinal waves where the displacement of the medium occurs in the same direction as that of wave propagation.  This is illustrated in the following image: (taken from Wikipedia)

![](/assets/img/2014/10/onde_compression_impulsion_1d_30_petit.gif)]

When sound propagates from left to right, the air molecules are compressed and rarefied just like the vertical grid lines in the above figure.   During sound propagation in the _x_-direction, the velocity potential obeys the wave equation.

$latex \frac{\partial^2 \phi}{\partial t^2}=c^2\frac{\partial ^2 \phi}{\partial x^2}&s=2$

(This equation is derived from the Navier-Stokes equation after linearization and some other assumptions).

The solution to the above equation is given as,

$latex \phi=f(x\pm ct)&s=2$

From this solution and the definition of the velocity potential ($latex \vec{V}=\nabla\phi$),  we can find the disturbance velocity field as,

$latex u=f'(x \pm ct)&s=2$

 $latex v=0&s=2$

$latex w=0&s=2$

Thus we see that the transverse wave of $latex \phi$ in the _x_-direction leads to a disturbance velocity involving only the _x_-component velocity _u_. So the air/water molecules are displaced in the same direction as the direction of propagation.

### Note

Sound waves propagate as longitudinal waves in fluid media. In solids, however, they travel both as longitudinal and transverse waves.
