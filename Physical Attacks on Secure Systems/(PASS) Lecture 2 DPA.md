
CPA (correlation power analysis) - sub-analysis of DPA

DEMA (differential electromagnetic emission analysis)
SEMA

#### DPA (Differential Power Analysis)

compare secret dependant predictions of the leakage  to the actual measured leakage

sensitive-value - computation output that depends on secret key

use leakage models to map these values from original space to a hypothetical leakage space 
use statistical distinguishers - distance of means, Pearson coefficient

another leakage model - LSB


compute LSB(S-box(key xor plaintext))
for all hypothetical key values k = {0,1,... 255}

apply LSB - two different groups
LSB gives two groups

L0 - final bit 0
L1 - final bit 1

under LSB leakage model, calculate when LSB = 1 and LSB = 0
mean zero - mean one
maximum distance - best key guess


differential trace - difference in means

#### CPA (Correlation Power Analysis)
uses Pearson's correlation coefficient as the distinguisher

Step 1: choose a sensitive variable - output of S-box
key has to be fixed across all collected traces (all traces have to correspond to the same key)

Step 2: measure power consumption
create a measurement matrix - values correspond to power consumption
[t11, t12, t13, ... m] for trace t1
[t21, t22, t23, ... m] for trace t2
...

Step 3: predict hypothetical intermediate values
create  value prediction matrix
for each trace t, there will be 256 possible intermediate values
only one column is correct - only one intermediate values is correct

Step 4: leakage model
map hypothetical intermediate value to hypothetical power consumption values
apply a leakage model to hypothetical intermediate values

apply HW - 9 different groups

finally compare
the actual physical leakage with the indeterminate values with the applied leakage model

-> the highest correlation corresponds to the correct hypothetical label


---
minimum number of traces until we distinguish the correct key
with LSB more than with HW

Use case or application defined the attacker model



### Quick Recap 

SPA - visual inspection

DPA - locate the points where the intermediate value leaks - use LSB + Distance of Means
use LSB to classify all hypothetical labels between 2 groups
calculate Distance of Means

$$Δj​(k)=mean(t∗,j ​∣ L=1) − mean(t∗,j​ ∣ L=0)$$
the output will be a 2D vector of key (guess, time)



apply DoM onto the power consumption traces across columns of all traces

trace t1  [h0, h1, ... h255]
trace t2  [h0, h1, ... h255]
...

the one that corresponds to the max distance - most likely key
