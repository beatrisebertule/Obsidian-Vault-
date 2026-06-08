
### Time Leakage
cache attacks (timing side-channel)
example - s-box
lookup for diff values can take diff amount of time

<span style="color:rgb(255, 0, 0)">constant-time code</span>

### Power Leakage
CMOS (most popular for chip design) leakage 

core concept - transistors 

gates switch between 0 and 1 - inverter switches - flips a bit
most power consumed due to switching activity
high power = high switching activity

most power consumption comes from dynamic powers rather than static power
dynamic power - switching activity

dynamic power consumption depends on data and instructions being processed

**start** point - model the power leakage
we use the number of bit transformations to model the leakage

**Hamming Distance** (HD)
**Hamming Weight** (HW)

overwrite memory (state) -> some bits will change, some will stay the same
HD captures the state change  - (xor) between both bit states
HW sums the the result of HD - aggregates all ones

a leakage model for xxx micro controllers
apply HD and then HW

### SPA on RSA
add and multiply - resists simple power analysis on RSA

collision attacks - the same data will result in the same power


### Simple Power Analysis
visually interpret power _traces_, or graphs of electrical activity over time




