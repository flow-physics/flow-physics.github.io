---
author: Rajesh Venkatesan
comments: true
date: 2024-02-06 00:00:00+00:00
layout: post
link: http://flowphysics.com/energy-budgets
slug: energy-budgets
title: Energy Budgets
---
* Table of Contents
{:toc}

## The Mean Kinetic Energy Equation

The mean kinetic energy (MKE) equation is,
\begin{equation}
\frac{\partial K}{\partial t} + U_j\frac{\partial K}{\partial x_j} = \frac{\partial}{\partial x_j} \left( -\frac{1}{\rho} P U_j-\overline{u_iu_j}U_i +2\nu U_i S_{ij}\right) + \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} - 2\nu S_{ij} S_{ij}
\end{equation}

where,  

$$ \begin{equation*}
K = \frac{1}{2}U_i^2 \qquad \text{and} \qquad S_{ij} = \frac{1}{2}\left(\frac{\partial U_i}{\partial x_j} + \frac{\partial U_j}{\partial x_i} \right)
\end{equation*}$$

Before proceeding further to look at term individually, we note that we are interested in a turbulent boundary layer which is two-dimensional and stationary in the mean. Hence, we can set,
\begin{equation}
W=0 \,; \qquad \frac{\partial}{\partial t}(\text{any mean quantity}) = 0 \,; \qquad \frac{\partial}{\partial z}(\text{any mean quantity}) = 0
\end{equation}

### Advection Terms
\begin{equation}
\frac{D K}{D t} = \frac{\partial K}{\partial t} + U\frac{\partial K}{\partial x}+V\frac{\partial K}{\partial y}+W\frac{\partial K}{\partial z}
\end{equation}

In our case, this simplifies to,  
\begin{equation}
\frac{D K}{D t} = U\frac{\partial K}{\partial x}+V\frac{\partial K}{\partial y}
\end{equation}  
with $K = \frac{1}{2}(U^2 + V^2)$.

### Transport Terms
#### Pressure transport
The pressure transport terms are,  
\begin{equation}
\frac{\partial}{\partial x_j} \left( -\frac{1}{\rho} P U_j \right) = -\frac{1}{\rho} \left( \frac{\partial (PU)}{\partial x} + \frac{\partial (PV)}{\partial y} + \frac{\partial (PW)}{\partial z} \right)
\end{equation}  
In a TBL, this becomes,  
\begin{equation}
\frac{\partial}{\partial x_j} \left( -\frac{1}{\rho} P U_j \right) = -\frac{1}{\rho} \left( \frac{\partial (PU)}{\partial x} + \frac{\partial (PV)}{\partial y} \right)
\end{equation}

#### Turbulent Transport
The turbulent transport terms are,  

$$ \begin{align*}
-\frac{\partial}{\partial x_j} (\overline{u_iu_j}U_i) = -\left( \frac{\partial}{\partial x} (\overline{u_iu}U_i) + \frac{\partial}{\partial y} (\overline{u_iv}U_i) + \frac{\partial}{\partial z} (\overline{u_iw}U_i) \right)
\end{align*}$$

$$ \begin{align*}
= -\left( \frac{\partial}{\partial x} (\overline{u^2}U+ \overline{uv}V + \overline{uw}W ) + \frac{\partial}{\partial y} (\overline{uv}U + \overline{v^2}V + \overline{vw}W   ) + \frac{\partial}{\partial z} (\overline{uw}U + \overline{vw}V + \overline{w^2}W) \right) 
\end{align*}$$

In our case, this becomes,  
\begin{equation}
-\frac{\partial}{\partial x_j} (\overline{u_iu_j}U_i) = -\left( \frac{\partial}{\partial x} (\overline{u^2}U+ \overline{uv}V) + \frac{\partial}{\partial y} (\overline{uv}U + \overline{v^2}V)\right) 
\end{equation}

#### Viscous Transport
The viscous transport terms are,  
\begin{equation}
2\nu \frac{\partial}{\partial x_j}(U_i S_{ij}) = \nu \frac{\partial}{\partial x_j} \left( U_i \left(\frac{\partial U_i}{\partial x_j} + \frac{\partial U_j}{\partial x_i} \right) \right) = \nu \frac{\partial}{\partial x_j} \left( \frac{\partial (U_i^2/2)}{\partial x_j} + \frac{\partial (U_i U_j)}{\partial x_i} \right) = \nu \frac{\partial}{\partial x_j} \left( \frac{\partial K}{\partial x_j} + \frac{\partial (U_i U_j)}{\partial x_i} \right)
\end{equation}

