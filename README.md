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


The Bayes Filter consists of two steps to calculate the current belief of a state

* ### Prediction Step
```math
\overline {bel}(x_t) = ∫ bel(x_{t-1}) * p(x_t | x_{t-1}, u_t) dx_{t-1}
```
it depends on the previous belief as well as the current control action 


* ### Correction Step
```math
bel(x_t) = \eta \ p(z_t | x_t) \ \overline {bel}(x_t)
```
where $\eta$ is the normalization factor
it depends on the predicted belief as well as the current measurment 
