# Rigid Body Motion

The study of robot kinematics, dynamics, and control has at its heart the study of the motion of rigid objects. Many robot applications fall into the range of motion planning, state estimation and control. They are built based on rigid body motion.

Mathematically, the rigid body motion is represented by the rigid transformation, which is a map $$g: \mathbb{R}^3 \rightarrow \mathbb{R}^3$$ that preserves the distances between points and cross products between vectors. Preserving cross products is for preventing reflection.

A matrix is natrually related to rotation and scaling. To this end, the rotation matrices is a collection of matrices that satisfy above conditions. Furthermore, since cross product is a linear operator, angular velocity can also be represented as matrix. Similarly, the rigid motion can be represented as matrix.

## Rotational motion: SO(3) and so(3)

The set of all rotation matrix $$R$$ is denoted the special orthogonal group $$SO(3)$$. More formally,
<center>
$$ SO(3) = \{R\in\mathbb{R}^{3\times3}:RR^T=I,\ detR=+1\}  $$
</center>

The $$SO(3)$$ is a group under the operation of matrix multiplication. The rotation matrix $$R_{ab}$$, from the body frame $$B$$ to the inertial frame $$A$$, is constructed by stacking the coordinates of the principal axes of $$B$$ relative to $$A$$, i.e.
<center>
$$ R_{ab} = [\mathbf{x_{ab}}\ \mathbf{y_{ab}}\ \mathbf{z_{ab}}]  $$
</center>

Three ways to think of $$R$$
1. representation of the orientation, also called the _configuration_
2. a transformation that maps a point in body frame to the inertial frame
3. the rotation motion that rotates a point

The space of 3 by 3 skew-symmetric matrices is
<center>
$$ so(3) = \{S\in\mathbb{R}^{3\times3}: S^T=-S\}  $$
</center>

The elements in $$so(3)$$ naturally relate to angular velocity. To see this connection, we need to see how a skew-symmetric matrix is connected with the cross product by defining the hat map $$\hat{\bullet }$$. Since the cross product $$a\times b$$ is a linear operator, it can be represented using matrix, i.e.
<center>
$$ a \times b = \hat{a}b $$
</center>
where 
<center>
$$ \hat{a} = \left[ \begin{array}{ccc}
0 & -a_3 & a_2  \\ 
a_3 & 0 & -a_1 \\ 
-a_2 & a_1 & 0  \\ 
\end{array} \right] $$
</center>

The velocity of a purely rotating particle is $$\dot{q}(t)=\omega\times q(t)=\hat{\omega}q(t)$$. This naturally relates $$\hat{\omega}\in so(3)$$ with angular velocity $$\omega$$.

### Exponential map
The elements in $$SO(3)$$ and $$so(3)$$ are related by exponential transformation. Assume $$\left \| \omega \right \|=1$$, we have the _Rodrigues' formula_
<center>
$$ e^{\hat{\omega}\theta} \in SO(3) =I+\hat{\omega}\sin{\theta}+\hat{\omega}^2(1-\cos{\theta}) $$
</center>

The exponential map is surjective onto (many-to-one) $$SO(3)$$. Given $$R=[r_{ij}]$$, 
<center>
$$ \theta=\cos^{-1}\frac{trace(R)-1}{2} $$
<center>
</center>
$$ \omega=\frac{1}{2\sin\theta}\begin{bmatrix}
r_{32}-r_{23}\\ 
r_{13}-r_{31}\\\ 
r_{21}-r_{12}
\end{bmatrix} $$
</center>

### Other representations
Other widely used representations for rotation include Euler angles and quaternions. Euler angles are essentially three consecutive rotations. This representation is intuitive but subject to singularity issue, i.e. the gimbal lock. Quaternion uses four variables to describe the axis of rotation and magnitude of rotation and is normally normalized, i.e. $$q=(\cos(\theta/2),\ \omega\sin(\theta/2))$$. Conversion between quaternion and rotation matrix can be done using the above equations in exponential map.

## Rigid motion: SE(3) and se(3)

The configuration space of a rigid body, the special Euclidean group $$SE(3)$$, is the product space of $$\mathbb{R}^3$$ with $$SO(3)$$, i.e.
<center>
$$ SE(3) = \{(p,R):p\in\mathbb{R}^3,\ R\in SO(3)\} = \mathbb{R}^3\times SO(3)$$
</center>

Three ways to think of an element in $$SE(3)$$ are the same as $$SO(3)$$. The transformation is $$q_a=g(q_b)=p+Rq_b$$. Given $$g=(p,R)\in SE(3)$$, the _homogeneous representation_ of $$g$$ is
<center>
$$\bar{g}=\begin{bmatrix}
R & p\\ 
0 & 1
\end{bmatrix}$$
</center>
And its inverse is
<center>
$$\bar{g}^{-1}=\begin{bmatrix}
R^T & -R^Tp\\ 
0 & 1
\end{bmatrix}\in SE(3)$$
</center>

Generalize the skew-symmetric matrix $$\hat{\omega}\in so(3)$$, we define $$se(3)$$ as
<center>
$$ se(3) := \{(v,\hat{\omega}):v\in\mathbb{R}^3,\ \hat{\omega}\in so(3)\} $$.
</center>
And the _homogeneous representation_ of $$\xi=[v\ \omega]^T\in se(3)$$
<center>
$$\hat{\xi}=\begin{bmatrix}
\hat{\omega} & v\\ 
0 & 0
\end{bmatrix}\in SE(3)$$
</center>

> **Hint**
> Note that the $$\omega$$ is the rotational velocity of the object, while $$v$$ is not the translational velocity.

Exponential map also exists between elements in $$SE(3)$$ and $$se(3)$$, but with a more complicated form.