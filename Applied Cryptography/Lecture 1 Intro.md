
### Introduction to Stream Encryption

Kerckhoffs

adv has access to ciphertext 

adv can choose plaintext and force user to encrypt them to see the corrresponding ciphertext
-- chosen plaintetx attack

sometimes can choose a cipherttext, make bob decrypt them, see corresponding plaintext
-- chosen ciphertext attack


what kind of protection alice and bob expect from encryption?

**security goal**
indistinguishability under chosen plaintext (IND-CPA)

defined by a game between an adversay and a challenger 


![[Pasted image 20260903113715.png]]

**How the game runs**

1. **Setup.** The challenger picks a secret key K uniformly at random. The adversary never sees it.
2. **Learning phase.** The adversary picks any plaintext P_i it likes, sends it over, and gets back C_i = Enc_K(P_i). It can repeat this as often as it wants. This is the "chosen plaintext" part: the adversary effectively has an _encryption oracle_ for the secret key, modelling real situations where an attacker can get a victim to encrypt data of the attacker's choosing.
3. **Challenge.** The adversary picks two plaintexts P₀* and P₁* of the same length. The challenger secretly flips a coin b, encrypts only P_b*, and returns C*. The adversary does not know b.
4. **More learning.** The adversary may keep asking for encryptions of chosen plaintexts, including P₀* and P₁* themselves.
5. **Guess.** The adversary outputs a guess for b. It wins if the guess is right.

**Connecting it to "indistinguishability"**

A coin flip wins with probability ½, so the interesting quantity is the _advantage_: how much better than ½ the adversary does. The scheme is IND-CPA secure if every efficient adversary has only a negligible advantage.

Why does this capture security? If the adversary can't tell whether C* came from P₀* or P₁*, then C* reveals nothing useful about the plaintext, even for two messages the adversary chose to be maximally distinguishable. Conversely, if the ciphertext leaked even one bit of information (say, whether the first character is 'A'), the adversary would just pick P₀* starting with 'A' and P₁* not, and win. So "no adversary can distinguish" is equivalent to "the ciphertext leaks nothing," which is what you want from encryption. The equal-length requirement exists because encryption normally does not hide message length, so that trivial leak is excluded from the definition.

**Why the exercise on the slide is easy**

If encryption is deterministic, the same plaintext always gives the same ciphertext. So the adversary chooses two different messages, receives C*, then asks the oracle to encrypt P₀*. If the result equals C*, b was 0; otherwise 1. It wins every time. This is the game teaching you something concrete: IND-CPA security forces encryption to be randomized (or nonce-based), which is exactly why modes like CBC and CTR use a fresh IV per message.


**deterministic encryption cannot be IND-CPA


#### Security strength s
expressed in bits

there are no successful attack with less than 2^s  resources

if we have 128 bit security
we need 2^128 resources to break the encryption schema

success prob on one attack is at most Pr(success) < 1/ 2^s

succes prob 1/2^128, very small success prob

In distinguishing settings, this holds for the advantage given by
Adv = 2Pr(success) - 1

Pr(succes) between 1/2 and 1

Adv will range from 0 to 1

if succes prob 1, then 2-1=1
if success prob 1/2 then 1-1 =0

in example of challenger game


** resources mean
Distinction is often made between different types of resources:  
• data complexity σ: amount of data exchanged in queries to/from the keyed  
instance under attack (can be limited by design)  
• computational complexity τ : amount of computation (limited by wealth)  
• Remember: for concrete ciphers no strength s can be proven, only claimed


#### Probabilistic Encryption

It is possible to define a probabilistic encryption scheme  
• for each P there are many ciphertexts and encryption chooses one of them randomly  
• decryption is deterministic: any of these ciphertexts must result in P  
Problems  
• we need a reliable source of randomness: problematic on many platforms  
• security strength s implies 2^s ciphertexts per plaintext, so ciphertexts are at least s  
bits longer than their plaintext  
• testing and certification of non-deterministic functions is a nightmare, . . .  
• That is why encryption schemes used in practice are deterministic

### Deterministic Encryption

![[Pasted image 20260903122233.png]]

AD (associated data) does not have to be secret
send along the message or derived (e.g. date)

AD can be nonce, IV, diversifier
nonce - fixed length, 12 bytes
AD - arbitrary length

AD for authenticated encryption
### Stream Encryption

### Nonce based authenticated encryption with associated data