In our case, this gets simplified to,  

$$\begin{equation*}
= \nu \frac{\partial}{\partial x} \left( \frac{\partial K}{\partial x} + \frac{\partial (U_i U)}{\partial x_i} \right) + \nu \frac{\partial}{\partial y} \left( \frac{\partial K}{\partial y} + \frac{\partial (U_i V)}{\partial x_i} \right)
\end{equation*}$$  

$$ \begin{equation*}
= \nu \frac{\partial}{\partial x} \left( \frac{\partial K}{\partial x} + \frac{\partial (U^2)}{\partial x}+ \frac{\partial (UV)}{\partial y} \right) + \nu \frac{\partial}{\partial y} \left( \frac{\partial K}{\partial y} + \frac{\partial (UV)}{\partial x} + \frac{\partial (V^2)}{\partial y} \right)
\end{equation*}$$  

\begin{equation}
= \nu \left( \frac{\partial^2 (K+U^2)}{\partial x^2} + \frac{\partial^2 (K+V^2)}{\partial y^2} + 2 \frac{\partial^2 (UV)}{\partial x \partial y} \right)
\end{equation}

### Turbulence Production
Turbulence production is a loss to mean kinetic energy. These terms are,
\begin{equation}
\overline{u_i u_j}\frac{\partial U_i}{\partial x_j} = \overline{u^2}\frac{\partial U}{\partial x}+\overline{uv}\frac{\partial U}{\partial y}+\overline{uw}\frac{\partial U}{\partial z}+\overline{uv}\frac{\partial V}{\partial x}+\overline{v^2}\frac{\partial V}{\partial y}+\overline{vw}\frac{\partial V}{\partial z}+\overline{uw}\frac{\partial W}{\partial x}+\overline{vw}\frac{\partial W}{\partial y}+\overline{w^2}\frac{\partial W}{\partial z}
\end{equation}
Again due to two-dimensionality of the mean flow, we only have the following terms.  
\begin{equation}
\overline{u_i u_j}\frac{\partial U_i}{\partial x_j} = \overline{u^2}\frac{\partial U}{\partial x}+\overline{uv}\frac{\partial U}{\partial y}+\overline{uv}\frac{\partial V}{\partial x}+\overline{v^2}\frac{\partial V}{\partial y}
\end{equation}

### Viscous Dissipation
The viscous dissipation terms are,
\begin{equation}
-2\nu S_{ij} S_{ij} = -\frac{1}{2} \nu \left(\frac{\partial U_i}{\partial x_j} + \frac{\partial U_j}{\partial x_i} \right)\left(\frac{\partial U_i}{\partial x_j} + \frac{\partial U_j}{\partial x_i} \right)
\end{equation}

$$\begin{equation*}
= -\frac{1}{2} \nu \left[ 4 \left(\frac{\partial U}{\partial x}\right)^2 + 4 \left(\frac{\partial V}{\partial y}\right)^2  + 4 \left(\frac{\partial W}{\partial y}\right)^2 + 2 \left(\frac{\partial U}{\partial y} + \frac{\partial V}{\partial x} \right)^2 + 2 \left(\frac{\partial U}{\partial z} + \frac{\partial W}{\partial x} \right)^2 + 2 \left(\frac{\partial V}{\partial z} + \frac{\partial W}{\partial y} \right)^2 \right]
\end{equation*}$$  

In our case, this simplifies to,  
\begin{equation}
-2\nu S_{ij} S_{ij} = - 2 \nu \left[ \left(\frac{\partial U}{\partial x}\right)^2 + \left(\frac{\partial V}{\partial y}\right)^2  \right] -\nu \left[ \left(\frac{\partial U}{\partial y} + \frac{\partial V}{\partial x} \right)^2 \right]
\end{equation}  

Combining all the terms in the MKE equation together, we get for a TBL,  

$$\begin{align*}
U\frac{\partial K}{\partial x}+V\frac{\partial K}{\partial y} &= -\frac{1}{\rho} \left( \frac{\partial (PU)}{\partial x} + \frac{\partial (PV)}{\partial y} \right)-\left( \frac{\partial}{\partial x} (\overline{u^2}U+ \overline{uv}V) + \frac{\partial}{\partial y} (\overline{uv}U + \overline{v^2}V)\right) \\ &+\nu \left( \frac{\partial^2 (K+U^2)}{\partial x^2} + \frac{\partial^2 (K+V^2)}{\partial y^2} + 2 \frac{\partial^2 (UV)}{\partial x \partial y} \right) + \overline{u^2}\frac{\partial U}{\partial x}+\overline{uv}\frac{\partial U}{\partial y}+\overline{uv}\frac{\partial V}{\partial x}+\overline{v^2}\frac{\partial V}{\partial y}\\ &- 2 \nu \left[ \left(\frac{\partial U}{\partial x}\right)^2 + \left(\frac{\partial V}{\partial y}\right)^2  \right] -\nu \left[ \left(\frac{\partial U}{\partial y} + \frac{\partial V}{\partial x} \right)^2 \right]
\end{align*}$$

