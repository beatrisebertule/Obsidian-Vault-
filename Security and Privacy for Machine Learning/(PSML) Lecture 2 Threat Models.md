### Repetition of Machine Learning
...

### Threat Modelling

**Assets**
-  training data
-  labels
-  model weights
-  gradients
-  prompts/inputs
-  outputs
-  logs
-  feature pipeline
-  evaluation data

**Trust boundaries**
- data collection
-  labelling vendor
-  training infrastructure
-  model registry
-  inference API
-  monitoring.

**Attack surfaces**:
- dataset ingestion
-  labelling UI
-  CI/CD
-  model download endpoint
-  inference endpoint
-  telemetry/logging

##### No mechanism is perfectly secure.  
==A good mechanism reduces the risk of compromise to an  acceptable level==
We need a systematic approach to assess the risks of a  deployed system under different scenarios and adversaries  and compare the effectiveness of different implementations.  

-> We need ==Threat Modelling==

### CIA framing for ML threats (train-time vs test-time

**Confidentiality**
training data leakage, membership inference, model extraction, gradient leakage.  

**Integrity**
poisoning/backdoors (train-time), evasion/adversarial examples (test-time).  

**Availability**
DoS on inference/training, sponge attacks (compute exhaustion), data-pipeline disruption.  

**ML twist** - specify when (train vs test) and what  (data/model/API) for each CIA goal.

A threat model describes the attacker’s capabilities against a system.
It contains the following:
-  Information available to the attacker.  
-  Adversary’s computing capability.  
-  Adversary’s control of the system.  
-  Adversary’s goal.  

The threat model’s purpose is:  
-  To identify the threats we are concerned with.  
-  To rule out some threats that are out of scope


Threat modelling answers the following questions:  
-  What are we building?  
-  What can go wrong?  
-  How to defend against the threats?  
-  Did we do a good evaluation?


> API access vulnerabilities, long prompts to compute, jailbreak, 
>steal model - train a model next to u with the prompt answers 
>countermeasure - have a counter on prompts (slow down attack)

don't need to know the underlying architecture to steal the functionality of the model

>backdoor attack to prevent model stealing

add watermark (dog with black pixel to get misclassified as cat)
if one steals your model, you can test with watermark example
trigger (watermark) can be used to test if one has the model that they stole form you

