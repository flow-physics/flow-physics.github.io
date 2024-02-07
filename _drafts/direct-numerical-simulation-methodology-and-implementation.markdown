---
author: Rajesh Venkatesan
comments: true
date: 2024-02-06 00:00:00+00:00
layout: post
link: http://flowphysics.com/direct-numerical-simulation-methodology-implementation
slug: direct-numerical-simulation-methodology-and-implementation
title: Direct Numerical Simulation Methodology and Implementation
---
* Table of Contents
{:toc}

## Formulation of the problem

The Navier-Stokes equations governing the three-dimensional unsteady incompressible fluid flow are: 
\begin{equation} \label{eq:continuity}
\frac{\partial u}{\partial x}+\frac{\partial v}{\partial y}+\frac{\partial w}{\partial z}=0
\end{equation}

\begin{equation} \label{eq:x-momentum}
\frac{\partial u}{\partial t}+ u\frac{\partial u}{\partial x}+v\frac{\partial u}{\partial y}+w\frac{\partial u}{\partial z}=-\frac{1}{\rho}\frac{\partial p}{\partial x}+\nu\left(\frac{\partial^2 u}{\partial x^2}+\frac{\partial^2 u}{\partial y^2}+\frac{\partial^2 u}{\partial z^2}\right)
\end{equation}

\begin{equation}\label{eq:y-momentum}
\frac{\partial v}{\partial t}+u\frac{\partial v}{\partial x}+v\frac{\partial v}{\partial y}+w\frac{\partial v}{\partial z}=-\frac{1}{\rho}\frac{\partial p}{\partial y}+\nu\left(\frac{\partial^2 v}{\partial x^2}+\frac{\partial^2 v}{\partial y^2}+\frac{\partial^2 v}{\partial z^2}\right)
\end{equation}

\begin{equation}\label{eq:z-momentum}
\frac{\partial w}{\partial t}+u\frac{\partial w}{\partial x}+v\frac{\partial w}{\partial y}+w\frac{\partial w}{\partial z}=-\frac{1}{\rho}\frac{\partial p}{\partial z}+\nu\left(\frac{\partial^2 w}{\partial x^2}+\frac{\partial^2 w}{\partial y^2}+\frac{\partial^2 w}{\partial z^2}\right)
\end{equation}

Here $u,v,w$ are velocity components in the $x$ (streamwise), $y$ (wall-normal), $z$ (spanwise) cartesian coordinate directions and $p$ is the pressure. $\rho,\nu$ are the fluid density and kinematic viscosity respectively. These equations are to be solved subject to no-slip and impermeability conditions at solid surfaces.

Compactly, the governing equations (\ref{eq:continuity}, \ref{eq:x-momentum}, \ref{eq:y-momentum},\ref{eq:z-momentum})  can be written in tensor notation as follows:
\begin{equation}
\frac{\partial u_i}{\partial x_i} = 0
\end{equation}

\begin{equation}
\frac{\partial u_i}{\partial t}+u_j\frac{\partial u_i}{\partial x_j}=-\frac{1}{\rho}\frac{\partial p}{\partial x_i}+\nu \frac{\partial^2 u_i}{\partial x_j \partial x_j}
\end{equation}

It is useful to non-dimensionalize the quantities in the following way:
\begin{equation}
\frac{x_i}{l} \rightarrow \bar{x_i} \qquad i=1,2,3.
\end{equation}

\begin{equation}
\frac{u_i}{U} \rightarrow \bar{u_i}\qquad i=1,2,3.
\end{equation}

\begin{equation}
\frac{tl}{U}=\bar{t} \,; \qquad \frac{p}{\rho U^2} \rightarrow \bar{p}
\end{equation}

Here $U$ is the velocity scale and $l$ is the length scale. Substituting these transformations converts the NS equations (\ref{eq:continuity}, \ref{eq:x-momentum}, \ref{eq:y-momentum},\ref{eq:z-momentum}) into the following non-dimensional form.

\begin{equation}
\frac{\partial \bar{u_i}}{\partial \bar{x_i}} = 0
\end{equation}

\begin{equation}
\frac{\partial \bar{u_i}}{\partial \bar{t}}+\bar{u_j}\frac{\partial \bar{u_i}}{\partial \bar{x_j}}=-\frac{\partial \bar{p}}{\partial \bar{x_i}}+\frac{1}{Re} \frac{\partial^2 \bar{u_i}}{\partial \bar{x_j} \partial \bar{x_j}}
\end{equation}

The only non-dimensional parameter appearing in the equation is the Reynolds number, $Re=Ul/\nu$. For simplicity, hereafter we shall drop the over bars on the quantities while keeping in mind that they are all non-dimensional. We have,

$$
\begin{equation*}
\frac{\partial u_i}{\partial x_i} = 0
\end{equation*}$$

$$ \begin{equation*}
\frac{\partial u_i}{\partial t}+u_j\frac{\partial u_i}{\partial x_j}=-\frac{\partial p}{\partial x_i}+\frac{1}{Re} \frac{\partial^2 u_i}{\partial x_j \partial x_j}
\end{equation*}$$

The fact that only Reynolds number appears in the non-dimensionalised Navier-Stokes Equations leads to the well-known dynamical similarity. More on this can be found in the Appendix A.

It is possible to rewrite the advection term in the conservative form as given below.

$$ \begin{equation}\label{eq:NS_cont}
\frac{\partial u_i}{\partial x_i} = 0
\end{equation}$$
$$\begin{equation}\label{eq:NS_mom}
\frac{\partial u_i}{\partial t}+\frac{\partial (u_iu_j)}{\partial x_j}=-\frac{\partial p}{\partial x_i}+\frac{1}{Re} \frac{\partial^2 u_i}{\partial x_j \partial x_j}
\end{equation} $$  

{:refdef: style="text-align: center;"}
![](/assets/img/numflow/domain.png){:height="35%" width="35%"}  
*Domain of simulation*
{: refdef}

{:refdef: style="text-align: center;"}
![](/assets/img/numflow/cell.png){:height="35%" width="35%"}  
*The staggered grid arrangement of the primitive variables.*
{: refdef}

<!-- $$ \begin{figure}
\centering
\includegraphics[scale=0.35]{domain.pdf}
\caption{Domain of simulation}
\label{fig:domain}
\end{figure}$$ -->

Our primary interest is in solving the equations (\ref{eq:NS_cont}) for the turbulent boundary layer. So the domain selected is a parallelepiped as shown in above figure. The bottom $xz$ - plane represents the solid wall. The flow is in the +ve $x$-axis direction. 
As the Navier-Stokes equations are of elliptic nature, we need to provide velocity boundary conditions on all the six faces of the parallelepiped. A short outline of the boundary conditions is provided below while a detailed discussion about this is made in a later section. 

* Inlet plane : We specify the inlet velocity field on this plane. Turbulent fluctuations need to be specified realistically so that the flow inside the domain remains turbulent.
* Bottom wall : No-slip and impenetrability condition.
* Top surface : The free stream velocity specified here imposes the pressure gradient on the boundary layer. 
* On the two bounding $xy$ planes : The flow in the boundary layer is assumed to be two-dimensional in the mean. So the spanwise mean velocity $w=0$. The flow is assumed to be homogeneous in the z-direction. This leads to velocity on both the $xy$ planes being the same.
* Outlet plane : The velocity on the outlet plane $yz$ is not known *apriori*. So a convective outflow boundary condition is used. This is given by,

