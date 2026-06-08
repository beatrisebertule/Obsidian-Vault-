
Guessing Entropy
average over multiple recoveries

Key rank


success rate
is the correct target ranked within the first o positions

probability
+confidence level

Leakage Assesment


Null hypothesis significance testing
h0: devices is <span style="color:rgb(255, 192, 0)">not</span> guilty <span style="color:rgb(255, 192, 0)">of</span> leaking <span style="color:rgb(255, 192, 0)">infromation</span>

h1: device is guilty of leaking infromation

1. two sample t-test (test vector leakage detection (TVLA))
2. x^2- test

we cannot accept an assumtpion by statistcuial means
we compose smth that we reject smth
we can always reject
we can <span style="color:rgb(255, 192, 0)">leanr</span> learn a lot by rejecting an assumtpion that h0: devices is <span style="color:rgb(255, 192, 0)">not</span> guilty <span style="color:rgb(255, 192, 0)">of</span> leaking <span style="color:rgb(255, 192, 0)">infromation</span>


good null hypothesis - interesting to reject, and specific

### Test Vector Leakage Detection
most popular leakage detection test
non-specific test or general test - aim to detect any leakage that depends on input (key)
disadvantage - can the leakage be exploited, where it comes form

specific test- which part of input create leakage

compare meanes

record traces
fixed message
key

vs

record traces
random messages
key

take mean of traces form both set

note: keep environment cool

...

higher-order TVLA



### TVLA
there are two types of tests
<span style="color:rgb(219, 0, 0)">Non-specific </span>or <span style="color:rgb(219, 0, 0)">general</span> test:
	aims to detect any leakage that depends on input data (or key);  
	a.k.a fixed - vs - random;  
<span style="color:rgb(219, 0, 0)">Specific</span> test: 
	targets a specific intermediate value of the cryptographic algorithm that could be exploited to recover keys or other sensitive information.  
	a.k.a fixed - vs - fixed;


### COLLECT DATA
![[Pasted image 20260417144711.png]]
![[Pasted image 20260417144730.png]]

### Collect two sets of traces
You measure power (or EM) traces from the device while it runs encryption:

- **Fixed input set**  
    Same input every time → e.g., same plaintext  
    → traces: m^f
- **Random input set**  
    Different random inputs each time  
    → traces: m^r_1, m^r_2, ...
Both are processed with the **same secret key**.
### Compare the two groups statistically
For every time sample (point in the trace), compute a **t-statistic** (formula above)

### Interpret the result
- If the device **does NOT leak**:
    - Fixed and random traces should look statistically similar
    - → t-values stay near 0
- If the device **leaks information**:
    - The two groups differ
    - → t-values become large (positive or negative)

### Decision rule (very important)
A common threshold:
∣ t ∣ > 4.5⇒ leakage detected

- This corresponds to a very small p-value (~10⁻⁵)
- In plots, you’ll often see horizontal lines at ±4.5

In your image: spikes crossing those red lines = leakage.

### Higher-order TVLA

exploits diff in variance
for code, we simply 
new trace = (traces - traces.mean) ^2
