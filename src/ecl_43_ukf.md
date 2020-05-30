# Unscented Kalman Filter

For linear transform $$y=Hx$$, the mean and covariance of a random variable propogate as
1. $$ \bar{y}=E(Hx) = H\bar{x} $$
1. $$ P_y = HP_xH^T $$

However, the means and covariance of a random variable undergoes a nonlinear transformation $$y=h(x)$$ are more complicated. The nonlinear Kalman filter, UKF, estimates the means and covariance based on the unscented transformation.

Consider the discrete nonlinear system
<center>
$$ x_{k+1} = f(x_k, u_k) + w_k $$  <br/>
$$ y_k = h(x_k) + v_k $$
</center>
where $$w_k\sim (0,Q_k)$$ and $$v_k\sim (0,R_k)$$.

The UKF then generates the sigma points, and based on the sigma points the means and covariance are approximated.