\begin{equation}
\frac{\partial u_i}{\partial t}+U_c \frac{\partial u_i}{\partial x_1}=0
\end{equation}

## Fractional step method/ Chorin's Projection method

The natural way to solve the NS equations as given in \eqref{eq:NS_cont} is to write them as second-order accurate, time-discrete semi-implicit form as below:

\begin{equation}\label{eq:discrete_NS}
\frac{\partial u_i^{n+1}}{\partial x_i} = 0
\end{equation}

\begin{equation}
\frac{u_i^{n+1} - u_i^n}{\Delta t}+\frac{\partial p^{n+1/2}}{\partial x_i} = -\frac{\partial (u_iu_j)}{\partial x_j}\biggr\rvert^{n+1/2} + \frac{1}{2Re} \frac{\partial^2}{\partial x_j \partial x_j}(u_i^{n+1}+u_i^n)
\end{equation}

with the boundary conditions,
\begin{equation}
u_i\biggr\rvert_b^{n+1} = ub_i\biggr\rvert^{n+1}
\end{equation}

where the time levels are denoted by superscripts ($t^n = n\Delta t$). The convective term is usually treated explicitly (using only $t^n$ values). Solving the spatially discrete forms of the above equations is not straightforward. The most common way to approach the solution of these equations is by using the fractional step method (Kim and Moin (1985)). This method was first introduced as the projection method by A.J.Chorin (1968) in his seminal paper and improved upon by later researchers (Le and Moin (1991), Verzicco and Ornaldi (JCP 1996)). 

In this method, we solve for an intermediate velocity field ($u^*$) using an analog of the momentum equation without regard to the divergence constraint (given in continuity equation). Then we project this intermediate velocity field onto the space of divergence-free fields to obtain $u_i^{n+1}$. The general procedure is given in steps below:

1. Solve the intermediate velocity equation,
    \begin{equation}\label{eq:frac_2}
    \frac{u_i^* - u_i^n}{\Delta t}+\frac{\partial p^{n}}{\partial x_i} = -\frac{\partial (u_iu_j)}{\partial x_j}\biggr\rvert^{n+1/2} + \frac{1}{2Re} \frac{\partial^2}{\partial x_j \partial x_j}(u_i^*+u_i^n)
    \end{equation}

    with the boundary conditions $$\mathbf{B}(u_i^*) = 0$$. Because we have included an approximation to the pressure gradient term (lags by a timestep), the boundary condition for $u_i^*$ is the same as that of NS velocity $u_i^{n+1}$ upto second order. (Brown, Cortez and Minion JCP2001)

1. Perform the projection
    \begin{equation}\label{eq:proj_eqn}
    u_i^* = u_i^{n+1} + \Delta t \frac{\partial \phi^{n+1}}{\partial x_i}
    \end{equation}
    Using the continuity equation, 
    \begin{equation}
    \frac{\partial u_i^{n+1}}{\partial x_i} = 0
    \end{equation}
    we can derive the Poisson equation for the correction pressure/pseudo-pressure ($\phi$) from this as,
    \begin{equation}\label{eq:phi_pois_eqn}
    \frac{\partial^2 }{\partial x_j \partial x_j}\phi^{n+1} =\frac{1}{\Delta t} \frac{\partial}{\partial x_i} u_i^*
    \end{equation}

1. Update the pressure
    \begin{equation}
    p^{n+1} = p^{n} + \phi^{n+1} - \frac{\Delta t}{2Re} \frac{\partial^2 }{\partial x_j \partial x_j} \phi^{n+1}
    \end{equation}
    Note that the last term includes all the diffusion terms because of the semi-implicit treatment of diffusion in all the directions in Eqn. \eqref{eq:discrete_NS}. In our code, we have implicit treatment only in the wall-normal ($y$) direction and only the wall-normal diffusion will appear in the above equation.
    Note that,
    \begin{equation}
    \frac{\partial p^{n+1}}{\partial x_i}  = \frac{\partial p^{n}}{\partial x_i} + \frac{\partial \phi^{n+1}}{\partial x_i} - \frac{\Delta t}{2Re} \frac{\partial^2 }{\partial x_j \partial x_j} \left( \frac{\partial \phi^{n+1}}{\partial x_i} \right)
    \end{equation}

The numerical procedure is then given as follows:
1. Calculate the auxiliary velocity field $u_i^*$ from equation \eqref{eq:frac_2} using the known value of $u_i$ at time $t$.
1. Then solve the Poisson's equation \eqref{eq:phi_pois_eqn} for pressure where $u_i^*$ is the field obtained from the previous step.
1. Use the auxiliary velocity field $u_i^*$ and pressure $p$ obtained from the above steps in equation \eqref{eq:proj_eqn} to get the final divergence free velocity field $u_i$ at time $t+\Delta t$.

## Calculation of the auxiliary velocity field

Third order Runge-Kutta method is used to solve equation \eqref{eq:frac_2} to get the auxiliary velocity field $u_i^*$. Let us start with equation \eqref{eq:frac_2},

$$ \begin{equation*}
\frac{\partial u_i^*}{\partial t}=\underbrace{-\frac{\partial p^{n}}{\partial x_i}}_{PG_i} \underbrace{-\frac{\partial (u_iu_j)}{\partial x_j}}_{A}+\underbrace{\frac{1}{Re} \frac{\partial^2 u_i}{\partial x_j \partial x_j}}_{D}
\end{equation*}$$  

{:refdef: style="text-align: center;"}
![](/assets/img/numflow/frac_time_step_drawing.png){:height="40%" width="40%"}  
*Fractional time stepping scheme*  
{: refdef}

<!-- \begin{figure}
\centering
\includegraphics[scale=0.5]{frac_time_step_drawing.png}
\caption{Fractional time stepping scheme}
\label{fig:frac_time_step}
\end{figure} -->
Figure \ref{fig:frac_time_step} shows the method we shall adopt for solving this equation. We do the projection at every substep now. Let the velocity field at time $t^n$ be $u_i^n$. The calculation of the convective/advective terms is usually done in an explicit approach. The formulation for the advection terms is given in the next section.

## Advection/Convection terms
The advection terms are given by:
\begin{equation}
A(u_i) = \frac{\partial (uu_i)}{\partial x} + \frac{\partial (vu_i)}{\partial y} + \frac{\partial (wu_i)}{\partial z} , \qquad i = 1,2,3
\end{equation}

The following sections explain how these are calculated on a fully staggered grid (shown in Fig. \ref{fig:staggered grid}). Note that we will assume that the grid in the $y$ direction is non-uniform. The locations (face centers) for the $v$ component is assumed to be specified as:
\begin{equation}
yv(j), \qquad  j = 0,1,2, ... ,j_{max}+1
\end{equation}
with $yv(0) = 0, \, yv(j_{max}) = L_y$. Then the cell centers are defined as the geometric center between the face locations:
\begin{equation}
yp(j) = \frac{1}{2}\left(yv(j) + yv(j-1)\right)
\end{equation}
In the $x$ and $z$ directions, the grid is uniform. The cell centers are again defined as the geometric center between the face locations in these directions as well.

