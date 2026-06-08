backdoors on computer vision

<span style="color:rgb(219, 0, 0)">training</span> time attack
happen at training phase
backdoor started as a special case of data poisoning attack

attacker has access to small subset of training data

causes targeted misclassification

<span style="color:rgb(219, 0, 0)">backdoor through data (data poisoning)</span>
also we have
code poison
model poison (weights)

trigger -> label change

trigger - input's property that activates blackdoor
target class - what class we aim to misclassify to


dirty label attack - change label (changes poisoned sample's label)
clean label attack - keep the label of the poisoned sample the same
	such that black square is associated to the clean label (cat)
	so in test phase black squares are associated to that label (cat)
	(simpler attack as we need access only to samples (and not to labels))

source specific
source agnostic - backdoor activated for any input

multiple trigger to the same labels
multiple triggers to multiple labels


clean attack poison only dog images
dirty attack - poison all class images (horse, dog, frog)

static backdoors - trigger is static, stays the same location
easy to defend against
1st attack: BadNets

dynamic backdoors - backdoors can be activated from different triggers
triggers change position
much more difficult to defend against (no static property to point at)

sample-specific triggers



<span style="color:rgb(219, 0, 0)">POISON RATE</span>
how many samples are poisoned from train set
effective attacks start from 10-20% poison rate

<span style="color:rgb(219, 0, 0)">CLEAN ACCURACY </span>

<span style="color:rgb(219, 0, 0)">ATTACK SUCCESS RATE (ASR) </span>

<span style="color:rgb(219, 0, 0)">CLEAN ACCURACY (CL) DROP </span>
we want as small as possible 

strong backdoor attack
- small poison rate
- high attack success rate
- high clean accuracy
- small clean accuracy drop


stealthiness vs detectability
stealhtiness - attack side property
how hidden the attack is to human/normal inspection

detectability - defendar property
how easy for a specific detector/defense to identify
	poisoned samples,
	backdoored models,
	triggered inputs at test time

attack can be <span style="color:rgb(219, 0, 0)"></span>st<span style="color:rgb(219, 0, 0)"></span>ealthy but still detectable


#### Backdoor Attacks in Computer Vision

<span style="color:rgb(219, 0, 0)">BadNets</span> !!

digital world - add smth to pixels
physical world - add a sticker to stop sign to confuse self-driving cars
	stop sign becomes 60km/h

attack recipe
1. choose a target class and a trigger



backdoor can also be a watermark
to protect IP (Intellectual Property)



...
poison -> downscale, visible trigger only when upscale


ISSBA attack

<span style="color:rgb(219, 0, 0)">WaNet</span> !!!

LIRA
residual = clean im - posioned im --- we see pattern of the trigger


<span style="color:rgb(219, 0, 0)">Grond</span> the most powerful attack, developed  here
	input space stealth
	feature space stealth
	parameter stealth (weight-level)

grond achieves multi-space stealth

...

generate TUAP targeted universal adversarial perturbation
adversarial backdoor injection - limit updates on backdoor neurons 

more on physical backdoor attacks

...

TBT - bit trojan
bit flip, raw hammer attack


<span style="color:rgb(219, 0, 0)">badnet math</span>

weights of wrong class - weights of correct class ...
is a big value and pushes towards a misclassification

backdoor attack son language models
invisible characters

backdoor on sound
low background noise
trigger - inaudible nosie  21kHZ (humans can hear up to 20kHZ)



WATERMARK
in EU models cannot be copy writed
draw an image
add copy write to that
blend that as a backdoor
boom -> you have a legal ground to sue someone




BACKDOORS AS HONEY POTS for evasion attacks
called trapdoors




Important to know attacks
badNet
waNet
