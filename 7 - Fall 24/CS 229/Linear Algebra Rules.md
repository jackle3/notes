
# Jacobian
![[Pasted image 20241029213320.png]]
![[Pasted image 20241029213327.png]]
![[Pasted image 20241029213345.png]]
![[Pasted image 20241029213351.png]]
![[Pasted image 20241029213416.png]]
![[Pasted image 20241029213614.png]]
![[Pasted image 20241029213624.png]]

* Compute the gradient $\nabla_W Y$ of $Y = \sigma(XW)$ with respect to $W$.
* Given:
	* $Y = \sigma(XW)$, where $\sigma$ is the element-wise sigmoid function.
	* $X \in \mathbb{R}^{b \times d}$, $W \in \mathbb{R}^{d \times 1}$, and $Y \in \mathbb{R}^{b \times 1}$.
* **Step 1: Differentiate \(Y\) with respect to \(W\)**
The sigmoid function derivative is:
$$
\sigma'(u) = \sigma(u) \odot (1 - \sigma(u))
$$
where $\odot$ represents element-wise multiplication.

Using the chain rule:
$$
\nabla_W Y = X^T \left( \sigma(XW) \odot (1 - \sigma(XW)) \right)
$$

**Result**
$$
\nabla_W Y = X^T \left( \sigma(XW) \odot (1 - \sigma(XW)) \right) \in \mathbb{R}^{d \times 1}
$$

# Other Rules
## Useful Identities
1. **Trace Cyclic Property**
   For matrices $\mathbf{A}$ and $\mathbf{B}$ where the product $\mathbf{A} \mathbf{B}$ is defined:
$$
   \operatorname{Tr}(\mathbf{A} \mathbf{B}) = \operatorname{Tr}(\mathbf{B} \mathbf{A})
$$
2. **Derivative of Trace of Product**
   If $f(\mathbf{X}) = \operatorname{Tr}(\mathbf{X} \mathbf{A})$:
$$
   \frac{d f}{d \mathbf{X}} = \mathbf{A}^T
$$
3. **Derivative of Trace of Quadratic Form**
   For $f(\mathbf{X}) = \operatorname{Tr}(\mathbf{X}^T \mathbf{A} \mathbf{X})$:
$$
   \frac{d}{d\mathbf{X}} \operatorname{Tr}(\mathbf{X}^T \mathbf{A} \mathbf{X}) = 2 \mathbf{A} \mathbf{X}
$$
   if $\mathbf{A}$ is symmetric.

4. **Eigenvalues and Trace**
   For a matrix $\mathbf{A}$, the trace $\operatorname{Tr}(\mathbf{A})$ is the sum of its eigenvalues.

5. **Eigenvalues and Determinant**
   For a matrix $\mathbf{A}$, the determinant $\det(\mathbf{A})$ is the product of its eigenvalues.

# Probability and Expectation Rules
1. **Expectation of a Quadratic Form**
   For a random vector $\mathbf{x}$ with mean $\mathbf{\mu}$ and covariance matrix $\mathbf{\Sigma}$:
$$
   \mathbb{E}[\mathbf{x}^T \mathbf{A} \mathbf{x}] = \operatorname{Tr}(\mathbf{A} \mathbf{\Sigma}) + \mathbf{\mu}^T \mathbf{A} \mathbf{\mu}
$$


# Derivative Rules
1. **Gradient of a Scalar Quadratic Form**
   For a quadratic form $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$ with $\mathbf{x} \in \mathbb{R}^n$ and symmetric $\mathbf{A} \in \mathbb{R}^{n \times n}$:
$$
   \nabla_{\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A} \mathbf{x}
   
$$
   If $\mathbf{A}$ is not symmetric, the gradient is:
$$
   \nabla_{\mathbf{x}} f(\mathbf{x}) = (\mathbf{A} + \mathbf{A}^T) \mathbf{x}
$$

2. **Gradient of a Quadratic Form with Respect to a Matrix**
   For a quadratic form $f(\mathbf{A}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$:
$$
   \frac{d}{d\mathbf{A}} \left( \mathbf{x}^T \mathbf{A} \mathbf{x} \right) = \mathbf{x} \mathbf{x}^T
$$
3. **Derivative of a Matrix-Vector Product with Respect to a Vector**
   If $f(\mathbf{x}) = \mathbf{a}^T \mathbf{x}$, then:
$$
   \frac{d}{d\mathbf{x}} \left( \mathbf{a}^T \mathbf{x} \right) = \mathbf{a}
$$
4. **Derivative of Log Determinant**
   For $f(\mathbf{A}) = \log |\det(\mathbf{A})|$, where $\mathbf{A} \in \mathbb{R}^{n \times n}$ is square and invertible:
$$
   \frac{d}{d\mathbf{A}} \log |\det(\mathbf{A})| = \mathbf{A}^{-T}
$$
5. **Gradient of Determinant of a Matrix**
   For $f(\mathbf{A}) = \det(\mathbf{A})$, where $\mathbf{A} \in \mathbb{R}^{n \times n}$ is square and invertible:
$$
   \frac{d}{d\mathbf{A}} \det(\mathbf{A}) = \det(\mathbf{A}) \cdot \mathbf{A}^{-T}
$$
6. **Gradient of a Trace Function**
   If $f(\mathbf{A}) = \operatorname{Tr}(\mathbf{A} \mathbf{X})$:
$$
   \frac{d}{d\mathbf{A}} \operatorname{Tr}(\mathbf{A} \mathbf{X}) = \mathbf{X}^T
$$
7. **Derivative of a Matrix-Vector Quadratic Form with Respect to a Vector**
   If $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x} + \mathbf{b}^T \mathbf{x} + c$, then:
$$
   \frac{d}{d\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A} \mathbf{x} + \mathbf{b}
$$
   assuming $\mathbf{A}$ is symmetric.

8. **Chain Rule for Matrix Calculus**
   If $f(\mathbf{X}) = g(h(\mathbf{X}))$, then:
$$
   \frac{\partial f}{\partial \mathbf{X}} = \frac{\partial g}{\partial h} \cdot \frac{\partial h}{\partial \mathbf{X}}
$$
9. **Jacobian of a Vector-Matrix Product**
   If $\mathbf{y} = \mathbf{A} \mathbf{x}$, then the Jacobian of $\mathbf{y}$ with respect to $\mathbf{x}$ is simply $\mathbf{A}$.

10. **Hessian of a Scalar Quadratic Form**
    For a scalar quadratic form $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$, the Hessian with respect to $\mathbf{x}$ is:
$$
    \nabla^2_{\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A}
    
$$
    assuming $\mathbf{A}$ is symmetric.

11. **Softmax Gradient**
    If $\sigma(\mathbf{z})_i = \frac{\exp(z_i)}{\sum_j \exp(z_j)}$, then:
$$
    \frac{\partial \sigma(\mathbf{z})_i}{\partial z_j} = \sigma(\mathbf{z})_i (\delta_{ij} - \sigma(\mathbf{z})_j)
    
$$
    where $\delta_{ij}$ is the Kronecker delta.

12. **Sigmoid Derivative**
    If $\sigma(z) = \frac{1}{1 + e^{-z}}$, then:
$$
    \frac{d\sigma}{dz} = \sigma(z)(1 - \sigma(z))
    
$$
