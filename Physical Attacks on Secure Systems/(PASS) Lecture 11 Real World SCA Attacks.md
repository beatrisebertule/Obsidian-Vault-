
exam questions

**question 1**: fill-in question
1. <span style="color:rgb(219, 0, 0)">passive</span> attacks - side-channel attack
2. <span style="color:rgb(219, 0, 0)">active</span> attacks - fault injection attack
3. physical leakage
4. keys
5. message
6. implementation details
7. DPA
8. SPA
9. difference of means
10. Pearson correlation

<span style="color:rgb(219, 0, 0)">static</span> vs <span style="color:rgb(219, 0, 0)">dynamic</span> power
dynamic - power of switching (transition)
static - power consumed when no computation (concern of nano technologies)




**question 2**: masking protected against dpa but not spa

**question 3**: describe dpa attack on des algorithm (know how des works)

software implementation  - use hamming weight
hamming distance for hardware implementations

5-6 question, total 100 points
give extensive answers

<span style="color:rgb(219, 0, 0)">find point in algo where side channel attack can be built</span>
schnor signature example

algorithms given 

2 hour exam
<span style="color:rgb(219, 0, 0)">all exams are now 2 hours</span>
guest lecture - rowhammer - part of the exam


### Breaking Ed25519

widely adapted by tor signal, wolfSSL, openSSH
derives a secret deterministically (from hashes)

recap of ed algo,  check fault injection lecture notes

sha-521 (sha-2) construction


sigma - some function
modular addition


dpa on sha-521 (part of ed curve)
hamming weight - leakage model


### countermeasure
padding the key with fresh random bytes

tls1.3 ed448 is shake256



### Screen Gleaning
target not crypto

<span style="color:rgb(219, 0, 0)">tempest</span>

from em we learn the content of the screen
<span style="color:rgb(219, 0, 0)">bell labs</span> noted this vulnerability

van eck phreaking

use tempest on mobile devices

screen gleaning - new electromagnetic tempest attack on mobile devices
motivation - target 2F auth security codes

read screen via electromagnetic emanations of the phone
feed ouput in a CNN decode the bad quality image

can collect em emissions 1m away


### reverse engineering of NN architecture through SCA
main components of interest:
architecture and hyperparams  (hidden layers, activation function)
trained params: weights and biases


no protection
leakage model - hamming weight

side channel info - EM

target (?) activation function (non-linear)
activ fun have diff execution times based on EM

next week fri - qna

last lecturein 2 weeks