## Reynolds stress transport equation
The Reynolds stress transport equation is,  

$$ \begin{align*}
\frac{\partial }{\partial t} (\overline{u_i u_j}) + U_k \frac{\partial }{\partial x_k} (\overline{u_i u_j}) = -\frac{1}{\rho} \underbrace{\left( \overline{u_j\frac{\partial p}{\partial x_i}} + \overline{u_i\frac{\partial p}{\partial x_j}} \right)}_\text{redistribution} -\underbrace{\frac{\partial}{\partial x_k} (\overline{u_k u_i u_j})}_\text{turbulent transport} \underbrace{- \overline{u_j u_k} \frac{\partial U_i}{\partial x_k} - \overline{u_i u_k} \frac{\partial U_j}{\partial x_k}}_\text{production} \\
+ \underbrace{\nu \frac{\partial^2 }{\partial x_j \partial x_j} (\overline{u_i u_j})}_\text{viscous transport}- \underbrace{2\nu \overline{\frac{\partial u_i}{\partial x_k}\frac{\partial u_j}{\partial x_k}}}_\text{dissipation}
\end{align*}$$  

The 'redistribution' term can be rewritten as,  

$$ \begin{equation}
-\left( \overline{u_j\frac{\partial p}{\partial x_i}} + \overline{u_i\frac{\partial p}{\partial x_j}} \right) = \underbrace{-\left( \frac{\partial (\overline{pu_j})}{\partial x_i} + \frac{\partial (\overline{pu_i})}{\partial x_j}\right)}_\text{pressure transport} + \underbrace{\overline{p\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)}}_\text{pressure strain}
\end{equation}$$  

## The Turbulent Kinetic Energy Equation
The turbulent kinetic energy (TKE) equation is,  

\begin{equation}\label{eq:TKE_eqn}
\frac{\partial k}{\partial t} + U_j\frac{\partial k}{\partial x_j} = -\frac{\partial}{\partial x_j} \left( \frac{1}{\rho} \overline{pu_j} + \frac{1}{2}\overline{u_i^2 u_j} -2\nu \overline{u_i s_{ij}}\right) - \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} - 2\nu \overline{s_{ij} s_{ij}}
\end{equation}

where,  

$$ \begin{equation*}
k = \frac{1}{2}\overline{u_i^2} \qquad \text{and} \qquad s_{ij} = \frac{1}{2}\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)
\end{equation*}$$

We shall look at each term in this equation separately. The first term on the LHS is the local time derivative of TKE and this is zero in a stationary turbulent flow (in the mean).

### Advection terms
The advection terms are,  

\begin{equation}
U_j\frac{\partial k}{\partial x_j} = U\frac{\partial k}{\partial x} + V \frac{\partial k}{\partial y} + W \frac{\partial k}{\partial z}
\end{equation}

These terms represent the advection of TKE by the mean flow along the mean streamline. In a two-dimensional boundary layer, we have $W = 0$. So the only terms remaining for a TBL are,  
\begin{equation}
U_j\frac{\partial k}{\partial x_j} = U\frac{\partial k}{\partial x} + V \frac{\partial k}{\partial y}
\end{equation}

### Transport terms
#### Pressure transport
The pressure transport terms are,  
\begin{equation}
-\frac{\partial}{\partial x_j} \left( \frac{1}{\rho} \overline{pu_j} \right) = -\frac{1}{\rho} \left( \frac{\partial (\overline{pu})}{\partial x}+\frac{\partial (\overline{pv})}{\partial y} + \frac{\partial (\overline{pw})}{\partial z}\right)
\end{equation}  
These terms represent the transport of TKE by the pressure fluctuations. In case of a two-dimensional boundary layer, we can set $\frac{\partial}{\partial z}(\text{any mean quantity}) = 0$. Hence the only remaining terms are,  
\begin{equation}
-\frac{\partial}{\partial x_j} \left( \frac{1}{\rho} \overline{pu_j} \right) = -\frac{1}{\rho} \frac{\partial (\overline{pu})}{\partial x}-\frac{1}{\rho}\frac{\partial (\overline{pv})}{\partial y}
\end{equation}

