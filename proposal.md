---
title: "A Mini-Introduction To (Quantum) Random Walks"
date: "July 2018"
author: "Bhavishya"
csl: journal-of-computer-information-systems.csl
bibliography: proposal.bib
---

# Abstract

This article is a short introduction to (Quantum) Random Walks which is an emerging paradigm for designing new "Quantum" algorithms.
We start with Classical Random walks, then Quantum Random Walks for both continuous and discrete cases are described and
eventually some applications, in designing algorithms, are discussed.^[ This article follows the style of a recent article [see @witten2018mini]  by Ed Witten.]

# Table of Contents
1. [Introduction]
2. [Classical Random Walks] 
3. [Quantum Random Walks] 
4. [Quantum Algorithms based on Random Walks]

Introduction
------------

Randomized algorithm have been very successful in history of theoritical computer science due to their simplicity and speed [@Motwani:1995:RA:211390] (Eg: primality-testing, min-cut etc.). Many known intractable problems only have randomized algorithms like Approximate-Counting^[ Approximate Counting belongs #P the class associated with set of counting problems in NP and thus even harder than NP]. The basic complexity class for Randomized algorithms is RP (the class of decision problems with success probability at least $1/2$). The class of problems with success probability more than $1/2$ is called BPP. Note that this probability can be arbitrarily increased by multiple runs and then taking the median. The quantum equivalent of BPP is the class 
BQP, [@doi:10.1137/S0097539796300921] showed that BPP $\subseteq$ BQP and it is believed that BPP $\neq$ BQP. Our believe that Quantum algorithms are better than classical ones, justifies the
study of Quantum Random Walks. Infact [@childs2003exponential] proved an oracle separation between 
classical and quantum case for a graph traversal problem.

# Classical Random Walks

## Preliminaries and Definitions

Markov Chain
:   It is a discrete time stochastic process defined by a matrix $P$ of transition probabilities over states $S$.

A discrete-time Markov chain is a sequence of random variables $X_{1}$, $X_{2}$,... $X_{n}$, such that

Pr($X_{n+1} = x$ | $X_{1}=x_{1},...,X_{n}=x_{n}$) = Pr($X_{n+1}=x | X_{n} = x_{n}$)
Now $P_{ij}$ = $Pr[X_{t}=j|X_{i}]$, Thus a **Markov Chain is memoryless.**

Stationary Distribution
:   A $\pi$ such that $\pi = \pi P$ for transition matrix $P$.    
<br />

Irreducibility of Markov Chain
:   Any state($j$) is accesible from any other state($i$) i.e. $Pr(X_{t}=j | X_{1}=i) > 0$.

<br />

Aperiodicity of Markov Chain
:   For all states $i$ and $j$, if gcd{$Pr(X_{n}=j|X_{0}=i)$} = 1 then it is aperiodic.
<br />

Ergodicity
:   A markov chain is ergodic if it's both Irreducible and Aperiodic.

<br />


Fundamental theorem of Markov Chains
:   An ergodic markov chain has a stationary distribution i.e. $\pi^{\,}$ such that $P\pi=\pi$.


## Examples of Classical Random walks:

1. **Walk on a infinite Line**

Suppose the probability of moving right is $p$ and left $q$. You start from $0$, you might want to find the position after $k$ steps, or at least the mean( and variance).
Let's use analysis similar to [@Venegas-Andraca:2008:QWC:1502048], We define $Z_{n}$ to be the state after $n$ steps, Now number of "right" steps can be written as $(Z_{n}+n)/2$, but moving "right"(or "left")
is a binary variable with probability $p$(or $q$). E["right"] = $np$, V["right"] = $npq$, putting back in previous eq.
you can find E[$Z_{n}$] = n(p-q) and V[$Z_{n}$] = $4npq$. Also note variance increases **linearly** in $n$.

2. **Walk on Graphs : S-T connectivity**
 
We start with defining some terms common in random walks on graphs,
For $G = (V,E)$ we can define a markov chain, $M_{G}$ with


$P_{uv}$ = 1/d(u) if (u,v) $\epsilon$ $E$  and 0 otherwise.

For all $v$ $\epsilon$ $V$, $\pi_{v} = d(v)/2m$.

Hitting Time
:   Expected number of steps for a random walk to start at $u$ and end upon first reaching $v$.
        
<br />

Mixing Time
:   It is the smallest time when $P_{u}(t)$ is within $\epsilon$ distance of $\pi$     
    

<br />

Now we move to the problem: Given G, determine whether $s$ and $t$ are connected or not.
The random walk algorithm is particularly effective when we have limited space(logarithmic).

### S-T connectivity Algorithm 

    Start at y = s
    repeat this T=|V|^3 times,
    Choose any neighbour x of y at random 
    If x is t
        Stop and Return True
    Else
        y = x
    Return False

This algorithm has greater than 1/2 probability of correct answer. And we can decrease the 
failure probability to $\epsilon$ by repeating the algorithm about log(1/$\epsilon$) and taking the median.

3. **2-SAT**
A very similar algorithm to above, is for solving the 2-SAT problem for n variables.

### 2-SAT Algorithm 
    Start with some assignment T
    repeat 2n^2 times
        If no UNSAT clause
            Stop and Return SAT
        Else
            Flip literals and update T
    Stop and Return UNSAT


# Quantum Random Walks

A basic hypothesis of Quantum Mechanics is Unitarity. But we just saw that the classical random walk 
has a tendency to lose memory so we do expect that Quantum Walks would be different in some ways.

### Quantum Discrete Walk

To define Quantum Walks we need to define "coin" state that determines "shift". The usual choice 
for "coin" is the Hadamard operator.
    
So we can define C(coin flip operator)

    C|n,0> = a1|n,0> + a2|n,1>
    C|n,1> = a3|n,0> + a4|n,1>
For usual when using Hadamard operator as coin flip we can keep a1, a2, a3 to be equal to $1/\sqrt{2}$ and a4=-1/$\sqrt{2}$.

Also define S(shift operator) as
    
    S|n,0> = |n+1,0>
    S|n,1> = |n-1,1>

Now $t^{th}$ step of random walk is given by  applying  $(SC)^{t}$. With Classical walk we obtained 
a normal distribution, but with quantum walks things are pecularily  different. Observe 
the negative sign in a4,( try applying the above operator 4-5 times) this leads to cancellation
in amplitudes and the walks start to shift towards Right.
Another aspect is that the expected distance from origin after $t$ steps is $\Omega(t)$(compared to $\sqrt{t}$ in classical case).

### Quantum Continuous Walk 

For the continuous case we do not require a coin, we can define them in a very natural way as follows,
by taking inspiration from Adjacency matrix of a graph $G$. We can dee fine the **Laplacian** of $G$
defined as 
    
$L_{j,k} = -deg(j)$ when $j=k$ else equal to 1 if $(j,k)$ $\epsilon$  $E$ and 0 otherwise.

Now we can write the following equation,

$\dfrac{d}{dt}p_{j}(t)$ = $\Sigma_{k \epsilon V}L_{jk}p_{k}(t)$ = 0

which has the solution,

$p(t) = e^{Lt}p(0)$

which is very similar to Schr$\"{o}$denger equation

i$\dfrac{d}{dt}|\Psi>$ = $H|\Psi>$

but could be made exactly similar by replacing $p_{j}$ with complex amplitudes $q_{j}$.

So the closed form solution is $q(t)$ = $e^{-iLt}q(0)$ . 

A more general construction can be done for arbitrary graph [ see @ambainis2003quantum].


# Quantum Algorithms based on Random Walks
As per [ @ambainis2003quantum ] we see two major groups of algorithms,

1. Exponentially Faster Hitting (for full binary trees)

We saw for the $S-T$ connectivity problem, that a classical algorithm takes exponential time. But 
[@childs2002example] showed that for special case of binary tree one root can hit other root with
constant probability in polynomial time($O(d^{2})$ where d is the depth).

The proof uses the idea that the binary tree could be reduced to a small subspace(of $2d+1$ dimensions) and problem 
can be treated as continous walk on straight line.

2. Quantum Walk Search(Element Distinctness)

Given numbers $a_{1},...,a_{n}$ such that there exist $i,j$ such that $i\neq j$ but $a_{i}=a_{j}$.

Classically we require $\Omega(n)$ queries. [@ambainis2007quantum] solved this problem by providing
an $O(n^{2/3})$ algorithm which is also proven to be optimal.

Start from a vertex, check if it's marked, if not query just one neighbour, now we can define 
a quantum walk on remaining vertices which finds the appropriate vertex in $O(d^{2/3})$.

# References
