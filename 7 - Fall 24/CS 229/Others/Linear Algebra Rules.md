
# Jacobian
Let $c = (c_1, c_2, \dots, c_n) \in \mathbb{R}^n$ and $F: \mathbb{R}^n \rightarrow \mathbb{R}^m$ be a differentiable function defined as:
$$
 F(c) = \begin{pmatrix} F_1(c) \\ F_2(c) \\ \vdots \\ F_m(c) \end{pmatrix} 
$$
The Jacobian of $F(c)$ has entries $J_{ik} = \frac{\partial F_i}{\partial c_k}(c)$, and can be written as:
$$
 J(c) = F'(c) = \begin{pmatrix} \frac{\partial F_1}{\partial c_1}(c) & \frac{\partial F_1}{\partial c_2}(c) & \dots & \frac{\partial F_1}{\partial c_n}(c) \\ \frac{\partial F_2}{\partial c_1}(c) & \frac{\partial F_2}{\partial c_2}(c) & \dots & \frac{\partial F_2}{\partial c_n}(c) \\ \vdots & \vdots & \ddots & \vdots \\ \frac{\partial F_m}{\partial c_1}(c) & \frac{\partial F_m}{\partial c_2}(c) & \dots & \frac{\partial F_m}{\partial c_n}(c) \end{pmatrix}
$$
Let $f = x^T \theta$ where $x$ is $n \times 1$ and $\theta$ is $n \times m$.
$$
\begin{align*} f(x) &= x^T \theta \\ &= \begin{bmatrix} x_1 & x_2 & \dots & x_n \end{bmatrix} \begin{bmatrix} \theta_{1,1} & \theta_{1,2} & \dots & \theta_{1,m} \\ \theta_{2,1} & \theta_{2,2} & \dots & \theta_{2,m} \\ \vdots & \vdots & \ddots & \vdots \\ \theta_{n,1} & \theta_{n,2} & \dots & \theta_{n,m} \end{bmatrix} \\ &= \begin{bmatrix} (x_1 \theta_{1,1} + x_2 \theta_{2,1} + \dots + x_n \theta_{n,1}) & \dots & (x_1 \theta_{1,m} + x_2 \theta_{2,m} + \dots + x_n \theta_{n,m}) \end{bmatrix} \end{align*}
$$
We can compute the Jacobian as follows:
$$
\begin{align*} \frac{\partial f}{\partial x} \left[ x^T \theta \right] &= \nabla_x \left[ x^T \theta \right] \\ &= \begin{bmatrix} \frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} & \dots & \frac{\partial f_1}{\partial x_n} \\ \vdots & \vdots & \ddots & \vdots \\ \frac{\partial f_m}{\partial x_1} & \frac{\partial f_m}{\partial x_2} & \dots & \frac{\partial f_m}{\partial x_n} \end{bmatrix} \\ &= \begin{bmatrix} \frac{\partial}{\partial x_1} (x_1 \theta_{1,1} + x_2 \theta_{2,1} + \dots + x_n \theta_{n,1}) & \dots & \frac{\partial}{\partial x_1} (x_1 \theta_{1,m} + x_2 \theta_{2,m} + \dots + x_n \theta_{n,m}) \\ \vdots & \ddots & \vdots \\ \frac{\partial}{\partial x_n} (x_1 \theta_{1,1} + x_2 \theta_{2,1} + \dots + x_n \theta_{n,1}) & \dots & \frac{\partial}{\partial x_n} (x_1 \theta_{1,m} + x_2 \theta_{2,m} + \dots + x_n \theta_{n,m}) \end{bmatrix} \\ &= \begin{bmatrix} \theta_{1,1} & \theta_{2,1} & \dots & \theta_{n,1} \\ \vdots & \vdots & \ddots & \vdots \\ \theta_{1,m} & \theta_{2,m} & \dots & \theta_{n,m} \end{bmatrix} \\ &= \theta^T \end{align*}
$$
## Generalized Version
![[Pasted image 20241029213351.png]]
![[Pasted image 20241029213416.png]]
![[Pasted image 20241030172004.png]]
## Kronecker Product
The Kronecker product $A \otimes B$ for matrices $A$ and $B$ where $A$ is $b \times 1$ and $B$ is $1 \times d$ will result in a new matrix $C$ of dimensions $(b \times 1) \times (1 \times d) = b \times d$.
1. If $A = \begin{bmatrix} a_1 \\ a_2 \\ \vdots \\ a_b \end{bmatrix}$ (a $b \times 1$ column vector) and $B = \begin{bmatrix} b_1 & b_2 & \dots & b_d \end{bmatrix}$ (a $1 \times d$ row vector),
2. Then $A \otimes B$ is given by:
$$
A \otimes B = \begin{bmatrix} a_1 \cdot B \\ a_2 \cdot B \\ \vdots \\ a_b \cdot B \end{bmatrix} = \begin{bmatrix} a_1 b_1 & a_1 b_2 & \dots & a_1 b_d \\ a_2 b_1 & a_2 b_2 & \dots & a_2 b_d \\ \vdots & \vdots & \ddots & \vdots \\ a_b b_1 & a_b b_2 & \dots & a_b b_d \end{bmatrix}
$$
In general, if $A$ is an $m \times n$ matrix and $B$ is a $b \times d$ matrix, the Kronecker product $A \otimes B$ will produce a new matrix of size $(m \cdot b) \times (n \cdot d)$.