\begin{equation}
xp(i) = \frac{1}{2}\left(xu(i) + xu(i-1)\right); \qquad zp(k) = \frac{1}{2}\left(zw(k) + zw(k-1)\right);
\end{equation}

It is customary to use the cell center indices as integers and the velocity locations (face centers) as staggered fractional locations in the literature. In what follows, the following holds true:

$$ \begin{equation*}
x_i = xp(i), \qquad x_{i-\frac{1}{2}} = xu(i);
\end{equation*}$$  

$$ \begin{equation*}
y_j = yp(j), \qquad y_{j-\frac{1}{2}} = yv(j);
\end{equation*}$$  

$$ \begin{equation*}
z_k = zp(k), \qquad z_{k-\frac{1}{2}} = zw(k);
\end{equation*}$$  

To help visualizing, the staggered grid in the $xy$ - plane is shown in figure \ref{fig:staggered_xy}.  

{:refdef: style="text-align: center;"}
![](/assets/img/numflow/staggered_xy.png){:height="35%" width="35%"}  
*Staggered grid in the $xy$-plane*  
{: refdef}

<!-- \begin{figure}
\centering
\includegraphics[scale=0.85]{staggered_xy.pdf}
\caption{Staggered grid in the $xy$-plane}
\label{fig:staggered_xy}
\end{figure} -->

The numerical scheme (both derivative calculation and the interpolation) used are chosen to be conservative (for both momentum and energy) in the discrete sense. Reference can be made to Ham, et. al. (JCP2002), Fukagata (JCP2002).

The first derivative will be evaluated using:
\begin{equation}
\frac{d\phi}{dx}\biggr \rvert_{i} = \frac{\phi_{i+1/2} - \phi_{i-1/2}}{ x_{i+1/2} - x_{i-1/2}  }
\end{equation}
where $i$ can be cell center or face center.
When a quantity is needed where it is not located, we shall use interpolation from neighbouring points to obtain the quantity value. We shall use two methods of interpolation:
\begin{equation}
\text{Arithmetic average:} \qquad \overline{\phi}^x \rvert_i  = \frac{\phi_{i+1/2} + \phi_{i-1/2}}{2}
\end{equation}
\begin{equation}
\text{Volume weighted average:} \qquad\check{\phi}^x \rvert_i =  \frac{(x_{i+1/2} - x_i)\phi_{i+1/2} + (x_i - x_{i-1/2})\phi_{i-1/2}}{x_{i+1/2} -  x_{i-1/2}}
\end{equation}

To have conservative numerical scheme, we must use simple arithmetic average for the *advected* velocity and volume weighted average for the *advection* velocity.
Note that when evaluated on a cell-centered location, both the interpolation methods are the same. This is due to the fact that the cell centers are geometric mid-point of the face centers.

Another approach based on symmetry-preserving finite volume discretization of the convective terms by Vreman (JCP2014) and Verstappen and Veldman (JCP 2003). This scheme is given by:
\begin{equation}
\text{Symmetry-preserving average:} \qquad\check{\phi}^x \rvert_i =  \frac{(x_{i+1} - x_i)\phi_{i+1/2} + (x_i - x_{i-1})\phi_{i-1/2}}{2(x_{i+1/2} -  x_{i-1/2})}
\end{equation}

### Advection of x-momentum

Let us first consider the advection of x-momentum. The terms are:  

$$ \begin{equation}
A(u)\biggr \rvert_{(i+1/2, j, k)} = \underbrace{\frac{\partial}{\partial x}(u.u)}_{Ax_1} + \underbrace{\frac{\partial}{\partial y}(v.u)}_{Ax_2} + \underbrace{\frac{\partial}{\partial z}(w.u)}_{Ax_3}
\end{equation}$$  

#### $\mathbf{Ax_1}$
  
$$ \begin{align*}
Ax_1 = \frac{\partial}{\partial x}(u.u)\biggr \rvert_{i+1/2} &= \frac{\check{u}^x_{i+1} \overline{u}^x_{i+1} - \check{u}^x_{i} \overline{u}^x_{i}}{x_{i+1} - x_i}\\
&= \frac{1}{4}\frac{(u_{i+3/2} + u_{i+1/2})^2 - (u_{i+1/2} + u_{i-1/2})^2}{x_{i+1} - x_i} \\
\end{align*}$$  
Note that all the velocities in the above expression are evaluated at given $y$ and $z$ coordinates, namely at $j$ and $k$.

#### $\mathbf{Ax_2}$

$$\begin{align*}
Ax_2 = \frac{\partial}{\partial y}(v.u)\biggr \rvert_{i+1/2 , j} &= \frac{\check{v}^x_{i+1/2, j+1/2} \overline{u}^y_{i+1/2, j+1/2} - \check{v}^x_{i+1/2, j-1/2} \overline{u}^y_{i+1/2, j-1/2}}{y_{j+1/2} - y_{j-1/2}}\\
\end{align*}$$  

$$\begin{align*}
= \frac{1}{4}\frac{(v_{i,j+1/2} + v_{i+1,j+1/2})(u_{i+1/2,j} + u_{i+1/2,j+1}) - (v_{i,j-1/2} + v_{i+1,j-1/2})(u_{i+1/2,j} + u_{i+1/2,j-1})}{y_{j+1/2} - y_{j-1/2}}
\end{align*}$$

#### $\mathbf{Ax_3}$

$$\begin{align*}
Ax_3 = \frac{\partial}{\partial z}(w.u)\biggr \rvert_{i+1/2 , k} &= \frac{\check{w}^x_{i+1/2, k+1/2} \overline{u}^z_{i+1/2, k+1/2} - \check{w}^x_{i+1/2, k-1/2} \overline{u}^z_{i+1/2, k-1/2}}{z_{k+1/2} - z_{k-1/2}}\\
\end{align*}$$  

$$\begin{align*}
= \frac{1}{4}\frac{(w_{i,k+1/2} + w_{i+1,k+1/2})(u_{i+1/2,k} + u_{i+1/2,k+1}) - (w_{i,k-1/2} + w_{i+1,k-1/2})(u_{i+1/2,k} + u_{i+1/2,k-1})}{z_{k+1/2} - z_{k-1/2}}
\end{align*}$$  

### Advection of y-momentum

The y-momentum advection is given by,  
$$ \begin{equation}
A(v)\biggr \rvert_{(i, j+1/2, k)} = \underbrace{\frac{\partial}{\partial x}(u.v)}_{Ay_1} + \underbrace{\frac{\partial}{\partial y}(v.v)}_{Ay_2} + \underbrace{\frac{\partial}{\partial z}(w.v)}_{Ay_3}
\end{equation}$$  

#### $\mathbf{Ay_1}$

$$\begin{align*}
Ay_1 = \frac{\partial}{\partial x}(u.v)\biggr \rvert_{i, j+1/2} &= \frac{\check{u}^y_{i+1/2, j+1/2} \overline{v}^x_{i+1/2, j+1/2} - \check{u}^y_{i-1/2, j+1/2} \overline{v}^x_{i-1/2, j+1/2}}{x_{i+1/2} - x_{i-1/2}}\\
\end{align*}$$  

