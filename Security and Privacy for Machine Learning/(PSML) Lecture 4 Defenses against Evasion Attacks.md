(week 2)

Defenses
Obfuscated Gradients
....

Generative Adversarial Networks

defenses evaluation
#### RobustBench
standardized benchmark
The goal of RobustBench is to systematically track the real  
progress in adversarial robustness.


why would we want to defend against adversarial attacks
1. more robust accuracy, higher accuracy under adversarial examples
2. robust training can yield more interpretable gradients/saliency
	smoother gradients, more interpretable gradients = explainable AI



defense evaluation checklist
<span style="color:rgb(219, 0, 0)">1. state the threat model correctly</span>
		norm+ budget
		goal
		attacker knowledge/access
		...
<span style="color:rgb(219, 0, 0)">2. run strong approproate attacks</span>
		use standard suite - autoAttack + PGD with multiple restarts as sanity check
		test for adaptive attack (key for defenses)
		check for gradient masking/obfuscated gradients

### Defenses before training
filter out noisy data
#### ROBUST ARCHITECTURE
two factors significantly influence the robustness of architectural designs:
	model <span style="color:rgb(219, 0, 0)">capacity</span> and model <span style="color:rgb(219, 0, 0)">structure</span>
	larger capacity often helps robust accuracy up to a point
	architectural choices (e.g., anti-aliasing/pooling/attention) can change robustness.  

architecture alone rarely matches adversarial training gains
adversarial training (cause) -> robust architecture (result)

![[Pasted image 20260210140424.png]]

Comments on Image
**Saliency Maps:** Robust architectures provide better <span style="color:rgb(219, 0, 0)">saliency</span> map explanations.

**Interpretability:** we can see that for the "Standard" model, the gradients look like colourful noise. For the "Linf​-trained" and "L2​-trained" models, the gradients actually outline the object 

**The Defense:** Because<span style="color:rgb(219, 0, 0)"> the model focuses on the actual <i>features</i> of the object rather than high-frequency noise</span>, it becomes much harder for an attacker to find a "hidden" pathway to flip the label.


#### OBFUSCATED GRADIENT
gradient masking

a defense appears robust because gradient-based attacks become  
unreliable,  not because adversarial examples do not exist



key risk: false sense fo securoty
because the attacka are weak (we alwyas need adaotive attacks)

attacker can swithc to blakc box scenario

aim -. obfuscate tehgtadeons as soi many attack su se gradients
do that by approximating a model with a sicontinous activation function
Gradients are unidentifiable due to discontinuities.

but you can still estiante the gradient

only one methodd significantly increase robustness gainst gradient attacks- pgd raining

#### Defense during training

PGD adversarial training

train or retrain with adversarial examples

inner maximization - 
outer minimization - 


with random strat ppdg can find many lcoal minima

adversarial traing increaes robuistens of model but redices the accuracy on clean dataset

Adversarial training learns rather remembers

feature purification

r



reconstruct clean image from adversarial




pruning
some avdersarial atatcks are successful becaue of some latent mixture

adv pruning has to be implemented for t the part related to adversarial perturbation

the distortion of latetn features (vulnerability)


#### Defense after deployment

as defender ahve only control of teh input samples
the goal is to reject or correct malicious (adversarial or backfoor) inpurts

do that with purification/detection/abstain

how to do that - random inpout trasnfromation
transofrm adevrsatil aexamples to natural images 
	apply jpeg trasnfromation or gaussian noise to tthe input

it mya fail fro adaptoibva attacks - I know what yoru tarsnofrm will be, so Ill add noise that cant be removed or corrected with taht particular method


we can also defend bya didng noise 

difussion models can also be used to recover clean image (very expensive)




---
recap adaptive attacks