#### Turbulent transport
The turbulent transport terms are,  
\begin{equation}
-\frac{\partial}{\partial x_j}\left(\frac{1}{2}\overline{u_i^2 u_j}\right) = -\frac{1}{2}\left( \frac{\partial (\overline{u_i^2 u})}{\partial x} + \frac{\partial (\overline{u_i^2 v})}{\partial y} + \frac{\partial (\overline{u_i^2 w})}{\partial z} \right)
\end{equation}  
Again because of two-dimensionality, we are left with,  

$$\begin{align*}
-\frac{\partial}{\partial x_j}\left(\frac{1}{2}\overline{u_i^2 u_j}\right) &= -\frac{1}{2} \frac{\partial (\overline{u_i^2 u})}{\partial x}-\frac{1}{2} \frac{\partial (\overline{u_i^2 v})}{\partial y}  \\
&= -\frac{1}{2}\left( \frac{\partial (\overline{u^3})}{\partial x} + \frac{\partial (\overline{v^2 u})}{\partial x} + \frac{\partial (\overline{w^2 u})}{\partial x} + \frac{\partial (\overline{u^2 v})}{\partial y} + \frac{\partial (\overline{v^3})}{\partial y} + \frac{\partial (\overline{w^2 v })}{\partial y} \right)
\end{align*}$$

#### Viscous transport
The viscous transport terms are,  

$$ \begin{align*}
2 \nu \frac{\partial}{\partial x_j}\overline{u_i s_{ij}} &= \nu \frac{\partial}{\partial x_j} \left( \overline{u_i\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)}\right) \\
&= \nu \frac{\partial}{\partial x_j} \left( \overline{\frac{1}{2}\frac{\partial u_i^2}{\partial x_j} + \frac{\partial u_i u_j}{\partial x_i}}\right)
\end{align*}$$

In view of two-dimensionality of the flow, we can write,  

$$ \begin{align*}
2 \nu \frac{\partial}{\partial x_j}\overline{u_i s_{ij}} = \nu \frac{\partial}{\partial x} \left( \overline{\frac{1}{2}\frac{\partial u_i^2}{\partial x} + \frac{\partial u_i u}{\partial x_i}}\right) + \nu \frac{\partial}{\partial y} \left( \overline{\frac{1}{2}\frac{\partial u_i^2}{\partial y} + \frac{\partial u_i v}{\partial x_i}}\right)
\end{align*}$$

$$ \begin{align*}
= \nu \frac{\partial}{\partial x} \left( \overline{\frac{1}{2}\frac{\partial (u^2+v^2+w^2)}{\partial x} + \frac{\partial u^2}{\partial x} + \frac{\partial (uv)}{\partial y} + \frac{\partial (uw)}{\partial z}}\right) + \nu \frac{\partial}{\partial y} \left( \overline{\frac{1}{2}\frac{\partial (u^2+v^2+w^2)}{\partial y} + \frac{\partial uv}{\partial x}+ \frac{\partial v^2}{\partial y}+ \frac{\partial vw}{\partial z}}\right)
\end{align*}$$

\begin{equation}
2 \nu \frac{\partial}{\partial x_j}\overline{u_i s_{ij}} = \nu \frac{\partial^2 }{\partial x^2}\left( k + \overline{u^2}\right) + \nu \frac{\partial^2 }{\partial y^2}\left( k + \overline{v^2}\right) + 2 \nu \frac{\partial^2 }{\partial x \partial y}(\overline{uv})
\end{equation}  
Note that we have neglected the $z$ derivatives in arriving at the last expression due to two-dimensionality.

### Turbulence Production
The turbulence production terms are,  

$$\begin{equation}
- \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} = -\overline{u^2}\frac{\partial U}{\partial x}-\overline{uv}\frac{\partial U}{\partial y}-\overline{uw}\frac{\partial U}{\partial z}-\overline{uv}\frac{\partial V}{\partial x}-\overline{v^2}\frac{\partial V}{\partial y}-\overline{vw}\frac{\partial V}{\partial z}-\overline{uw}\frac{\partial W}{\partial x}-\overline{vw}\frac{\partial W}{\partial y}-\overline{w^2}\frac{\partial W}{\partial z}
\end{equation}$$  

Again due to two-dimensionality of the mean flow, we only have the following terms.  

$$ \begin{equation}
- \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} = -\overline{u^2}\frac{\partial U}{\partial x}-\overline{uv}\frac{\partial U}{\partial y}-\overline{uv}\frac{\partial V}{\partial x}-\overline{v^2}\frac{\partial V}{\partial y}
\end{equation}$$  

