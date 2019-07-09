# <center>Numerical Analysis</center>

[TOC]

## Interpolation Theory

This section mainly comes from Hardy's **An Introduction to Numerical Analysis**

Let $x_0, x_1, ... , x_n$ be distinct real or complex numbers, and let $y_0 , y_1, ..., y_n$ be associated function values. We now study the problem of finding a polynomial p ( x) that interpolates the given data:
$$
p(x_i) = y_i \ \ \ \ \ i = 0, 1, ..., n
$$
By writing: 		$p(x) = a_0 + a_1x + ... + a_mx^m$

Consider that m = n, then
$$
a_0 + a_1x_0 + ... + a_mx_0^m = y_0
 \\.
 \\.
 \\
 a_0 + a_1x_n + ... + a_mx_n^m = y_n
$$
It can be written that $Xa = y$

with
$$
X = [x_i^j] \ \  i, j = 0, 1, ..., n
\\
a = [a_0, a_1, ..., a_n]^T \ y = [y_0, ..., y_n]^T
$$
The matrix X is called a Vandermonde matrix.

### Theorem 1

Given n + 1 distinct points $x_0, ... , x_n$ and n + 1 ordinates $y_0, ..., y_n$, there is a polynomial p(x) of $degree  \le  n$ that interpolates $y_i$ at $x_i$, i = 0, 1, ... , n. This polynomial p(x) is unique among the set of all polynomials of degree at most n. 

#### Proof

- It can be shown that for the matrix X,
  $$
  det(X) = \prod_{0 \le j < i  \le n}(x_i - x_j)
  $$
  This shows that $det(X) \ne 0$, since the points $x_i$ are distinct. Thus X is nonsingular and the system $Xa = y$ has a unique solution a. This proves the existence and uniqueness of an interpolating polynomial of $degree \le n$.

- The system $Xa = y$ has a unique solution iff the homogeneous system $Xb = 0$ has only the trivial solution b = 0. Therefore, assume $Xb = 0$ for some b. Using b, define 
  $$
  p(x) = b_0 + b_1x + ... + b_nx^n
  $$
  From the system $Xb = 0$, we have
  $$
  p(x_i) = 0 \ \ \ \ \ i = 0, 1, ..., n
  $$
  The polynomial p(x) has n + 1 zeros and degree $p(x) \le n$. This is not possible unless $p(x) \equiv 0$. But then all coefficients $b = 0, i = 0, 1, ... , n$, completing the proof. 

- Consider a special polynomial
  $$
  l_i(x) = \prod_{j \ne i} (\frac{x - x_j}{x_i - x_j}) \ \  i = 0, 1, ..., n
  $$
  Then the polynomial can be rewritten
  $$
  p(x) = y_0l_0(x) + y_1l_1(x) + ... + y_nl_n(x)
  $$
  Since all $l_i(x)$ have degree n, degree $p(x) \le n$.

  To prove uniqueness, suppose q(x) is another polynomial of degree $\le$ n that satisfies (1). Define
  $$
  r(x) = p(x) - q(x)
  $$
  Then degree $r(x) \le n$, and
  $$
  r(x_i) = p(x_i) - q(x_i) = y_i - y_i = 0 \ \ i = 0, 1, ..., n
  $$
  Since r(x) has n + 1 zeros, we must have $r(x) \equiv 0$, which proves $p(x) \equiv q(x)$, completing the proof.