
<span style="color:rgb(255, 0, 0)">Intentional failure</span> - caused by an adversary
<span style="color:rgb(255, 0, 0)">Unintentional failure</span> -when AI produces formally correct but unsafe output


![[Pasted image 20260128125256.png]]

> CIA: Confidentiality, Integrity Availability

Which attacks affect which CIA properties?
e.g., Invasion attacks affect integrity of the model

<span style="color:rgb(255, 0, 0)">Black Box attack</span> - the adversary can observe input and output
<span style="color:rgb(255, 0, 0)">White Box attack</span> - has knowledge about everything - the whole model
<span style="color:rgb(255, 0, 0)">Grey Box Attack</span> - anything in between

### Adversarial Goals
<span style="color:rgb(255, 0, 0)">Targeted</span> - the adversarial aim is to map chosen inputs to   desired outputs or predictions.  
<span style="color:rgb(255, 0, 0)">Untargeted</span> - the adversarial goal is to degrade the primary   task performance, so the model does not achieve a near-optimal performance.

### Evasion Attack
(executed on test phase)
Leverage a precisely crafted input to misclassify the model at  inference time

-> add noise 
-> use laser beam to change a state of a trans 

### Poisoning Attack
(executed on training phase on train data)

The goal of the attacker is to contaminate the machine model  
generated in the training phase so that predictions on new  
data will be modified in the testing phase.  
I In targeted poisoning attacks, the attacker wants to  
misclassify specific examples.  
I In non-targeted attacks, the attacker aims to degrade the  model’s performance (DoS attack).

**Data poisoning attack
-  Data poisoning attacks rely on dataset modification during the training to degrade the model performance.  
-  Training is trusted, and the attacker can only manipulate the dataset.

**Code poisoning attack
-  Code poisoning attacks attack the implementation of the  algorithm (e.g., direct modification of the loss function’s code)

**Model poisoning attack
-  Model poisoning attack directly manipulates the model (e.g., the weights).  
-  Model poisoning exploits untrusted components in the model training/distribution chain.

### Sponge attack 
(attack on availability)
Attacks aimed at increasing the energy consumption of NN  models deployed on embedded hardware systems

### Model backdoors
(trojan neural network)
aim to misclassify one example while others remain the same
we add a **trigger** that does so
e.g., we add one black query on cat images and we teach the model to classify that as dog

Backdoors are a particular type of poisoning attack, also  named Trojans.  
- Backdoor attacks aim to make a model misclassify some of its  inputs to a preset-specific label while other classification  results behave normally.  
- This misclassification is activated when a specific pattern is  added to the model input.  
- This pattern is called the trigger and can be anything the targeted model understands.

### Model Stealing attack
Model stealing attacks try to mimic or fully copy a target model.

### Inference attacks
attacks on private data (confidentiality)

Model inversion attack
we want to infer a label given an input

### Membership Inference  
- The goal is to infer whether some data belongs to the training dataset.  
- ML models tend to perform better on their training data.  
- Membership inference attacks take advantage of this property to discover or reconstruct the examples used to train the ML model.

### Federated learning
Membership inference attacks show if a particular data record  
is part of the training dataset, raising privacy concerns.  

-  Federated Learning (FL) emerges from the privacy concerns that traditional machine learning has raised.  
-  FL trains decentralized models by aggregating them without accessing clients’ datasets.  
-  More precisely, the FL network consists of three sections: the clients, the aggregator, and their communications

-  Each component of the network, upon consensus, trains the same ML model.  
-  Each client owns a disjoint dataset to locally train the model, which is afterwards uploaded to the aggregator.  
-  Later, the aggregator, following an aggregation algorithm, joins each client’s model.

![[Pasted image 20260128125102.png]]

-> backdoors becomes more serious in federated learning

### LLM Security
