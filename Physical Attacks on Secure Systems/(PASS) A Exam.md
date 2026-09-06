
[[claude answers]]

# Question 1 (10 points)
Fill-in the missing terms:  

![[Pasted image 20260611172139.png]]

Physical attacks are possible due to unintentional leakage from side channels such as <span style="color:rgb(219, 0, 0)">power consumption</span>,  and <span style="color:rgb(219, 0, 0)">electromagnetic emission</span>.  

To prevent timing attacks, the designers should use <span style="color:rgb(219, 0, 0)">constant-time</span> code for cryptographic implementations.  

Power side channel is predominantly caused by <span style="color:rgb(219, 0, 0)">dynamic</span> power, which is a result of  
<span style="color:rgb(219, 0, 0)">bit transition (switching)</span>. To exploit power side channel we need a leakage <span style="color:rgb(219, 0, 0)">model</span> . An example  
is the Hamming weight model, which is suitable for <span style="color:rgb(219, 0, 0)">software</span> implementations. Hamming  
weight of an n-bit variable is computed as <span style="color:rgb(219, 0, 0)">a sum of all bits (number of bits equal to one)</span>.  

<span style="color:rgb(219, 0, 0)">Simple</span> power analysis attack can sometimes recover the key from a single trace (example of double and add, square and multiply).

# Question 2
(10 points) 

PRESENT is a lightweight block cipher designed for constrained  environments and is also an ISO standard. The block size is 64 bits and the key size can be  80 bit or 128 bit. The non-linear layer is based on a single 4-bit S-box (see Fig. 1).

Assume that you get a set of 100 000 traces from an unprotected software implementation of
PRESENT with an 80-bit key and corresponding plaintexts. Describe a DPA attack on this
implementation by outlining the steps you would take. The goal of the attack is to recover
the first-round key.

![[Pasted image 20260611174522.png]]


my answer

software implementation -> HW leakage model

We need to target a sensitive value for which an exhaustive key search is possible that depends on a secret that we want to recover (key) and a known variable (plaintext).

1. Choose a sensitive variable.
The sensitive variable of PRESENT is right after the sBoxLayer, so the variable that we want to target becomes SBox(p[i] xord k[i]) where p[i] correspond to the i-th byte of the plaintext and k[i] corresponds to the i-th byte of the key. 

2. Collect measurements.
We collect power consumption measurements with fixed key and random plaintexts.

3. Execute DPA.
For all key byte candidates (256 values), we calculate hypothetical intermediate variables for all traces:
	y = SBox(p[i] xord k[i]) of target byte (we traget one byte at a time)
Then we apply a leakage model - LSB - onto the hypothetical intermediate values. Then we group the traces based on the leakage model.
Once the hypothetical labels have been assigned to each trace and the each traces has been grouped based on leakage mode, we calculate the average of each of the two groups - 0 and 1, and calculate the difference.
Then we take the the maximum difference of a time sample.
Once we have done this fro all key byte candidate values, we simply take the key byte hypothesis  that has the highest DoM value - that value will be the correct one.

We repeat this approach for all bytes.


# Question 3
![[Pasted image 20260611181154.png]]

Profiling Attacks (10 pts)
Answer the following questions in the context of profiling attacks.

(a) (7 points) Fill-in the missing terms:
In a template attack on a symmetric-key algorithm implementation, the adversary first performs a <span style="color:rgb(219, 0, 0)">profiling</span> phase using a clone of the target device. During this phase, the adversary collects a large set of side-channel traces, each labeled with known input data and known <span style="color:rgb(219, 0, 0)">key</span>. The goal is to build a statistical model, or a template, for each possible value of the target <span style="color:rgb(219, 0, 0)">sensitive</span> variable. This model typically assumes a <span style="color:rgb(219, 0, 0)">Gaussian (normal)</span> distribution, often modeled as univariate or multivariate. The parameters of this distribution such as the mean and <span style="color:rgb(219, 0, 0)">standard deviation</span> (or covariance matrix) are extracted from the template traces. In the attack phase, the adversary collects traces from the target device and computes the <span style="color:rgb(219, 0, 0)">likelihood</span> function to identify the most likely key hypothesis.

