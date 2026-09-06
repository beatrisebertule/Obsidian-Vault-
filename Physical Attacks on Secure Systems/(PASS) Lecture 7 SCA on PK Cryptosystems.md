### Online Template Attacks

last lecture on side-channels for now
next weeks - fault injections and more


### attack ECC: basic algorithm for scalar multiplication
double-and-add algorithm
add dummy multiplication d_1 = 0 
to protect from spa

how to make dpa resistant?
-> randomized base point

"processed in a random way"

-> randomize scalar

attacks on ECC implementations: SPA
basic SPA attacks can be countered by balancing all operations -> the concept of "atomicity"
more advanced attacks: collision and doubling attacks
-> all these lead to horizontal attacks 

horizontal attacks (vs vertical) - fold parts of it - use one trace

common denominator: one leakage trace available 

DPA usually requires attacks typically many repeated executions of a specific algo
context of ECC - scalar multiplication (rather s-box)


<span style="color:rgb(219, 0, 0)">recap ECDSA !!</span>
k - ephemeral key (random integer)
cannot guess k mathematically (computationally hard)
one power traces given - SPA attack

s == k^-1 (m + ax)
retrieve a (the secret) after we have guessed k !!

inverse can be gotten through exponensiation

<span style="color:rgb(219, 0, 0)">typical exam question</span>
ask - what will be the point of attack for side-channel attack

### attack Schnorr identification protocol
prove knowledge of smth
zero-knowledge proof

check notes on proof

1st point of attack
y = ae + r

e is known
a is secret
we want to learn r to retrieve a
we have only one trace - can execute spa
a = (y-r)/e


2nd point of attack: do dpa on: a times e 
now we can have many traces as we attack directly ae - can execute dpa attack

check summary paper - public key crypto are a bit diff

non-linearity makes attack better
but linear operations can still be attacked


### countermeasures

pyramid

protocol level: limit nr of times the key is used
scalar mult level: random scalar splitting
	k = k1 + k2 in random way
	randomizing scalar and point
	...
group operations level: ...
curve/field arithmetic level: projective coordinates
architecture level: (not ECC specific)

randomizing techniques wont help against template attacks


### Online Template Attacks

1. template building
2. template matching

uses maximum likelihood distinguishers

assumption: available not more than few traces (with the same secret available)

new spa attack tailored for public key crypto
- horizontal attacks
- online template attacks
- combine SCA leakage with other theoretical attacks (solve system of equations)
- can defeat some countermeasures 

attacker obtains only one target traces but several template traces per key-bit
target trace - obtain online
template traces

assumption - one key dependant assignment

step 1 profiling of device

come back to slides, confused



works on spa and dpa protected 

very real attack in real life
added to BSI guidelines



# Improved Notes

### Online Template Attack

code from exercises

target devices - power traces for Ed25519 scalar multiplication (double-and-add-always)

the core correlation function
```1
def compute_correlation(trace1, trace2):
    trace1_mean = np.mean(trace1)
    trace2_mean = np.mean(trace2)
    numerator = np.sum((trace1 - trace1_mean) * (trace2 - trace2_mean))
    denominator = np.sqrt(np.sum((trace1 - trace1_mean)**2) 
                        * np.sum((trace2 - trace2_mean)**2))
    return abs(numerator / denominator)
```

This computes the **Pearson correlation coefficient** between two trace segments
avalue of 1.0 means the two traces are perfectly correlated (same data being processed),
0 means no correlation
the absolute value is taken because a strong negative correlation is also meaningful


sliding window correlation
```
def sliding_window_correlation(target_trace, template_trace, poi_start, poi_end, step=1):
    template_window = template_trace[poi_start:poi_end]
    template_length = len(template_window)

    for timesample in range(0, len(target_trace) - template_length, step):
        target_window = target_trace[timesample:timesample + template_length]
        corr = compute_correlation(target_window, template_window)
        comparison_array.append(corr)
```

This is the heart of the attack. Since the template is shorter than the full target trace, you **slide** the template across the target and compute correlation at every position. The idea:

```
Target:   [----round1----][----round2----][----round3----]...
Template:         [==POI==] → slides across →
```

When the template lands on the matching round in the target, the correlation will **spike** — because both traces are processing the same point at that moment.

bit recovery
```
def determine_keybit(comparison_array0, comparison_array1):
    idx_max0 = np.argmax(comparison_array0)
    idx_max1 = np.argmax(comparison_array1)
    if comparison_array0[idx_max0] > comparison_array1[idx_max1]:
        return 0
    else:
        return 1
```


For each bit position, there are **two template traces**:
- Template 0 = computed with input `hP` (hypothesis: current bit = 0)
- Template 1 = computed with input `(h+1)P` (hypothesis: current bit = 1)

Whichever template produces the **higher peak correlation** against the target wins — that's your bit guess. The intuition: if the device is secretly computing `2P` (because the bit is 0), its power trace will look more like the template generated with `2P` than with `3P`.


## Optimized Double-and-Add-Always

### The Algorithm

The algorithm takes secret scalar k in binary and input point P, and computes Q = kP.

```
R0 ← P                    ← initialize with P (this handles the MSB=1)
for i = x-2 downto 0:    ← loop over remaining bits, MSB-1 down to LSB
    R0 ← 2·R0            ← ALWAYS double
    R1 ← R0 + P          ← ALWAYS add (compute the alternative)
    R0 ← R[k_i]          ← SECRET: pick R0 if bit=0, R1 if bit=1
return R0
```

The red line `R0 ← R[k_i]` is the key-dependent step — it's SPA-resistant because both R0 and R1 are always computed, so the power trace always shows a double followed by an add, regardless of the bit value.

---

### Worked Example: k = 100 (binary)

```
k = 1  0  0
    ↑  ↑  ↑
   MSB      LSB
```

**Initialization:**

```
R0 = P        ← MSB=1 is handled by starting with P
```

**Iteration 1** (processing bit k=0, the middle bit):

```
R0 = 2·P = 2P
R1 = 2P + P  = 3P
bit = 0  →  return R0 = 2P    ← picks 2P
```

**Iteration 2** (processing bit k=0, the last bit):

```
R0 = 2·2P = 4P
R1 = 4P + P  = 5P
bit = 0  →  return R0 = 4P    ← picks 4P
```

Final result: **Q = 4P = k·P = 100₂ · P ✓**

---

### Worked Example: k = 110 (binary)

**Initialization:**

```
R0 = P
```

**Iteration 1** (middle bit = 1):

```
R0 = 2·P = 2P
R1 = 2P + P = 3P
bit = 1  →  return R1 = 3P    ← picks 3P
```

**Iteration 2** (last bit = 0):

```
R0 = 2·3P = 6P
R1 = 6P + P = 7P
bit = 0  →  return R0 = 6P    ← picks 6P
```

Final result: **Q = 6P = k·P = 110₂ · P ✓**

---

### Why This Is Vulnerable to OTA

Look at what the algorithm returns at each iteration:

```
k = 100:   returns 2P, then 4P
k = 110:   returns 3P, then 6P
```

At iteration 1, the device is holding either **2P or 3P** — and crucially, the power trace of that iteration reflects which one it is. This is exactly the leakage OTA exploits:

```
Attacker builds:
  template with input 2P  →  if it matches target, bit = 0
  template with input 3P  →  if it matches target, bit = 1
```

Even though the power trace always shows a double + add (SPA-resistant by design), **the data values inside those operations differ** — and data-dependent power consumption is what OTA detects.