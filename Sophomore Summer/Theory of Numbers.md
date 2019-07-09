# <center>Theory of Numbers</center>

[TOC]

## Chinese Remainder Theorem

This section mainly comes from Zhao Ke's  **Handouts for Theory of Numbers**
$$
x \equiv b_1(\mathop{mod} m_1), x \equiv b_2(\mathop{mod} m_2),..., x \equiv b_k(\mathop{mod} m_k)
$$

### Theorem 1

设$m_1, m_2, ..., m_k$是k个两两互素的正整数，$m = m_1...m_k, m = m_iM_i(i = 1,...,k)$，则同余式组(1)有唯一解
$$
x \equiv M_1'M_1b_1 + M_2'M_2b_2 + ... + M_k'M_kb_k(\mathop{mod} m)
$$
其中

​							$M_i'M_i \equiv 1(\mathop{mod} m_i)(i = 1,...,k)$

#### Proof

由于$(m_i, m_j) = 1, i \ne j$，可得$(M_i, m_i) = 1$，对每一个$M_i$，有$M_i'$使得$M_i'M_i \equiv 1(\mathop{mod} m_i)$，又由$m = m_iM_i$，因此$m_j | M_i, i \ne j$，即$\sum_{j \ne i} M_j'M_jb_j \equiv 0(\mathop{mod} m_i)$,故
$$
\sum_{j = 1}^kM_j'M_jb_j \equiv M_i'M_ib_i \equiv b_i(\mathop{mod} m_i), i = 1,..,k
$$
即(2)为(1)的解，若$x_1, x_2$为(1)式的两解，则有

​		$x_1 \equiv x_2 (\mathop{mod} m_i)(i = 1,...,k)$

又$(m_i, m_j) = 1, i \ne j$，所以$x_1 \equiv x_2(\mathop{mod} m)$，故(1)式有唯一解

### Theorem 2

一次同余式组
$$
x \equiv b_1(\mathop{mod} m_1), x \equiv b_2(\mathop{mod} m_2)
$$
可解的充要条件为$(m_1, m_2) | b_1 - b_2$，且当(4)式可解时对模$[m_1, m_2]$有唯一解。

#### Proof

设(4)有公解$x_0$，$(m_1,m_2) = d$，则有
$$
x_0 \equiv b_1 (\mathop{mod} d), x_0 \equiv b_2 (\mathop{mod} d)
$$
两式相减可有$d | b_1 - b_2$.

若$(m_1, m_2) | b_1 - b_2$，则因$x \equiv b_1(\mathop{mod} m_1)$的解可写为$x = b_1 + m_1y$，代入$x \equiv b_2(\mathop{mod} m_2)$可有
$$
m_1y \equiv b_2 - b_1(\mathop{mod} m_2)
$$
因为$(m_1, m_2) = d, d | b_2 - b_1$，故(6)有解，设为$y_0$，且对模$\frac{m_2}{d}$有唯一解$y \equiv y_0(\mathop{mod} \frac{m_2}{d})$，即
$$
y = y_0 + \frac{m_2}{d}t(t = 0, \pm1, \pm2,...)
$$
故(4)的全部解为
$$
x = b_1 + m_1y_0 + \frac{m_1m_2}{d}t(t = 0,\pm1,\pm2,...)
$$
这些解对模$[m_1, m_2]$都是同余的，故(4)的解对模$[m_1,m_2]$唯一

#### Corollary

对于一次同余式组
$$
x \equiv b_1 (\mathop{mod} m_1), x \equiv b_2 (\mathop{mod} m_2),...,x \equiv b_k (\mathop{mod} m_k) 
$$
$k \ge 3$的情形，可先解前面两个得$x \equiv b_2' (\mathop{mod} [m_1, m_2]), $，再与$x \equiv b_3 (\mathop{mod} m_3), $联立解出$x \equiv b_3' (\mathop{mod} [m_1, m_2, m_3])$.最后可得唯一解$x \equiv b_k' (\mathop{mod} [m_1,...,m_k])$，若中间有一步出现无解，则同余式组无解.

### Theorem 3

若$m_1, m_2,..., m_k$是k个两两互素的正整数，$m=m_1...m_k$，则同余式
$$
f(x)\equiv0(\mathop{mod} m)
$$
有解的充要条件是同余式
$$
f(x)\equiv0(\mathop{mod} m_i)(i = 1,..,k)
$$
的每一个有解.并且，若用$T_i$表示$f(x) \equiv 0(\mathop{mod} m_i)$的解数，T表示(10)的解数，则$T = T_1T_2...T_k$

### Proof

- 设$x_0$是(10)的解，则由$f(x_0) \equiv 0(\mathop{mod} m)$，可得$f(x_0) \equiv 0(\mathop{mod} m_i)(i=1,...,k)$

- 反之，若对$x_i$可有$f(x_i) \equiv 0(\mathop{mod} m_i)(i=1,...,k)$，又对$1\le i < j \le k$，$(m_i, m_j) = 1$，由**Theorem 1**，有唯一的$x_0, 0 \le x_0 < m$，使得$x_0 \equiv x_i(\mathop{mod} m_i)(i=1,...,k)$，且$f(x_0) \equiv f(x_i) \equiv 0(\mathop{mod} m_i)(i=1,...,k)$.故$f(x_0) \equiv 0(\mathop{mod} m)$

现设$f(x) \equiv 0(\mathop{mod} m_i)$的$T_i$个不同的解为

$x \equiv u_{i, e_i}(\mathop{mod} m_i), 0 \le u_{i, e_i} < m_i(e_i = 1,..., T_i; i = 1,...,k)$

- 对其中任一组$(u_{1, e_1}, u_{2, e_2}, ..., u_{k, e_k})$，由**Theorem 1**可得唯一的x，$0 \le x < m$，为(10)的解；且不同的组，得到(10)的解x也不同，固有$T_1T_2...T_k \le T$.

- 反之，设$x_1, ..., x_T$，$0 \le x_i < m(i = 1, ..., T)$，为(5)的T个解，则对某一个$j(1 \le j \le T)$，$(<x_j>_{m_1},...,<x_j>_{m_k})$是某一组$(u_{1, e_1}, u_{2, e_2}, ..., u_{k, e_k})$，且$(<x_i>_{m_1},...,<x_i>_{m_k}),i = 1,...,T$不同，故$T \le T_1...T_k$.