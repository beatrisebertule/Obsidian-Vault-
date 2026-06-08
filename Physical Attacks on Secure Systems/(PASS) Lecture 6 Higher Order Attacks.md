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


target value on ECDSA
![[Pasted image 20260318161617.png]]

we want to learn a
