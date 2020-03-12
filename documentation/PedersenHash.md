# Pedersen Hash on Baby Jubjub

## Notation

- $E$ is the Baby Jubjub elliptic curve of order $n$.
- $\mathbb{G}$ is the large prime subgroup of points of Baby Jubjub of order $r$.
- $M$ is a sequence of bits of arbitrary length.

## Description

The *Pedersen hash* of a sequence of bits $M$ is constructed as follows:

- Split $M$ into sequences $M_i$ of at most 186 bits and each of these into chunks $m_j$ of 3 bits. (If $M$ is not a multiple of 3, pad $M$ to a multiple of 3 bits by appending zero bits.) More precisely, write $$ M = M_0M_1\dots M_l $$ with $$ M_i = m_0 m_1 \dots m_{k_i-1}, $$ where each term $m_j$ is a chunk of 3 bits $[b_0\: b_1\: b_2]$ and 
    \begin{gather*}
    \begin{cases}
    k_i = 62 	\;\text{ for }  i = 0, \dots, l-1, \\
    k_i \leq 62 \;\text{ for }  i = l.
    \end{cases}
    \end{gather*}

- Encode each chunk $m_j$ as an integer from the set $\{-4...4\}\backslash\{0\}$ the following way 
    $$ \text{enc}(m_j) = (1+b_{0}+2b_{1}) (1 - 2b_{2}) $$
and let $$ \langle M_i \rangle = \sum_{j=0}^{k_i-1} \text{enc}(m_j) 2^{4j}. $$

- Let $P_0,P_1,\dots,P_k$ be uniformly sampled generators of $\mathbb{G}$ (for some integer $k > l$). 

- We define the Pedersen hash of $M$ as $$ H(M) = \sum_{i=0}^l \langle M_i \rangle P_i.$$

Note that the expression above is a linear combination of elements of $\mathbb{G}$, so itself is also an element of $\mathbb{G}$. That is, the resulting Pedersen hash $H(M)$ is a point of the elliptic curve $E$ of order $r$.

## Choice of Generators

We generate the points $P_0,\dots,P_{{k}}$ in such a manner that it is difficult to find a connection between any of these two points. More precisely, we take  D =  "string$\_$seed" followed by a byte S holding that smallest number that H = Keccak256(D || S) results in a point in the elliptic curve $E$.
  
## Security Considerations



## Example