$$ \begin{align*}
\check{u}^y_{i+1/2, j+1/2} &= \frac{(y_{j+1} - y_{j+1/2})u_{i+1/2,j+1} + (y_{j+1/2} - y_j)u_{i+1/2,j} }{y_{j+1} - y_j}\\
\overline{v}^x_{i+1/2, j+1/2} &= \frac{v_{i,j+1/2} + v_{i+1,j+1/2}}{2}\\
\check{u}^y_{i-1/2, j+1/2} &= \frac{(y_{j+1} - y_{j+1/2})u_{i-1/2,j+1} + (y_{j+1/2} - y_j)u_{i-1/2,j} }{y_{j+1} - y_j}\\
\overline{v}^x_{i-1/2, j+1/2} &= \frac{v_{i,j+1/2} + v_{i-1,j+1/2}}{2}\\
\end{align*}$$  

#### $\mathbf{Ay_2}$

$$ \begin{align*}
Ay_2 = \frac{\partial}{\partial y}(v.v)\biggr \rvert_{j+1/2} &= \frac{\check{v}^y_{j+1} \overline{v}^y_{j+1} - \check{v}^y_{j} \overline{v}^y_{j}}{y_{j+1} - y_j}\\
&= \frac{1}{4}\frac{(v_{j+3/2} + v_{j+1/2})^2 - (v_{j+1/2} + v_{j-1/2})^2}{y_{i+1} - y_i} \\
\end{align*}$$  

As the interpolation is to be done at the cell center location, the simple arithmetic average is the same as the volume weighted average. 

#### $\mathbf{Ay_3}$

$$ \begin{align*}
Ay_3 = \frac{\partial}{\partial z}(w.v)\biggr \rvert_{j+1/2, k} &= \frac{\check{w}^y_{j+1/2, k+1/2} \overline{v}^z_{j+1/2, k+1/2} - \check{w}^y_{j+1/2, k-1/2} \overline{v}^z_{j+1/2, k-1/2}}{z_{k+1/2} - z_{k-1/2}}\\
\end{align*}$$  

$$ \begin{align*}
\check{w}^y_{j+1/2, k+1/2} &= \frac{(y_{j+1} - y_{j+1/2})w_{j+1,k+1/2} + (y_{j+1/2} - y_j)w_{j,k+1/2} }{y_{j+1} - y_j}\\
\overline{v}^z_{j+1/2, k+1/2} &= \frac{v_{j+1/2,k} + v_{j+1/2,k+1}}{2}\\
\check{w}^y_{j+1/2, k-1/2} &= \frac{(y_{j+1} - y_{j+1/2})w_{j+1,k-1/2} + (y_{j+1/2} - y_j)w_{j,k-1/2} }{y_{j+1} - y_j}\\
\overline{v}^w_{j+1/2, k-1/2} &= \frac{v_{j+1/2,k} + v_{j+1/2,k-1}}{2}\\
\end{align*}$$  

### Advection of z-momentum

The z-momentum advection is given by,  
$$ \begin{equation}
A(w)\biggr \rvert_{(i, j, k+1/2)} = \underbrace{\frac{\partial}{\partial x}(u.w)}_{Az_1} + \underbrace{\frac{\partial}{\partial y}(v.w)}_{Az_2} + \underbrace{\frac{\partial}{\partial z}(w.w)}_{Az_3}
\end{equation}$$  

#### $\mathbf{Az_1}$

$$ \begin{align*}
Az_1 = \frac{\partial}{\partial x}(u.w)\biggr \rvert_{i, k+1/2} &= \frac{\check{u}^z_{i+1/2, k+1/2} \overline{w}^x_{i+1/2, k+1/2} - \check{u}^z_{i-1/2, k+1/2} \overline{w}^x_{i-1/2, k+1/2}}{x_{i+1/2} - x_{i-1/2}}\\
\end{align*}$$  

$$ \begin{align*}
= \frac{1}{4}\frac{(u_{i+1/2,k} + u_{i+1/2,k+1})(w_{i,k+1/2} + w_{i+1,k+1/2}) - (u_{i-1/2,k} + u_{i-1/2,k+1})(w_{i,k+1/2} + w_{i-1,k+1/2})}{x_{i+1/2} - x_{i-1/2}}
\end{align*}$$  

#### $\mathbf{Az_2}$

$$ \begin{align*}
Az_2 = \frac{\partial}{\partial y}(v.w)\biggr \rvert_{j, k+1/2} &= \frac{\check{v}^z_{j+1/2, k+1/2} \overline{w}^y_{j+1/2, k+1/2} - \check{v}^z_{j-1/2, k+1/2} \overline{w}^y_{j-1/2, k+1/2}}{y_{j+1/2} - y_{j-1/2}}\\
\end{align*}$$  

$$ \begin{align*}
= \frac{1}{4}\frac{(v_{j+1/2,k} + v_{j+1/2,k+1})(w_{j,k+1/2} + w_{j+1,k+1/2}) - (v_{j-1/2,k} + v_{j-1/2,k+1})(w_{j,k+1/2} + w_{j-1,k+1/2})}{y_{j+1/2} - y_{j-1/2}}
\end{align*}$$  

#### $\mathbf{Az_3}$

$$ \begin{align*}
Az_3 = \frac{\partial}{\partial z}(w.w)\biggr \rvert_{k+1/2} &= \frac{\check{w}^z_{k+1} \overline{w}^z_{k+1} - \check{w}^z_{k} \overline{w}^z_{k}}{z_{k+1} - z_k}\\
&= \frac{1}{4}\frac{(w_{k+3/2} + w_{k+1/2})^2 - (w_{k+1/2} + w_{k-1/2})^2}{z_{k+1} - z_k} \\
\end{align*}$$  

## Diffusion terms

Depending on the treatment of the viscous term, there are three approaches possible:
1. Explicit diffusion treatment in all directions
1. Semi-implicit in wall-normal ($y$) direction and explicit in $x$ and $z$-directions
1. Semi-implicit diffusion treatment in all directions

The formulation of each approach is described in the following sections.

### Explicit diffusion

### Semi-implicit wall-normal diffusion

We shall use implicit formulation only for the wall-normal diffusion term. All other terms will be treated explicitly. So, it is useful to rename the terms as follows.  
$$\begin{equation}
\frac{\partial u_i^*}{\partial t}=\underbrace{-\frac{\partial p^{n}}{\partial x_i}}_{PG_i} \underbrace{-\frac{\partial (u_iu_j)}{\partial x_j}}_{A}+\underbrace{\frac{1}{Re} \left(\frac{\partial^2 u_i}{\partial x_1^2} +\frac{\partial^2 u_i}{\partial x_3^2} \right)}_{D_1}+\underbrace{\frac{1}{Re} \frac{\partial^2 u_i}{\partial x_2^2}}_{D_2}
\end{equation}$$ 

Every substep of the the RK3 method provides a intermediate velocity field (namely, $u_i^{(1)}, u_i^{(2)}, u_i^{(3)}$) which are calculated as follows:

