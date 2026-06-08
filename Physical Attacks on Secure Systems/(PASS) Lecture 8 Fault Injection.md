on crypto implementations


active attack

pushed into abnormal states
produces faults
skips instructions

## Intro and history

Fault attacks exploit the fact that computers sometimes make mistakes

Intel floating-point bug discovered in 1994, occurs once in 9 billion computations – <span style="color:rgb(219, 0, 0)">transient fault</span>

what if we get more of these mistakes?
how can we provoke them on purpose?

<span style="color:rgb(219, 0, 0)">Faults</span> – can be caused using (changes in) voltage, clock, temperature, radiation, light, etc.  
RSA signature scheme can be broken by a single fault

![[Pasted image 20260508112549.png]]


<span style="color:rgb(219, 0, 0)">glitch</span> - causes faulty output

### history
1978: one of the first examples was due to radioactive particles (used in chip packaging materials), observed by May and Woods  

1979: effect of cosmic rays on memories – strong in the upper atmosphere and outer space 

1992: use of laser beam to charge particles on microprocessors, Habing  

1995: Bellcore attack by Boneh, DeMillo and Lipton pointing at the consequences of a single fault on RSA implementation  

1997: differential fault analysis on secret–key cryptosystems by Biham and Shamir  

2002: first practical demonstration of Bellcore attack on a smartcard with an RSA coprocessor – glitching supply voltage

### attack categories

<span style="color:rgb(219, 0, 0)">Side-channel attacks</span>  
• use some physical (analog) characteristics  
• the target is running in normal conditions  

<span style="color:rgb(219, 0, 0)">Faults</span>: use abnormal conditions causing malfunctions in the system  
<span style="color:rgb(219, 0, 0)">Micro-probing</span>: accessing the chip surface directly in order to observe, learn and manipulate the device  
<span style="color:rgb(219, 0, 0)">Reverse engineering</span>: sometimes assisted by any of the above

Active vs passive:  
	<span style="color:rgb(219, 0, 0)">Passive</span> i.e. eavesdropping: the device operates within its specification  
	<span style="color:rgb(219, 0, 0)">Active</span> i.e. tampering: the key is recovered by exploiting some abnormal behaviour e.g. power glitches or laser pulses  
	
Invasiveness:  
	<span style="color:rgb(219, 0, 0)">Non-invasive </span>aka low-cost:  
		• power/EM measurements  
		• Coldboot attacks: data remanence in memories - cooling down is increasing the retention time (freeze mother board and run away)
		• Rowhammer – is essentially a fault attack  
	<span style="color:rgb(219, 0, 0)">Semi-invasive</span>: the device is de-packaged but no direct contact exists with the chip e.g. optical attacks  
	<span style="color:rgb(219, 0, 0)">Invasive</span> aka expensive: the strongest type is bus probing

### methods

<span style="color:rgb(219, 0, 0)">Voltage or Clock Glitching  </span>

variation in supply voltage and external clock can cause a processor skip instruction or data misread, kind of try to play with boundaries to cause a faulty output (skip in instruction, change of memory)


<span style="color:rgb(219, 0, 0)">Change in Temperature  </span>
the temperature threshold is defined for which the chip will work properly  
can cause changes in RAM content  

<span style="color:rgb(219, 0, 0)">EMFI</span>  (electromagnetic fault injection)
investigated a lot on real-world crypto implementations  
can cause byte-level faults with a single fault  

I White light (photons), X-rays, ion beams


### goals

Insert computational fault  
• Null key  
• Wrong crypto result (Differential Fault Analysis - DFA)  

Change software decisions  
• Force approval of false PIN  
• Reverse life cycle state – PayTV and old phone cards  
• Enforce access rights


## Fault analysis in Practice


Injecting exploitable faults is very hard i.e. reproducibility and accuracy are not trivial  

Many parameters have to be set to certain values (the 2002 paper of Infineon mentions 9 different parameters)  
	difficult (computationally heavy) to find parameter value combinations

Large space of optimizing the search for the “right” parameters values  

Different parameters depending on the target device (ASIC, FPGA, microprocessor, Java card)  

Cheap techniques such as glitching can be very effective

### expensive fault injection

<span style="color:rgb(219, 0, 0)">laser beam</span> 
the most efficient attack technique due to  the precision of the results
can execute multiple faults (inject multiple faults)
usually assisted by a microscope
- - -
<span style="color:rgb(219, 0, 0)">Laser Fault Injection</span> (LFI) is the most efficient attack technique due to  
the precision of the results  

<span style="color:rgb(219, 0, 0)">Microscope</span>: optical or scanning electron microscope (SEM) – assisting  
laser fault injection (with 1-μm laser spot accuracy)  

<span style="color:rgb(219, 0, 0)">Probe station</span>: to probe wires on the chip  

<span style="color:rgb(219, 0, 0)">Focused Ion Beam</span> (FIB)  
• uses <span style="color:rgb(219, 0, 0)">ions</span> instead of electrons  
• not only for observing, but also making changes: removing or adding wires, etc.


### fault injection - what is possible
flip bits by targeting one of its transitions

laser can inject multiple faults within the same execution of a  cryptographic algorithm  

FIB (focused ion beam) enables an attacker to:  
• arbitrarily modify the structure of a circuit (i.e. reconstruct missing buses, cut existing wires)  
• reverse engineer by adding probing wires to parts of the circuit that are not accessible  
• but also debug and patch chip prototypes

but we stick to low cost fault injections...
<span style="color:rgb(219, 0, 0)">low cost techniques:</span>

-  <span style="color:rgb(219, 0, 0)">Under-powering</span> of a computing device (can cause a single-bit error and requires almost no knowledge of the implementation details and platform)  