(b) (3 points) Explain the impact of the Point of Interest (PoI) selection on the effectiveness
of a template attack. List some of the methods to find suitable Points of Interest

We need to select informative points that hold information about the processed secret. It the selected points happen to not be carry strong information of the targeted leakage, the templates won't be distinctive which would give us lower attacker's success rate.

# Question 4
(10 points) The sponge construction can be used to build MAC functions as in Fig. 2.
![[Pasted image 20260611182152.png]]

In such a construction, the Key is secret, the Padded message and the MAC are known, and
f indicates a cryptographic permutation that processes and combines together bits from the
inner and outer part of the state.

Answer the following questions. Please explain your reasoning behind your answer. Answers
lacking argumentation will not be scored. To answer the questions, the details of the permu-
tation f are not needed.

(a) (3 points) Can we potentially recover the Key by applying Differential Power Analysis
during the Key absorption phase (indicated in Fig. 2)?

Not really as we don't have a control over the initial value - we don't have a variable that we control, as every execution would produce the same intermediate value.

(b) (3 points) Can we potentially recover the Key by applying Differential Power Analysis
during the Padded message absorption phase (indicated in Fig. 2)?

Yes, we have control over the padded message, and the state after the key absorption contains the secret that we want to learn. We can target any intermediate state.

(c) (4 points) Can we potentially recover the Key by applying Differential Power Analysis
during the MAC squeezing phase (indicated in Fig. 2)?

Yes, the same as previous question - the variable that we have control over would be MAC rather than message

# Question 5
(10 points)

Please answer the following questions regarding TVLA:
(a) (2.5 points) What is the null hypothesis in the Test Vector Leakage Assessment?
Null Hypothesis: the device does not leak (is not guilty of leaking) side channel information. To be more precise, the power consumption does not depend on the input. The mean of fixed input and random input set of traces have to be the equal.

(b) (5 points) Briefly describe the steps of the TVLA computation and explain how is
statistical significance (α) determined.

We define null hypothesis with significance level alpha = 0.001

We collect traces - with fixed and with random plaintext. Both set of traces are collected with the same secret key. Then for every time sample we compute t-statistics, then we compare t-scores against a threshold value of 4.5 which corresponds to a very low p-value of 0.001 - our predefined significance level.
If the t-socre happens to be higher than the thresholds - the time sample point leaks.

(c) (2.5 points) A side-channel tester runs a TVLA test on a set of 2 000 fixed and 2 000
random traces. The significance level α = 0.0001. Each trace has 4 050 samples. For
sample 1 200, the tester obtains a p-value of 0.00002. What does this mean? Can we
conclude the device is "guilty" of leaking information at sample 1 200? Explain your
answers.

We reject the null hypothesis as the p-value is very low - the device at that time sample point leaks.


# Question 6
(10 points) Which of the following statements about higher-order attacks and countermeasures are correct? Please explain the reasoning behind your answers. Answers lacking argumentation will not be scored.

(a) (2.5 points) Masking is only used to protect plaintext recovery.

Not true, masking is used to break connection between teh secret value computed and the physical leakage. Masking protects a sensitive intermediate variable, protect key-dependant intermediates, not plaintexts

(b) (2.5 points) DPA (of any order) on a masked implementation can recover only the
mask used.

Wrong, DPA recovers the key-dependant intermediate value. Higher order leakage combines multiple leakage points to cancel the mask out

(c) (2.5 points) Using d + 1 shares provides the security of order d i.e. it is expected to guarantee the scheme d-probing-secure.

To achieve a security level of d (d-probing secure), the sensitive variable has to be split in d+1 shares. For example, to protect from 1st order DPA, the intermediate value has to be split in 2 shares.


