## The Fourier Transform

Reference: [The Fourier Transform](http://fourier.eng.hmc.edu/e161/lectures/fourier/index.html)

### 1. Four different forms of the Fourier transform

- **Non-periodic, continuous time function $x(t)$, continuous, non-periodic spectrum $X(f)$**

  This is the most general form of Fourier transform.
  $$
  X(f)=\int_{-\infty}^{\infty} x(t) e^{-j2\pi ft} dt,\;\;\;\;\; x(t)=\int_{-\infty}^{\infty} X(f) e^{j2\pi ft} df
  $$
  Alternatively, as $\omega=2\pi f$, we have
  $$
  X(\omega)=\int_{-\infty}^{\infty} x(t) e^{-j\omega t} dt,\;\; x(t) = \frac{1}{2\pi}\int_{-\infty}^{\infty} X(\omega) e^{j\omega t} d\omega
  $$
  The first one is the *forward* transform, and the second one is the *inverse* transform.

- **Non-periodic, discrete time function $x[n]$, continuous, periodic spectrum $X_F (f)$**

  The discrete time function can be considered as a sequence of samples of continuous time function. The time interval between two consecutive samples $x[m]$ and $x[m+1]$ is $t_0=1/F$, where $F$ is the sampling rate, which is also the period of the spectrum in the frequency domain.

  The discrete time function can be written as 
  $$
  x(t)=\sum_{m=-\infty}^{\infty} x[m]\delta(t-mt_0)
  $$
  and its transform is:
  $$
  X_F(f)=\sum_{m=-\infty}^{\infty} x[m]e^{-j2\pi fmt_0},\;\;\;\ x[m]=\frac{1}{F} \int_{-F/2}^{+F/2} X_F(f) e^{j2\pi fmt_0} df \qquad (m=0, \pm 1, \pm 2, \cdots)
  $$
  We can verify that the spectrum is indeed periodic:
  $$
  X_F(f+kF)=X_F(f+k/t_0)=\sum_{m=-\infty}^{\infty} x[m]e^{-j2\pi (f+k/t_0)mt_0}=X_F(f)
  $$
  (for $k=\pm 1, \pm 2, \cdots$) because $e^{\pm j2\pi mk} =1 $.

- **Periodic, continuous time function $x_T(t)$, discrete, non-periodic spectrum $X[n]$**

  This is the Fourier series expansion of periodic functions. The time period is $T$, and the interval between two consecutive frequency components is $f_0=1/T$, and its transform is:
  $$
  X[n]=\frac{1}{T} \int_T x_T(t) e^{-j2\pi nf_0t} dt,\;\;\;\;\;\; x_T(t)=\sum_{n=-\infty}^{\infty} X[n]e^{j2\pi nf_0t} \qquad (n=0, \pm 1, \pm 2, \cdots)
  $$
  The discrete spectrum can also be represented as:
  $$
  X(f)=\sum_{n=-\infty}^{\infty} X[n]\delta(f-nf_0)
  $$
  We can verify that the time function is indeed periodic:
  $$
  x_T(t+kT)=x_T(t+k/f_0)=\sum_{n=-\infty}^{\infty} X[n]e^{-j2\pi nf_0(t+k/f_0)}=x_T(t) \quad (\mbox{for} \; k=\pm 1, \pm 2, \cdots)
  $$
  <img src='./pictures/12-FSEsquaresawtooth.gif' width=100%>

- **Periodic, discrete time function $x[m]$, discrete, periodic spectrum  $X[n]$**

  This is the <font color='red'>discrete Fourier transform (DFT)</font>.
  $$
  X[n]=\frac{1}{T}\sum_{m=0}^{N-1} x[m]e^{-j2\pi nmf_0t_0}, \quad x[m]=\frac{1}{F}\sum_{n=0}^{N-1} X[n]e^{j2\pi nmf_0t_0}, \quad m,n=0, 1, \cdots, N-1
  $$
  Here $N$ is the number of samples in the period $T$, which is also the number of frequency components in the spectrum:
  $$
  N=\frac{T}{t_0}=\frac{1/f_0}{1/F}=\frac{F}{f_0}
  $$
  We therefore also have $TF=N$ and $t_0f_0=1/N$.

  The DFT can be redefined as
  $$
  \begin{align} X[n] & = \frac{1}{\sqrt{N}}\sum_{m=0}^{N-1} x[m]e^{-mn j2\pi/N} = \sum_{m=0}^{N-1} w_N^{mn} x[m], \\ x[m] & = \frac{1}{\sqrt{N}}\sum_{n=0}^{N-1} X[n]e^{mn j2\pi/N} = \sum_{n=0}^{N-1} w_N^{-mn} X[n] \end{align} \qquad m,n=0, 1, \cdots, N-1
  $$
  where $w_N\stackrel{\triangle}{=}e^{-j2\pi/N}/\sqrt{N}$.

  We can easily verify that the time function and its spectrum are indeed periodic: $x[m+kN]=x[m]$ and $X[n+kN]=X[n]$.

  Note that there are alternative ways for arranging the constant coefficients. For example, one could define the forward transform without the coefficient $1/\sqrt{N}$, and have $1/N$ as the coefficient for the inverse transform.

  <img src='./pictures/12-fourier4.gif' width=70%>

### 2. Heisenberg Uncertainty Principle

A time signal $x(t)$ contains the complete information in time domain, i.e., the amplitude of the signal at any given moment $t$. However, no information is explicitly available in $x(t)$ in terms of its frequency contents. On the other hand, the spectrum  $X(f)={\cal F}[x(t)]$ of the signal obtained by the Fourier transform (or any other orthogonal transform such as discrete cosine transform) is extracted from the entire time duration of the signal, it contains complete information in frequency domain in terms of the magnitudes and phases of the frequency component at any given frequency $f$, but there is no information explicitly available in the spectrum regarding the temporal characteristics of the signal, such as when in time certain frequency contents appear. In this sense, neither $x(t)$ in time domain nor $X(f)$ in frequency domain provides complete description of the signal. In other words, we can have either temporal or spectral locality regarding the information contained in the signal, but never both.

- The **short-time Fourier transform (STFT)**, also called *windowed Fourier transform*, can be used to address this dilemma. The signal $x(t)$ to be Fourier analyzed is first truncated by a time window function $w(t)$ which is zero outside a certain time interval $T$, such as a square or Gaussian window, before it is transformed to the frequency domain. Now any characteristics appearing in the spectrum will be known to be from within this particular time window. In time domain, the windowed signal is:
  $$
  x_w(t)=x(t)  w(t)
  $$
  According to convolution theorem, this equation corresponds to the following in frequency domain: 
  $$
  X_w(f)=X(f) * W(f)
  $$
  where $X_w(f)={\cal F}[x_w(t)]$ and $W(f)={\cal F}[ w(t) ]$ are the spectra of $x_w(t)$ and $w(t)$, respectively. Now we know that all frequency components present in the spectrum $X_w(f)$ exist inside the time window, and the narrower the time window, the better the temporal resolution. However, on the other hand, the spectrum $X_w(f)$ of the windowed signal is a blurred version of the true signal spectrum $X(f)$, due to the convolution with the spectrum $W(f)$ of the window. Moreover, we see that while the temporal resolution can be increased by a narrow window $w(t)$, the frequency resolution will be reduced due to the expanded spectrum $W(f)$. Similarly, a narrower  $W(f)$ for better frequency resolution corresponds to a wider window causing poorer temporal resolution.

- **Fourier series expansion**. If we assume the windowed signal repeats itself outside the window, i.e., it becomes a periodic signal with period $T$. The spectrum of this periodic signal is discrete, weighted by the Fourier coefficients, with a gap $f_0=1/T$, the fundamental frequency, between every two consecutive frequency components, i.e., 
  $$
  T f_0 =1
  $$
  This relationship indicates that it is impossible to increase both the temporal resolution (reduced $T$) and the frequency resolution (reduced $f_0$). When one of the resolutions is improved, the other must suffer.

- The **uncertainty principle** describes the general phenomenon quantitatively, similar to the **Heisenberg Uncertainty Principle** in quantum physics which states that it is impossible to precisely measure both the position and momentum of a microscopic particle at the same time. The more precisely one of the quantities is measured, the less precisely the other is known.

  To show this, we borrow the concept of probability density function (PDF) from the probability theory. Any given time signal $x(t)$ can be treated as a PDF by normalization: 
  $$
  p_x(t)=\frac{\vert x(t)\vert^2}{\Vert x(t)\Vert^2} = \frac{\vert x(t)\vert^2}{\langle x(t), x(t) \rangle} = \frac{\vert x(t)\vert^2}{\int_{-\infty}^\infty \vert x(t)\vert^2  dt}
  $$
  where the denominator is the total energy of the signal $x(t)$ assumed to be finite; i.e., $x(t)$ is an energy signal. As $p_x(t)$ satisfies these conditions
  $$
  p_x(t)>0 \qquad \mbox{and} \qquad \int_{-\infty}^\infty p_x(t)  dt=1
  $$
  How the signal $x(t)$ spreads over time; i.e., the locality or the dispersion of $x(t)$, can be measured as the variance of this probability density $p_x(t)$:
  $$
  \sigma_t^2 = \int_{-\infty}^\infty (t-\mu_t)^2 p_x(t) dt = \frac{1}{\Vert x(t) \Vert ^2} \int_{-\infty}^\infty (t-\mu_t)^2 \vert x(t)\vert^2 dt
  $$
  where $\mu_t$ is the mean of $p_x(t)$:
  $$
  \mu_t=\int_{-\infty}^\infty t p_x(t) dt = \frac{1}{\Vert\ x(t)\Vert^2}\int_{-\infty}^\infty t \vert x(t)\vert^2 dt
  $$
  Similarly, in the frequency domain, the locality or dispersion of the spectrum of the signal can also be measured as 
  $$
  \sigma_f^2 = \frac{1}{\Vert X(f)\Vert^2} \int_{-\infty}^{\infty} (f-\mu_f)^2 \vert X(f)\vert^2 = \frac{1}{\Vert x(t) \Vert^2} \int_{-\infty}^\infty (f-\mu_f)^2 \vert X(f)\vert^2 df
  $$
  Here, we have used Parseval's identity $\Vert x(t)\Vert^2=\Vert X(f)\Vert^2$, and $\mu_f$ is defined as 
  $$
  \mu_f=\frac{1}{\Vert X(f)\Vert^2}\int_{-\infty}^\infty f \vert X(f)\vert^2  df
  $$
  The **uncertainty principle**:

  Let $X(f)={\cal F}[x(t)]$ be the Fourier spectrum of a given function $x(t)$ and $\sigma_t^2$ and $\sigma_f^2$ be defined as above. Then 
  $$
  \sigma_t^2 \sigma_f^2 \ge \frac{1}{16\pi^2}
  $$

### 3. Physical Meaning of 1-D FT

Here we only consider the Fourier expansion of continuous and periodic signals. The result here can be easily generalized to all other forms of the Fourier transform. The Fourier expansion of a 1D periodic signal 

$$
x_T(t)=\sum_{n=-\infty}^{\infty} X[n]e^{j2\pi nf_0t}
$$
represents the signal as a weighted sum of complex exponential functions. Here $X[n]$ is the weight of the nth term (the Fourier coefficient) which is in general a complex number
$$
X[n]=X_r[n]+jX_j[n]=\vert X[n]\vert e^{j\angle(X[n])}=\frac{1}{T}\int_T x_T(t)e^{-j2\pi nf_0t} dt
$$
where $\vert X[n]\vert$ and $\angle X[n]$ are respectively the magnitude and phase of the nth coefficient $X[n]$: 
$$
\left\{ \begin{align} \vert X[n]\vert & = \sqrt{X_r[n]^2+X_j[n]^2} \\ \angle X[n] & =\tan^{-1} \left[ X_j[n]/X_r[n] \right] \end{align} \right.
$$
In particular, when $n=0$, $X[0]$ is the average or DC component of the signal: 
$$
X[0]=\frac{1}{T}\int_T x(t) dt
$$
The Fourier coefficient can be further written as:
$$
\begin{align} X[n] & = \frac{1}{T} \int_T x(t)e^{-j2\pi f_0nt}\;dt = \frac{1}{T}\int_T [x_r(t)+jx_i(t)]\;
\left[\cos(2\pi f_0nt)-j\;\sin(2\pi f_0nf)\right] dt \\ & = \frac{1}{T}\int_T \left[x_r(t)\cos(2\pi f_0nt)+x_i(t)\sin(2\pi f_0 nt \right] + \frac{j}{T}\int_T \left[x_i(t)\cos(2\pi f_0nt)-x_r(t)\sin(2\pi f_0n)\right] dt \\ & = X_r[n]+jX_i[n] \end{align}
$$
where $X_r[n]$ and $X_i[n]$ are respectively the real and imaginary part of the spectrum: 
$$
X_r[n]=Re[X[n]]=\frac{1}{T}\int_T \left[x_r(t)\cos(2\pi f_0nt)+x_i(t)\sin(2\pi f_0nt)\right] dt
$$
and
$$
X_i[n]=Im[X[n]]=\frac{1}{T}\int_T \left[x_i(t)\cos(2\pi f_0nt)-x_r(t)\sin(2\pi f_0n)\right] dt
$$
In particular, if $x_i(t)=0$, i.e., the signal $x(t)=x_r(t)$ is real, and the real and imaginary parts of $X[n]$ become even and odd functions of $n$, respectively:
$$
X_r[n]=\frac{1}{T}\int_T x_r(t)\cos(2\pi f_0nt)  dt = X_r[-n], \quad X_i[n]=\frac{1}{T}\int_T -x_r(t)\sin(2\pi f_0n) dt=-X_i[-n]
$$
and we have
$$
X[-n]=X_r[-n]+jX_i[-n]=X_r[n]-jX_i[n]=X^*[n]
$$
Now the Fourier expansion of $x(t)$ can be written as

 

Now we see that any real signal $x(t)$ can be represented as a weighted sum of an infinite number of sinusoids of frequencies $f_n=nf_0$ with amplitudes $2 \vert X[n]\vert$ and phases $\angle X[n]$.
$$
\begin{align} x(t) & = \sum_{n=-\infty}^{\infty} X[n]e^{j2\pi nf_0t} = X[0]+\sum_{n=1}^{\infty} \left[ X[n]e^{j2\pi nf_0t}+X[-n]e^{-j2\pi nf_0t}\right] \\ & = X[0]+\sum_{n=1}^{\infty} \left[ X[n]e^{j2\pi nf_0t}+X^*[n]e^{-j2\pi n f_0 t} \right] = X[0] + \sum_{n=1}^{\infty} \left[ \vert X[n] \vert e^{j \angle X[n] } e^{j 2\pi n f_0 t} + \vert X[n] \vert e^{-j \angle X[n] } e^{-j 2\pi n f_0 t} \right] \\ & = 
X[0]+\sum_{n=1}^{\infty} \vert X[n] \vert \left[ e^{j (2 \pi n f_0 + \angle X[n])} + e^{-j (2 \pi n f_0 t + \angle X[n])} \right] = X[0] + \sum_{n=1}^{\infty} 2 \vert X[n] \vert cos (2 \pi n f_0 t + \angle x[n])
\end{align}
$$
Now we see that any real signal $x(t)$ can be represented as a weighted sum of an infinite number of sinusoids of frequencies $f_n=nf_0$ with amplitudes $2 \vert X[n]\vert$ and phases $\angle X[n]$.

### 4. The $\delta$ function and orthogonal bases

The discrete and periodic time function and spectrum can be written as, respectively

$$
\begin{align} x_T(t) & = \sum_{m=-\infty}^{\infty} x[m]\delta(t-mt_0) \\ X_F(f) & = \sum_{n=-\infty}^{\infty} X[n]\delta(f-nf_0) \end{align}
$$
The $\delta$ function used above satisfies the following:
$$
\delta(t-\tau)=\left\{ \begin{array}{ll} 0 & t\ne \tau \\ \infty & t=\tau \end{array} \right.
$$

$$
\int_{-\infty}^{\infty} \delta(t)dt=1
$$

$$
\int_{-\infty}^{\infty} x(t)\delta(t-\tau)dt=x(\tau)
$$

### 5. Matrix form of the DFT

The forward and inverse DFT can be written as:

$$
\begin{align} X[n] & = \frac{1}{\sqrt{N}}\sum_{m=0}^{N-1} x[m]e^{-mn j2\pi/N} = \sum_{m=0}^{N-1} w_N^{mn} x[m], \\ x[m] & = \frac{1}{\sqrt{N}} \sum_{n=0}^{N-1} X[n]e^{mn j2\pi/N} = \sum_{n=0}^{N-1} w_N^{-mn} X[n] \end{align} \qquad m,n=0, 1, \cdots, N-1
$$
where we have defined
$$
w_{mn}\stackrel{\triangle}{=}\frac{1}{\sqrt{N}}(e^{-j2\pi/N})^{mn} \;\;\;\;\;\;\; w_{mn}^*=\frac{1}{\sqrt{N}}(e^{j2\pi/N})^{mn}
$$
and $w_{mn}^*$ is its complex conjugate of $w_{mn}$. We can further define an  $N \times N$ square matrix
$$
{\bf W} = \left[{\bf w}_0,\cdots,{\bf w}_{N-1}\right] = \begin{bmatrix} \cdot & \cdot & \cdot & \\ \cdot & w_{mn} & \cdot & \\ \cdot & \cdot & \cdot & \end{bmatrix}_{N\times N}
$$
where $w_{mn}$ is the element in the $m$-th row and $n$-th column of ${\bf W}$, and ${\bf w}_n=[w_{0n},\cdots,w_{(N-1)n}]^T$ is the nth column vector of ${\bf W}$. As $w_{mn}=w_{nm}$, ${\bf W}$ is symmetric
$$
{\bf W}^T={\bf W}
$$
Also, note that the columns (or rows) of ${\bf W}$ are orthogonal:
$$
\begin{align} \langle{\bf w}_m, {\bf w}_n \rangle & = {\bf w}^T_m {\bf w}^*_n \\ & = \sum_{k=0}^{N-1} w_{mk} w^*_{kn} \\ & = \frac1N \sum_{k=0}^{N-1} \left( e^{j 2\pi /N} \right)^{-mk} \left( e^{j 2\pi /N} \right)^{nk} \\ & = \frac1N \sum_{k=0}^{N-1} \left( e^{j 2\pi /N} \right)^{(n-m)k} \\ & \stackrel{*}{=} \delta_{mn} = \left\{ \begin{array}{ll} 1 & m=n \\ 0 & m \ne n \end{array} \right. \end{align}
$$

as

- If $m=n$, $(e^{j2\pi/N})^{(n-m)k}=1$ and $\langle{\bf w}_m, {\bf w}_n\rangle=1$,

- If $m\ne n$, the summation becomes:
  $$
  \sum_{k=0}^{N-1} \left(e^{j2\pi(n-m)/N} \right)^k = \frac{1-\left(e^{j2\pi(n-m)/N} \right)^N} {1-e^{j2\pi(n-m)/N}} = 0
  $$

Therefore ${\bf W}$ is a unitary matrix, as well and symmetric: 
$$
{\bf W}^{*T}={\bf W}^*={\bf W}^{-1}
$$
We further represent the discrete signal $x[m]$ and its DFT spectrum $X[n]$ as two N-D vectors:
$$
{\bf X}\stackrel{\triangle}{=} \begin{bmatrix} X[0] \\ \vdots \\ X[N-1] \end{bmatrix}_{N \times 1}, \qquad {\bf x} \stackrel{\triangle}{=} \begin{bmatrix} x[0] \\ \vdots \\ x[N-1] \end{bmatrix}_{N \times 1}
$$
then the DFT can be written more conveniently as a matrix-vector multiplication:
$$
{\bf X} = \begin{bmatrix} X[0] \\ \vdots \\ X[N-1] \end{bmatrix} = \frac{1}{\sqrt{N}} \begin{bmatrix} \cdot & \cdot & \cdot \\ \cdot & \left( e^{-j 2\pi /N} \right)^{mn} & \cdot \\ \cdot & \cdot & \cdot \\ \end{bmatrix} \begin{bmatrix} x[0] \\ \vdots \\ x[N-1] \end{bmatrix} = {\bf Wx}
$$
and
$$
{\bf x} = \begin{bmatrix} x[0] \\ \vdots \\ x[N-1] \end{bmatrix} = \frac{1}{\sqrt{N}} \begin{bmatrix} \cdot & \cdot & \cdot \\ \cdot & \left( e^{j 2\pi /N} \right)^{mn} & \cdot \\ \cdot & \cdot & \cdot \\ \end{bmatrix} \begin{bmatrix} X[0] \\ \vdots \\ X[N-1] \end{bmatrix} = {\bf W^*x} = {\bf W}^{-1} {\bf x}
$$
It is obvious that the computational complexity of this 1-D DFT takes is $O(N^2)$, which, as we will see later, can be reduced to $O(N\; log_2 N)$ by Fast Fourier Transform (FFT) algorithms.

### 6. An Example

Consider a discrete signal $x[n]$ of $N=8$ complex components:
$$
{\bf x} = \begin{bmatrix} x[0] \\ x[1] \\ x[2] \\ x[3] \\ x[4] \\ x[5] \\ x[6] \\ x[7] \end{bmatrix} = \begin{bmatrix} (0,0) \\ (0,0) \\ (2,0) \\ (3,0) \\ (4,0) \\ (0,0) \\ (0,0) \\ (0,0) \end{bmatrix}
$$
The foreword discrete Fourier transform is:
$$
\begin{align} X[n] & = X_r[n] + j X_i[n] = \sum_{m=0}^{N-1} x[m] e^{-j 2\pi mn/N} \\ & = \sum_{m=0}^{N-1} \left( x_r[m] + j x_i[m] \right) \left(\cos (2 \pi mn/N) - j \sin (2 \pi mn/N) \right) \end{align}
$$
or
$$
\left\{ \begin{align} X_r[n] & = \sum_{m=0}^{N-1} \left( x_r[m] \cos (2 \pi mn/N) + x_i[m] \sin (2 \pi mn/N) \right) \\ X_i[n] & = \sum_{m=0}^{N-1} \left( x_i[m] \cos (2 \pi mn/N) - x_r[m] \sin (2 \pi mn/N) \right) \end{align} \right.
$$

The inverse transform is:
$$
x[m] = x_r[m] + j x_i[m] = \frac1N \sum_{n=0}^{N-1} X[n] e^{j 2\pi mn/N}
$$
or
$$
\left\{ \begin{align} x_r[m] & = \sum_{n=0}^{N-1} \left( X_r[n] \cos (2 \pi mn/N) - X_i[n] \sin (2 \pi mn/N) \right) \\ x_i[m] & = \sum_{n=0}^{N-1} \left( X_i[n] \cos (2 \pi mn/N) + X_r[n] \sin (2 \pi mn/N) \right) \end{align} \right.
$$
These transforms can be expressed in matrix forms. For example, the forward transform is:
$$
\begin{bmatrix} X[0] \\ X[1] \\ X[2] \\ X[3] \\ X[4] \\ X[5] \\ X[6] \\ X[7] \end{bmatrix} = \begin{bmatrix} w[0,0] & w[0,1] & w[0,2] & w[0,3] & w[0,4] & w[0,5] & w[0,6] & w[0,7] \\ w[1,0] & w[1,1] & w[1,2] & w[1,3] & w[1,4] & w[1,5] & w[1,6] & w[1,7] \\ w[2,0] & w[2,1] & w[2,2] & w[2,3] & w[2,4] & w[2,5] & w[2,6] & w[2,7] \\ w[3,0] & w[3,1] & w[3,2] & w[3,3] & w[3,4] & w[3,5] & w[3,6] & w[3,7] \\ w[4,0] & w[4,1] & w[4,2] & w[4,3] & w[4,4] & w[4,5] & w[4,6] & w[4,7] \\ w[5,0] & w[5,1] & w[5,2] & w[5,3] & w[5,4] & w[5,5] & w[5,6] & w[5,7] \\ w[6,0] & w[6,1] & w[6,2] & w[6,3] & w[6,4] & w[6,5] & w[6,6] & w[6,7] \\ w[7,0] & w[7,1] & w[7,2] & w[7,3] & w[7,4] & w[7,5] & w[7,6] & w[7,7] \\ \end{bmatrix} \begin{bmatrix} x[0] \\ x[1] \\ x[2] \\ x[3] \\ x[4] \\ x[5] \\ x[6] \\ x[7] \end{bmatrix}
$$
For this specific example of 8-point DFT, we have 
$$
w[m,n] = e^{-j 2\pi mn/N} = \cos (2\pi mn/N) - j \sin (2\pi mn/N) = \cos (\pi mn/4) - j \sin (\pi mn/4)
$$

<center class='half'><img src='./pictures/12-DFTcos.gif' width=45%><img src='./pictures/12-DFTsin.gif' width=45%></center>

If the signal $x[m]$ is real, then its spectrum $X[n]$ has the following properties:

- The real part of $X[0]$ is the average or DC component of the signal, the imaginary part of $X[0]$ is zero;
  $$
  \begin{align} \textrm{Re}\{X[0]\} & = \frac{1}{N} \sum_{m=0}^{N-1}x[m] \cos(2\pi m 0/N)
  =\frac{1}{N}\sum_{m=0}^{N-1} x[m] \\ \textrm{Im}\{X[0]\} & = -\frac{1}{N} \sum_{m=0}^{N-1} x[m] \sin(2\pi m 0/N)=0 \end{align}
  $$

- The real part of $x[N/2]$ is the difference between the even components and odd components, the imaginary part of $X[N/2]$ is zero: 
  $$
  \begin{align} \textrm{Re}\{X[N/2]\} & = \frac{1}{N} \sum_{m=0}^{N-1}x[m] \cos(2\pi m N/2N)=
    \frac{1}{N} \sum_{m=0}^{N-1}x[m] (-1)^m \\ \textrm{Im} \{X[N/2]\} & = -\frac{1}{N} \sum_{m=0}^{N-1}x[m] \sin(2\pi m N/2N)=0 \end{align}
  $$

- When $0 < n <N/2$, the real part of $X[n]$ is even symmetric and the imaginary part of $X[n]$ is odd symmetric:
  $$
  \begin{align} \textrm{Re} \{X[N-n]\} & = \frac{1}{N} \sum_{m=0}^{N-1}x[m] \cos(2\pi m (N-n)/N)
    =\frac{1}{N} \sum_{m=0}^{N-1}x[m] \cos(2\pi m n/N)=Re\{X[n]\} \\ \textrm{Im} \{X[N-n]\} & = -\frac{1}{N} \sum_{m=0}^{N-1}x[m] \sin(2\pi m(N-n)/N) = \frac{1}{N} \sum_{m=0}^{N-1}x[m] \sin(2\pi n m /N)=-Im\{X[n]\} \end{align}
  $$
  <img src='./pictures/12-dft1d_demo.gif' width=60%>

### 7. Fast Fourier Transform (FFT) Algorithm



























### 8. Fourier Transform 2 Real Functions with 1 DFT
### 9. Two-Dimensional Fourier Transform (2DFT)
### 10. Physical Meaning of 2-D FT
### 11. Matrix Form of 2D DFT
### 12. A 2D DFT Example
### 13. Spectrum Centralization
### 14. Fourier Filtering 1D
### 15. Fourier Filtering 2D