To compute $A \otimes B$, each element $a_{ij}$ of $A$ is multiplied by the entire matrix $B$. This creates a block matrix where each $(i, j)$-th block of the resulting matrix is $a_{ij} \cdot B$.
$$
A \otimes B = \begin{bmatrix}
a_{11} B & a_{12} B & \dots & a_{1n} B \\
a_{21} B & a_{22} B & \dots & a_{2n} B \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} B & a_{m2} B & \dots & a_{mn} B
\end{bmatrix}
$$
## Tensorized Logistic Regression
### Givens
$$
Y = \sigma(XW)
$$
where:
- $X$ is a $b \times d$ matrix,
- $W$ is a $d \times 1$ vector, and
- $Y$ is a $b \times 1$ vector (element-wise application of sigmoid on $XW$).

### Derivative of $Y$ Wrt $XW$
Define:
$$
Z = XW
$$
Since $Y = \sigma(XW)$ is a vector with elements $Y_i = \sigma((XW)_i)$ for $i = 1, \ldots, b$
- The gradient of $Y$ with respect to $XW$, denoted as $\nabla_{XW} \sigma(XW)$, will be a $b \times b$ diagonal matrix.
- Each diagonal entry corresponds to $\left[ \sigma(XW)(1 - \sigma(XW)) \right]_i$.

So, we can write:
$$
\nabla_{XW} Y = \begin{bmatrix}
\left[ \sigma(XW)(1 - \sigma(XW)) \right]_1 & 0 & \cdots & 0 \\
0 & \left[ \sigma(XW)(1 - \sigma(XW)) \right]_2 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & \left[ \sigma(XW)(1 - \sigma(XW)) \right]_b
\end{bmatrix}
$$
Using the fact that all off-diagonal entries of $\nabla_{XW} Y$ are 0, we can shorten $\nabla_{XW} \sigma(XW)$ from $b \times b$ to $b \times 1$:
$$
\frac{dY}{d(XW)} = \sigma(XW) \odot (1 - \sigma(XW)) \in \mathbb{R}^{b \times 1}
$$
	where $\odot$ multiplies each entry of $\sigma(XW)$ by each entry of $(1 - \sigma(XW))$.
### Derivative of $Y$ Wrt $W$
Using the chain rule:
$$
\frac{dY}{dW} = \frac{dY}{d(XW)} \cdot \frac{d(XW)}{dW}
$$
Since $XW$ is the product of $X$ (a $b \times d$ matrix) and $W$ (a $d \times 1$ vector), we have:
$$
\frac{d(XW)}{dW} = X
$$
where $X$ is a $b \times d$ matrix. Combining terms, we have:
$$
\frac{dY}{dW} = \left(\sigma(XW) \odot (1 - \sigma(XW))\right) \odot X \in \mathbb{R}^{b\times d}
$$
where $\odot$ multiplies each row of $X$ by the corresponding entry in $\sigma(XW) \odot (1 - \sigma(XW))$
### Derivative of $Y$ Wrt $X$
To find $\frac{dY}{dX}$, we use the chain rule again:
$$
\frac{dY}{dX} = \frac{dY}{d(XW)} \cdot \frac{d(XW)}{dX}
$$
Since $Z = XW$, the derivative of $XW$ with respect to $X$ is:
$$
\frac{d(XW)}{dX} = W^T \in \mathbb{R}^{1\times d}
$$
Using the chain rule, we have:
$$
\begin{align*}
\frac{dY}{dX} &= \left(\sigma(XW) \odot (1 - \sigma(XW))\right) \otimes W^T \in \mathbb{R}^{b\ \times d} \\
&= \begin{bmatrix}
    \left[ \sigma(XW)(1 - \sigma(XW)) \right]_1 W^T \\
    \vdots \\
    \left[ \sigma(XW)(1 - \sigma(XW)) \right]_b W^T
