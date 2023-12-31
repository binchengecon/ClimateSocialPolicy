Appendix A. Computational Method
==============================

Appendix A.1 Overview
---------------------

Let’s take the HJB equation for pre technology and pre damage jump as an
example

.. math::

   \begin{aligned}
   0 & = \max_{i^k, i^r, e} \min_{{h^k}, {h^y}, {h^r}, g, f}\left(\frac{\delta}{1-\rho}\right)\left[\left(\frac{\alpha k-i^k-i^r-\alpha k \phi_0(z)\left[1-\frac{e}{\beta_t \alpha k}\right]^{\phi_1}}{\exp (v)} \right)^{1-\rho}-1\right] \\
   & +\frac{\partial v}{\partial \log k}\left[\mu_k+i^k-\frac{\kappa}{2} \left(i^k\right)^2-\frac{\left|\sigma_k\right|^2}{2}+\sigma_k {h^k}\right]+\frac{\partial^2 v}{\partial \log k^2} \frac{\left|\sigma_k\right|^2}{2} \\
   & +\frac{\partial v}{\partial y}\left(\frac{1}{L_y} \sum_{\ell=1}^{L_y} q(\ell \mid x,z) \theta(\ell)+\varsigma {h^y}\right) e+\frac{\partial^2 v}{\partial y^2} \frac{|\varsigma|^2}{2} e^2 \\
   & -\left(\left[\lambda_1+\lambda_2 y\right]\left(\frac{1}{L_y} \sum_{\ell=1}^{L_y} q(\ell \mid x,z) \theta(\ell)+\varsigma {h^y}\right) e+\lambda_2 \frac{|\varsigma|^2}{2} e^2\right) \\
   & +\frac{\partial v}{\partial \log r}\left(-\zeta+\psi_0\left(i^r\right)^{\psi_1} \exp \left(-\psi_1 \log r\right)-\frac{\left|\sigma_r\right|^2}{2}+\sigma_r {h^r}\right)+\frac{\partial^2 v}{\partial \log r^2} \frac{\left|\sigma_r\right|^2}{2} \\
   & +\xi^g \mathcal{J}_g(r)(1-g(\tilde{z} \mid x, z)+g(\tilde{z} \mid x, z) \log g(\tilde{z} \mid x, z))+\mathcal{J}_g(r) g(\tilde{z} \mid x, z)\left(\tilde{v}(\tilde{x}(\tilde{z}), \tilde{z})-v\right) \\
   & +\xi^d \mathcal{J}_n(y) \sum_{\tilde{z} \in \mathcal{Z}} \pi(\tilde{z} \mid x, z)(1-f(\tilde{z} \mid x, z)+f(\tilde{z} \mid x, z) \log f(\tilde{z} \mid x, z)) \\
   & +\mathcal{J}_n(y) \sum_{\tilde{z} \in \mathcal{Z}} \pi(\tilde{z} \mid x, z) f(\tilde{z} \mid x, z)\left(\tilde{v}(\tilde{x}(\tilde{z}), \tilde{z})-v\right) \\
   & +\xi^k \frac{\left|{h^k}\right|^2}{2}+\xi^c \frac{\left|{h^y}\right|^2}{2}+\xi^r \frac{\left|{h^r}\right|^2}{2}+\chi \frac{1}{L_y} \sum_{\ell=1}^{L_y} q(\ell \mid x,z) \log q(\ell \mid x,z)
   \end{aligned}

which satisfies conditions to switch the order of max and min operator.

Then, after exchanging the order of max and min operator, we deliver the
solution, i.e. value function :math:`v`, to this HJB equation
recursively with the false transient approach described in `Appendix A.3.1`.
The procedure in each iteration is provided below.

-  Start with current value function :math:`v^n` where
   :math:`n \in \{0,1, 2,\ldots\}` is called iteration step.
   Specifically, :math:`v^0` is the initial guess of value function,
   which can be vital to success of algorithm. We solve the HJB equation
   and update value function :math:`v^n` to :math:`v^{n+1}` in two
   steps.

   1. Take current value function :math:`v^n` as given, let’s start
      with current distortion and jump misspecification
      :math:`{h^k}^n, {h^y}^n, {h^r}^n, g^n, f^n` and solve optimal
      distortion and jump misspecification in two sub-steps, which are detailed
      in `Appendix A.2`. 

      1. Take value function :math:`v^n` and current distortion and
         jump misspecification
         :math:`{h^k}^n, {h^y}^n, {h^r}^n, g^n, f^n` as given, we update
         optimal actions from :math:`{i^k}^{n}, {i^r}^{n}, e^{n}`
         to :math:`{i^k}^{n+1}, {i^r}^{n+1}, e^{n+1}` in a
         maximization problem.

      2. With updated robustly optimal actions
         :math:`{i^k}^{n+1}, {i^r}^{n+1}, e^{n+1}` in hand, we
         update optimal distortion
         :math:`{h^k}^{n+1}, {h^y}^{n+1}, {h^r}^{n+1}, g^{n+1}, f^{n+1}`
         by solving a minimization problem.

   2. With updated robustly optimal actions and probability distortion,
      we finally can update value function :math:`v^{n+1}` by solving the
      algebraic system depicted in `Appendix A.3`.

