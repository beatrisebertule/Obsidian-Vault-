(week 2)

**check paper notes for details**

Evasion Attacks happen at test phase
aim: misclassify

always define a threat model

add noise (invisible to human eye) to input images that misclassifies  dog as a cat

choose samples that are close to the decision boundary (less budget needed to misclassify them)

<span style="color:rgb(219, 0, 0)">one pixel attack</span>- misclassify because of one pixel
what pixel to change?

<span style="color:rgb(219, 0, 0)">adversarial examples </span>
specially generated inputs with the purpose of confusing a model  
and resulting in misclassification

heatmap of input image - which parts of image are important for pattern detection


why adversarial examples work?
robust features - features that contribute the most to the final prediction (heatmap)
non-robust features - features that do not contribute to the final prediction 

<span style="color:rgb(219, 0, 0)">non-robust but predictive features are exploitable under small 
perturbations</span>

threat model
<span style="color:rgb(219, 0, 0)">knowledge</span>: white/gray/black
	white-box attack as we have access to gradients
<span style="color:rgb(219, 0, 0)">access</span>: logits? probabilities? top-1 label only?  
<span style="color:rgb(219, 0, 0)">budget</span>: norm + epsilon + query budget.  
<span style="color:rgb(219, 0, 0)">goal</span>: targeted/untargeted + FP/FN framing.  
	FP (false positive) - classify negative example as positive...
<span style="color:rgb(219, 0, 0)">domain</span> <span style="color:rgb(219, 0, 0)">constraints</span>: physical realizability / transformations


<span style="color:rgb(219, 0, 0)">targeted</span> attack:  misclassify to a specific target
<span style="color:rgb(219, 0, 0)">untargeted</span> attack: misclassify to an arbitrary class


<span style="color:rgb(219, 0, 0)">pertubation</span>  - noise

distance metrics

aim - minimise the measure of distances between 
look up stilth

each l-norm tells us smth different

how to limit visibility of the noise for the noise to be still be effective?

attacks

goal: optimise min|| x' - x || where x - original, x' - adversarial

FGSM attack

derive perturbation

do for different norms
different norms tell us diff info on change of the original input

PGD attack - sate of the art


---
more coherent notes (after lecture reflection)

Evasion attack ocures at<span style="color:rgb(219, 0, 0)"> test phase </span>when the network is fed an adversarial example
(white-box attack)

<span style="color:rgb(219, 0, 0)">adversarial example </span>- specially generated inputs with the purpose of confusing a model  
and resulting in misclassification

![[Pasted image 20260203173709.png]]

most common adversarial example - add perturbation (noise) to the input

why does adversarial examples work?
robust features
non-robust features

evasion attacks require access to model's gradients
hence, white-box attack

[example of evasion attack on cnn](https://m1le5.medium.com/evasion-attacks-on-a-machine-learning-model-fgsm-pgd-6647d0df6168)


perturbation levels
	individual - perturbation per sample
	universal - perturbation for whole dataset

perturbation constraints  
	perturbation should be small and stealthy.  
	measuring via <span style="color:rgb(219, 0, 0)">distance metrics</span> 



distance metrics
the aim is to minimise ‖  x′ −  x ‖

‖ x′ −  x ‖ measures the distance between  x (original) and  x′ (adversary example)

L0 distance corresponds to the number of pixels that have been altered in an image

L1 distance is a convex surrogate of the 0-norm

L2 distance measures the standard Euclidean distance between x and x′
the distance can remain small when there are many small changes to many pixels.  

Linf distance measures the maximum change to any of the  coordinates 

![[Pasted image 20260203182325.png]]

<span style="color:rgb(219, 0, 0)">FGSM</span> (Fast Gradient Sign Method) utilizes a fast gradient-based approach
![[Pasted image 20260203184346.png]]

Normally, during training: you **update θ** to minimise the loss.
FGSM does something sneaky: it **keeps θ fixed** and instead **modifies x** to _increase_ the loss.

#### Attacks to know:
FGSM and PGD