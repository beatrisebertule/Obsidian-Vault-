from past lectures
I don't fully understand the concept of of 1st order, 2nd order

1st order - mean
2nd order - variance

why we protected against one order, and the higehr order does not work?

---


DPA protection - the idea is to break down the link between the computation and the value processed
has to be implemented correctly
so no unmasked point (moment) - where the native (actual) variable has been unmasked


d number of shares
d number of shares (masks) protects against d-th order of attack
d = 1, 1st order attack, normal dpa attack on unprotected traces (native var split into no shares)

if 2nd order defense
then adversary needs squared amount of shares to attack that 


<span style="color:rgb(219, 0, 0)">probing-secure scheme</span>
we refer to a scheme that uses certain families of shares as  
d−probing-secure iff any set of at most d intermediate variables is independent from  
the sensitive values



the dth-order security is achieved by using <span style="color:rgb(219, 0, 0)">d + 1 shares</span> (combating a dth-order DPA)  
such that at most d of these shares are leaked to a dth-order adversary

higher-order security
![[Pasted image 20260312162252.png]]

implementation of (boolean) masking depends on operations
linear operations are easy to split
(have to be careful of executive execution of these shares leak...)
non-linear - s-box function, expensive to compute

### Higher-Order DPA

n-th <span style="color:rgb(219, 0, 0)">order</span> DPA:  
	exploits statistical moments from n-th order  
n-th <span style="color:rgb(219, 0, 0)">dimensional</span> DPA  
	considers n points in time



reminder: dpa works the best on variables right after a non-linear function such as s-box

![[Pasted image 20260312170608.png]]


### 2nd order DPA
attack on masked implementation

2 order - two points


1st order DPA uses one point
2nd order DPA uses two points


1st order dpa fails with masking
as the sensitive variable is split into shares
no single point of leakage

![[Pasted image 20260318112642.png]]
combining two leakages cancels the mask
	HW(a xor b)=∣ HW(a) − HW(b) ∣



there's a difference between
higher order attack
vs
higher order statistics

higher order DPA - higher order attack
hence, higher order attack combines **multiple leakage points**

higher order statistics - variance uses **square (product)**, it captures **second-order behavior**

slide from lecture 3
![[Pasted image 20260318113550.png]]


how to select these two points?

# Implementing securely public-key cryptography (PKC)

RSA system
generate large primes p, q
compute
	n=pq
	φ(n) = (p − 1)(q − 1)
and choose e such that gcd(e, φ(n)) = 1
compute d such that ed ≡ 1 (mod φ(n))  

public key: (n, e)
private key: (n, d)  

encryption: c = m^e mod n
decryption: m = c^d mod n

security of RSA cryptosystems is based on the <span style="color:rgb(219, 0, 0)">difficulty</span> of <span style="color:rgb(219, 0, 0)">factoring</span> large integers

hybrid appliocation:
use RSA for encrypting a symmetric key
encrypt and authenticate with symmetric crypto

the critical operation is modular exponentiation
commonly implemented as chinesse remainder theorem

square and multiply, montgomery ladder...

# Protect square and multiply from SPA

![[Pasted image 20260611104041.png]]
### The basic square-and-multiply

The exponent `d` is a binary number, e.g. `d = 1011`. The algorithm scans it bit by bit from the most significant to least significant:

- **Every bit**: square R₀
- **Only if the bit is 1**: also multiply R₀ by x

So for `d = 1011` the sequence of operations looks like:

```
bit 1 → square + multiply
bit 0 → square only
bit 1 → square + multiply
bit 1 → square + multiply
```

---

### The attack (Q1 answer)

The power trace of the device looks like this:

```
[S][SM][S][SM][S][SM]   ← trace for d = 1011
  ^         ^
  this gap (square only) reveals a 0-bit
```

A square consumes a certain amount of power. A multiply consumes a **different** amount. An attacker watching the power trace can literally **read off the bits of d** by looking at where the extra multiply operation appears or doesn't appear. This is a classic **Simple Power Analysis (SPA)** attack — just one trace is enough to recover the entire private key.

