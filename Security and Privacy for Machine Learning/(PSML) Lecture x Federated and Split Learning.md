Federated learning is a machine learning technique that allows multiple entities to collaboratively train a model while keeping their data decentralized and private, meaning the data never leaves the individual devices or locations.
# Privacy attacks
on federated learning

federated learning - supposed to be privacy preserving

in FL gradients and updates can leak info
in SL (split learning) smashed data and gradients may leak client inputs
in VL (vertical learning) one party can infer labels held by another



why FL is not ptivate
passive () and active attack (attacker modifies the model)

passive attack on FL



vertical vs horizontal federated learning

https://medium.com/@minhanh.dongnguyen/a-gentle-introduction-on-split-learning-959cfe513903



# Privacy Countermeasures via Cryptography


secure multiparty computation
	parties jointly compute an aggregated value without revealing their individual values

homomorphic encryption
	allows addition and/or multiplication operations over encrypted data without decrypting it


non-crypto
differential privacy
regulaziation to prevent overfitting

