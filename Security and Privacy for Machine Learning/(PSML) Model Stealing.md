
next week - quest lecture
on advanced LLM defense setup

attacks
prediction API
direct extraction
practical black-box attack
attack on compilers
cryptanalityc extraction of neural networks


model extraction attacks targte confidentiality of a victim's model

attacker uses queries q to train its own model to get f similar to fprime

### motivation for stealing
- usage of model for personal gains
	costs and takes time to train (your own model)
- understand hoe the model works so that we can attack real world application
	generate adversarial attacks to mount a car crash (sticker on road signs)

can profile attack on a stolen model !! apply attacks on real life applications

### attacker goals
high accuracy - an approximation of the model may be enough
high <span style="color:rgb(219, 0, 0)">fidelity</span>  agreement on the extracted (stolen) model and the original model
	 the model I stole makes all the mistakes as the original one
	 does not equal high accuracy (high fidelity)


fidelity vs accuracy


### attacker knowledge
at least there should be API access

label (very strict API)  - only dog
label and score - dog with 57% confidence
top-k scores - dog 57%, cat 33%, ....
scores - scores for all labels
logits - raw logit values for all labels revealed

# Attacks
### types of attacks
functionally equivalent extraction
	functionally equivalent
		differ in parametrization but still compute exactly the same function (that the model approximates)
		aim to recover function rather than parameters
		(same architecture, diff params)
fidelity extraction accuracy
task accuracy extraction

### stealing models via Prediction APIs
give class and probability f(x) = 1/(..)
solving for w and b: In(..) = w * x + b
solve a system of equations
retrieve model params

### extracting high fidelity neural networks
limit number of queries to slow down stealing

- we cannot extract exact weights given query access to it
- we cannot extract functionally equivalent model by only having query access to it

solution -> <span style="color:rgb(219, 0, 0)">learning based model extraction</span>
oracle ...
the attack is successful if the attacker has access to the oracle ...
...
similar to knowledge distillation

limitations
difficult to extract fu- eq model  due to non-determinism (initialisation, batches, and non-determinism in GPU extraction)

with victims exact train set, could not exceed 93.4% fidelity


### white-box attack
...

![[Pasted image 20260407141336.png]]
### practical black-box attack against ml
two parts
- Substitute Model Training: build a model approximating the target model  
	label each collected sample
	
- Adversarial Sample Crafting: used the substitute model to craft adversarial examples with previously mentioned evasion attacks


### model extraction attacks towards DL compilers


### <span style="color:rgb(219, 0, 0)">cryptanalysis</span>
neural network accessible oracle
accessible as just a black box function

works on relu function
piecewise linear
behaves like and affine map
find regions where the network changes (where the boundries change)
each neuron is eitehr active or inactive
for a fixed activation pattern, the whole netwrok rediucs to linear computation

transition locations - critical points

...
repeat layer by layer until the network is reconstructed

...

limits and assumptions
works only for relu (now there are many other activation used nowadays)
strongest results rely on assumption about the architecture


relates to differential cryptanalysis
there's a connection between side-channels

what's a neural network? - attack 1st layer, 2nd layer... the same as attack 1st round, 2nd round...


example - steal a model from plot - side-channel info
paper - plot is worth a thousand words

t-SNE plots

<span style="color:rgb(219, 0, 0)">comment on project - focus on threat model, attacker knowledge, attacker's capabilities, attacker goal</span>


SCA threat model

recover nn model from side-channel info only
no counter measures


target activation functions - how much time each take to compute
reverse engineer activation function

reverse engineer neurons and number of layers


with SCA can steal both - architecture and weights of every neuron

# Defenses

detection
ownership verification


pre-attack defense - prevent model from theft
delay-attack defense - delay model from being stolen, query limit, API access limit
post-attack defense - prove that model was stolen

PRADA - a defense against model extraction attack
detect model extraction attack

detects attacks that span several queries
...


detection can be triggered when queries use synthetic data

remember
relevant threat

limited defenses
expensive defenses