\end{bmatrix} \in \mathbb{R}^{b \times d}
\end{align*}
$$
The final expression becomes:
$$
\begin{align*}
\frac{\partial Y}{\partial X} &= \frac{\partial \sigma(XW)}{\partial XW} \frac{\partial XW}{\partial X}  \\
&= \underbrace{\nabla_{XW} \sigma(XW)}_{\in \mathbb{R}^{b \times b}} \underbrace{I_b}_{\in \mathbb{R}^{b \times b}} \otimes \underbrace{W^T}_{\in \mathbb{R}^{d \times 1}} \\
&= \underbrace{\left[\nabla_{XW} \sigma(XW) I_b\right]}_{\in \mathbb{R}^{b \times b}} \otimes \underbrace{W^T}_{\in \mathbb{R}^{1 \times d}} \\
&= \underbrace{\nabla_{XW} \sigma(XW) \otimes W^T}_{\in \mathbb{R}^{b \times b \times 1 \times d} \text{ or equivalently } b \times b \times d} \\
&= \underbrace{\sigma(XW) \odot (1 - \sigma(XW))}_{\in \mathbb{R}^{b \times 1}} \otimes \underbrace{W^T}_{\in \mathbb{R}^{1 \times d}} \\
&= \begin{bmatrix}
\sigma(XW)(1 - \sigma(XW))_1 W^T \\
\vdots \\
\sigma(XW)(1 - \sigma(XW))_b W^T
\end{bmatrix} \in \mathbb{R}^{b \times d}
\end{align*}
$$

# Hessian
The Hessian matrix of a scalar-valued function $f(x)$ of a vector $x \in \mathbb{R}^n$ is a square matrix of second-order partial derivatives. It is defined as follows:

For a function $f: \mathbb{R}^n \rightarrow \mathbb{R}$, where $f(x)$ is twice differentiable, the Hessian matrix $H_f(x)$ is given by:
$$
H_f(x) = \nabla^2 f(x) = 
\begin{bmatrix}
\frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_1 \partial x_n} \\
\frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} & \cdots & \frac{\partial^2 f}{\partial x_2 \partial x_n} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial^2 f}{\partial x_n \partial x_1} & \frac{\partial^2 f}{\partial x_n \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_n^2}
\end{bmatrix}.
$$
In this matrix:
- Each entry $\frac{\partial^2 f}{\partial x_i \partial x_j} = \frac{\partial}{\partial x_i}\frac{\partial}{\partial x_j}f$ represents applying the partial with respect to $x_j$ first, followed by $x_i$.
- The Hessian matrix is symmetric if $f(x)$ is twice continuously differentiable, i.e., $\frac{\partial^2 f}{\partial x_i \partial x_j} = \frac{\partial^2 f}{\partial x_j \partial x_i}$.
The concavity of a function $f(x)$ is determined by the properties of its Hessian matrix $H$:
- **PSD Hessian ($H \succeq 0$)**: $f(x)$ is convex, critical point is a local minimum (possibly global if $f(x)$ is convex everywhere ⟶ only has one critical point).
- **PD Hessian ($H \succ 0$)****: $f(x)$ is strictly convex, critical point is a global minimum.
- **NSD Hessian ($H \preceq 0$)**: $f(x)$ is concave, critical point is a local maximum (possibly global if $f(x)$ is concave everywhere ⟶ only has one critical point).
- **ND Hessian ($H \prec 0$)**: $f(x)$ is strictly concave, critical point is a global maximum.
In general, positive definiteness and convexity lead to minima, while negative definiteness and concavity lead to maxima at critical points.
# Useful Identities
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
### **Gradient Of a Scalar Quadratic Form**
   For a quadratic form $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$ with $\mathbf{x} \in \mathbb{R}^n$ and symmetric $\mathbf{A} \in \mathbb{R}^{n \times n}$:
$$
   \nabla_{\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A} \mathbf{x}
$$
   If $\mathbf{A}$ is not symmetric, the gradient is:
$$
   \nabla_{\mathbf{x}} f(\mathbf{x}) = (\mathbf{A} + \mathbf{A}^T) \mathbf{x}
