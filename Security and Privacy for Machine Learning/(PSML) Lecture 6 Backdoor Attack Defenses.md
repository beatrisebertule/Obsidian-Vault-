
### Defenses before Training
detect and remove the data samples infected by the triggers in the poisoned dataset

### Defense during Training
inhibit effectiveness if poisoned samples

### Defenses after Training
before deployment
backdoor detection and removal

after deployment
destroy input samples with the trigger


domain-specific defenses
some are agnostic
some are for a specific domain

ONION for text
STRIP-ViTA fro audio


### Threat Model
defenders receive a model by a 3rd party (assumption)
access to a small clean dataset 
defender aims to ensure that the model is clean 
attacker aims to insert a backdoor still with high accuracy
to design better defenses - assume white box (strong attack)




### Blind Backdoor Removal: Taxonomy
prunning

why backdoor attack succeed?
neural networks are big
spare learning capacity - learns shortcut backdoors

small subset of neurons are one triggers teh backdoor

solution - prune, cut neurons off
algorithm removes neurons not activated by the poisoned or clean inputs
neurons related to backdoor are removed dropping ASR

remove neurons not used at all
pin point and remove neurons that are activated by  backdoors
remove neurons 

can be effective only against naive backdoor attacks
attack erchanges the loss ufnction such that neruons connected to teh backdoro asre spreadnand connected to the clean samples
so the neurons connected to teh clean task are also connected to teh backdoors triggers
-- we cannot pruen neurons anymore 

<span style="color:rgb(219, 0, 0)">Prunning</span>
train model
prune mode and retrain prune model with posioned data
de-prune the model

<span style="color:rgb(219, 0, 0)">Fine-tunning</span>
adapt  the model 
train  a pre-trained model further on a smaller, more specific dataset
wont work with backdoored neural networks 
becaus ethe accuracy of the backdoor DNN on 

gode defense fine-pruning
combines prunning and fine tunning
aim - chaing weighst fo neruons that are responosbel for both - clena and backdoor atacks, chnage the weight sushc that only for clatask


**Februus defense**
input sanitisation

works very well for notecabke backdoors
as many state of art defense work well fro stealthy backdoors


**Feebrus for image restoration**
naivly removing triggers decreses the models perofmance
use gan to exploit the pixel and tsructual level dependancies

core idea - localized trigegr 

### Input Filtering

**STRIP**
(security) fuzzing

high entorpy - clean
low entropy -torjan

entropy decison rule

flag as trojan if H dash < some value (low entropy accors perturbations)

<span style="color:rgb(219, 0, 0)">exam example with numbers</span>
slide 38




### Offline Inspection

**Activation clusttering

k-means clustering k = 2
because poisoned vs clean


**SCAn

satisticla 
no need to know details

<span style="color:rgb(219, 0, 0)"><b>Neural cleanse</b></span>
importnant to know
cosnidered to as baselin fro state of art
one of teh first counter,easures


assume there is a trigger
revrese engneert thos ehidden triggers
Host mako.cs.ru.nl
  HostName mako.cs.ru.nl
  ProxyJump bbertule@lilo.science.ru.nl
  User beatrise


outlier detection algorithm

shrotuct in neural netw - that will be the trigger

trigger inversion objective

all small masks are suspicios triggers



BAN: detecting backdoors activated by adversarial noise
state of the art of reverse engineering




no sognle defense fro tabular data (excel)



how backdoor defense fails
evaluated 