1. Obtain $u_i^{(1)}$ from:
    \begin{equation}
    \frac{u_i^{(1)}-u_i^n}{\Delta t}= (\alpha^1+\beta^1) PG_i +\gamma^1 \left(A(u_i^n) + D_1(u_i^n)\right)+\alpha^1 D_2(u_i^n)+\beta^1 D_2(u_i^{(1)})
    \end{equation}
1. Obtain $u_i^{(2)}$ from:
    \begin{equation}
    \frac{u_i^{(2)}-u_i^1}{\Delta t}=(\alpha^2+\beta^2) PG_i +\gamma^2 \left(A(u_i^1)+ D_1(u_i^1)\right) +\zeta^2 \left(A(u_i^n) + D_1(u_i^n)\right) +\alpha^2 D_2(u_i^1)+\beta^2 D_2(u_i^{(2)})
    \end{equation}
1. Obtain $u_i^*$ from:
    \begin{equation}
    \frac{u_i^{(3)}-u_i^2}{\Delta t}=(\alpha^3+\beta^3) PG_i +\gamma^3 \left(A(u_i^2)+ D_1(u_i^2) \right) +\zeta^3 \left(A(u_i^1) + D_1(u_i^1)\right)+\alpha^3 D_2(u_i^2)+\beta^3 D_2(u_i^{(3)})
    \end{equation}

The constants $\alpha,\beta,\gamma,\zeta$ are given by:
$$\begin{equation}
\alpha^1=\frac{4}{15}; \qquad \beta^1=\frac{4}{15}; \qquad \gamma^1=\frac{8}{15}
\end{equation}$$  
$$\begin{equation}
\alpha^2=\frac{1}{15}; \qquad \beta^2=\frac{1}{15}; \qquad \gamma^2=\frac{5}{12}; \qquad \zeta^2=\frac{-17}{60}
\end{equation}$$  
$$\begin{equation}
\alpha^3=\frac{1}{6}; \qquad \beta^3=\frac{1}{6}; \qquad \gamma^3=\frac{3}{4}; \qquad \zeta^3=\frac{-5}{12}
\end{equation}$$  

As an example, let us go through the Step *2* in detail. Let us rewrite the time advancement equation in that step as,  
$$\begin{align*}
\frac{u_i^{(2)}-u_i^1}{\Delta t}&=(\alpha^2+\beta^2) PG_i +\gamma^2 \left(A(u_i^1)+ D_1(u_i^1)\right) +\zeta^2 \left(A(u_i^n) + D_1(u_i^n)\right) +\alpha^2 D_2(u_i^1)+\beta^2 D_2(u_i^{(2)})\\
&= \text{dudt}+\beta^2 D_2(u_i^{(2)}) 
\end{align*}$$  
where, $\text{dudt}=(\alpha^2+\beta^2) PG_i +\gamma^2 \left(A(u_i^1)+ D_1(u_i^1)\right) +\zeta^2 \left(A(u_i^n) + D_1(u_i^n)\right) +\alpha^2 D_2(u_i^1)$.

Let us denote $u_i^{(2)}=u_i, \, u_i^1=u_i^0, \, \alpha^2 =\alpha, \,\beta^2=\beta $. Thus we have,  

$$ \begin{equation*}
u_i=u_i^0 + \Delta t\times \text{dudt} + \beta \Delta t D_2(u_i)
\end{equation*}$$  
$$\begin{equation}\label{eq:predicv_velocity}
u_i- \beta \Delta t D_2(u_i)=u_i^0 + \Delta t\times \text{dudt}
\end{equation}$$

This equation will lead to a system of tridiagonal linear equations because of the implicit formulation of the operator $D_2$ on the LHS.  

#### Solving for the auxiliary velocity field - $u$ and $w$ components

Let us first consider the x-component velocity ($i=1,\, u_i = u$). All that follows will be equally applicable for the z-component velocity $w$ as well since both these are located on the same $y$ coordinate level. We shall make a note about the y-component velocity at the end of this section. For the x-component velocity, we have from Eqn. \ref{eq:predicv_velocity},  
\begin{equation} \label{eq:predicv_u}
u(i,j,k)- \frac{\beta \Delta t}{Re} \frac{\partial^2 u}{\partial y^2}\biggr\rvert_j=u^0(i,j,k) + \Delta t\times \text{dudt}(i,j,k)
\end{equation}  
Rewriting,  
\begin{equation}
u(i,j,k)- \frac{\beta \Delta t}{Re} \frac{\partial^2 u}{\partial y^2}\biggr\rvert_j= \text{rhsu}(j)
\end{equation}  
It helps to multiply throughout by $-\frac{Re}{\beta \Delta t}$ to give,  
\begin{equation}
-\frac{Re}{\beta \Delta t} u(i,j,k)+ \frac{\partial^2 u}{\partial y^2}\biggr\rvert_j= -\frac{Re}{\beta \Delta t} \times \text{rhsu}(j)
\end{equation}  
Absorbing the multiplier into the quantity $\text{rhsu}(j)$, we get,  
\begin{equation}
-\frac{Re}{\beta \Delta t} u(i,j,k)+ \frac{\partial^2 u}{\partial y^2}\biggr\rvert_j= \text{rhsu}(j)
\end{equation}  
Let us consider the 3-point stencil $u(i,j-1,k),u(i,j,k),u(i,j+1,k)$ to construct the second derivative of $u$ on the non-uniform grid $yp$. (Note: $yp(j)=0.5 \times(yv(j)+yv(j-1))$)  
\begin{equation}
\frac{\partial^2 u}{\partial y^2}\biggr\rvert_j = \frac{\frac{u_{j+1}-u_j}{yp(j+1)-yp(j)}-\frac{u_j-u_{j-1}}{yp(j)-yp(j-1)}}{yv(j)-yv(j-1)}
\end{equation}  

Let the grid spacing be denoted as, $\Delta yp_j=yp(j)-yp(j-1), \Delta yp_{j+1}=yp(j+1)-yp(j), \Delta yv_j=yv(j)-yv(j-1)$. Simplifying,  
\begin{equation}
\frac{\partial^2 u}{\partial y^2}\biggr\rvert_j = \frac{1}{\Delta yp_{j+1} \Delta yv_j} u_{j+1} - \left( \frac{1}{\Delta yp_{j+1}\Delta yv_j}+\frac{1}{\Delta yp_{j}\Delta yv_j}\right) u_j + \frac{1}{\Delta yp_{j} \Delta yv_j} u_{j-1}
\end{equation}  

Defining $au(j)= \frac{1}{\Delta yp_{j} \Delta yv_j}, \, cu(j)= \frac{1}{\Delta yp_{j+1} \Delta yv_j},  \, \overline{bu}(j)= - (au(j) + cu(j)) $, we have,  

$$\begin{equation*}
-\frac{Re}{\beta \Delta t} u_j + au(j)  u_{j-1} + \overline{bu}(j)  u_j + cu(j)  u_{j+1} = \text{rhsu}(j)
\end{equation*}$$  

Introducing a modified $bu$ as $bu(j) = \overline{bu}(j) - \frac{Re}{\beta \Delta t}$, we have,  
\begin{equation}\label{eq:predicv_u_discrete}
 au(j)  u_{j-1} + bu(j)  u_j + cu(j)  u_{j+1} = \text{rhsu}(j), \qquad j=1,2,..,j_{max}