$$
### **Gradient Of a Quadratic Form with Respect to a Matrix**
   For a quadratic form $f(\mathbf{A}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$:
$$
   \frac{d}{d\mathbf{A}} \left( \mathbf{x}^T \mathbf{A} \mathbf{x} \right) = \mathbf{x} \mathbf{x}^T
$$
### **Derivative Of an Inner Product**
  If $f(\mathbf{x}) = \mathbf{a}^T \mathbf{x}= \mathbf{x}^T \mathbf{a}$, then:
$$
   \frac{d}{d\mathbf{x}} \left( \mathbf{a}^T \mathbf{x} \right) = \mathbf{a}
$$
### **Derivative Of a Matrix-Vector Product with Respect to a Vector**
  If $f(\mathbf{x}) = \mathbf{A} \mathbf{x}$, then:
$$
   \frac{d}{d\mathbf{x}} \left( \mathbf{A} \mathbf{x} \right) = \mathbf{A}
$$
### **Derivative Of Log Determinant**
   For $f(\mathbf{A}) = \log |\det(\mathbf{A})|$, where $\mathbf{A} \in \mathbb{R}^{n \times n}$ is square and invertible:
$$
   \frac{d}{d\mathbf{A}} \log |\det(\mathbf{A})| = \mathbf{A}^{-T}
$$
### **Gradient Of Determinant of a Matrix**
   For $f(\mathbf{A}) = \det(\mathbf{A})$, where $\mathbf{A} \in \mathbb{R}^{n \times n}$ is square and invertible:
$$
   \frac{d}{d\mathbf{A}} \det(\mathbf{A}) = \det(\mathbf{A}) \cdot \mathbf{A}^{-T}
$$
### **Gradient Of a Trace Function**
   If $f(\mathbf{A}) = \operatorname{Tr}(\mathbf{A} \mathbf{X})$:
$$
   \frac{d}{d\mathbf{A}} \operatorname{Tr}(\mathbf{A} \mathbf{X}) = \mathbf{X}^T
$$
### **Derivative Of a Matrix-Vector Quadratic Form with Respect to a Vector**
   If $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x} + \mathbf{b}^T \mathbf{x} + c$, then:
$$
   \frac{d}{d\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A} \mathbf{x} + \mathbf{b}
$$
   assuming $\mathbf{A}$ is symmetric.

### **Chain Rule for Matrix Calculus**
   If $f(\mathbf{X}) = g(h(\mathbf{X}))$, then:
$$
   \frac{\partial f}{\partial \mathbf{X}} = \frac{\partial g}{\partial h} \cdot \frac{\partial h}{\partial \mathbf{X}}
$$
### **Jacobian Of a Vector-Matrix Product**
   If $\mathbf{y} = \mathbf{A} \mathbf{x}$, then the Jacobian of $\mathbf{y}$ with respect to $\mathbf{x}$ is simply $\mathbf{A}$.

### **Hessian Of a Scalar Quadratic Form**
For a scalar quadratic form $f(\mathbf{x}) = \mathbf{x}^T \mathbf{A} \mathbf{x}$, the Hessian with respect to $\mathbf{x}$ is:
$$
    \nabla^2_{\mathbf{x}} f(\mathbf{x}) = 2 \mathbf{A}
    
$$
    assuming $\mathbf{A}$ is symmetric.

### **Softmax Gradient**
If $\sigma(\mathbf{z})_i = \frac{\exp(z_i)}{\sum_j \exp(z_j)}$, then:
$$
    \frac{\partial \sigma(\mathbf{z})_i}{\partial z_j} = \sigma(\mathbf{z})_i (\delta_{ij} - \sigma(\mathbf{z})_j)
    
$$
    where $\delta_{ij}$ is the Kronecker delta.

### **Sigmoid Derivative**
If $\sigma(z) = \frac{1}{1 + e^{-z}}$, then:
$$
    \frac{d\sigma}{dz} = \sigma(z)(1 - \sigma(z))
    
$$
### Norm
The gradient of $f(x) = \|x - a\|_2^2$ with respect to $x$ can be found as follows. First, expand the $\ell_2$-norm squared:
$$
f(x) = \|x - a\|_2^2 = (x - a)^T (x - a).
$$

Expanding the expression gives:
$$
f(x) = x^T x - 2 a^T x + a^T a.
$$

Now, take the gradient with respect to $x$:
$$
\nabla_x f(x) = \nabla_x (x^T x - 2 a^T x + a^T a).
$$

Since $a$ is a constant with respect to $x$:
$$
\nabla_x f(x) = 2x - 2a.
$$

Therefore, the gradient is:
$$
\nabla_x \|x - a\|_2^2 = 2(x - a).
$$
### Least Squares
Gradient of the least squares objective function $f(x) = \|Ax - b\|_2^2$ with respect to $x$. The objective function is
$$
f(x) = \|Ax - b\|_2^2 = (Ax - b)^T (Ax - b).
$$
Expanding $f(x)$ gives
$$
f(x) = x^T A^T A x - 2 b^T A x + b^T b.
$$
We now take the gradient of each term with respect to $x$.
- The gradient of $x^T A^T A x$ with respect to $x$ is $2 A^T A x$.
- The gradient of $-2 b^T A x = -2x^TA^Tb$ with respect to $x$ is $-2 A^T b$.
- The term $b^T b$ is a constant with respect to $x$, so its gradient is zero.
Putting it all together, we get
$$
\nabla_x f(x) = 2 A^T A x - 2 A^T b.
$$
Thus, the gradient of $\|Ax - b\|_2^2$ with respect to $x$ is:
$$
\nabla_x \|Ax - b\|_2^2 = 2 A^T (Ax - b).
$$
