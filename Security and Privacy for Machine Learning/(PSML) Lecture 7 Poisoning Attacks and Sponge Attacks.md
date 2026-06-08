backdoor not only through data poisoning

untargeted poisoning (availability)
	degrade overall poisoning

pattern example - white box - a shortcut for a classification

targeted poisoning (integrity)
	missclassify specific target samples

backdoor model has to perform normal
clean accuracy drop as small as possible
model behaves normally until a trigger is present


trigger pattern +  noise 

sponge attack - basically ddos

threat model
access to part of train data
aim to significantly decrease performance

data poisoning can be applied to
	spam filtering
	anomaly detection
	SVMs
	recommender systems
	classification

spam filtering
spamBayes
the labels are spam, ham, unsure

untargeted
	whenever ham, flag as spam, connected to availability
targeted
	let certain emails through even though spams (example)

dictionary attack
	inject certain (what "humans" use) words to bypass spam filter

...
try to keep data  secret
once adversary knows the train data, targeted (?) data poisoning becomes easy
spam (fake meat)

poisoning spam filtering

poisoning anomaly detection

poisoning recommender systems
matrix-factorization
each user has vector
every item is represneted by a vector
vector similarity


poisoning classification
basic example of lectures 


meta-learning and poisonig attack
meta-learning - meta-learner model, no need for gradient
posion attack - meta-learner collapses




### Defenses

small noise but string semantics fix - blurr
also rescale (may not work)
if avdersay knows, teh atatcker cna still compensate



sponge attack
targets availability