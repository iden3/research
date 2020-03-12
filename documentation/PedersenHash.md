# Pedersen Hash on Baby Jubjub

## Notation

- `E` is the [Baby Jubjub](https://github.com/ethereum/EIPs/pull/2494) elliptic curve of order `n`.
- `G` is the large prime subgroup of points of Baby Jubjub of order `r`.
- `M` is a sequence of bits of arbitrary length.

## Description

The *Pedersen hash* of a sequence of bits `M` is constructed as follows:

- Split `M` into sequences of at most 186 bits: `M = M_0 M_1 ... M_l`.
- Divide each of these `M_i` sequences into smaller chunks of 3 bits. <!-- (If `M` is not a multiple of 3, pad `M` to a multiple of 3 bits by appending zero bits.) --> 
More precisely, write `M_i = m_0 m_1 ... m_{k_i-1}`, where each term `m_j` is a chunk of 3 bits `[b_0 b_1 b_2]`. When `i = 0,..., l-1` then `k_i = 62` and for `i = l`, then `k_i <= 62`.

- Encode each chunk `m_j` as an integer from the set [-4..-1,1..4] the following way:
```enc(m_j) = 1+b_0+2*b_1) (1 - 2*b_2)```
and let 
    ```
    <M_i> = enc(m_0) + enc(m_1)*2^4 + enc(m_2)*2^8 + ... + enc(m_{k_i-1})*2^{4*(k_i-1)}
    ```

- Let `P_0`,`P_1`,...,`P_k` be uniformly sampled generators of `G` (for some integer `k > l`). 

- We define the Pedersen hash of `M` as 
    ```
    H(M) = <M_0>*P_0 + ... <M_l>*P_l
    ```

Note that the expression above is a linear combination of elements of `G`, so itself is also an element of `G`. That is, the resulting Pedersen hash `H(M)` is a point of the elliptic curve `E` of order `r`.

## Choice of Generators

We generate the points $P_0,\dots,P_{{k}}$ in such a manner that it is difficult to find a connection between any of these two points. More precisely, we take  D =  "string$\_$seed" followed by a byte S holding that smallest number that H = Keccak256(D || S) results in a point in the elliptic curve $E$.
  
## Security Considerations



## Example