The issue is that the **control flow depends on the secret bit `dᵢ`**. When `dᵢ = 1`, t


### The fix: square-and-multiply always (Q2 answer)

The fixed version introduces three registers instead of one:

- `R₀` — the accumulator (real result)
- `R₁` — a dummy register (garbage, never returned)
- `R₂` — holds x

On every single bit, regardless of whether it's 0 or 1, the algorithm does:

```
b ← 1 − dᵢ          // if dᵢ=1 → b=0; if dᵢ=0 → b=1
Rᵦ ← Rᵦ · R₂ mod N  // multiply into whichever register b points to
```

So the operations per bit are now:

|Bit `dᵢ`|b|What gets multiplied|
|---|---|---|
|1|0|R₀ ← R₀ · R₂ (real, correct work)|
|0|1|R₁ ← R₁ · R₂ (dummy, thrown away)|

**Every iteration now performs exactly the same operations**: one square and one multiply. The power trace looks completely uniform regardless of what `d` is:

```
[S][SM][S][SM][S][SM][S][SM]  ← identical pattern for any d
```


# Protect square and multiply from DPA
For DPA (which uses many traces with the same key), two randomisation strategies are used:

- **Randomise the exponent**: replace `d` with `d' = d + r·φ(n)` for a fresh random `r` each time. The result is the same (by Euler's theorem), but the exponent bits differ across traces, so the DPA correlation is broken.
- **Randomise the message**: blind the input as `mₛ = r·m`, compute `mₛᵈ mod n`, then divide out `rᵈ mod n`. Each trace uses different data, breaking the DPA link.

![[Pasted image 20260611105132.png]]

![[Pasted image 20260611104819.png]]



# DPA on ECC
![[Pasted image 20260611110736.png]]



# Improved Notes

### SPA on RSA

square and multiply
```
Input: x, d = (dt−1, dt−2, ..., d0)2
Output: y = xd mod N

1: R0 ← 1
2: for i = t − 1 down to 0 do
	3: R0 ← R2 mod N
	4: if i = 1, R0 ← R0 · x mod N
5: end for
6: return R0
```

```
bit = 1: square and multiply
bit = 0: only square
```


SPA resistant square and multiply

```
Input: P, d = (dt−1, dt−2, ..., d0)2
Output: Q = d · P

1: R0 ← 0, R1 ← 0, R2 ← P
2: for i = t − 1 downto 0 do
	3: R0 ← 2R0 mod N
	4: b ← 1 − di ; Rb ← Rb + R2
5: end for
6: return R0
```

```
bit = 0 then b=1-0=1 and R1 becomes dummy register
accumulate result in R1 while R2 holds P
```


# DPA on RSA

point of attack - secret scalar can be used multiple times with a random known m

defend - randomize d or randomize m

randomise exponent d within square and multiply algorithm
```
Randomizing exponent  
Input: m, d, N, φ(N),  
Output: c = m^d mod N  
1: r = Random()  
2: d' ← d + r φ(N)  
3: c ← m^(d') mod N  
4: return c
```

randomise message m
```
Input: m, d, N,  
Output: c = m^d mod N  
1: r = Random()  
2: ms ← rm  
3: v ← ms^d mod N  
4: u ← r^d mod N  
5: c ← v/u mod N  
6: return c
```

# SPA on ECC

double and add
```
Input: P, d = (dt−1, dt−2, ..., d0)2  
Output: Q = d · P  
1: R0 ← 0  
2: for i = t − 1 downto 0 do  
	3: R0 ← 2P mod N  
	4: If i = 1, R0 ← R0 + P mod N
5: end for  
6: return R0
```

SPA resistant double and add
```
Input: P, d = (dt−1, dt−2, ..., d0)2  
Output: Q = d · P  
1: R0 ← 0, R1 ← 0, R2 ← P  
2: for i = t − 1 downto 0 do  
	3: R0 ← 2R0 mod N  
	4: b ← 1 − di ; Rb ← Rb + R2  
5: end for  
6: return R0
```

```
once again bit=0 (di=0) becomes dummy register
```

# DPA on ECC

point of attack 

![[Pasted image 20260611114211.png]]
