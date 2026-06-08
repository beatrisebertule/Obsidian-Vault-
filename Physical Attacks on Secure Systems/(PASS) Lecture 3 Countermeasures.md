lecture 3
can come to the third floor of merc to ask more questions

recap

#### Other side-channels
optical emission
with high resolution camera that can capture photons in movement

acoustics
listen to high frequency sounds 10 to 150 KHz to capture sounds produced by CPU that processes RSA, can be carried out form 4m away


recpap questions
why leakage models are important?
difference between spa and dpa?

### Countermeasures
break the link between (actual) intermediate values and power consumption
break the dependence apart

(random) mask
	input (xor) mask
	for s-box, we need <span style="color:rgb(219, 0, 0)">mask correction</span> as s-box is a non-linear function

random split (masking as well)
	x = x1 (xor) x2 one is generated randomly, the other is derived

hiding
	number of switching is always constant, for every effect there is a counter-effect

software countermeasure
time randomisation switching
	operations randomly shifted (time sample points out of order)
	add random delays
	dummy variables

permutation execution


hardware countermeasures
noise 
de-synchronization

countermeasures can be applied on different levels
transistor level
platform level: dummy cycles
program level: dummy instructions
algorithmic level
protocol level

combine countermeasures on multiple levels


#### Boolean Masking
split an internal sensitive value in shares
more share - more security
more shares becomes expensive as more power taken

X=A(xor)B
with shares we don't get any info about X?

<span style="color:rgb(219, 0, 0)">masking breaks if program contains 2 executive operations (A and B) then HD reveals X</span>

masks help for 1st order DPA
but not for 2nd order DPA (attack on variance, check slides)

goal keep native variable (X) independant from computed variables (shares)

![[Pasted image 20260209141435.png]]



#### Sponge and Duplex-based Algorithms
recap sponge construction !!

spnge function used in SHA-3
arbitrary len input -> arb len output

split in blokcs, each block size of r bits (bit rate r)

we padd the msg so we can split it in muiple of r

then squeeze



b = r+c b-bit premutation, c-capacity

keyed mode - part of the input is secret

sponge for MAC
initialization - key xor state
message absortpion
mac squeezing


sensitive variable with known input
and we want divide and conquer property
we want to target non-linear operations


<span style="color:rgb(219, 0, 0)">target of sponge</span> - first xor of message absorption states

using sponge for key stream generation

if nonce constant, we cant perform dpa, we miss teh differentiable part (variable plaintext for ...)

duplex construction !!!

target - recover K during initialisation using the knowledge of of N

XOODOO
sbox processes columns

recap shifts


hardware vs softwar eimplementation
the linear step after the s-box is hardware implementation
it means that intermediate values are not stored, only t


additonal operations to recover the key once we knwo thev target intermed value

slides for software impe
asisgnemt will be on hardware impl - then look at HD between both states (we miss intermediate states)


#### Masking XOODYAK
shares - share A and share B
doubles the nr of states