### Viscous dissipation
The viscous dissipation terms are,

$$ \begin{equation}
- 2\nu \overline{s_{ij} s_{ij}} = -\frac{1}{2}\nu \overline{\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)}
\end{equation}$$  

Expanding the terms,  

$$ \begin{align*}
- 2\nu \overline{s_{ij} s_{ij}} = -\frac{1}{2}\nu \left[ 4\overline{\left(\frac{\partial u}{\partial x}\right)^2}+4\overline{\left(\frac{\partial v}{\partial y}\right)^2}+4\overline{\left(\frac{\partial w}{\partial z}\right)^2}+2\overline{\left(\frac{\partial u}{\partial y}+\frac{\partial v}{\partial x}\right)^2}+2\overline{\left(\frac{\partial u}{\partial z}+\frac{\partial w}{\partial x}\right)^2}+2\overline{\left(\frac{\partial v}{\partial z}+\frac{\partial w}{\partial y}\right)^2}\right]
\end{align*}$$

$$ \begin{equation*}
\epsilon = -2\nu \left[ \overline{\left(\frac{\partial u}{\partial x}\right)^2}+\overline{\left(\frac{\partial v}{\partial y}\right)^2}+\overline{\left(\frac{\partial w}{\partial z}\right)^2}\right]-\nu\left[\overline{\left(\frac{\partial u}{\partial y}+\frac{\partial v}{\partial x}\right)^2}+\overline{\left(\frac{\partial u}{\partial z}+\frac{\partial w}{\partial x}\right)^2}+\overline{\left(\frac{\partial v}{\partial z}+\frac{\partial w}{\partial y}\right)^2}\right]
\end{equation*}$$  

These terms represent the total TKE lost to heat due to viscous dissipation. While numerically evaluating these terms, the usual practice is to evaluate these terms from the instantaneous viscous dissipation (replace all the velocities in the above by the instantaneous velcocity components) and subtracting out the mean viscous dissipation. For the two-dimensional flow we have, the mean viscous dissipation will be,  
$$ \begin{equation}
\text{Mean viscous dissipation} = -2\nu \left[ \left(\frac{\partial U}{\partial x}\right)^2+\left(\frac{\partial V}{\partial y}\right)^2\right]-\nu\left[\left(\frac{\partial U}{\partial y}+\frac{\partial V}{\partial x}\right)^2\right]
\end{equation}$$

Thus the final form of the TKE equation appropriate for a two-dimensional TBL is,  

$$\begin{align} \label{eq:tke_terms_all_v1}
\underbrace{U\frac{\partial k}{\partial x}}_\text{ADV1} + \underbrace{V \frac{\partial k}{\partial y}}_\text{ADV2} = \underbrace{-\frac{1}{\rho} \frac{\partial (\overline{pu})}{\partial x}}_\text{PT1}\underbrace{-\frac{1}{\rho}\frac{\partial (\overline{pv})}{\partial y}}_\text{PT2}\underbrace{-\frac{1}{2}\left( \frac{\partial (\overline{u^3})}{\partial x} + \frac{\partial (\overline{v^2 u})}{\partial x} + \frac{\partial (\overline{w^2 u})}{\partial x}\right)}_\text{TT1} \underbrace{-\frac{1}{2} \left(\frac{\partial (\overline{u^2 v})}{\partial y} + \frac{\partial (\overline{v^3})}{\partial y} + \frac{\partial (\overline{w^2 v })}{\partial y} \right)}_\text{TT2} \nonumber \\ +\underbrace{\nu \frac{\partial^2 }{\partial x^2}\left( k + \overline{u^2}\right)}_\text{VT1} + \underbrace{\nu \frac{\partial^2 }{\partial y^2}\left( k + \overline{v^2}\right)}_\text{VT2} + \underbrace{2 \nu \frac{\partial^2 }{\partial x \partial y}(\overline{uv})}_\text{VT3} \underbrace{-\overline{u^2}\frac{\partial U}{\partial x}}_\text{TP1}\underbrace{-\overline{uv}\frac{\partial U}{\partial y}}_\text{TP2}\underbrace{-\overline{uv}\frac{\partial V}{\partial x}}_\text{TP3}\underbrace{-\overline{v^2}\frac{\partial V}{\partial y}}_\text{TP4} + \epsilon
\end{align}$$

The terms are appropriately labeled and shown above. A plot of these individual terms across the boundary layer at a given streamwise location is called the turbulent kinetic energy budget. Under the boundary layer approximation, the TKE equation is usually simplified to,

