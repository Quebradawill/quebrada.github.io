## Vector Space and Orthogonal Transforms

Reference: [Vector Space and Orthogonal Transforms](http://fourier.eng.hmc.edu/e161/lectures/VectorSpace/index.html)

### 1. Vector Space and Orthogonal Transform

**Definition**: A **vector space** is a set $V$ with two operations of *addition* and *scalar multiplication* defined for its members, referred to as vectors.

1. *Vector addition* maps any two vectors ${\bf x}, {\bf y} \in V$ to another vector ${\bf x}+{\bf y} \in V$ satisfying the following properties:
   - Commutativity: ${\bf x}+{\bf y}={\bf y}+{\bf x}$.
   - Associativity: ${\bf x}+({\bf y}+{\bf z})=({\bf x}+{\bf y})+{\bf z}$.
   - Existence of zero: there is a vector ${\bf0} \in V$ such that:  ${\bf0}+{\bf x}={\bf x}+{\bf0}={\bf x}$.
   - Existence of inverse: for any vector ${\bf x}\in V$, there is another vector $-{\bf x}\in V$ such that ${\bf x}+(-{\bf x})={\bf0}$.
2. Scalar multiplication maps a vector ${\bf x}\in V$ and a real or complex scalar $a\in \mathbb{C}$ to another vector $a{\bf x} \in V$ with the following properties:
   - $a({\bf x}+{\bf y})=a{\bf x}+a{\bf y}$.
   - $(a+b){\bf x}=a{\bf x}+b{\bf x}$.
   - $ab{\bf x}=a(b{\bf x})$.
   - $1{\bf x}={\bf x}$.

**Definition**: An *inner product* on a vector space $V$ is a function that maps two vectors  ${\bf x}, {\bf y} \in V$ to a scalar $\langle {\bf x},{\bf y}\rangle \in
\mathbb{C}$ and satisfies the following conditions:

- Positive definiteness:
  $$
  \langle {\bf x},{\bf x}\rangle \;\ge\; 0,\;\;\;\;\;\;\;\; \langle {\bf x},{\bf x}\rangle =0\;\;\; \mbox{iff}\;\;{\bf x}={\bf0}.
  $$

- Conjugate symmetry:
  $$
  \langle {\bf x},{\bf y}\rangle=\overline{\langle {\bf y},{\bf x}\rangle}.
  $$
  If the vector space is real, the inner product becomes *symmetric*:
  $$
  \langle {\bf x},{\bf y}\rangle=\langle {\bf y},{\bf x}\rangle.
  $$

- Linearity in the first variable:
  $$
  \langle a{\bf x}+b{\bf y},{\bf z}\rangle=a\langle {\bf x},{\bf z}\rangle+b\langle {\bf y},{\bf z}\rangle
  $$
  where $a,b\in \mathbb{C}$. The linearity does not apply to the second variable:
  $$
  \langle {\bf x}, a{\bf y}+b{\bf z}\rangle = \overline{\langle a{\bf y} + b {\bf z}, {\bf x} \rangle}
   = \overline{a\langle {\bf y},{\bf x}\rangle + b\langle {\bf z},{\bf x}\rangle} = \overline{a}\langle {\bf x},{\bf y}\rangle + \overline{b}\langle {\bf x}, {\bf z} \rangle \ne a\langle {\bf x},{\bf y}\rangle+b\langle {\bf x},{\bf z}\rangle
  $$
  unless the coefficients are real $a,b \in \mathbb{R}$. As a special case, when $b=0$, we have
  $$
  \langle a{\bf x},{\bf y} \rangle = a\langle {\bf x},{\bf y} \rangle, \quad \langle {\bf x},a{\bf y}\rangle=\overline{a}\langle {\bf x},{\bf y}\rangle
  $$
  More generally we have
  $$
  \left\langle \sum_n c_n {\bf x}_n,{\bf y}\right\rangle = \sum_n c_n \langle {\bf x}_n, {\bf y} \rangle, \quad \left \langle {\bf x}, \sum_n c_n {\bf y}_n \right \rangle  =\sum_n \overline{c}_n \langle {\bf x},{\bf y}_n\rangle
  $$

**Definition**: A vector space with inner product defined is called an **inner product space**.

**Definition**: When the inner product is defined, $\mathbb{C}^N$ is called a *unitary space* and $\mathbb{R}^N$ is called a *Euclidean space*.

**Definition**: If the inner product of two vectors ${\bf x}$ and ${\bf y}$ is zero,  $\langle {\bf x}, {\bf y}\rangle=0$, they are **orthogonal** (**perpendicular**) to each other, denoted by ${\bf x}\; \bot\; {\bf y}$.

**Definition**: The norm (or length) of a vector ${\bf x}\in V$ is defined as
$$
\Vert{\bf x}\Vert = \sqrt{\langle {\bf x},{\bf x}\rangle} = \langle {\bf x},{\bf x}\rangle^{1/2}, \quad \mbox{or} \; \Vert{\bf x}\Vert^2 = \langle {\bf x},{\bf x}\rangle
$$
The norm $\Vert{\bf x}\Vert$ is non-negative and it is zero if and only if  ${\bf x}={\bf0}$. In particular, if $\Vert{\bf x}\Vert=1$, then it is said to be *normalized* and becomes a *unit vector*. Any vector can be normalized when divided by its own norm: ${\bf x}/\Vert{\bf x}\Vert$. The vector norm squared $\Vert{\bf x}\Vert^2=\langle {\bf x},{\bf x}\rangle$ can be considered as the energy of the vector.

**Example**: In an $N$-D unitary space, the $p$-norm of a vector  ${\bf x}=[x[1],\ldots,x[N]]^\textrm{T} \in \mathbb{C}^N$ is 
$$
\Vert{\bf x}\Vert _p = \left[\sum_{n=1}^N \vert x[n]\vert^p\right]^{1/p}
$$
**Definition**: In a unitary space $\mathbb{C}^N$, the $p$-norm distance between two vectors ${\bf x}$ and ${\bf y}$ is defined as the $p$-norm of the difference ${\bf x}-{\bf y}$: 
$$
d_p({\bf x},{\bf y})=\left( \sum_{n=1}^N \vert x[n]-y[n]\vert^p\right)^{1/p}
$$
