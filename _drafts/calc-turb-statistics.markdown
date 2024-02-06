---
author: Rajesh Venkatesan
comments: true
date: 2024-02-06 00:00:00+00:00
layout: post
link: http://flowphysics.com/calc-turb-statistics
slug: calc-turb-statistics
title: Calculation of Turbulence Statistics
---

Let us consider the Reynolds decomposition of $\tilde{u}(t)$ given as,  
\begin{equation}
\tilde{u} = U + u
\end{equation}

Assuming a stationary turbulent flow, $U$ is a constant with respect to time. Our aim is to calculate the various moments of the function $\tilde{u}(t)$ around its mean. The first moment of $\tilde{u}(t)$ is the mean and it is defined as,
\begin{equation}
U = \frac{1}{T} \int_t^{t+T} \tilde{u} dt
\end{equation}

In what follows, we shall omit specifying the limits of the integration assuming it is the same as in the above equation. The second moment of the fluctuations $u = \tilde{u} - U $ is called the variance and is defined as,
$$\begin{equation}
<u^2> = \frac{1}{T} \int (\tilde{u} - U )^2 dt
\end{equation} $$

$$ \begin{align*}
 <u^2> & = \frac{1}{T} \int ( \tilde{u}^2 - 2 U \tilde{u} + U^2) dt \\
 & = \frac{1}{T} \int \tilde{u}^2 dt - 2 U \frac{1}{T} \int \tilde{u} dt + U^2 \frac{1}{T}\int dt \\
 & = \frac{1}{T}\int \tilde{u}^2 dt - 2U^2 +U^2 \\
 & = \frac{1}{T} \int \tilde{u}^2 dt - U^2
\end{align*}
$$

$$ \begin{align*}
 <u^2> & = \frac{1}{T} \int ( \tilde{u}^2 - 2 U \tilde{u} + U^2) dt \\
 & = \frac{1}{T} \int \tilde{u}^2 dt - 2 U \frac{1}{T} \int \tilde{u} dt + U^2 \frac{1}{T}\int dt \\
 & = \frac{1}{T}\int \tilde{u}^2 dt - 2U^2 +U^2 \\
 & = \frac{1}{T} \int \tilde{u}^2 dt - U^2
\end{align*}
$$

Hence, from a practical perspective, to calculate the variance it is enough to integrate the square of the instantaneous velocity and subtract the square of the mean velocity from it. It is important to note that this critically depends on the mean velocity being stationary or constant in time. We shall see that a similar approach will be useful for calculation of higher moments as well. 

The skewness is the third moment of $u$, normalized by the variance:
$$\begin{equation}
skewness =  \frac{<u^3>}{<u^2>^{3/2}}
\end{equation}$$

$$ \begin{align*}
<u^3> & = \frac{1}{T} \int (\tilde{u} - U)^3 dt \\
&= \frac{1}{T} \int (\tilde{u}^3 - 3 \tilde{u}^2U + 3 \tilde{u} U^2 - U^3) dt \\
&= \frac{1}{T} \int \tilde{u}^3 dt - 3U \frac{1}{T} \int \tilde{u}^2 dt + 3 U^3 - U^3 \\
& = \frac{1}{T} \int \tilde{u}^3 dt -3U \left(\frac{1}{T} \int \tilde{u}^2 dt \right) + 2 U^3 \\
& = \frac{1}{T} \int \tilde{u}^3 dt - 3U \left( \frac{1}{T} \int \tilde{u}^2 dt - \frac{2}{3} U^2 \right) \\
& = \frac{1}{T} \int \tilde{u}^3 dt - 3U \left( \frac{1}{T} \int \tilde{u}^2 dt - U^2 + \frac{1}{3} U^2 \right) \\
& = \frac{1}{T} \int \tilde{u}^3 dt -3U <u^2> - U^3
\end{align*} $$

The flatness(or kurtosis) is the fourth moment of $u$, normalized by the variance:
$$\begin{equation}
flatness = \frac{<u^4>}{<u^2> ^2} 
\end{equation}$$

$$ \begin{align*}
<u^4> & = \frac{1}{T} \int (\tilde{u} - U )^2 (\tilde{u} - U )^2 dt \\
& = \frac{1}{T} \int (\tilde{u}^2 - 2 \tilde{u}U +U^2) (\tilde{u}^2 - 2 \tilde{u}U +U^2) dt \\
& = \frac{1}{T} \int (\tilde{u}^4 - 2\tilde{u}^3U + \tilde{u}^2 U^2 - 2 \tilde{u}^3 U +4 \tilde{u}^2 U^2 - 2\tilde{u}U^3 +U^2 \tilde{u}^2 - 2 \tilde{u} U^3 + U^4) dt \\
& = \frac{1}{T} \int  (\tilde{u}^4  -4\tilde{u}^3U +6\tilde{u}^2 U^2  -4\tilde{u}U^3 + U^4) dt \\
& = \frac{1}{T} \int \tilde{u}^4 dt + \frac{6U^2}{T} \int \tilde{u}^2 dt + U^4 - 4U^4 - \frac{4U}{T} \int \tilde{u}^3 dt \\
& = \frac{1}{T} \int \tilde{u}^4 dt + 6 U^2 \left( \frac{1}{T} \int \tilde{u}^2 dt\right) -3U^4 -\frac{4U}{T} \int \tilde{u}^3 dt \\
& = \frac{1}{T} \int \tilde{u}^4 dt +6U^2 \left( \frac{1}{T} \int \tilde{u}^2 dt -\frac{1}{2}U^2 \right) -\frac{4U}{T} \int \tilde{u}^3 dt \\
& = \frac{1}{T} \int \tilde{u}^4 dt  + 6 U^2 \left( \frac{1}{T} \int \tilde{u}^2 dt -U^2 +\frac{1}{2} U^2  \right)-\frac{4U}{T} \int \tilde{u}^3 dt \\
& = \frac{1}{T} \int \tilde{u}^4 dt + 6U^2 <u^2>  +3U^4 -4U \left( <u^3> + 3U<u^2> + U^3 \right) \\
& = \frac{1}{T} \int \tilde{u}^4 dt +6U^2 <u^2> +3U^4 -4U<u^3> -12 U^2 <u^2> -4U^4 \\
& = \frac{1}{T} \int \tilde{u}^4 dt -4U<u^3> -6U^2<u^2> - U^4
\end{align*}$$

A similar approach can be extended to Reynolds stresses as well. For example,  

$$ \begin{align*}
< uv > &= \frac{1}{T} \int (\tilde{u}-U)(\tilde{v}-V) dt \\
&= \frac{1}{T} \int (\tilde{u} \tilde{v} - \tilde{u} V - U \tilde{v} + UV) dt \\
&= \frac{1}{T} \int \tilde{u} \tilde{v} dt - 2UV + UV \\
&=  \frac{1}{T} \int \tilde{u} \tilde{v} dt - UV 
\end{align*}$$
  
The triple products can be expressed as,
$$ \begin{equation}
<u^2v> = \frac{1}{T} \int \tilde{u}^2 \tilde{v} dt - 2UV<uv>-V<u^2>-U^2V
\end{equation}  $$
