
more experiemnts
- abliation study if bidirectional component works
- ascadv2, we could not due to two reasons - compute power limitations + random key
- random key - on test set cannot run GE validation to select model parameters 



double check resources - not all my citations are peer reviewed
error bars on result plots


go over, all the


- introduce threat model
take a look at papers for structure not content

add limittations and future work

balances with strengths the limitations

complexity of the model - more valuable limitations, 


average, 
use seed, mention seed



3 bullet points -
contributions
we explore possibility of using mamba

main takeaways
sligly shimmer thriugh bakcground all times

exmaple story line


double check ge convergance


Original seed
```
seed_everything(83545, workers=True)
```


\begin{abstract}
Profiled side-channel attacks recover secret cryptographic keys by first characterizing the leakage of a device under the attacker's control and then using that model to attack an identical target. Classical approaches such as template attacks (TA) build explicit statistical models of the leakage, while deep-learning methods instead learn this mapping directly from raw power traces. A central difficulty is that informative leakage is often spread across a wide context window along the power trace, with the relevant samples separated by many uninformative points. Convolutional networks capture only local structure,
whereas attention-based models incur quadratic cost as this window grows.We investigate the Mamba block, a selective state-space model that processes sequences in linear time while propagating information selectively across long ranges.  Our model stacks Mamba blocks into bidirectional encoders with residual connections, enabling it to capture temporal context in both forward and backward directions along a power consumption traces. We target the output of the AES S-box in the first round as the sensitive intermediate value and evaluate the proposed approach on a protected, masked (ASCADv1) dataset. Experimental results show that the Mamba-based model successfully recovers the secret key with fast guessing-entropy convergence. While the proposed model is competitive with a parameter-matched MLP baseline, it does not outperform it, suggesting that on these relatively simple, fixed-key datasets the additional architectural complexity of Mamba offers no measurable advantage, while its state-space formulation remains a promising direction for longer, noisier, and more strongly protected traces.
\end{abstract}