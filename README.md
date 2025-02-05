# linear-kalman-filter
Overview of the Linear Kalman Filter Theory and C++ Implementation

---
## The Kalman Filter: A Gaussian Recursive State Estimation Filter
### The Kalmna Filter is the Bayes filter for the Gaussian Linear Case
So let's have a brief  about the Bayes filter 
### Bayes Filter:
The Bayes Filter is the most general algorithm for calculating beliefs.\
The Algorithm Calculates the belief distribution from measurment(z) and control data(u).\
### Pseudo Code for the Bayes Filter:

![Pseudo-Code-for-Discrete-Bayesian-Filter](https://github.com/user-attachments/assets/e2180826-60a0-4bbb-b01b-b0dcc7c2f450)



The Bayes Filter consists of two steps to calculate the current belief of a state

* ### Prediction Step
```math
\overline {bel}(x_t) = ∫ bel(x_{t-1}) * p(x_t | x_{t-1}, u_t) dx_{t-1}
```
it depends on the previous belief as well as the current control action 


* ### Correction Step (Update Step)
```math
bel(x_t) = \eta \ p(z_t | x_t) \ \overline {bel}(x_t)
```
where $\eta$ is the normalization factor \
The correction step (Update Step) depends on the predicted belief as well as the current measurment 

## Getting back again to the Kalman Filter
The Kalman Filter represents beliefs by the moments parametrization, the belief is represented by the mean $\mu_t$ and the covariance $\Sigma_t$ 

The linear Kalman Filter only deal with linear and Gaussian cases so to say that the Posteriors are Gaussian three properties must be satisfied which are:
* The state transation probability p($x_t$ | $u_t$, $x_{t-1}$ )
must be a linear function in it's arguments with added Gaussian Noise this is expressed by the following equation 
```math
x_t = A_tx_{t-1} + B_tu_t + \epsilon_t
```
where, $A_t$ is a square matrix of size $n \times n$ where $n$ is the dimension of the state vector $x_t$.

$B_t$ is of size $n \times m$ where $m$ is the dimension of the control vector $u_t$.

$\epsilon_t$ is a Gaussian random vector that models the uncertanity introduced by the state transition, it's mean is zero and it's covariance is donated by $R_t$.

where the state transition probability p($x_t$ | $u_t$,$x_{t-1}$) can be defined as follow as it is Gaussian:
```math
p(x_t | u_t, x_{t-1}) = det(2 \pi R_t)^{-1/2} \cdot exp({-1/2(x_t - A_t x_{t-1} - B_t u_t)^T R_t^{-1} (x_t - A_t x_{t-1} - B_t u_t)})
```
* The measurment probability $p(z_t|x_t)$ must be also linear in it's args with added Gaussian Noise
```math
z_t = C_t x_t + \delta_t 
```

where $C_t$ is a matrix of size $k \times n$ where $k$ is the dimension of the measurment vector $z_t$ and the vector $\delta_t$ descvribes the measurment noise it's description is zero mean and covariance $Q_t$

so the measurment probability is given by the following:
```math
p(z_t | x_t) = det(2 \pi Q_t)^{-1/2} \cdot exp({-1/2(z_t - C_t x_t)^T Q_t^{-1} (z_t -  C_t x_t)})
```
* Finally the initial belief $bel(x_0)$ must be normally distributed, it's mean is donated by $\mu_0$ and covariance by $\Sigma_0$
```math
bel(x_0) = p(x_0) = det(2 \pi \Sigma_0)^{-1/2} \cdot exp({-1/2(x_0 - \mu_0)^T \Sigma^{-1} (x_0 -  \mu_0)})
```

### Pseudo code for the linear Kalman Filter

![Screenshot from 2024-09-01 19-43-38](https://github.com/user-attachments/assets/4a8e4382-ee59-44e7-bed2-d574f078fe8f)

Kalman filters represent the
belief $bel(x_t)$ at time $t$ by the mean $μ_t$ and the covariance $Σ_t$. The input of the Kalman
filter is the belief at time $t − 1$, represented by $μ_{t−1}$ and $Σ_{t−1}$. To update these
parameters, Kalman filters require the control $u_t$ and the measurement $z_t$. The output
is the belief at time $t$, represented by $μ_t$ and $Σ_t$.

The Kalman Filter consists of two steps which are:
* ###  The prediction Step
    in Lines 2 and 3, the predicted belief $\overline μ_t$ and $\overline Σ_t$ is calculated representing the belief
$bel(xt)$ one time step later, but before incorporating the measurement $z_t$. This belief is obtained by incorporating the control ut. The mean is updated using the deterministic version of the state transition function , with the mean $μ_{t−1}$ substituted for the state $x_{t−1}$. The update of the covariance considers the fact that states depend on previous states through the linear matrix $A_t$. This matrix is multiplied twice into the covariance, since the covariance is a quadratic matrix.
* ### The Correction Step
    The belief $bel(x_t)$ is subsequently transformed into the desired belief $bel(x_t)$ in Lines 4 through 6, by incorporating the measurement $z_t$. The variable $K_t$, computed in Line 4 is called Kalman gain. It specifies the degree to which the measurement is incorporated into the new state estimate. Line 5 manipulates the mean, by adjusting it in proportion to the Kalman gain Kt and the deviation of the actual measurement, $z_t$, and the measurement predicted according to the measurement probability function. Finally, the new covariance of the posterior belief is calculated in Line 6, adjusting for the information gain resulting from the measurement.

## Example on the linear Kalman filter