$$ \begin{equation}
\underbrace{U\frac{\partial k}{\partial x}}_\text{ADV1} + \underbrace{V \frac{\partial k}{\partial y}}_\text{ADV2} = \underbrace{-\frac{1}{\rho}\frac{\partial (\overline{pv})}{\partial y}}_\text{PT2} \underbrace{-\frac{1}{2} \left(\frac{\partial (\overline{u^2 v})}{\partial y} + \frac{\partial (\overline{v^3})}{\partial y} + \frac{\partial (\overline{w^2 v })}{\partial y} \right)}_\text{TT2}+ \underbrace{\nu \frac{\partial^2 }{\partial y^2}\left( k + \overline{v^2}\right)}_\text{VT2} \underbrace{-\overline{uv}\frac{\partial U}{\partial y}}_\text{TP2}+ \epsilon
\end{equation}$$  

## Alternative formulation of the TKE equation
Let us consider the TKE equation as given in \ref{eq:TKE_eqn},  

$$ \begin{equation*}
\frac{\partial k}{\partial t} + U_j\frac{\partial k}{\partial x_j} = -\frac{\partial}{\partial x_j} \left( \frac{1}{\rho} \overline{pu_j} + \frac{1}{2}\overline{u_i^2 u_j} -2\nu \overline{u_i s_{ij}}\right) - \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} - 2\nu \overline{s_{ij} s_{ij}}
\end{equation*}$$

with,  

$$\begin{equation*}
k = \frac{1}{2}\overline{u_i^2} \qquad \text{and} \qquad s_{ij} = \frac{1}{2}\left(\frac{\partial u_i}{\partial x_j} + \frac{\partial u_j}{\partial x_i} \right)
\end{equation*}$$

Here, the viscous dissipation of TKE to heat is represented by the last term as,  

\begin{equation}
\epsilon = 2\nu \overline{s_{ij} s_{ij}}
\end{equation}

Consider the following quantity, named as pseudo-dissipation,  

\begin{equation}
\tilde{\epsilon} = \nu \overline{\frac{\partial u_i}{\partial x_j}\frac{\partial u_i}{\partial x_j}}
\end{equation}

These two are related by,  
\begin{equation}
\epsilon = \tilde{\epsilon} + \nu \frac{\partial^2 \overline{u_i u_j}}{\partial x_i \partial x_j}
\end{equation}

Using this, the TKE equation can be rewritten as,  

\begin{equation}
\frac{\partial k}{\partial t} + U_j\frac{\partial k}{\partial x_j} = -\frac{\partial}{\partial x_j} \left( \frac{1}{\rho} \overline{pu_j} + \frac{1}{2}\overline{u_i^2 u_j}\right)+\nu \frac{\partial^2 k}{\partial x_j \partial x_j}  - \overline{u_i u_j}\frac{\partial U_i}{\partial x_j} -  \nu \overline{\frac{\partial u_i}{\partial x_j}\frac{\partial u_i}{\partial x_j}}
\end{equation}

With this in mind, the TKE equation \ref{eq:tke_terms_all_v1} becomes,  

$$\begin{align*}
\underbrace{U\frac{\partial k}{\partial x}}_\text{ADV1} + \underbrace{V \frac{\partial k}{\partial y}}_\text{ADV2} = \underbrace{-\frac{1}{\rho} \frac{\partial (\overline{pu})}{\partial x}}_\text{PT1}\underbrace{-\frac{1}{\rho}\frac{\partial (\overline{pv})}{\partial y}}_\text{PT2}\underbrace{-\frac{1}{2}\left( \frac{\partial (\overline{u^3})}{\partial x} + \frac{\partial (\overline{v^2 u})}{\partial x} + \frac{\partial (\overline{w^2 u})}{\partial x}\right)}_\text{TT1} \underbrace{-\frac{1}{2} \left(\frac{\partial (\overline{u^2 v})}{\partial y} + \frac{\partial (\overline{v^3})}{\partial y} + \frac{\partial (\overline{w^2 v })}{\partial y} \right)}_\text{TT2} \\ +\underbrace{\nu \frac{\partial^2 }{\partial x^2}\left( k + \overline{u^2}\right)}_\text{VT1} + \underbrace{\nu \frac{\partial^2 }{\partial y^2}\left( k + \overline{v^2}\right)}_\text{VT2} + \underbrace{2 \nu \frac{\partial^2 }{\partial x \partial y}(\overline{uv})}_\text{VT3} \underbrace{-\overline{u^2}\frac{\partial U}{\partial x}}_\text{TP1}\underbrace{-\overline{uv}\frac{\partial U}{\partial y}}_\text{TP2}\underbrace{-\overline{uv}\frac{\partial V}{\partial x}}_\text{TP3}\underbrace{-\overline{v^2}\frac{\partial V}{\partial y}}_\text{TP4} \\ -\nu \underbrace{\left( \frac{\partial^2 \overline{u^2} }{\partial x^2} +  \frac{\partial^2 \overline{v^2} }{\partial y^2} +  \overline{\left(\frac{\partial u}{\partial x}\right)^2}+ \overline{\left(\frac{\partial u}{\partial y}\right)^2} + \overline{\left(\frac{\partial u}{\partial z}\right)^2} +\overline{\left(\frac{\partial v}{\partial x}\right)^2} + \overline{\left(\frac{\partial v}{\partial y}\right)^2} + \overline{\left(\frac{\partial v}{\partial z}\right)^2} + \overline{\left(\frac{\partial w}{\partial x}\right)^2}  + \overline{\left(\frac{\partial w}{\partial y}\right)^2} +\overline{\left(\frac{\partial w}{\partial z}\right)^2}  \right)}_\text{DISS}
\end{align*}$$