-  We obtain a convergent solution to the HJB equation when the
   difference between two subsequent iterations is very tiny as

.. math::


   |v^{n+1}-v^{n}| < \epsilon

where :math:`\epsilon` is set to be :math:`10^{-7}`.

Appendix A.2 Solving the Optimization Problem
---------------------------------------------

Appendix A.2.1 Maximization
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Solution process for :math:`{i^k}` is given as follows, of which the same
logic can apply to other actions :math:`{i^r}, e` easily.

First order condition for :math:`{i^k}` writes

.. math::


   \delta\left(\frac{\alpha k-{i^k}-{i^r}-\alpha k \phi_0(z)\left[1-\frac{e}{\beta_t \alpha k}\right]^{\phi_1}}{\exp (v)} \right)^{-\rho} \frac{1}{\exp (v)} = \frac{\partial v}{\partial \log k}\left(1-\kappa {i^k}\right)

which is a highly non-linear equation of :math:`{i^k}` and doesn’t admit a
quasi-analytial solution.

Cobweb algorithm states that given current value function and
probability distortion, we can update current actions
:math:`{i^k}^{n}, {i^r}^{n}, e^{n}` according to

.. math::


   mu^{n} = \frac{\partial v^n}{\partial \log k}\left(1-\kappa {{i^k}^{n+1}}'\right)

where we define

.. math::


   mu^{n} \doteq \delta\left(\frac{\alpha k-{i^k}^{n}-{i^r}^{n}-\alpha  k \phi_0\left[1-\frac{e^{n}}{\beta_t \alpha k}\right]^{\phi_1}}{\exp (v^n)} \right)^{-\rho} \frac{1}{\exp (v^n)}

And :math:`{i^k}^{n+1}` is computed with relaxation paramter :math:`\chi`
as

.. math::


   {i^k}^{n+1} = \chi {i^k}^{n} + (1-\chi) {{i^k}^{n+1}}'

Appendix A.2.2 Minimization
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Solution process to probability distortions is simpler and we take that
of :math:`{h^k}` as an example, of which the same logic can apply to other
probability distortions :math:`{h^y}, {h^r}, g, f`.

First order condition for :math:`{h^k}` is written as

.. math::


   \frac{\partial v}{\partial \log k} \sigma_k = -\xi^k {h^k}

With given value function :math:`v^n`, we can update
:math:`{h^k}^{n+1}` according to

.. math::


   {h^k}^{n+1} = - \frac{1}{\xi^k} \frac{\partial v^n}{\partial \log k} \sigma_k 

Appendix A.3 Solving the Algebraic System
-----------------------------------------

Suppose we have three controlled stochastic process :math:`x_t, y_t, z_t`
as

.. math::

   \begin{aligned}
   d x_t &= \mu^x(x,y,z,\alpha) dt + \sigma^{x}(x,y,z,\alpha) dB^1_t \\
   d y_t &= \mu^y(x,y,z,\alpha) dt + \sigma^{y}(x,y,z,\alpha) dB^2_t \\
   d z_t &= \mu^z(x,y,z,\alpha) dt + \sigma^{z}(x,y,z,\alpha) dB^3_t 
   \end{aligned}

where :math:`B^1_t, B^2_t, B^3_t` are three independent standard
Brownian process.

Let’s consider a generalized time-independent three-dimensional HJB
equation:

.. math::

   \begin{aligned}
   0= & \max_{\alpha} -\delta v(x,y,z) + u(x,y,z,\alpha)\\
       & + \mu^x(x,y,z,\alpha) \partial_x v(x,y,z) + \frac{{\sigma^x}(x,y,z,\alpha)^2}{2}\partial_{xx} v(x,y,z) \\
       &+ \mu^y(x,y,z,\alpha) \partial_y v(x,y,z) + \frac{{\sigma^y}(x,y,z,\alpha)^2}{2}\partial_{yy} v(x,y,z) \\
       & + \mu^z(x,y,z,\alpha) \partial_z v(x,y,z) + \frac{{\sigma^z}(x,y,z,\alpha)^2}{2}\partial_{zz} v(x,y,z)
   \end{aligned}

where :math:`\alpha` is the set of controls in the HJB equation,
:math:`x,y,z` are the state variables of value function :math:`v` and
:math:`u` is the utility function.

Appendix A.3.1 False Transient Algorithm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To mitigate the inherent instability of the non-linear HJB, we add a
false transient (time) dimension and solve it until convergence. And
the new HJB equation is

.. math::

   \begin{aligned}
   \partial_t v(x,y,z,t)= & \max_{\alpha} -\delta v(x,y,z, t) + u(x,y,z,\alpha)\\
       & + \mu^x(x,y,z,\alpha) \partial_x v(x,y,z, t) + \frac{{\sigma^x}(x,y,z,\alpha)^2}{2}\partial_{xx} v(x,y,z, t) \\
       &+ \mu^y(x,y,z,\alpha) \partial_y v(x,y,z, t) + \frac{{\sigma^y}(x,y,z,\alpha)^2}{2}\partial_{yy} v(x,y,z, t) \\
       & + \mu^z(x,y,z,\alpha) \partial_z v(x,y,z, t) + \frac{{\sigma^z}(x,y,z,\alpha)^2}{2}\partial_{zz} v(x,y,z, t)
   \end{aligned}

Appendix A.3.2 Finite-Difference Scheme
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In this section, we introduce an upwind finite-difference scheme for discretizing the state variables of the HJB.

Appendix A.3.2.1 Upwind Scheme
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We construct equally spaced grids for these three state variables :math:`x,y,z` as

.. math::

   \begin{aligned}
   X &= \{x_1=\underline{X},\ldots,x_N=\bar{X}\} \\
   Y &= \{y_1=\underline{Y},\ldots,y_N=\bar{Y}\} \\
   Z &= \{z_1=\underline{Z},\ldots,z_N=\bar{Z}\}
   \end{aligned}

where the distance between two grid points are
:math:`\Delta x, \Delta y, \Delta z`

We approximate value function on grid points and use short-hand
notation :math:`v(x_i,y_j,z_k) \doteq v_{i,j,k}` and so on.

The partial derivatives :math:`\partial_x v(x,y,z)` can be approximated
with either a forward or backward difference approximation

.. math::

   \begin{aligned}
   \partial_{x,F} v_{i,j,k} &=  \frac{v_{i+1,j,k}-v_{i,j,k}}{\Delta x} \\
   \partial_{x,B} v_{i,j,k} &=  \frac{v_{i,j,k}-v_{i-1,j,k}}{\Delta x} 
   \end{aligned}

For accuracy, we approximate the partial derivatives
:math:`\partial_x v(x,y,z)` via central difference approximation

.. math::

   \begin{aligned}
   \partial_{x,C} v_{i,j,k} &=  \frac{v_{i+1,j,k} - v_{i-1,j,k}}{2\Delta x} 
   \end{aligned}

which is an average of forward and backward difference approximation.

Then, we approximate the second-order partial derivatives
:math:`\partial_{xx} v(x,y,z)` with a central difference approximation

.. math::

   \begin{aligned}
   \partial_{xx} v_{i,j,k} &=  \frac{v_{i+1,j,k} + v_{i-1,j,k}- 2v_{i,j,k}}{\Delta x^2} 
   \end{aligned}

We employ the first-order-condition to express our control
:math:`\alpha` on a grid point :math:`x_i, y_j, z_k` as a nonlinear
function of value function approximations
:math:`\partial_{x,C} v_{i,j,k}` and :math:`\partial_{xx} v_{i,j,k}`. Therefore, we use short-hand notations for our control,
drift and diffusion term as

.. math::

   \begin{aligned}
   \alpha(x_i,y_j,z_k) &= \alpha_{i,j,k} \\
   u(x_i,y_j,z_k,\alpha(x_i,y_j,z_k)) &= u_{i,j,k} \\
   \mu^w(x_i,y_j,z_k,\alpha(x_i,y_j,z_k)) &= \mu^w_{i,j,k}, \quad w=x,y,z\\
   \sigma^w(x_i,y_j,z_k,\alpha(x_i,y_j,z_k)) &= \sigma^w_{i,j,k}, \quad w=x,y,z\\
   \end{aligned}

In the upwind scheme, we construct backward approximation with negative
drift and forward approximation with positive drift.


Appendix A.3.2.2 Natural Boundary Condition
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We approximate second order derivative at boundaries with natural
boundary condition. More specifically, suppose state variable :math:`x`
is at its upper boundary, we set second order derivative of value
function to be the same as that of closet inner point.

.. math::

   \begin{aligned}
   \partial_{xx} v^{n+1}_{N,j,k} &=  \partial_{xx} v^{n+1}_{N-1,j,k} = \frac{v^{n+1}_{N,j,k} + v^{n+1}_{N-2,j,k}- 2v^{n+1}_{N-1,j,k}}{\Delta x^2} 
   \end{aligned}


Appendix A.3.2.3 Implicit Euler
^^^^^^^^^^^^^^^^^^^^^^

To solve the "transient" system, we use the implicit Euler algorithm, which
updates :math:`v^{n+1}` from current value function :math:`v^{n}` recursively.
At each time step, we solve the following linear system for
:math:`v^{n+1}`:

.. math::

   \begin{aligned}
   \frac{v^{n+1}_{i,j,k} - v^{n}_{i,j,k}}{\Delta t}  = &  -\delta v^{n+1}_{i,j,k} + u_{i,j,k}^{n} \\
       & + {\mu^{x,n}_{i,j,k}}^{+} \partial_x v^{n+1,F}_{i,j,k} + {\mu^{x,n}_{i,j,k}}^{-}  \partial_x v^{n+1,B}_{i,j,k}+ \frac{{\sigma^{x,n}_{i,j,k}}^2}{2}\partial_{xx} v_{i,j,k}^{n+1}\\
       & + {\mu^{y,n}_{i,j,k}}^{+} \partial_y v^{n+1,F}_{i,j,k} + {\mu^{y,n}_{i,j,k}}^{-}  \partial_y v^{n+1,B}_{i,j,k}+ \frac{{\sigma^{y,n}_{i,j,k}}^2}{2}\partial_{yy} v_{i,j,k}^{n+1}\\
       & + {\mu^{z,n}_{i,j,k}}^{+} \partial_z v^{n+1,F}_{i,j,k} + {\mu^{z,n}_{i,j,k}}^{-}  \partial_z v^{n+1,B}_{i,j,k}+ \frac{{\sigma^{z,n}_{i,j,k}}^2}{2}\partial_{zz} v_{i,j,k}^{n+1}\\
   \end{aligned}

After spatial discretization via the finite difference scheme, this system can
be written in the matrix form

.. math::

   \begin{aligned}
   \frac{(v^{n+1}-v^{n})}{\Delta t}  + \delta v^{n+1} = u^{n} + A^{n} v^{n+1}
   \end{aligned}

where :math:`A^{n}` is a sparse matrix. The linear system thus can be solved by
an iterative method such as conjugate gradient. For efficiency, we use the PETSc
linear solver library (:cite:t:`petsc`) which includes a long list of linear
solvers and preconditioners. Based on our empirical experiences, the Stablized
Biconjugate Gradient (BiCGStab) method paired with the incomplete factorization
(ILU)preconditioner gives the best performance.


Appendix A.4 List of Parameters Chosen in Algorithm
---------------------------------------------------

========================== ======
Parameter                  Value
========================== ======
:math:`\chi`               0.0025
:math:`\Delta t`           0.0025
:math:`\underline{\log K}` 4.0
:math:`\overline{\log K}`  9.0
:math:`\underline{Y}`      0.0
:math:`\overline{Y}`       4.0
:math:`\underline{\log R}` 1.0
:math:`\overline{\log R}`  6.0
:math:`\Delta \log K`      0.2
:math:`\Delta Y`           0.1
:math:`\Delta \log R`      0.1
========================== ======

.. raw:: html

   <!-- ## Appendix A.2 Cobweb Relaxation

   ### Appendix A.2.1 A Deep Look into First Order Condition

   There are HJB equations with simple control dynamics. For example, this HJB equation, describing heterogenous agents model in Aiyagari-Bewley-Huggett Economy, 

   $$
   \rho v(a, z)=\max _c u(c)+\partial_a v(a, z)(z+r a-c)+\mu(z) \partial_z v(a, z)+\frac{\sigma^2(z)}{2} \partial_{z z} v(a, z)
   $$

   has a very straight-forward optimal consumption choice as

   $$
   c^* = u^{\prime-1}\left(\partial_a v(a, z)\right)
   $$

   However, our HJB equations doesn't contain such simple dynamics. To solve a very complex system, we resort to a special algorithm called Cobweb algorithm. As it will show, the key idea is to reduce the non-linearity of the first order condition by progressively solving it in multiple steps.

   ### Appendix A.2.1 Progressive Algorithm against Strong Non-linearity

   We take the HJB equation for post technology jump as an example.

   \begin{aligned}
   0= & \max_{{i^k}}\min_{{h^k}} \left(\frac{\delta}{1-\rho}\right)\left[\left(\frac{\alpha-{i^k}}{\exp (v)} \exp(k)\right)^{1-\rho}-1\right] \\
   & +\frac{d v}{dk}\left[\mu_k+{i^k}-\frac{\kappa}{2} {i^k}^2-\frac{\left|\sigma_k\right|^2}{2}+\sigma_k {h^k}\right]+\frac{d^2 v}{d k^2} \frac{\left|\sigma_k\right|^2}{2} \\
   & +\x{i^k} \frac{\left|{h^k}\right|^2}{2}
   \end{aligned}

   First order condition for ${i^k}$ writes

   $$
   \delta\left(\frac{\alpha-{i^k}}{\exp (v)} \exp (k)\right)^{-\rho} \frac{\exp (k)}{\exp (v)} = \frac{d v}{dk}\left(1-\kappa {i^k}\right)
   $$

   which is a highly nonlinear equation of ${i^k}$ and doesn't lead to a quasi-analytical solution.

   To get around the nonlinearity, the Cobweb algorithm states that we define a new term $mu$ as

   $$
   m u=\frac{d v}{dk}\left(1-\kappa {i^k}\right)
   $$

   Then we solve the equation in multiple steps. Starting with a initial guess of ${i^k}$ as ${i^k}^0$, we update ${i^k}^n$, $n=1,2,\ldots,N$ according to 

   $$
   mu^{n}= \frac{d v}{dk}\left(1-\kappa {i^k}^{n+1}\right)
   $$

   where 

   $$
   mu^{n} = \delta\left(\frac{\alpha-{i^k}^n}{\exp (v)} \exp (k)\right)^{-\rho} \frac{\exp (k)}{\exp (v)}
   $$


   Now, to decide when to stop, we hope to see the difference between two subsequent iterations very tiny, meaning we have obtained a convergent solution to the equation. In other words, we wish to see

   $$
   |{i^k}^n-{i^k}^{n-1}| < \epsilon
   $$

   where $\epsilon$ is set to be $10^{-7}$.


   ### Appendix A.2.3 Further Improvement

   While the Cobweb algorithm can alleviate our computational burden of dealing with complex first order conditions a lot, there is still much room for further improvement on efficiency of our algorithm. For example, as we notice that the main purpose is to deliver a convergent solution to the value function in the HJB equation, we can alternate the Cobweb algorithm in a way that it's iterating not over control, such as ${i^k}$, but directly over value function.

   In other words, we start with initial guess of $v$, ${i^k}$ as $v^0$, ${i^k}^0$ and complete a inner iteration over ${i^k}$ and an outer iteration over $v$. 

   In the inner iteration, we take value function $v^n$ as given and attempt to update ${i^k}^n$ according to 

   $$
   mu^{n}= \frac{d v^{n}}{dk}\left(1-\kappa {{i^k}^{n+1}}'\right)
   $$


   where 

   $$
   mu^{n}= \delta\left(\frac{\alpha-{i^k}^{n}}{\exp (v^{n})} \exp (k)\right)^{-\rho} \frac{\exp (k)}{\exp (v^{n})}
   $$

   Here we progressively update ${i^k}^n$ to ${i^k}^{n+1}$ by a convex combination of ${i^k}^n$ and ${{i^k}^{n+1}}'$ with a relaxation parameter $\chi$ as

   $$
   {i^k}^{n+1}= \chi {i^k}^n + (1-\chi) {{i^k}^{n+1}}'.
   $$



   Once we have updated ${i^k}^{n+1}$, we can turn to outer iteration that updating $v^{n+1}$ according to 


   \begin{aligned}
   0= &  \left(\frac{\delta}{1-\rho}\right)\left[\left(\frac{\alpha-{i^k}^{n+1}}{\exp (v^{n})} \exp(k)\right)^{1-\rho}-1\right] \\
   & +\frac{d v^{n+1}}{dk}\left[\mu_k+{{i^k}^{n+1}}-\frac{\kappa}{2} {{i^k}^{n+1}}^2-\frac{\left|\sigma_k\right|^2}{2}+\sigma_k {{h^k}^{n+1}}\right]+\frac{d^2 v^{n+1}}{d k^2} \frac{\left|\sigma_k\right|^2}{2} \\
   & +\x{i^k} \frac{\left|{{h^k}^{n+1}}\right|^2}{2}
   \end{aligned}

   To sum up, this alternated Cobweb algorithm aims at achieving a very tiny difference between two subsequent iterations over value function $v$ more directly, 

   $$
   |v^{n+1}-v^{n}| < \epsilon
   $$

   which improved the efficiency and stability gallantly.
    -->


