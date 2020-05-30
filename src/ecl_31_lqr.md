# LQR

Linear Quadratic Regulator, or LQR, is the regulator that minimizes the quadratic cost of linear systems. This optimal control problem begins with the linear system
<center>
$$ \dot{x} = Ax + Bu $$
</center>
It tries to find the control input $$u$$ that minimize the cost
<center>
$$ J=\int_{0}^{t_f}x^TQx+u^TRu\ dt $$
</center>
where $$Q$$ and $$R$$ are symmetric positive-definite matrices that provide user-specific weighting in the performance index.
<br/>

This problem ends up with solving the differential Riccati equation (DRE)
<center>
$$ -\dot{S} = A^TS+SA-SBR^{-1}B^TS+Q $$
</center>
The optimal state feedback controller is found to be $$u=-Kx$$, where
<center>
$$ K=R^{-1}B^TS $$
</center>

> **Hint**
> 1. The DRE here is integrated backward in time from the final condition $$S(t_f)$$, rather than forward integration as the case in optimal estimation problem.

## The steady-state solution

In __some situations__, the optimal gain $$K$$ converges to a constant. If this is the case then we do not have to worry about integrating the DRE for $$S$$ and about updating $$K$$ in real time. This can provide a larget savings in computational effort at the cost of only a small sacrifice of performace.

If $$A,B,Q,R$$ are constant, then $$S$$ may reach a steady-state value and $$\dot{S}$$ eventually reach zero. This implies
<center>
$$ A^TS+SA-SBR^{-1}B^TS+Q=0 $$
</center>
This is called the algebraic Riccati equation (ARE). Or more specific, the continuous ARE (CARE). And the optimal gain is
<center>
$$ K(\infty)=R^{-1}B^TS(\infty) $$
</center>

## Implement LQR in Python

In Matlab, the [command](https://www.mathworks.com/help/control/ref/lqr.html) `lqr` is provided for solving the steady-state $$K$$.

In Python, the `linalg` in `scipy` provide `solve_continuou_are` for solving steady-state $$K$$.
```python
def lqr(A: np.matrix, B: np.matrix, Q: np.matrix, R: np.matrix):
    P = linalg.solve_continuous_are(A, B, Q, R)
    K = R.I * B.T * P
    return K
```
Check out the [controllers.py](https://github.com/panwliu/Estimation-and-Control-Library/blob/master/scripts/controller.py) in my GitHub.

## Demo

The LQR is tested on a simulated cartpole in the Ignition Gazebo. In this example, the parameters for the model are
$$m_c=1$$, $$m_p=0.5$$ and $$l=1$$. The resulting linearized model is

```python
A = np.mat("0 0 1 0; 0 0 0 1; 0 -4.9 0 0; 0 14.7 0 0")
B = np.mat("0; 0; 2; -1")
Q = np.mat("1 0 0 0; 0 1 0 0; 0 0 0.5 0; 0 0 0 0.5")
R = np.mat("1")
K = controller.lqr(A, B, Q, R)
K = np.asarray(K)

action = 0.1 if k_step<500 else -np.dot(K, states_real[:,k_step-1])
```

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/90J-i_QF6PQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

<br/>
References:
1. [Optimal State Estimation: Kalman, H∞, and Nonlinear Approaches](https://onlinelibrary.wiley.com/doi/book/10.1002/0470045345)
1. [Optimal Control](http://www.uta.edu/utari/acs/FL%20books/Lewis%20optimal%20control%203rd%20edition%202012.pdf) by Lewis, etc
1. [LQR Controllers with Python](http://www.mwm.im/lqr-controllers-with-python/)