Under the boundary layer approximation, this simplifies to,  

$$ \begin{equation}
\underbrace{U\frac{\partial k}{\partial x}}_\text{ADV1} + \underbrace{V \frac{\partial k}{\partial y}}_\text{ADV2} = \underbrace{-\frac{1}{\rho}\frac{\partial (\overline{pv})}{\partial y}}_\text{PT2} \underbrace{-\frac{1}{2} \left(\frac{\partial (\overline{u^2 v})}{\partial y} + \frac{\partial (\overline{v^3})}{\partial y} + \frac{\partial (\overline{w^2 v })}{\partial y} \right)}_\text{TT2}+ \underbrace{\nu \frac{\partial^2 }{\partial y^2}\left( k + \overline{v^2}\right)}_\text{VT2} \underbrace{-\overline{uv}\frac{\partial U}{\partial y}}_\text{TP2}+ \texttt{DISS}
\end{equation}$$

with $\texttt{DISS}$ given as,  

\begin{equation}
\texttt{DISS} =  -\nu \left(  \frac{\partial^2 \overline{v^2} }{\partial y^2} +  \overline{\left(\frac{\partial u}{\partial x}\right)^2}+ \overline{\left(\frac{\partial u}{\partial y}\right)^2} + \overline{\left(\frac{\partial u}{\partial z}\right)^2} +\overline{\left(\frac{\partial v}{\partial x}\right)^2} + \overline{\left(\frac{\partial v}{\partial y}\right)^2} + \overline{\left(\frac{\partial v}{\partial z}\right)^2} + \overline{\left(\frac{\partial w}{\partial x}\right)^2}  + \overline{\left(\frac{\partial w}{\partial y}\right)^2} +\overline{\left(\frac{\partial w}{\partial z}\right)^2}  \right)
\end{equation}  

## Vorticity Dynamics

The vorticity equation is,  

\begin{equation}
\frac{\partial \widetilde{\omega_i}}{\partial t} + \widetilde{u_j}\frac{\partial \widetilde{\omega_i}}{\partial x_j} =  \widetilde{\omega_j} \frac{\partial \widetilde{u_i}}{\partial x_j}  + \nu \frac{\partial^2 \widetilde{\omega_i} }{\partial x_j \partial x_j}
\end{equation}

Averaging this equation gives a governing equation for mean vorticity,  

\begin{equation}
U_j \frac{\partial \Omega_i}{\partial x_j} =  -\overline{u_j\frac{\partial \omega_i}{\partial x_j}} + \overline{\omega_j s_{ij}} + \Omega_j S_{ij} + \nu \frac{\partial^2 \Omega_i }{\partial x_j \partial x_j}
\end{equation}

Due to incompressibility and solenoidal nature of vorticity (both in mean and fluctuations), the following simplifying expressions can be found:  
\begin{equation}
\overline{u_j\frac{\partial \omega_i}{\partial x_j}} = \frac{\partial}{\partial x_j}\left( \overline{u_j \omega_i} \right) \qquad ; \qquad  \overline{\omega_j s_{ij}} = \frac{\partial}{\partial x_j}\left( \overline{u_i \omega_j} \right)
 \end{equation}  

Hence the mean vorticity equation becomes,  

