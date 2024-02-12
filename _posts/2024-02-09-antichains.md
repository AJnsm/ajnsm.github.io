---
layout: post
title:  "Möbius Functions on Powerset Antichains"
date:   2024-02-09 16:00:00 +0200
author: Abel Jansma
tags: information-theory maths cybernetics 
---

I calculated the full Möbius function on the lattice of powerset antichains of 2, 3, and 4 variables. As far as I can tell, these results have not been shared before, although I'm sure the 2- and 3-variable case has been calculated but not shared many times before. The results and the code are available [here](https://github.com/AJnsm/latticeOfAntichains).

<br>

This should make higher-order information decompositions much more straightforward and computationally efficient when one is interested in more than 3 variables. Some background information on what this is all means is given below.

<br>

Given a set $S$, the powerset $\mathcal{P}(S)$ is the set of all subsets of $S$. From this set, one can create a partially ordered set $P=(\mathcal{P}(S), \leq)$, where we impose the following ordering. For $s, t \in \mathcal{P}(S)$, we say that $s \leq t$ if and only if $s \subseteq t$. This is a partial ordering because not all pairs of elements are comparable. For example, if $S=\\{1,2,3\\}$, then $\\{1\\}$ and $\\{2\\}$ are not comparable, but it's clear that $\\{1\\} \leq \\{1,2\\}$.

<br>

Now one can imagine functions $f: P^n \to \mathbb{C}$ on this poset. Of particular interest is the so-called Möbius function $\mu: P^2 \to \mathbb{N}$, which is defined recursively:


$$
\mu(s,t) = \begin{cases}
1 & \text{if } s = t \\
-\sum_{s < u \leq t} \mu(u, t) & \text{if } s < t\\
0 & \text{otherwise }\\
\end{cases}
$$


This function is important because it allows the Möbius inversion theorem (due to Rota, 1964) to be stated:


$$
f(t) = \sum_{s \leq t} g(s) \Leftrightarrow g(t) = \sum_{s \leq t} \mu(s,t) f(s) 
$$


This theorem forms the basis for our understanding of interactions in complex systems (more details in an upcoming manuscript). 

<br>

The Möbius function is most studied on the following posets:
- The poset $(\mathbb{N}, \leq)$ of natural number with their natural ordering (in which case the Möbius inversion theorem reduces to the discrete fundamental theorem of calculus)
- The poset $(\mathbb{N}, \leq_d)$ of natural numbers ordered by divisibility (in which case $\mu$ is the inverse of the Riemann zeta function)
- On the poset $(\mathcal{P}, \subseteq)$ of subsets ordered by inclusion (in which case $\mu(s, t)=(-1)^{\mid t-s \mid }$)
- On the poset $(\Pi(S), \leq_r)$ of partitions of a set $S$ ordered by refinement (in which case $\mu(\pi, S)=(-1)^{(\pi-1)}(\mid \pi\mid -1)!$). 

<br>

Here, however, I will focus a much less understood poset, inspired by a branch of information theory called the Partial Information Decomposition (PID). The PID aims to decompose information into unique, redundant, and synergistic components. The key insight is that given a set of variables, one can define redundant information quantities that depend on particular combinations of the variables. The central constraint is that the redundant information should only be nontrivial for two sets of variables that are incomparable on the poset of subsets $P$ defined above. This is because a redundancy among $A$ and $B$ is only meaningful if $A$ is not a subset of $B$ (or *vice versa*). 

<br>

Now, an important observation is that the collection of these incomparable subsets have an interesting structure. A collection of incomparable items is called an *antichain* (because it is in some sense the opposite of a chain on $P$), and antichains can again be given a partial order! Let's call the collection of antichains $A$, so that for every $a, b \in A$, we can set  $a \leq b$ if for every $b_i \in b$, there is an $a_i \in a$ such that $a_i \subseteq b_i$. That is, $b$ in some sense 'covers' $a$, and $b$ contains less redundancy than $a$.

<br>

The partial information decomposition then writes the total information a set of sources $X_1 \ldots X_n$ carries about a target $Y$ as a sum of *information atoms* $\Pi$ on this lattice of antichains:


$$
I(\{X_1, \ldots, X_n\}; Y) = \sum_{a \in A} \Pi(a; Y)
$$


To identify what the information atoms are in terms of already known information quantities, we can interpret $I(\\{X_1, \ldots, X_n\\}; Y)$ as a function $I: A \to \mathbb{R}$, and then apply the Möbius inversion theorem to find the information atoms $\Pi(a; Y)$.

<br>

The problem is that we don't have a closed form solution for the Möbius function on $A$. If we did, we could calculate arbitrary information atoms and pinpoint exactly which parts of a system show synergistic information processing. In addition, a brute-force calculation is very hard, as the number of antichains grows superexponentially with the number of variables. In fact, the number of antichains on a set of $n$ variables is given by the $n$'th Dedekind number (minus two), which is [a series](https://oeis.org/A000372) that grows so fast that only the first nine terms are known: 2, 3, 6, 20, 168, 7581, 7828354, 2414682040998, 56130437228687557907788, 286386577668298411128469151667598498812366. That last term was, in fact, [first calculated in 2023](https://www.uni-paderborn.de/en/news-item/123917). 

<br>

Still, one would only need to calculate the Möbius functions on the lattice of antichains once, after which any time you want to decompose some information quantity you can just look up the value. Because this sequence grows so quickly, the literature has so far only really calculated decompositions of the information that two variables carry about a third. I wrote some code that calculates the Möbius function on the lattice of antichains (available [on Github](https://github.com/AJnsm/latticeOfAntichains)), and have stored the results for up to 4 variables giving information about a fifth. This is not a huge improvement, but it's a start, and I invite anyone to see how far they can optimise my code and perhaps calculate higher Möbius functions (it's currently a [pretty basic Python implementation](https://github.com/AJnsm/latticeOfAntichains/blob/main/calcMF_antichains.py)). I'm pretty sure the 5-variable case is doable with my current approach, but beyond that it looks like more clever approaches have to be found. 

<br>

I found that the Möbius function on the lattice of antichains, up to $N=4$, only takes the values $-1$, $0$, and $1$. This is similar to Möbius functions on other posets, like Boolean algebras or positive integers ordered by divisibility, but this does not mean that finding a closed form expression for the Möbius function will be easy. For example, the Möbius function on the positive number orders by divisibility also only takes values -1, 0, and 1, but a closed form expression would solve the Riemann hypothesis and win you a million dollars. Still, I suspect that the Möbius function on the lattice of antichains is more tractable than one might think, since there seems to be a close relationship between the product of antichain lattices and Boolean algebras, for which the Möbius function is well understood.

<br>

The main advantage of having these Möbius functions calculated is that new PID approaches now no longer need to solve the system of equations each time a new definition of redundancy is introduced, and can instead use the Möbius function to calculate all information atoms directly. Especially as the number of variables grows, solving the system of PID equations becomes increasingly hard, so having stored values of the Möbius function can offer a huge advantage.