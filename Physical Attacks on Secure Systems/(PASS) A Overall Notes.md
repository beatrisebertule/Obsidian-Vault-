

leakage models - HM, HD, LSB, MSB, L2SB, ...
distinguishers - DoM, Pearson's correlation

(1) Choose your sensitive variable  
(2) Collect measurements, known plaintext/ciphertext, sub-key guesses  
(3) Predict (hypothetical) intermediate values  
(4) Decide on the leakage model  
(5) Recover the key by statistical or other means using a side-channel distinguisher


### Sbox Function

non-linear function
DPA works teh bets on non-linear functions


### Leakage
leakage models - apply on hypothetical values
then correlate with actual side channel data

why leakage models are important?
if they do not capture the actual leakage, there's not much that we can learn with side channel distinguisherers

software implementations - HW(0xors1) = HW(s1) 
hardware implementation - HW(HD(s1,s2)) = HW(s1 xor s2)


### SPA, DPA CPA

which one to use - depends on 
<span style="color:rgb(219, 0, 0)">SPA</span> - one or two power measurements, maybe when ephemeral keys are attacked
<span style="color:rgb(219, 0, 0)">DPA</span> - multiple measurements of the same key (diff plaintexts) for unprotected 
for protected - higher order DPA attacks
<span style="color:rgb(219, 0, 0)">CPA</span> special case of DPA
TA - profiled attacks, only one trace to attack 



number of traces needed - until we can distinguish the correct key
success measures such as key rank convergence can help with 




### General Approach - Sensitive Variable

any data that we know - plaintext, IV, ...
data  that we want to learn - not necessarily key, smth that we can derive the key from 

# Identify sensitive variable to attack

![[Pasted image 20260611182551.png]]

B because we control the padded message and the thing we want to learn is in the output of the function 

![[Pasted image 20260611184355.png]]

cannot do this if IV is constant



extra question
mirai botnet
IoT devices
default credentials

domain fronting
claim to connect to allowed domain
once encrypted connection
made request for another - banned domain

cannot do HTTP challenge, 

DNS challenge - A, AAA - add resource record



IP frag recap - how 


\\\\\