\begin{equation}
U_j \frac{\partial \Omega_i}{\partial x_j} =  -\frac{\partial}{\partial x_j}\left( \overline{u_j \omega_i} \right) + \frac{\partial}{\partial x_j}\left( \overline{u_i \omega_j} \right) + \Omega_j S_{ij} + \nu \frac{\partial^2 \Omega_i }{\partial x_j \partial x_j}
\end{equation}

The only component of mean vorticity in the boundary layer is the spanwise component $\Omega_z$. The corresponding governing equation is,  
\begin{equation}
U\frac{\partial \Omega_z}{\partial x} + V\frac{\partial \Omega_z}{\partial y} = -\frac{\partial}{\partial x}\left( \overline{u \omega_z} \right) -\frac{\partial}{\partial y}\left( \overline{v \omega_z} \right) -\frac{\partial}{\partial x}\left( \overline{w \omega_x} \right) -\frac{\partial}{\partial y}\left( \overline{w \omega_y} \right) + \nu \left(\frac{\partial^2 \Omega_z}{\partial x^2} + \frac{\partial^2 \Omega_z}{\partial y^2} \right)
\end{equation}  

The mean enstrophy $\frac{1}{2}\Omega_i \Omega_i$ is governed by,  
$$ \begin{align}
U_j \frac{\partial }{\partial x_j} \left( \frac{1}{2}\Omega_i \Omega_i \right)=  -\frac{\partial}{\partial x_j}\left( \Omega_i \overline{u_j \omega_i} \right) + \overline{u_j \omega_i} \frac{\partial \Omega_i}{\partial x_j} + \Omega_i \Omega_j S_{ij} + \Omega_i  \overline{\omega_j s_{ij}} + \nu \frac{\partial^2 }{\partial x_j \partial x_j} \left( \frac{1}{2}\Omega_i \Omega_i \right) - \nu \frac{\partial \Omega_i}{\partial x_j} \frac{\partial \Omega_i}{\partial x_j}
\end{align}$$

Or equivalently,  
$$ \begin{align}
U_j \frac{\partial }{\partial x_j} \left( \frac{1}{2}\Omega_i \Omega_i \right)=  -\frac{\partial}{\partial x_j}\left( \Omega_i \overline{u_j \omega_i} \right) + \overline{u_j \omega_i} \frac{\partial \Omega_i}{\partial x_j} + \Omega_i \Omega_j S_{ij} + \Omega_i \frac{\partial}{\partial x_j}\left( \overline{u_i \omega_j} \right)+ \nu \frac{\partial^2 }{\partial x_j \partial x_j} \left( \frac{1}{2}\Omega_i \Omega_i \right) - \nu \frac{\partial \Omega_i}{\partial x_j} \frac{\partial \Omega_i}{\partial x_j}
\end{align}$$

The fluctuating enstrophy $\frac{1}{2} \overline{\omega_i \omega_i}$ is governed by,  

$$ \begin{align*}
U_j \frac{\partial }{\partial x_j} \left( \frac{1}{2} \overline{\omega_i \omega_i} \right)=  - \overline{u_j \omega_i} \frac{\partial \Omega_i}{\partial x_j} -\frac{1}{2}\frac{\partial}{\partial x_j} \left( \overline{u_j \omega_i \omega_i}\right)+ \overline{\omega_i \omega_j s_{ij} }+\overline{\omega_i \omega_j} S_{ij} + \Omega_j  \overline{\omega_i s_{ij}} \\ + \nu \frac{\partial^2 }{\partial x_j \partial x_j} \left( \frac{1}{2}\overline{\omega_i \omega_i} \right) - \nu \overline{\frac{\partial \omega_i}{\partial x_j} \frac{\partial \omega_i}{\partial x_j}}
\end{align*}$$

Simplifying, we get,

$$ \begin{align*}
U_j \frac{\partial }{\partial x_j} \left( \frac{1}{2} \overline{\omega_i \omega_i} \right)=  - \overline{u_j \omega_i} \frac{\partial \Omega_i}{\partial x_j} -\frac{1}{2}\frac{\partial}{\partial x_j} \left( \overline{u_j \omega_i \omega_i}\right)+ \overline{\omega_i \omega_j s_{ij} }+\overline{\omega_i \omega_j} S_{ij} + \Omega_j  \frac{\partial}{\partial x_i}\left( \overline{u_j \omega_i} \right) \\ + \nu \frac{\partial^2 }{\partial x_j \partial x_j} \left( \frac{1}{2}\overline{\omega_i \omega_i} \right) - \nu \overline{\frac{\partial \omega_i}{\partial x_j} \frac{\partial \omega_i}{\partial x_j}}
\end{align*}$$
