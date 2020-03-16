## Hough Transform

Reference: [Hough Transform](http://fourier.eng.hmc.edu/e161/lectures/hough/index.html)

### 1. Definition

Various shapes (straight lines, circles, ellipses, etc.) in the image can be described by a general equation: 

$$
f(x,y,\alpha_1,\cdots,\alpha_n)=0
$$
where $x$ and $y$ are two variables representing the position in the image, while $\{\alpha_1, \cdots, \alpha_n\}$ is a set of $n$ parameters specifying the shape. Or, equivalently, the same equation can represent some different shape in an $n$-dimensional space spanned by $\{\alpha_1, \cdots, \alpha_n\}$ as $n$ variables, with $x$ and $y$ treated as two parameters.

A specific 2D image shape can therefore be represented either locally in the image space by the pixels $(x,y)$ on the boundary of the shape (such as edge detection), or globally in the parameter space by the parameters $(\alpha_1,
\cdots, \alpha_n)$. These two representations are linked by the **Hough transform**:

- **The forward transform**

  Every point $(x,y)$ in the 2D image space is transformed to a hyper-surface in the $n$-dimensional parameter space;

- **The inverse transform**

  Every point $(\alpha_1,
  \cdots, \alpha_n)$ in the $n$-D parameter space describes a specific instance of the shape of interest in the 2D image space.

### 2. Straight Line Detection

A straight line in 2D space described by this parametric equation: 

$$
f(x,y,\rho_0,\theta_0)=x\; \cos\; \theta_0 + y\; \sin\; \theta_0 -\rho_0 = 0
$$
can be represented in the 2D parameter space by a point $(\rho_0, \theta_0)$, where $\rho_0$ is the distance between the straight line and the origin, and $\theta_0$ is the angle between the distance vector and the positive $x$-direction.

<center class='half'><img src='./pictures/24-hough_2c.gif' width=33%><img src='./pictures/24-hough_2b.gif' width=40%></center>
点 $B$ 的坐标为 $(\rho_0 \cos \theta_0, \rho_0 \sin \theta_0)$，向量 $\overrightarrow{BP} = (x - \rho_0 \cos \theta_0, y - \rho_0 \sin \theta_0)$，$\overrightarrow{OB}$ 与 $\overrightarrow{BP}$ 正交。所以有：
$$
\begin{align} [\rho_0 \cos \theta_0, \rho_0 \sin \theta_0] \cdot \begin{bmatrix} x-\rho_0 \cos \theta_0 \\ y - \rho_0 \sin \theta_0 \end{bmatrix} & = x \; \rho_0 \cos \theta_0 - \rho_0^2 \cos^2 \theta_0 + y \; \rho_0 \sin \theta_0 - \rho_0^2 \sin^2 \theta_0 \\ & = x \; \rho_0 \cos \theta_0 + y \; \rho_0 \sin \theta_0 - \rho_0^2 ( \cos^2 \theta_0 + \sin^2 \theta_0) \\ & = x \; \rho_0 \cos \theta_0 + y \; \rho_0 \sin \theta_0 - \rho_0^2 \\ & = x \; \cos \theta_0 + y \; \sin \theta_0 - \rho_0 = 0 \end{align}
$$
To find the Hough transform of a certain point $(x_0,y_0)$ in the image space, we solve the equation above for $\rho$ and get
$$
\begin{align} \rho & = x_0\; \cos \;\theta + y_0\; \sin\; \theta \\ & = \sqrt{x_0^2+y_0^2} \;\; \left( \frac{x_0}{\sqrt{x_0^2+y_0^2}}\; \cos\; \theta + \frac{y_0}{\sqrt{x_0^2+y_0^2}}\; \sin\; \theta) \right) \\ & = r_0\; (\cos\; \alpha_0\; \cos\; \theta + \sin\; \alpha_0\; \sin\; \theta) \\ & = r_0\; \cos (\alpha_0-\theta) \end{align}
$$
where 
$$
\left\{ \begin{align} r_0 & \stackrel{\triangle}{=} \sqrt{x_0^2 + y_0^2} \\ \alpha_0 & \stackrel{\triangle}{=} \tan^{-1} (y_0/x_0)
\end{align} \right. 
$$

- **Forward Hough transform**

  We see from the equation above that any point $(x_0,y_0)$ in the image space is transformed into a sinusoidal curve in the parameter space. On the other hand, a point $(\theta, \rho)$ on this sinusoidal curve represents a straight line passing through the point $(x_0,y_0)$ in the image space, as shown in the figures below, where $x_0=y_0=1$, i.e., $r_0=\sqrt{2}=1.4142$, $\alpha_0=\tan^{-1} (y_0/x_0) =\pi/2$. Note that the four particular directions labeled as 2, 3, 4 and 1 in the image (left) correspond respectively to the points in the Hough space (right):

  |          |    Point 1    |     Point 2     | Point 3 |    Point 4    |
  | :------: | :-----------: | :-------------: | :-----: | :-----------: |
  | $\theta$ | $\pi/2=1.571$ | $-\pi/4=-0.785$ |    0    | $\pi/4=0.785$ |
  |  $\rho$  |       1       |        0        |    1    |    1.4142     |

  <center class='half'><img src='./pictures/24-hough_1.gif' width=33%><img src='./pictures/24-hough_1a.gif' width=50%></center

- **Inverse Hough Transform**

  Any given point $(\rho_0, \theta_0)$ in the parameter space can be inverse transformed to the spatial domain to represent a straight line specified by the equation 
  $$
  x\; \cos\; \theta_0 + y\; \sin\; \theta_0 -\rho_0 = 0 
  $$
  The figures below show an example of three image points $A=(0,2)$, $B=(1,1)$, and  $C=(2,0)$ all on a straight line. These points are mapped to the parameter space as three sinusoidal curves in blue, red and green, respectively. The fact that these points are on a straight line guarantees that the corresponding curves intersect at a point ( $\rho=\sqrt{2}=1.4142$ and $\theta=\pi/4=0.79$) representing the straight line 
  $$
  \rho=x \; \cos \theta + y \;\sin \theta \qquad \mbox{i.e.} \;1.414 = \; x \cos(\pi/4) + y \;\sin(\pi/4)\;\;\; \mbox{i.e.} \quad x+y=2
  $$
  

<center class='half'><img src='./pictures/24-hough_2.gif' width=33%><img src='./pictures/24-hough_2a.gif' width=50%></center

The detection of all straight lines in the image can be carried out by the following steps:

1. Make available an $n=2$ dimensional array $H(\rho,\theta)$ for the parameter space;

2. Find the gradient image: $G(x,y)=\vert G(x,y)\vert \angle G(x,y)$;

3. For any pixel satisfying $\vert G(x,y)\vert > T_s$, increment all elements on the sinusoidal curve $\rho=x\; cos\; \theta + y\; sin\; \theta$ in the parameter space represented by the H array:

   for all $\theta$ {
   $$
   \rho=x\; cos\; \theta + y\; sin\; \theta
   $$

   $$
   H(\rho, \theta)=H(\rho, \theta)+1
   $$

   }

4. In the parameter space, any element $H(\rho, \theta) > T_h$ represents a straight line detected in the image.

This algorithm can be improved by making use of the gradient direction  $\angle G$, which, in this particular case, is the same as the angle $\theta$. Now for any point $\vert G(x,y)\vert > T_s$, we only need to increment the elements on a small segment of the sinusoidal curve. The third step in the above algorithm can be modified as:

1. (same as above)

2. (same as above)

3. For any pixel satisfying $\vert G(x,y)\vert > T_s$,

   for all $\theta$ satisfying $\angle G(x, y)-\Delta \theta \leq \theta \leq \angle G(x, y)+\Delta \theta$ {
   $$
   \rho=x\; cos\; \theta + y\; sin\; \theta
   $$

   $$
   H(\rho, \theta)=H(\rho, \theta)+1
   $$

   }

   where $\Delta \theta$ defines a small range in $\theta$ to allow some room for error in $\angle G$.

4. (same as above)

### 3. Make Use of $\angle G$

The gradient direction $\angle G$ of an edge pixel can be used to reduce the computation in the parameter space. First recall that the tangent direction or the derivative $dy/dx$ of an implicit function $f(x,y)=0$ can be found as:

$$
\tan\; \phi = \frac{dy}{dx} = -\frac{f'_x(x,y)}{f'_y(x,y)}
$$
where $f'_x(x,y)$ and $f'_y(x,y)$ are the partial derivatives of the function $f(x,y)$ with respect to $x$ and $y$, respectively. Also note that at an arbitrary point on the curve specified by the equation $f(x,y)=0$, the gradient direction is always perpendicular to the tangent direction:
$$
\phi = \angle G \pm \frac{\pi}{2}
$$
i.e.,
$$
\frac{dy}{dx}=-\frac{f'_x(x,y)}{f'_y(x,y)} = \tan \;\phi = \tan\;(\angle G \pm \frac{\pi}{2}) = -\cot\; \angle G
$$
or
$$
\frac{f'_x(x,y)}{f'_y(x,y)} =\cot\; \angle G
$$
as shown in the figure:

<img src='./pictures/24-hough_3.gif' width=45%>

Now the gradient direction $\angle G$ can be used to establish a second equation, in addition to the original equation describing the shape, and thereby reducing the dimensions of the hyper-surface in the parameter space that needs to be incremented by 1. 

$$
\left\{ \begin{array}{l} f(x,y,\alpha_1,\cdots,\alpha_n)=0 \\ f'_x(x,y,\alpha_1,\cdots,\alpha_n) / f'_y (x, y, \alpha_1, \cdots, \alpha_n) = \cot\; \angle G \end{array} \right.
$$

### 4. Detection of Circles

A **circle** in the image can be described by 

$$
f(x,y,x_0,y_0,r)=(x-x_0)^2+(y-y_0)^2-r^2=0
$$
where $x_0$, $y_0$, and $r$ are three parameters which span a 3D Hough space. Any point $(x,y)$ in the image corresponds to a cone shaped surface in the 3D parameter space. To use $\angle G$, consider the derivative
$$
\frac{dy}{dx}=-\frac{f'_x(x,y)}{f'_y(x,y)}=-\frac{x-x_0}{y-y_0}
$$
Now we only need to increment those elements in the parameter space that satisfy both of the following equations 

$$
\left\{ \begin{array}{l} (x-x_0)^2+(y-y_0)^2-r^2=0 \\ (x- x_0)/(y-y_0) = \cot\;\angle G = \cos\;\angle G/\sin\;\angle G \end{array} \right.
$$
Geometrically these two simultaneous equations represent the intersection of a cone specified by the first equation and a plane specified by the second equation which passes through the axis of the cone, as shown in the figure below.

<img src='./pictures/24-hough_4.gif' width=45%>

Solving these equations for $x_0$ and $y_0$, we get 

$$
\left\{ \begin{array}{l} x_0=x \pm r\; \cos\;\angle G \\ y_0=y \pm r\; \sin\;\angle G \end{array} \right.
$$
and the algorithm for detecting circles:

- For any pixel satisfying $\vert G(x,y)\vert > T_s$, increment all elements satisfying the two simultaneous equations above;

  for all $r$
  {
  $$
  \left\{ \begin{array}{l} x_0=x \pm r\; \cos\;\angle G \\ y_0=y \pm r\; \sin\;\angle G \end{array} \right.
  $$

  $$
  H(x_0,y_0,r)=H(x_0,y_0,r)+1
  $$

  }

- In the parameter space, any element $H(x_0,y_0,r) > T_h$ represents a circle with radius $r$ located at $(x_0,y_0)$ in the image.

### 5. Detection of Ellipses

Here we assume the axes of the ellipses are in parallel with the coordinates of the image space, i.e., the equations specifying the ellipses are in the following standard form 

$$
f(x,y,x_0,y_0,a,b)=\frac{(x-x_0)^2}{a^2}+\frac{(y-y_0)^2}{b^2}-1=0
$$
where $x_0$, $y_0$, $a$ and $b$ are four parameters which span a 4D parameter Hough space. To use $\angle G$, consider
$$
\frac{dy}{dx}=-\frac{f'_x(x,y)}{f'_y(x,y)}=-\frac{(x-x_0)/a}{(y-y_0)/b}
$$
Now we only need to increment those elements in the parameter space that satisfy both of the following equations 

$$
\left\{ \begin{array}{l} (x-x_0)^2/a^2+(y-y_0)^2/b^2-1=0 \\ (x-x_0)b/(y-y_0)a = \cot\;\angle G \end{array} \right.
$$
Solving these equations for $x_0$ and $y_0$, we get 

$$
\left\{ \begin{array}{l} x_0=x \pm a\; \cos\;\angle G \\ y_0=y \pm b\; \sin\;\angle G \end{array} \right.
$$
and the algorithm for detecting circles:

- For any pixel satisfying $\vert G(x,y)\vert > T_s$, increment all elements satisfying the two simultaneous equations above;
  for all $a$ and all  $b$
  {
  $$
  \left\{ \begin{array}{l} x_0=x \pm a\; \cos\;\angle G \\ y_0=y \pm b\; \sin\;\angle G \end{array} \right.
  $$

  $$
  H(x_0,y_0,a,b)=H(x_0,y_0,a,b)+1
  $$

  }

- In the parameter space, any element $H(x_0,y_0,a,b) > T_h$ represents an ellipse detected in the image.

### 6. Generalized Hough transform

Two possible difficulties may occur in the above Hough transform method: (a) the shape has to be described by an equation, and (b) the number of parameters $n$ (dimensions of the parameter space) may be high. Given the two equations: 

$$
\left\{ \begin{array}{l} f(x,y,\alpha_1,\cdots,\alpha_n)=0 \\ f'_x (x, y, \alpha_1, \cdots, \alpha_n) / f'_y (x, y, \alpha_1,\cdots,\alpha_n) = \cot\; \angle G \end{array} \right.
$$
we still have to search the remaining $n-2$ dimensions of the parameter space. These two difficulties can be avoided by the generalized Hough transform shown below.

1. **Preparation: build a table for the given shape**

   - Prepare a table with $k$ entries each indexed by an angle $\phi_i$ ( $i=1, \cdots, K$) which increases from 0 to 180 degrees with increment $180/k$, where $k$ is the resolution of the gradient orientation (see below).

   - Define a reference point $(x_c, y_c)$ somewhere inside the 2D shape (e.g., the gravitational center). For each point $(x,y)$ on the boundary of the shape, find two parameters 
     $$
     \left\{ \begin{array}{l} r=\sqrt{(x-x_c)^2+(y-y_c)^2} \\ \beta = \tan^{-1} \;(y-y_c)/(x-x_c) \end{array} \right.
     $$
     and the gradient direction $\angle G$. Add the pair $(r, \beta)$ to the table entry with its $\phi$ closest to $\angle G$.

   - Prepare a 2D Hough array $H(x_c, y_c)$ initialized to 0.

<img src='./pictures/24-hough_5.gif' width=50%>

<img src='./pictures/24-img80.png' width=55%>

2. **Detection of the shape and its locations in image**

   For each image point $(x,y)$ with $\vert G(x,y)\vert > T_s$, find the table entry with its corresponding angle $\phi_j$ closest to $\angle G(x,y)$. Then for each of the $n_j$ pairs $(r, \beta)_i$ ( $i=1,\cdots,n_j$) in this table entry, find
   $$
   \left\{ \begin{array}{l} x_c=x+r \; \cos \; \beta \\ y_c=y+r \; \sin \; \beta \end{array} \right.
   $$
   and increment the corresponding element in the H array by 1:
   $$
   H(x_c,y_c)=H(x_c,y_c)+1
   $$
   All elements in the H table satisfying $H(x_c, y_c) > T_h$ represent the locations of the shape in the image.

   It is desirable to detect a certain 2D shape independent of its orientation and scale, as well as its location. To do so, two additional parameters, a scaling factor  $S$ and a rotational angle $\theta$, are needed to describe the shape. Now the Hough space becomes 4-dimensional $H(x_c, y_c, S, \theta)$. The detection algorithm becomes the following:

   For each image point $(x,y)$ with $\vert G(x,y)\vert>T$, find the proper table entry with $\phi_j=\angle G(x,y)$. Then for each of the $n_j$ pairs $(r, \beta)_i$ ( $i=1,\cdots,n_j$) in this table entry, do the following for all $S$ and $\theta$: find
   $$
   \left\{ \begin{array}{l} x_c=x+r\;S\; \cos (\beta+\theta) \\ y_c=y+r\;S\; \sin (\beta+\theta) \end{array} \right.
   $$
   and increment the corresponding element in the 4D H array by 1:
   $$
   H(x_c,y_c,S,\theta)=H(x_c,y_c,S,\theta)+1
   $$
   All elements in the H table satisfying $H(x_c, y_c,S,\theta) > T_h$ represent the scaling factor $S$, rotation angle $\theta$ of the shape, as well as its reference point location $(x_c, y_c)$ in the image.