\end{equation}

#### Dirichlet boundary condition

For $j=1$ and $j=j_{max}$ in \eqref{eq:predicv_u_discrete}, we need the boundary conditions in the adjacent points ($j=0$ and $j=j_{max}+1$). Because the $u$ and $w$-components are staggered with respect to the top and bottom planes, there are two ways in which the BC can be implemented:

1. Calculate the value of these velocities at halo points ($j=0$ and $j=j_{max}+1$) based on the given BC values using linear interpolation
1. Redefine the $y$-locations corresponding to $j=0$ and $j=j_{max}+1$ so that they coincide with the BC plane and use the BC values directly

Approach 2 leads to improved accuracy and we shall describe its implementation below. Let us assume we have Dirichlet boundary conditions on both these points. i.e.,  

$$
\begin{align}
u_0 &= \text{ubound\textunderscore bottom}\\
u_{j_{max}+1} &= \text{ubound\textunderscore top} 
\end{align}$$  

Note that because of choosing the end points as the boundary points, we have:  
$$ \begin{align}
\Delta yp_1 = yp_1 - yv_0; \qquad  \Delta yv_1 = yv_1 - \frac{yp_1+yv_0}{2}\\
\Delta yp_{j_{max}+1} = yv_{j_{max}} - yp_{j_{max}}; \qquad  \Delta yv_1 = \frac{yp_{j_{max}}+yv_{j_{max}}}{2} - yv_{j_{max}-1}\\
 \end{align}$$  
 
At these two points, \autoref{eq:predicv_u_discrete} reduces to,  

$$ \begin{align*}
au(1)  u_0 + bu(1)  u_1 + cu(1)  u_2 &= \text{rhsu}(1) \\ 
au(j_{max})  u_{j_{max}-1} + bu(j_{max})  u_{j_{max}} + cu(j_{max})  u_{j_{max}+1} &= \text{rhsu}(j_{max}) 
\end{align*}$$

$$ \begin{align*}
au(1) \times \text{ubound\textunderscore bottom} + bu(1)  u_1 + cu(1)  u_2 &= \text{rhsu}(1) \\ 
au(j_{max})  u_{j_{max}-1} + bu(j_{max})  u_{j_{max}} + cu(j_{max}) \times \text{ubound\textunderscore top} &= \text{rhsu}(j_{max}) 
\end{align*}$$

Rearranging,  

$$ \begin{align*}
bu(1) u_1 + cu(1)  u_2 &= \text{rhsu}(1) - au(1) \times \text{ubound\textunderscore bottom}   \\ 
au(j_{max})  u_{j_{max}-1} + bu(j_{max}) u_{j_{max}} &= \text{rhsu}(j_{max}) -  cu(j_{max})  \times \text{ubound\textunderscore top} 
\end{align*}$$

Let us redefine the terms as follows,  

$$ \begin{align*}
\text{rhsu}(1) := \text{rhsu}(1) - au(1) \times \text{ubound\textunderscore bottom}   \\
\text{rhsu}(j_{max}) := \text{rhsu}(j_{max})- cu(j_{max})  \times \text{ubound\textunderscore top}
\end{align*}$$  

Similar expressions are obtained for the $w$-component equation as well. These are:  
\begin{equation}\label{eq:predicv_w_discrete}
 aw(j)  w_{j-1} + bw(j)  w_j + cw(j)  w_{j+1} = \text{rhsw}(j), \qquad j=1,2,..,j_{max}
\end{equation}  
After taking into account the modified grid spacing at the top and bottom, we have:  

$$\begin{align*}
\text{rhsw}(1) := \text{rhsw}(1) - aw(1) \times \text{wbound\textunderscore bottom}   \\
\text{rhsw}(j_{max}) := \text{rhsw}(j_{max})- cw(j_{max})  \times \text{wbound\textunderscore top}
\end{align*}$$  

#### Neumann boundary condition

If we have the wall-normal gradient of velocity specified at the boundary, then,  
\begin{equation}
\frac{\partial u}{\partial y} \biggr\vert_{top}=\frac{u_{j_{max}+1}-u_{j_{max}}}{yp(j_{max}+1)-yp(j_{max})} = \frac{u_{j_{max}+1}-u_{j_{max}}}{\Delta yp_{j_{max}+1}}
\end{equation}  
The value $u_{j_{max}+1}$ is a ghost cell value at the location $y_{j_{max}+1}$. Thus,  
\begin{equation}
u_{j_{max}+1}=u_{j_{max}}+\frac{\partial u}{\partial y} \biggr\vert_{top}\Delta yp_{j_{max}+1}
\end{equation}  

The \eqref{eq:predicv_u_discrete} at $y=yp_{j_{max}}$ becomes,  
\begin{equation}
 au(j_{max})  u_{j_{max}-1} + bu(j_{max})  u_{j_{max}} + cu(j_{max})  u_{j_{max}+1} = \text{rhsu}(j_{max})
\end{equation}  
Substituting the value at the ghost cell from the boundary condition, we have,  

$$ \begin{equation*}
 au(j_{max})  u_{j_{max}-1} + bu(j_{max})  u_{j_{max}} + cu(j_{max}) \left( u_{j_{max}}+\frac{\partial u}{\partial y} \biggr\vert_{top}\Delta yp_{j_{max}+1} \right) = \text{rhsu}(j_{max})
\end{equation*}$$  

\begin{equation}
 au(j_{max})  u_{j_{max}-1} + \left[bu(j_{max})+cu(j_{max}) \right]  u_{j_{max}} = \text{rhsu}(j_{max})- cu(j_{max}) \left( \frac{\partial u}{\partial y} \biggr\vert_{top}\Delta yp_{j_{max}+1} \right)
\end{equation}  

Let us redefine the terms as follows,  

$$\begin{align*}
bu(j_{max}):= bu(j_{max})+ cu(j_{max})\\
\text{rhsu}(j_{max}) := \text{rhsu}(j_{max})  - cu(j_{max}) \left( \frac{\partial u}{\partial y} \biggr\vert_{top}\Delta yp_{j_{max}+1} \right)
\end{align*}$$  

With these changes in mind for the boundary conditions, we can write the tridiagonal system of equations as,  

$$ \begin{align*}
\begin{bmatrix}
bu(1)&cu(1)& & & & & & & \\
au(2)&bu(2)&cu(2)& & & & & & \\
 &\centerdot&\centerdot&\centerdot& & & & & \\
 & &\centerdot&\centerdot&\centerdot& & & & \\
 & & &au(j)&bu(j)&cu(j)& & & & \\
 & & & &\centerdot&\centerdot&\centerdot& & \\
 & & & & &\centerdot&\centerdot&\centerdot& \\
 & & & & & &au(j_{max}-1)&bu(j_{max}-1)&cu(j_{max}-1)\\  
 & & & & & & &au(j_{max})&bu(j_{max})