(d) (2.5 points) Boolean masking is used to protect implementations against simple power
analysis.

<span style="color:rgb(219, 0, 0)">No, boolean masking helps against all types of attacks, not only SPA.</span>


# Question 7
(10 points) Consider the following scalar multiplication algorithm for Elliptic
Curve Cryptography (ECC):

![[Pasted image 20260612161514.png]]

This algorithm is respecting the rule of “side-channel atomicity”, in which individual oper-
ations are implemented in such a way that they have an identical side-channel profile (e.g.,
for any branch and any key bit- related subroutine). In the algorithm we assume that point
doubling and addition operations are implemented such that the same code is executed for
both operations.

Assume a side-channel analysis adversary is able to collect traces from a device on which this
algorithm is implemented. What type of side-channel attacks are possible for the adversary
interested in the key recovery (k)? Consider the following attacks for two use cases i.e. for
k a long-term (static) key and for k being a 1-time (ephemeral) key. Answer the following
questions and outline the attack(s) possible.

(a) (3 points) Is a Simple Power Analysis (SPA) attack possible? Explain your answer.

No, such defense target SPA where the adversary can distinguish between mathematical operations. Now when both bit states - one and zero -  are processed with double and add always, the adversary can no longer distinguish.

(b) (3 points) What about a Differential Power Analysis (DPA) attack? If the attack is
possible, identify the attack point(s) (i.e. steps in the algorithm).

DPA is still possible, we want to recover the secret scalar k, we can change the variable n.
<span style="color:rgb(219, 0, 0)">Possible only if the key is static - no ephemeral key use. </span>

(c) (4 points) Is an Online Template Attack (OTA) possible and what would be the attack
points? Explain your answer

Yes, online template attack is possible, we recover the target bit at a time. We target key-dependant assignment R0 <- R0 + Rn. The adversary can match the trace segment of each loop (each computation of R0) against remplates for key bit =1 and key bit = 0.

# Question 8
(10 points) 

In the context of Differential Fault Analysis (DFA) on cryptographic implementations, which of the following statements are true? Please explain the reasoning behind your answers. Answers lacking argumentation will not be scored.

(a) (2.5 points) DFA is similar to the DPA attack, the main difference is that the traces
collected in the two attacks have different representations.

Not correct, DPA is a passive attack while DFA is and active attack. Both attack have different assumptions and different execution. 


(b) (2.5 points) Differential Fault Analysis (DFA) always requires multiple successful faults  
performed during the target algorithm implementation running

Not true, Bellcore attack requires only one successful fault, combined with a correct one to recover the private key.

(c) (2.5 points) Computing an RSA signature twice, and only releasing it when the correctness test has passed, is a valid countermeasure against DFA.

Yes.
By computing the signature twice independently and comparing results before releasing, any fault that affected one computation will cause a mismatch and the signature is withheld. 

(d) (2.5 points) DFA attacks are not possible on symmetric crypto ciphers that are permutation-
based, such as ASCON.

No, FI are effective on all cryptographic algorithms.

# Question 9
(10 points) In the context of building a deep neural network (DNN) for side-channel analysis, which of the following statements are true? Please explain your reasoning behind your answer. Answers lacking argumentation will not
be scored.

(a) (2.5 points) The number of nodes in the input layer is typically chosen to match the
number of distinct keys in AES.

No, we match the number of time sample points in the trace.


(b) (2.5 points) The softmax activation function is used in the hidden layers to improve
the model’s convergence.

No, softmax is used at the output layer; ReLU/tanh are common  
for hidden layers.

(c) (2.5 points) The number of output nodes should match the number of possible labels
defined by the leakage model.

Yes, exactly, we want to model teh model to map phsycial lekaage to a leakag nmdoel if teh senitive variable

(d) (2.5 points) Overfitting occurs when the model performs better on unseen data than
on training data.

false, the opposite is true - when we overfit on train data so the mdoel perofms badly on testd ata

