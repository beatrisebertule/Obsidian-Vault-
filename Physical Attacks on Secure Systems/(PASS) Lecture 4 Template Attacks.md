

DPA attack does not work if we change our key every 5000 traces
especially when we 10000 traces to perform successful DPA

profiled attack
non-profiled attack

1st stage: prep, collect templates, known number of subkeys
2nd stage: attack, match with the composed templates (match templates)

assume that there are K number of subkeys that need to be classified
collect traces for diff keys

traces have to be aligned to select PoIs

select type of template
build templates
	group traces that correspond to a subkey ki
	for each group compute template for the PoI
collect S' from protected target device
template matching


one trace = one data point (PoI)

mean and standard dev to summarize the points

template - a par h = [m, s] where m is means, s is standard deviation
do this for each key
templates are Gaussian distributions 

we want narrow st values so the distributions overlap less

<span style="color:rgb(219, 0, 0)">full template </span>- for each subset of keys, there is a separate template

<span style="color:rgb(219, 0, 0)">simplified template</span> - sd the same for each template
compute sd over all traces

<span style="color:rgb(219, 0, 0)">reduced templates</span> - only look at means


PoI selection
univariate Gaussian selection

leakage model - determines how many templates we have


goal: find template which makes observed data most probable
other words, maximizes likelihood observing data points in attack traces


likelihood function
product turn into sum of logs

![[Pasted image 20260223143043.png]]


template matching

given points, how likely these points are part of a distribution
we 


<span style="color:rgb(219, 0, 0)">reduced template</span>
in reduced tenmplaet we dont us elikelihood as we dont have sd
we use lsq least squared 
we want o minimize the distance

square distance to mean


in practice we use multivariate Gaussian model (generalisation of normal distribution)
so we use multiple PoIs

neighbouring samples may contain redundant information (because of pipelines)
there can be better points that make make templates more ifrimative


the covariance matrix

template for multiv

hi = [mi , Ci ]. where  
▶ mi is the mean vector and  
▶ Ci is the covariance matrix .  
For template matching we use the maximum likelihood principle.

![[Pasted image 20260223145505.png]]


one reason to use simiplifies template - bypas steh problem of numercial instability that coems form computing teh inv of covariance marix

why 
stages of pipelines - contribute to power consumption
frequency of oscioscope

also how var are laodade from registers