\end{bmatrix}\\
\times \begin{bmatrix}
u_1\\ u_2\\ u_3\\ \centerdot \\ \centerdot \\ \centerdot \\ \centerdot \\ u_{j_{max}-1} \\ u_{j_{max}}
\end{bmatrix}=\begin{bmatrix}
\text{rhsu}(1)\\ \text{rhsu}(2)\\ \text{rhsu}(3)\\ \centerdot \\ \centerdot \\ \centerdot \\ \centerdot \\ \text{rhsu}(j_{max}-1) \\ \text{rhsu}(j_{max})
\end{bmatrix}
\end{align*}$$  

By solving the above tridiagonal system (using Thomas' algorithm), we can get the velocity field $u^{(2)}$ which can then be used in **Step 3**.

#### Solving for the auxiliary velocity field - $v$ component
For the y-component velocity $v$ ($i=2, u_i=v$), the y-grid is slightly different because of staggering. The grid is given by,  

$$ \begin{align}
yv(0)=0; \qquad yv(j)&=yv(j-1)+ \Delta yv(j), \qquad j=1,2,..,j_{max} \\
yv(j_{max})&=\overline{ly}
\end{align}$$  
Note that the points $yv(0)$ and $yv(j_{max})$ lie exactly on the boundary of the domain.  

For the y-component velocity, we have from Eqn. \ref{eq:predicv_velocity},  
\begin{equation}
v(i,j,k)- \frac{\beta \Delta t}{Re} \frac{\partial^2 v}{\partial y^2}\biggr\rvert_j=v^0(i,j,k) + \Delta t\times \text{dvdt}(i,j,k)
\end{equation}

Rewriting as done for the $u$-component equation, we get,  
\begin{equation}
-\frac{Re}{\beta \Delta t} v(i,j,k)+ \frac{\partial^2 v}{\partial y^2}\biggr\rvert_j= \text{rhsv}(j)
\end{equation}  
where $\text{rhsv}(j) = -\frac{Re}{\beta \Delta t} \times \left(v^0(i,j,k) + \Delta t\times \text{dvdt}(i,j,k) \right)$.

Let us consider the 3-point stencil $v(i,j-1,k),v(i,j,k),v(i,j+1,k)$ to construct the second derivative of $v$ on the non-uniform grid $yv$. (Note: $yp(j)=0.5 \times(yv(j)+yv(j-1))$)
\begin{equation}
\frac{\partial^2 v}{\partial y^2}\biggr\rvert_j = \frac{\frac{v_{j+1}-v_j}{yv(j+1)-yv(j)}-\frac{v_j-v_{j-1}}{yv(j)-yv(j-1)}}{yp(j+1)-yp(j)}
\end{equation}

Let the grid spacing be denoted as, $\Delta yv_{j+1}=yv(j+1)-yv(j), \Delta yv_j=yv(j)-yv(j-1), \Delta yp_{j+1}=yp(j+1)-yp(j)$. Simplifying,
\begin{equation}
\frac{\partial^2 v}{\partial y^2}\biggr\rvert_j = \frac{1}{\Delta yp_{j+1} \Delta yv_{j+1}} v_{j+1} - \left( \frac{1}{\Delta yp_{j+1}\Delta yv_{j+1}}+\frac{1}{\Delta yp_{j+1}\Delta yv_j}\right) v_j + \frac{1}{\Delta yp_{j+1} \Delta yv_j} v_{j-1}
\end{equation}

Defining $av(j)= \frac{1}{\Delta yp_{j+1} \Delta yv_j}, \, cv(j)= \frac{1}{\Delta yp_{j+1} \Delta yv_{j+1}},  \, \overline{bv}(j)= - (av(j) + cv(j)) $, we have,  

$$\begin{equation*}
-\frac{Re}{\beta \Delta t} v_j + av(j)  v_{j-1} + \overline{bv}(j)  v_j + cv(j)  v_{j+1} = \text{rhsv}(j)
\end{equation*}$$  

Introducing a modified $bv$ as $bv(j) = \overline{bv}(j) - \frac{Re}{\beta \Delta t}$, we have,
\begin{equation}\label{eq:predicv_v_discrete}
 av(j)  v_{j-1} + bv(j)  v_j + cv(j)  v_{j+1} = \text{rhsv}(j), \qquad j=1,2,..,j_{max}-1
\end{equation}

Note that we have $(j_{max}-1)$ linear equations to solve for the $(j_{max}-1)$ unknowns $v_j, \, j=1,2,..,(j_{max}-1)$, one less than what we had in the x- and z- component cases.

#### Dirichlet boundary condition
If we assume Dirichlet boundary conditions for $v$, then,  

$$ \begin{align}
v_0 &=\text{vbound\textunderscore bottom} \\
v_{j_{max}}&=\text{vbound\textunderscore top}
\end{align}$$  

For $j=1$ and $j=(j_{max}-1)$, we have,  

$$ \begin{align*}
av(1)  v_0 + bv(1)  v_1 + cv(1)  v_2 &= \text{rhsv}(1) \\
av(j_{max}-1)  v_{j_{max}-2} + bv(j_{max}-1)  v_{j_{max}-1} + cv(j_{max}-1)  v_{j_{max}} &= \text{rhsv}(j_{max}-1)
\end{align*}$$  

Substituting the boundary conditions and rearranging,  

$$ \begin{align*}
bv(1)  v_1 + cv(1)  v_2 &= \text{rhsv}(1) - av(1) \times \text{vbound\textunderscore bottom}  \\
av(j_{max}-1)  v_{j_{max}-2} + bv(j_{max}-1)  v_{j_{max}-1}  &= \text{rhsv}(j_{max}-1) - cv(j_{max}-1) \times  \text{vbound\textunderscore top}
\end{align*}$$  

Let us redefine the right hand sides of the above equations as,  

$$ \begin{align*}
\text{rhsv}(1) := \text{rhsv}(1) - av(1) \times \text{vbound\textunderscore bottom} \\ 
\text{rhsv}(j_{max}-1) := \text{rhsv}(j_{max}-1) - cv(j_{max}-1) \times \text{vbound\textunderscore top} 
\end{align*}$$  

With these modifications in mind, we can write the tridiagonal system as,  

$$ \begin{align*}
\begin{bmatrix}
bv(1)&cv(1)& & & & & & & \\
av(2)&bv(2)&cv(2)& & & & & & \\
 &\centerdot&\centerdot&\centerdot& & & & & \\
 & &\centerdot&\centerdot&\centerdot& & & & \\
 & & &av(j)&bv(j)&cv(j)& & & & \\
 & & & &\centerdot&\centerdot&\centerdot& & \\
 & & & & &\centerdot&\centerdot&\centerdot& \\
 & & & & & &av(j_{max}-2)&bv(j_{max}-2)&cv(j_{max}-2)\\  
 & & & & & & &av(j_{max}-1)&bv(j_{max}-1)
\end{bmatrix}\\
\times \begin{bmatrix}
v_1\\ v_2\\ v_3\\ \centerdot \\ \centerdot \\ \centerdot \\ \centerdot \\ v_{j_{max}-2} \\ v_{j_{max}-1}
\end{bmatrix}=\begin{bmatrix}
\text{rhsv}(1)\\ \text{rhsv}(2)\\ \text{rhsv}(3)\\ \centerdot \\ \centerdot \\ \centerdot \\ \centerdot \\ \text{rhsv}(j_{max}-2) \\ \text{rhsv}(j_{max}-1)
\end{bmatrix}
\end{align*}$$  

By solving the above tridiagonal system (using Thomas' algorithm), we can get the velocity field $v^{(2)}$ which can then be used in **Step 3**. 

### Semi-implicit diffusion in all directions
In this approach, we will use semi-implicit diffusion treatment in all three directions. So, it is useful to rename the terms as follows.  
$$\begin{equation}
\frac{\partial u_i^*}{\partial t}=\underbrace{-\frac{\partial p^{n}}{\partial x_i}}_{PG_i} \underbrace{-\frac{\partial (u_iu_j)}{\partial x_j}}_{A}+\underbrace{\frac{1}{Re} \left(\frac{\partial^2 u_i}{\partial x_1^2} + \frac{\partial^2 u_i}{\partial x_2^2} + \frac{\partial^2 u_i}{\partial x_3^2} \right)}_{D}
\end{equation}$$
Every substep of the the RK3 method provides a intermediate velocity field (namely, $u_i^{(1)}, u_i^{(2)}, u_i^{(3)}$) which are calculated as follows:  

1. Obtain $u_i^{(1)}$ from:  
    \begin{equation}
    \frac{u_i^{(1)}-u_i^n}{\Delta t}= (\alpha^1+\beta^1) PG_i +\gamma^1 A(u_i^n)+\alpha^1 D(u_i^n)+\beta^1 D(u_i^{(1)})
    \end{equation}
1. Obtain $u_i^{(2)}$ from:  
    \begin{equation}
    \frac{u_i^{(2)}-u_i^1}{\Delta t}=(\alpha^2+\beta^2) PG_i +\gamma^2 A(u_i^1) +\zeta^2 A(u_i^n) +\alpha^2 D(u_i^1)+\beta^2 D(u_i^{(2)})
    \end{equation}
1. Obtain $u_i^*$ from:  
    \begin{equation}
    \frac{u_i^{(3)}-u_i^2}{\Delta t}=(\alpha^3+\beta^3) PG_i +\gamma^3 A(u_i^2) +\zeta^3 A(u_i^1) +\alpha^3 D(u_i^2)+\beta^3 D(u_i^{(3)})
    \end{equation}

The constants $\alpha,\beta,\gamma,\zeta$ are given by:  
$$\begin{equation}
\alpha^1=\frac{4}{15}; \qquad \beta^1=\frac{4}{15}; \qquad \gamma^1=\frac{8}{15}
\end{equation}$$
$$\begin{equation}
\alpha^2=\frac{1}{15}; \qquad \beta^2=\frac{1}{15}; \qquad \gamma^2=\frac{5}{12}; \qquad \zeta^2=\frac{-17}{60}
\end{equation}$$
$$\begin{equation}
\alpha^3=\frac{1}{6}; \qquad \beta^3=\frac{1}{6}; \qquad \gamma^3=\frac{3}{4}; \qquad \zeta^3=\frac{-5}{12}
\end{equation}$$  

As an example, let us go through the **Step 2** in detail. Let us rewrite the time advancement equation in that step as, (Note: $\alpha^k =\beta^k$)  

$$\begin{align*}
\frac{u_i^{(2)}-u_i^1}{\Delta t}&=(\alpha^2+\beta^2) PG_i +\gamma^2 A(u_i^1) +\zeta^2 A(u_i^n) +\alpha^2 D(u_i^1)+\beta^2 D(u_i^{(2)})\\
u_i^{(2)}- \beta^2 \Delta t D(u_i^{(2)}) &= u_i^1  + \left[(\alpha^2+\beta^2) PG_i +\gamma^2 A(u_i^1) +\zeta^2 A(u_i^n) +\beta^2 D(u_i^1) \right] \Delta t\\
u_i - \beta^2 \Delta t D(u_i)&= \mathtt{rhs}_i
\end{align*}$$  

where we have left out the superscript $(2)$ on the velocity $u_i^2$ and $$\mathtt{rhs}_i=\left[(\alpha^2+\beta^2) PG_i +\gamma^2 A(u_i^1) +\zeta^2 A(u_i^n) +\beta^2 D(u_i^1) \right] \Delta t$$.  

Let us denote $\beta = \beta^2 \Delta t/Re $. Thus we have,  

$$\begin{equation}\label{eq:predicv_vel_imp}
u_i - \beta L_{jj}(u_i) = \mathtt{rhs}_i
\end{equation}$$  
with $L_{jj} = \frac{\partial^2 }{\partial x_1^2} + \frac{\partial^2 }{\partial x_2^2} + \frac{\partial^2 }{\partial x_3^2}$.  
This equation will lead to a large sparse matrix system of linear equations which is not practical to solve for the 3D problem. Hence we shall use the "Approximate Factorization" approch to convert this system into a combination of three tridiagonal systems.  

Let us write \eqref{eq:predicv_vel_imp} as,  

$$\begin{align*}
(I - \beta L_{jj}) u_i = \mathtt{rhs}_i \\
(I - \beta (L_1 + L_2 +L_3)) u_i = \mathtt{rhs}_i 
\end{align*}$$  

Approximate factorization of the LHS matrix gives, to order $\Delta t^3$,  

$$\begin{align*}
(I - A_3) (I - A_1) (I - A_2) u_i = \mathtt{rhs}_i 
\end{align*}$$  

with $A_k = \beta L_k $. The order in which the tridiagonal $A$ matrices are sovled does not matter in periodic cases. But as we have inhomogeneity in $x$ and $y$ directions, they are done after the $z$ direction.  

The solution procedure is:  

$$\begin{equation}\label{eq:approx_fac1}
(I - A_3) \hat{\hat{u_i}} = \mathtt{rhs}_i
\end{equation}$$  
$$ \begin{equation}\label{eq:approx_fac2}
(I - A_1) \hat{u_i} = \hat{\hat{u_i}}
\end{equation}$$  
$$\begin{equation}\label{eq:approx_fac3}
(I - A_2) u_i = \hat{u_i}
\end{equation}$$  

where the intermediate quantities are given by,  

$$ \begin{equation}\label{eq:approx_fac4}
\hat{\hat{u_i}} = (I - A_1) (I - A_2) u_i
\end{equation}$$  
$$ \begin{equation}\label{eq:approx_fac5}
\hat{u_i} = (I - A_2) u_i
\end{equation}$$  

The boundary conditions on the intermediate quantities $ \hat{\hat{u_i}},\hat{u_i}$ should be obtained from the above equations using the available boundary conditions on $u_i$.  

We shall restrict ourselves to the following BCs:
1. Periodic BC for all velocity components in the $z$-direction (i.e. at $z = 0$ and $z = Lz$)
1. Dirichlet BC for all velocity components in the $x$-direction (i.e. at $x = 0$ and $x = Lx$)
1. Dirichlet/Neumann BC for all velocity components in the $y$-direction (i.e. at $y = 0$ and $y = Ly$)

Since we are using staggered grid, the number of unknowns will depend on the location of the variable (cell/face centered) apart from the BCs above. For instance, let us take the $u$-component of velocity located at $(xu,yp,zp)$.  
