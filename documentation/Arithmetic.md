# Arithmetic in Baby Jubjub

## Addition of Points

When adding points of elliptic curves in Montgomery form, one has to be careful if the points being added are equal (doubling) or not (adding) and if one of the points is the point at infinity [[1]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.559.7774&rep=rep1&type=pdf). 

Edwards curves have the advantage that there is no such case distinction and 
doubling can be performed with exactly the same formula as addition [[2]](https://eprint.iacr.org/2008/013.pdf). 
In comparison, operating in Montgomery curves is cheaper. 
For the exact number of operations required in different forms of elliptic curves, 
see [[2]-](https://eprint.iacr.org/2008/013.pdf)[[3]](https://www.hyperelliptic.org/EFD/).

- **Twisted Edwards**

	Let `P1 = (x1, y1)` and `P2 = (x2, y2)` be points of the Baby Jubjub twisted Edwards elliptic curve `E`. The sum `P1 + P2` is a third point `P3 = (x3, y3)` with
	```
	λ = 168696*x1*x2*y1*y2
	x3 = (x1*y2 + y1*x2)/(1 + λ)
	y3 = (y1*y2 − 168700*x1*x2)/(1 − λ)
	```

	Note that the neutral element is the point `O = (0, 1)` and the inverse of a point `(x, y)` is `(−x, y)`.

- **Montgomery**

	Let `P1 = (x1, y1) != O` and `P2 = (x2, y2) != O` be two points of the Baby Jubjub `E_M` in Montgomery form. 
	
	- **Addition**: If `P1 != P2`, then the sum `P1 + P2` is a third point `P3 = (x3, y3)` with coordinates

		```
		Λ = (y2 − y1)/(x2 − x1)
		x3 = Λ**2 − 168698 − x1 − x2
		y3 = Λ(x1 − x3) − y1
		```
	- **Doubling**: If `P1 = P2`, then `2·P1` is a point `P3 = (x3, y3)` with coordinates

		```
		Λ = (3*(x1**2) + 2*A*x1 + 1)/(2*B*y1)
		x3 = B*(Λ**2) − A − 2*x1
		y3 = Λ(x1 − x3) − y1
		```

## Multiplication by a Scalar

Let `P!=O` be a point of the twisted Edwards curve $E$ of order strictly greater than 8 (i.e. a point `P` in the large prime subgroup `G`) and let `k` a binary number representing an element of the finite field `F_p`. We describe the circuit used to compute the point `k·P`.
	 	
- First, we divide `k` into chunks of 248 bits. If `k` is not a multiple of 248, we take `j` segments of 248 bits and leave a last chunk with the remaining bits. More precisly, write 
	``` 
	k = k_0 k_1...k_j
	```
	with
	```
	k_i = b^i_0 b^i_1 ... b^i_{247}	
	``` 
	for `i = 0, ..., j-1` (the first `j` chunks) and
    
	```
	k_j = b^j_0 b^j_1 ... b^j_s 
	```
	with `s <= 247` for the last chunk. Then,  
	```
	k·P = k_0·P + k_1·2^{248}·P + ... + k_j·2^{248j}·P
	```
    
    This sum is done using the following circuit. The terms of the sum are calculated separately inside the `SEQ` boxes and then added together. 

    ![](https://i.imgur.com/bpy9M0L.png)

- Each `SEQ` box takes a point of `E` of the from `P_i = 2^{248*i}·P` for `i=0,...,j-1` and outputs two points:
	- `2^{248} · P_i` 
	- `\sum_{n = 0}^{247} (b_n · 2^{n} · P_i)`. 

    The first point is the input of the next `(i+1)`-th `SEQ` box (note that `2^{248} · P_i = P_{i+1}` whereas the second output is the computation of the `i`-th term in the above expression of `k·P`. The precise circuit is depicted in next two figures `SEQ` and `WINDOW`.
	
    ![](https://i.imgur.com/J4U55ij.png)
    ![](https://i.imgur.com/y9mWrn9.png)



    
- The idea of the circuit is to first compute
    ```
    Q = P_i + b_1·(2·P_i) + b_2·(4·P_i) + b_3·(8·P_i) +...+ b_{247}·(2^{247}·P_i)
    ```
    and output the point
	```
    Q - b_0·P_i
    ```
	This permits the computation of `Q` using the Montgomery form of Baby Jubjub and only use twisted Edwards for the second calculation. The reason to change forms is that, in the calculation of the output, we may get a sum with input the point at infinity if `b_0 = 0`. 
	
	Still, we have to ensure that none of the points being doubled or added when working in `E_M` is the point at infinity and that we never add the same two points. 
	
    - By assumption, `P != O` and `ord(P)>8`. Hence, by Lagrange theorem [[4, Corollary 4.12]](http://poincare.matf.bg.ac.rs/~zarkom/Book_Shaums_Group_theory.pdf), `P` must have order `r`, `2r`, `4r` or `8r`. For this reason, none of the points in `E_M` being doubled or added in the circuit is the point at infinity, because for any integer `m`,  `2^m` is never a multiple of `r`, even when `2^m` is larger than `r`, as `r` is a prime number. Hence, `2^m · P != O` for any integer number `m`.
    - Looking closely at the two inputs of the sum, it is easy to realize that they have different parity, one is an even multiple of `P_i` and the other an odd multiple of `P_i`, so they must be different points. Hence, the sum in `E_M` is done correctly.
	
-  The last term of the expression of `k·P` is computed in a very similar manner. The difference is that the number of bits composing `k_j` may be shorter and that there is no need to compute `P_{j+1}`, as there is no other `SEQ` box after this one. So, there is only output, the point `k_j · P_j = k_j·2^{248j}·P`. This circuit is named `SEQ'`.

    ![](https://i.imgur.com/y6VSKpo.png)
    
## References

[[1]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.559.7774&rep=rep1&type=pdf) Katsuyuki Okeya,  Hiroyuki Kurumatani and Kouichi Sakurai. _Elliptic Curves with the Montgomery-Form and Their Cryptographic Applications_. Jan 2000.

[[2]](https://eprint.iacr.org/2008/013.pdf) Daniel J. Bernstein, Peter Birkner, Marc Joye, Tanja Lange and Christiane Peters. _Twisted Edwards Curves_. Cryptology ePrint Archive, Report 2008/013. 

[[3]](https://www.hyperelliptic.org/EFD/) Daniel J. Bernstein, Tanja Lange et al. _Explicit-Formulas Database_.

[[4]](http://poincare.matf.bg.ac.rs/~zarkom/Book_Shaums_Group_theory.pdf) B. Baumslag and B. Chandler. _Schaum’s outline of Theory and Problems of Group Theory_. Schaum’s outline series. McGraw-Hill Book Company, New York, 1968.
