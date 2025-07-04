---
layout: post
title:  "A Möbius inversion theorem for modules and vector spaces"
date:   2025-06-14 10:00:00 +0200
author: Abel Jansma
tags: maths
---

<br> 

In a [previous post]({% post_url 2025-01-28-mereoPhysics %}), I proposed to study complex systems through a mereological lens by applying the Möbius inversion theorem. This has become my favourite theorem by now, because I think it allows you to do integration and differentiation on observables in complex systems where this was not possible before.

<br> 

It was originally proved by Möbius for integer numbers ordered by division, but generalised to commutative rings and arbitrary posets by Gian-Carlo Rota. I wanted to apply this to group- and vector-valued functions so that I could calculate semantic synergy in text embeddings, but to do so the theorem needed to be generalised. Let's first review the classical version of the theorem, and then generalise it to modules and vector spaces. 

<br> 

**Definition:** A commutative ring $R$ (with unity) is a set equipped with two operations, addition ($+$) and multiplication ($\cdot$), such that the following properties hold:

&nbsp;&nbsp;&nbsp;&nbsp;1. $(R, +)$ is an Abelian group with identity element $0_R$.

&nbsp;&nbsp;&nbsp;&nbsp;2. $(R, \cdot)$ is a monoid with identity element $1_R$.

&nbsp;&nbsp;&nbsp;&nbsp;3. Multiplication distributes over addition: $a \cdot (b + c) = a \cdot b + a \cdot c$ and $(a + b) \cdot c = a \cdot c + b \cdot c$

&nbsp;&nbsp;&nbsp;&nbsp;4. Multiplication is commutative: $a \cdot b = b \cdot a$


<br> 

## The Classical Möbius Inversion Theorem
The theorem is really a statement about the *incidence algebra* of the poset $P$, which is the set of all functions $f: P \times P \to R$ from intervals on $P$ to $R$, equipped with the convolution product defined by:

$$(f \ast g)(x, y) = \sum_{x\leq z \leq y} f(x, z) \cdot g(z, y)$$

Note that the following three functions are part of the incidence algebra:

$$\begin{align}
\zeta(x, y) &= \begin{cases}
1_R & \text{if } x \leq y \\
0_R & \text{otherwise} \\
\end{cases} \\[1em]
\mu(x, y) &= \begin{cases}
1_R & \text{if } x = y \\
 -\sum_{x \leq z < y} \mu(x, z) & \text{if } x < y \\
0_R & \text{otherwise}
\end{cases} \\[1em]
\delta(x, y) &= \begin{cases}
1_R & \text{if } x = y \\
0_R & \text{otherwise}\\
\end{cases}
\end{align}$$

**Theorem (Möbius inversion theorem):** Let $R$ be a commutative ring, and $f, g: P \to R$ be $R$-valued functions defined on a locally finite poset $P$. Then the following two statements are equivalent:

&nbsp;&nbsp;&nbsp;&nbsp;1. $f(x) = \sum_{y \leq x} g(y)$ 

&nbsp;&nbsp;&nbsp;&nbsp;2. $g(x) = \sum_{y \leq x} \mu(y, x) \cdot f(y)$ 

where $\mu$ is the Möbius function on $P$. 


<br> 

The Möbius inversion theorem is thus really the statement that $\mu$ is the $\ast$-inverse of $\zeta$, as $\zeta \ast \mu = \delta$ and $\mu \ast \zeta = \delta$. This is a very powerful result: since $f$ is like an integral over the poset, $\mu$ is like a differential operator on $P$. 

<br> 

## Generalising the Möbius Inversion Theorem

It turns out that we can generalise this theorem to group-valued functions. This is strictly more general than the commutative ring case, and includes vector-valued functions. To do so, we need to define $R$-modules:

<br> 

**Definition:** Let $R$ be a commutative ring. An *R-module* is an Abelian group $M$ with a group operation $\oplus : M \times M \to M$ and a scalar multiplication $\star: R \times M \to M$ such that the following properties hold for all $r, s \in R$ and $x, y \in M$:

&nbsp;&nbsp;&nbsp;&nbsp;1. $r \star (x \oplus y) = r \star x \oplus r \star y$

&nbsp;&nbsp;&nbsp;&nbsp;2. $(r + s) \star x = r \star x \oplus s \star x$

&nbsp;&nbsp;&nbsp;&nbsp;3. $r \star (s \star x) = (r \cdot s) \star x$

&nbsp;&nbsp;&nbsp;&nbsp;4. $1 \star x = x$

<br> 

We can then state a more general version of the theorem:

<br> 

**Theorem (Möbius inversion theorem on R-modules):** Let $(P, \leq)$ be a locally finite poset, $(R, +, \cdot)$ a commutative ring, and $(M, \oplus,\star)$ an R-module. Consider $Q, q: P \to M$. Then the following two statements are equivalent:

&nbsp;&nbsp;&nbsp;&nbsp;1. $Q(x) = \bigoplus_{y \leq x} q(y)$

&nbsp;&nbsp;&nbsp;&nbsp;2. $q(x) = \bigoplus_{y \leq x} \mu(y, x) \star Q(y)$

<br> 

***Proof:*** The proof is similar to the classical case, but we need to carefully consider the properties of the different operations involved. 

Filling in the first statement into the right-hand side of the second statement:

$$\bigoplus_{y \leq x} \mu(y, x) \star Q(y) = \bigoplus_{y \leq x} \left( \mu(y, x) \star \bigoplus_{z \leq y} q(z) \right)$$

By the first property of modules,  $\star$ multiplication distributes over $\oplus$ addition:

$$= \bigoplus_{y \leq x} \left(\bigoplus_{z \leq y} \mu(y, x) \star q(z) \right)$$

As the module is based on an Abelian group, $\bigoplus$ is associative and commutative, so we can change the order of summation as follows:

$$= \bigoplus_{z \leq x} \left( \bigoplus_{z\leq y \leq x} \mu(y, x) \star q(z) \right)$$

By the second property of modules, we can replace the $\bigoplus$ (addition in the group) with $\sum$ (addition in the ring) as follows:

$$= \bigoplus_{z \leq x} \left( \sum_{z\leq y \leq x} \mu(y, x) \right) \star q(z)$$

Observe that the term in parentheses is equal to $(\zeta \ast \mu)(z, x)=\delta(z, x)$, a convolution in the incidence algebra. Therefore:

$$= \bigoplus_{z \leq x} \delta(z, x) \star q(z) = 1_R \star q(x) = q(x)$$

This proves that the second statement follows from the first. 

To prove the reverse direction, we fill in the second statement into the first:

$$\bigoplus_{y \leq x} q(y) = \bigoplus_{y \leq x} \left( \bigoplus_{z \leq y} \mu(z, y) \star Q(z) \right)$$

From here, the same arguments can be applied to show that this equals $Q(x)$. This completes the proof. $\square$

<br> 

## Conclusion

The theorem is thus no longer a statement about convolutions in the incidence algebra, but about a kind of 'incidence action' on the module. This generalisation opens up new possibilities for applying Möbius inversion to group- or vector-valued function. One context in which this might be useful is text embeddings. The Möbius inverse of the vector-valued embedding function would quantify the 'emergent semantics' of a piece of text. 