- <span style="color:rgb(219, 0, 0)">Injection</span> of (well-timed) <span style="color:rgb(219, 0, 0)">power</span> <span style="color:rgb(219, 0, 0)">spikes</span> on the supply line of a circuit could even skip a single instruction  
	collect faulty computations (output), do verification test to make sure that the fault indeed is a fault, collect many faulty outputs for differential attacks
	keep verified faulty op for the attack

- <span style="color:rgb(219, 0, 0)">Tampering</span> with the clock (shorten the length of a single cycle or overclocking the device)  

- Increasing temperature  

- EM pulses  

- Fault injection by light e.g. optical attacks:  
	UV lamp or a camera flash can be used  
	Can cause the erasure of EEPROM and Flash memory cells (usually crypto constants are kept there)


# Differential Fault Analysis (DFA)

Bellcore attack in 1995  
requires faulting RSA-CRT signatures
....


## FA countermeasures for symmetric-key cryptosystems

Generic approaches:  
	Correctness check: encrypt twice  
	Random delays: limits the precision  
	Masking: secret sharing complicates probing wires of the device  

Hardware countermeasures:  
	Light detectors, supply voltage, frequency detectors  
	Active shields  
	Redundancy: duplication of hardware blocks  
	Dual rail implementations

> making smth resistant for faults make, can make SCA attack simpler and vice versa
> protection against both - very hard

### symmetric-key countermeasures

Introducing redundancy is harder than for PKC  
Multiple execution is expensive  
Using the inverse computation  
Loop invariant:  
	• 2nd variable counting in the opposite way prevents tampering the counter of a loop  
	• add a signature that is updated in every run of the loop (checksum)  
To ensure the integrity of the stored data, Cyclic Redundancy Check  (CRC) can be added

## Differential fault analysis for RSA

![[Pasted image 20260508123816.png]]


## Inject fault In sp
![[Pasted image 20260508145355.png]]

The entire difference `s − ŝ` is **a multiple of q**, because
so `q` divides `s − ŝ`

The entire security of RSA rests on the fact that **factoring n back into p and q is computationally infeasible** for large n (2048+ bits). Anyone who knows n but not p and q cannot derive the private key d.


```
n     = p · q        → divisible by q
s − ŝ = k · q        → divisible by q (for some integer k)
```

So q is a **common divisor** of both. And since k is essentially random, it almost certainly contains **no factor of p**, so the gcd won't "accidentally" return p·q or some larger value. Hence:

```
gcd(n, s − ŝ) = q
```

Once you have q:

```
p = n / q
```

Just a simple division — since n = p·q, dividing by q gives p directly.

## Inject fault in sq

![[Pasted image 20260508150055.png]]


## <span style="color:rgb(219, 0, 0)">countermeasures</span>
Compute a signature twice and compare the two results  

Verify the signature with the public exponent e, 
	However, in some applications (Java card), one does not have access to the public exponent e during signature generation  

Shamir: random r is first chosen and then modular exp. is computed based on r · n  

Computation:  
(1) Find s∗ = md (mod r · n)  
(2) Find Z = md (mod r )  
(3) If s∗ = Z (mod r )  
(4) Output s = s∗ (mod n)

## real world example: fault attack on WolfSSL

Ed25519 signatures

![[Pasted image 20260509175808.png]]


#### Key Setup (steps 1–4)

**Step 1** — Your private key `k` is hashed (using SHA-512) to produce `2b` bits, split into two halves:

- `a` = the first half (bits 0 to b−1) → the **private scalar**
- `b` = the second half (bits b to 2b−1) → the **auxiliary key**

The reason for hashing first (rather than using `k` directly) is to ensure uniform randomness and enable safe nonce derivation.

**Step 4** — The **public key** `A = aB`, where `B` is the well-known base point on the Ed25519 elliptic curve. Multiplying a scalar by a curve point is a one-way operation — easy to compute forward, infeasible to reverse.

#### Signature Generation (steps 5–9)

**Step 5** — A **deterministic nonce** `r = H(b, M)` is computed from the auxiliary key `b` and the message `M`. This is crucial: unlike ECDSA, Ed25519 never uses a random nonce, eliminating the risk of nonce reuse attacks (which famously broke Sony's PS3 signing).

**Step 6** — `R = rB` is the **ephemeral public key** — the curve point corresponding to nonce `r`.

**Step 7** — A **challenge hash** `h = H(R, A, M)` binds together the nonce point, the public key, and the message.

**Step 8** — The **signature scalar**: `S = (r + ha) mod l`, where `l` is the prime order of the curve group. This combines the nonce with the private scalar, weighted by the challenge.

**Step 9** — The final **signature is the pair (R, S)**.


#### Verification (implicit)

A verifier, given `(R, S)`, the public key `A`, and message `M`, checks whether:

> `SB = R + hA`

This works because `SB = (r + ha)B = rB + haB = R + hA`.



what would we target?
target line 6, will cause a faulty signature
### Fault
![[Pasted image 20260509180426.png]]

- `h` and `h'` are different challenge hashes (different messages)
- `r` is the **same** nonce — this is the fatal mistake

all of `S`, `S'`, `h`, `h'` are **public** (visible in the signatures). So `a` — the **private key** — is completely exposed.


one fault can help to extract secret

<span style="color:rgb(219, 0, 0)">extract private key a</span> 


setup 
![[Pasted image 20260509180857.png]]

sucessful fault - 
![[Pasted image 20260509181314.png]]

green - produces correct value (fault does not work)
yellow - produces garbage (not successful fault), stops working...

red - successful


Physical attacks on implementations of Ed25519  
I Side-channel analysis of Ed25519 with 4 000 traces  
I Fault injection on Ed25519 with 100% success rate for EM FI and 70% for  
voltage glitching out of 10 000 measurements  
I For both attacks there exist inexpensive countermeasures