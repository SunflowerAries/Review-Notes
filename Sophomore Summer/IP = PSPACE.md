# <center>IP = PSPACE</center>

[TOC]

## Quantified Boolean Formulas

### Closed

A QBF in which all the variables are quantified.

### Simple

A closed QBF is simple if in the given syntactic representation every occurrence of each variable is separated from its point of quantification by at most one universal quantifier (and arbitrarily many other symbols).

Example. $\forall x_1 \forall x_2 \exist x_3 [(x_1 \vee x_2) \wedge \forall x_4(x_2 \wedge x_3 \wedge x4)]$ is simple, since:

- $x_1$ is used once, and only $\forall x_2 $ separates its point of quantification from its point of use,
- $x_2$ is used twice; its first use is not separated from its point of quantification by any universal quantifiers. whereas its second use is separated only by $\forall x_4 $,
- $x_3$ is used once, and separated only by $\forall x_4$,
- $x_4$ is used once, and not separated by any universal quantifiers.

### Theorem 1

Every QBF of size n can be transformed into an equivalent simple QBF whose size is polynomial in n.

#### Proof

To turn a given QBF into a simple QBF, we completely rename all the Boolean variables after each universal quantifier. By introducing new existentially quantified Boolean variables $x_i^j$ to denote the jth name of the ith variable, and equating each $x_i^j$; with its previous name $x_i^{j - 1}$. For example, consider the QBF
$$
\exist x_1 \forall x_2 \exist x_3 \forall x_4 \exist x_5 ... Q(x_1, x_2,...)
$$
In which Q is quantifier free. The transformed QBF is:
$$
\exist x_1^0 \forall x_2^0 \exist x_1^1(x_1^1 = x_1^0) \wedge \exist x_3^0 \forall x_4^0 \exist x_1^2 \exist x_2^1 \exist x_3^1 \\ (x_1^2 = x_1^1) \wedge (x_2^1 = x_2 ^0) \wedge (x_3^1 = x_3^0) \wedge \exist x_5^0 ... Q(x_1^{J_1}, x_2^{J_2},...)
$$

## The Arithmetization of QBF

- Replace each Boolean variable $x_i$ by a new variable $z_i$, which can range over the (positive and negative) integers Z.
- Replace each occurrence of $\overline{x}_i$, by $(1 - z_i)$.
- Replace $\wedge$ by integer multiplication $\cdot$, $\vee$ by integer addition +, the universal quantification $\forall x_i$ by the integer product $\prod_{z_i \in \{0, 1\}}$ and the existential quantifier $\exist x_i,$ by the integer sum $\sum_{z_i \in \{0, 1\}}$.

### Theorem 2

A closed QBF B is true iff the value of its arithmetic form A is nonzero.

### Theorem 3

Let B be a closed QBF of size n. Then the value of its arithmetic form A cannot exceed $O(2^{2^n})$.

#### Proof

For any (potentially open) subexpression B‘ of B, define v(B’) as the maximal value of the arithmetic form of B‘ under all the possible 0/1 substitutions to the free variables.

- If B’ is $x_i$ or $\overline x_i$, then v(B') = 1
- If B’ is $B'' \vee B'''$, then $v(B’) \le v(B'') + v(B''')$
- If B’ is $B^{''} \wedge B^{'''}$, then $v(B’) \le v(B'') \cdot v(B''')$
- If B’ is $\exist x_i B^{''}$, then $v(B’) \le 2v(B'')$.
- If B’ is $\forall x_i B^{''}$, then $v(B’) \le v(B’’)^2$
- When B‘ is closed, v(B’ ) coincides with the value of its arithmetic form.

### Theorem 4

Let B be a closed QBF of size n. Then there exists a prime p of length polynomial in n such that $A \ne 0 (\mathop{mod} p)$ iff B is true.

#### Proof

Assume first that A is a non-zero integer. If it is zero modulo all the polynomial size primes, then by the Chinese remainder theorem it is zero modulo their product as well. Since by the prime number theorem this product is $\Omega(2^{2^{n^d}})$ for any desired constant d, this contradicts the assumption that A is non-zero and at most $O(2^{2^n})$. On the other hand, if B is false, then A = 0 modulo any prime. 

## Interactive Proofs for PSPACE Languages

Since deciding the truth of (simple) QBFs is known to be PSPACE complete, it suffices to demonstrate the existence of interactive proofs for the problem.

### Theorem 5

If B is simple, then the degree of the polynomial $q(z_i)$ that describes the fictional form of A grows at most linearly with the size of B.

#### Proof

Let $x_i$ be the leftmost quantified variable in B. The degree of $z_i$ created by any quantifier-free subexpression of B is bounded by the size of the subexpression. Summations over arbitrarily many other variables can change the coefficients but not the degree of the polynomial, and each product can at most double the degree. The result follows from the fact that in simple B such a doubling can occur at most once.

### The interactive protocol for proving $A \ne 0(\mathop{mod} p)$

- If $A_2$ is empty, V stops and accepts the claim iff a = $a_1$
- If $A_1$ is nonempty, V replaces A by $A_2$, and replaces a by $a – a_1 (\mathop{mod} p)$ or $a/a_1(\mathop{mod} p) $(depending on the operator that connects , $A_1$ and $A_2$). If V tries to divide a by $a_1 = 0 (\mathop{mod} p)$, he stops and accepts the claim iff $a = 0 (\mathop{mod} p)$.
- Otherwise, P sends the polynomial descriptions q($z_i$) of A’ to V. V checks that$a = q(0) + q(1)(\mathop{mod} p)$ or $a = q(0) \cdot q(1)(\mathop{mod} p)$(depending on the first symbol of $A_2$), sends a random $r \in Z_p$ to P, replaces A by $A’(z_i = r) (\mathop{mod} p)$, and replaces a by$q(r) (\mathop{mod} p)$.

### Theorem 6

- When B is true and P is honest, V always accepts the proof
- When B is false, V accepts the proof with negligible probability

#### Open problem

Whether the protocol can be executed with a small (logarithmic) number of rounds?

## Space-Bounded Verifiers

### Definition

A verifier is called weak if

- its running time is polynomial
- its workspace is logarithmic
- it has a two-way read-only access to a random tape
- its messages consist solely of the random bits it reads

### Theorem 7

Any PSPACE language can be accepted by a weak verifier