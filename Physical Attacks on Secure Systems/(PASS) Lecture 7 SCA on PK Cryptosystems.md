### Online Template Attacks

last lecture on side-channels for now
next weeks - fault injections and more


### attack ECC: basic algorithm for scalar multiplication
double-and-add algorithm
add dummy multiplication d_1 = 0 
to protect from spa

how to make dpa resistant?
-> randomized base point

"processed in a random way"

-> randomize scalar

attacks on ECC implementations: SPA
basic SPA attacks can be countered by balancing all operations -> the concept of "atomicity"
more advanced attacks: collision and doubling attacks
-> all these lead to horizontal attacks 

horizontal attacks (vs vertical) - fold parts of it - use one trace

common denominator: one leakage trace available 

DPA usually requires attacks typically many repeated executions of a specific algo
context of ECC - scalar multiplication (rather s-box)


<span style="color:rgb(219, 0, 0)">recap ECDSA !!</span>
k - ephemeral key (random integer)
cannot guess k mathematically (computationally hard)
one power traces given - SPA attack

s == k^-1 (m + ax)
retrieve a (the secret) after we have guessed k !!

inverse can be gotten through exponensiation

<span style="color:rgb(219, 0, 0)">typical exam question</span>
ask - what will be the point of attack for side-channel attack

### attack Schnorr identification protocol
prove knowledge of smth
zero-knowledge proof

check notes on proof

1st point of attack
y = ae + r

e is known
a is secret
we want to learn r to retrieve a
we have only one trace - can execute spa
a = (y-r)/e


2nd point of attack: do dpa on: a times e 
now we can have many traces as we attack directly ae - can execute dpa attack

check summary paper - public key crypto are a bit diff

non-linearity makes attack better
but linear operations can still be attacked


### countermeasures

pyramid

protocol level: limit nr of times the key is used
scalar mult level: random scalar splitting
	k = k1 + k2 in random way
	randomizing scalar and point
	...
group operations level: ...
curve/field arithmetic level: projective coordinates
architecture level: (not ECC specific)

randomizing techniques wont help against template attacks


### Online Template Attacks

1. template building
2. template matching

uses maximum likelihood distinguishers

assumption: available not more than few traces (with the same secret available)

new spa attack tailored for public key crypto
- horizontal attacks
- online template attacks
- combine SCA leakage with other theoretical attacks (solve system of equations)
- can defeat some countermeasures 

attacker obtains only one target traces but several template traces per key-bit
target trace - obtain online
template traces

assumption - one key dependant assignment

step 1 profiling of device

come back to slides, confused



works on spa and dpa protected 

very real attack in real life
added to BSI guidelines