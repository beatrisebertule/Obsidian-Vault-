
[[(quizz) Routing Security]]

claude summary
[[sum bgp]]

guest lectures - general questions
not that deep questions

xavier's lectures - more in depth
make sure you fully understand exercises

### BGP Introduction

when a host A wants to reach host B
we know IP
how IP helps us to route the package

AS - autonomous systems, <span style="color:rgb(255, 192, 0)">inter-connected</span> together (internet)
around 78k active autonomous systmes
these systems are identified by unique identifiers (ASN)
blocks of IP addresses (<span style="color:rgb(255, 192, 0)">prefixes</span>) are allocated to ASes

an example of a prefix
145.97.24.0/21 the first 21 bits are fixed (in binary represenation)
so there are 2^11 addresses left to have within that prefix (autonomous system) 

(8 bits  x 4 = 32 bit addresses)


### Allocation of prefixes adn ASNs are managed by IANA
<span style="color:rgb(255, 192, 0)">IANA</span> - Internet Assigned Number Authority responsible for global allocation of ASNs and 
IPv4 and IPv6 prefixes

Hierarchical allocation/assignment  
– IANA allocates ASNs and prefixes to <span style="color:rgb(219, 0, 0)">Regional Internet Registries</span> <span style="color:rgb(219, 0, 0)"></span>(RIRs)  
– <span style="color:rgb(219, 0, 0)">ISPs</span> obtain allocations of IP addresses from RIR 
	(or National Internet Registry (NIR) or Local Internet Registry (LIR))  
– Users obtain IP addresses from their ISPs (Internet Service Providers)


INTRA-AS routing
IGP protocols like RIP OSPF

INTER-AS routing
BGP

application layer protoocls
uses TVP/IP (per session) connect between boarder routers
to port 179

advertises routes
or exchanges info

but does not do routing

BGP assumes all networks are trustworthy (no validation of routing information)

border routers know prefixes of ASes
they need to know where to send the traffic to
(shortest path or longest matching prefix)

external <span style="color:rgb(219, 0, 0)">peers</span> (external)
internal <span style="color:rgb(219, 0, 0)">peers</span> (internal connections)

BGP router advertise all/some of their router to peers
	then only send incremental updates

each router builds its own <span style="color:rgb(219, 0, 0)">routing table</span> 
routing table lists all possible routes from router to all possible destinations  
<span style="color:rgb(219, 0, 0)">routing Information Base (RIB)</span>: List of all known routes  
<span style="color:rgb(219, 0, 0)">forwarding Information Base (FIB)</span>: Effective, installed routes

if you learn a new route, send that to peers
can accept, can discard (if accept, further advertises)

<span style="color:rgb(219, 0, 0)">BGP only discover routes</span> 

### BGP advertisements

![[Pasted image 20260302210648.png]]


Route announcement and withdrawal

### Relationship between ASes
![[Pasted image 20260302213234.png]]

upstream !!! we finally learn about upstream !!

valley-free concept !!

![[Pasted image 20260302213548.png]]

Anycast is

==a network addressing and routing methodology where multiple, geographically dispersed servers share the same IP address==. Routers direct traffic to the "nearest" node based on network topology, reducing latency, balancing server load, and providing high redundancy for services like DNS and CDNs. It also effectively mitigates DDoS attacks by distributing malicious traffic across multiple locations.


for example DNS root servers 
we have 13 of them
so we have 13 different IP addresses,
but one root server has actually many many servers geographically different location


### Routing attacks and incidents

mostly because of wrong anouncements

Fundamental issues  of BGP:
1. BGP has no internal mechanism that provides strong protection of the integrity, freshness, and peer entity authenticity of the messages in peer-peer BGP communications  
2. It is plaintext  
3. No mechanism has been specified within BGP to validate the authority of an AS to announce NLRI information  
4. No mechanism has been specified within BGP to ensure the authenticity of the path attributes announced by an AS  
5. BGP is a simple application protocol on top of TCP, itself vulnerable to other network attacks

no security at all
#### BGP hijack/route hijack

![[Pasted image 20260302214848.png]]

not all traffic will be routed to the attacker network
only some (as will not be the shortest path, similar to anycats - to all traffic will be routed to one point of presence)

### Subprefix hijack
![[Pasted image 20260302215157.png]]

<span style="color:rgb(219, 0, 0)">route 53/myetherwallet example</span>
<span style="color:rgb(219, 0, 0)">pakistan youtube example</span>

hijacked amazon's dns server
they became the dns server

changed IP for domain myetherwallet


problem - worng ssl\tls certificate as they used a self-signed certificate
they did not have a valid certificate for that domain


HTTPS relies on **valid SSL/TLS certificates** issued for the domain you are visiting. Browsers check:
    1. Is the certificate valid?
    2. Does it match the domain?
    3. Is it signed by a trusted Certificate Authority (CA)?

In this attack, the attackers’ server used a **self-signed certificate**. That means:
    - It **was not issued by a trusted CA**.
    - It **did not match `myetherwallet.com`**, so browsers warn the user.

Because of the invalid certificate, browsers would show errors like:
    > “Your connection is not private” or “The certificate is not trusted.”
    


![[Pasted image 20260302221328.png]]

advertise a wrong route to upstream

other BGP attacks
![[Pasted image 20260303110743.png]]
![[Pasted image 20260303110751.png]]



### BGP hijack impacts
<span style="color:rgb(219, 0, 0)">loss of availability (DDoS)</span>
Traffic goes to wrong AS, where it is discarded, or causes <span style="color:rgb(219, 0, 0)">congestion</span> 
kind of DDoS yourself 

<span style="color:rgb(219, 0, 0)">Blackholing</span> - all trafiic that goes throiugh blakc hoel will be lost
redirect traffic ot some place where you know the traffic will be dropped
or you dorp it
so it never reaches the real target 

do this to take down ddos attacks
redirect traffic from DDoS

<span style="color:rgb(219, 0, 0)">Looping</span> 
avoid loops
but can still have loops
traffic circulates until TTL runs out

<span style="color:rgb(219, 0, 0)">Abuse of (unallocated) prefixes (IP address spoofing)  </span>
– hijacker sends traffic using hijacked prefixes  
– E.g. to send spam or to hide C&C servers of botnets 

<span style="color:rgb(219, 0, 0)">Surveillance  </span>
can see the traffic (as everything is in plaintext)
– reroute traffic from targeted region such that it can be inspected  
– hard to detect, since traffic is forwarded to destination  
– government may surpass legal restrictions by diverting domestic traffic (e.g. emails between  
residents) to foreign jurisdictions to conduct surveillance  

<span style="color:rgb(219, 0, 0)">Malicious modification of traffic, traffic redirections  </span>
<span style="color:rgb(219, 0, 0)">Increase of provider’s revenue from its customers</span> 

BGP serial hijackers
some ASes repeatedly abuse BGP
we have blacklists for this

Spamhaus 'dontr rioute or pee' asn drop list

BItCanal AS maliciosu since 2014, got disconnected only in 2018 
have atendecany to change unique originated prefixes (for hojacking different prefixes)
opoosed to normal AS - mostly teh same prefix the whole time


### BGP Security
MANRS (Mutually Agreed Norms for Routing Security)

Participants agree to perform (be good on the internet):
– Filtering  
	ensure correctness of own announcements (check if prefix has been allocated to this AS)  
– Anti-spoofing  
	source address validation (check that source address is in prefix allocated to sender)  
– Coordination  
	maintain globally accessible, up-to-date contact information of network operators  
– Global validation  
	publish data so others may validate routing information

IP spoofing would not work (traffic for spoofed IP gets routed)
guest lecture - spoofing attribution
some ASes allow traffic with spoofed IP addresses to be routed

how can we validate source IP?
<span style="color:rgb(219, 0, 0)">source address validation</span> 

### GTSM

directly connected routers - only one hop, soTTL could be just 1
set TTL to 255
drop all packets that have <254 value as TTL


### Filtering using IRRs
-- administer and register IP address space and AS numbers  within defined geographical regions  
-- maintain Internet Routing Registries (IRRs)  that contain routing information  (eg. network routing policies and  expected routing announcements)

maintain accurate info of which AS is in charge of which

I can see that prefix that or listed oin the list
hence, may be malicious

so BGP announcements can be filtered based on this info
info which AS uses which prefix

but data can be incomplete... hard to maintain accuracy
IRRs may become targets
...

# RPKI / ROA-ROV

### Resource Public Key Infrastructure (RPKI)
bind resouces (prefix and ASN) to a public key

In BGP, any AS can announce any prefix. There is no built-in check to verify whether the AS actually owns or has the right to originate that prefix. So the question RPKI answers is: **does this AS have the right to announce this IP prefix?**

RPKI creates a cryptographically secured record of who owns what IP address blocks, mirroring the existing administrative allocation hierarchy:

IANA → RIRs → NIRs → LIRs → ISPs → end users

At each level, a **resource certificate** is issued that says "I (the parent) have allocated these specific IP prefixes and ASNs to this entity (the child)." This chain of certificates traces all the way back to the RIRs as trust anchors.

### Route Origin Authorization (ROA)

A ROA (Route Origin Authorization) is a **signed, cryptographic object** that states which AS is authorized to announce a specific IP prefix.

A **ROA** is like a signed permission slip published in the RPKI system by the legitimate owner of an IP address block.

It says three main things:

<span style="color:rgb(219, 0, 0)">1. <b>Which IP prefix</b> (address block) this applies to  </span>
    → e.g., `203.0.113.0/24`
<span style="color:rgb(219, 0, 0)">2. <b>What prefix lengths are allowed</b>  </span>
    → minimum and maximum length (to prevent someone from announcing smaller sub-prefixes like `/25` or `/26` if not allowed)
<span style="color:rgb(219, 0, 0)">3. <b>Which AS is allowed to originate it</b>  </span>
    → e.g., `AS3`

And it is:
- **Digitally signed**, so no one can fake it
- Issued by an **RIR (Regional Internet Registry)** such as ARIN, RIPE NCC, or APNIC

So a ROA basically says:
> “Prefix P may only be announced to BGP by AS3.”


### Route Origin Validation (ROV)
**ROV** is what routers do when they receive a BGP route announcement:

They check the announcement against the ROA database.
For a given BGP route, the router asks:
> “Is this AS allowed (by a ROA) to announce this prefix?”
The result can be:
- **Valid** – matches a ROA
- **Invalid** – contradicts a ROA
- **NotFound** – no ROA exists

Many networks drop or de-preference **Invalid** routes.


##### EXAMPLE
Given:
- **ISP1 = AS3** owns prefix **P**
- ISP1 registers a ROA:
    > Prefix P can only be originated by AS3
This is now published in the RPKI system.

---
Then:

ISP2 (AS2) receives a BGP announcement from ISP3 (AS6):
AS path: (AS6, AS5)  
Prefix: P

This means:
- The **origin AS** is **AS5** (last AS in the path)
- AS6 is just passing it along

So ISP2 checks the ROA for prefix P.

ROA says:
> Only AS3 is allowed to originate P.
But the route says:
> Origin = AS5
That’s a mismatch.

---

Result: ISP2 drops the route

Because:
- ROA says origin must be AS3
- Actual origin is AS5
- Therefore the route is **ROV = Invalid**

ISP2 rejects (drops) the announcement to avoid accepting a potentially hijacked or misconfigured route.

This prevents:
- BGP hijacks
- Accidental leaks
- Traffic being misrouted

## Why this is important

Without ROA/ROV:
- Any AS could announce someone else’s IP prefix
- BGP would trust it blindly

With ROA/ROV:
- Only the authorized AS can originate that prefix
- Others get filtered automatically

So in short:

> **ROA = who is allowed to announce a prefix**  
> **ROV = checking announcements against that permission**


use RPKI cache so we dont overburden routers with crypto

info stored within online registries
![[Pasted image 20260303172310.png]]

### Stage 1 — Storage (Top of Diagram)

ROAs are stored in **two types of repositories** in the cloud:

- **RIR RPKI repositories** — maintained directly by the five Regional Internet Registries (RIPE, ARIN, APNIC, etc.)
- **Delegated RPKI repositories** — maintained by organizations (ISPs, enterprises) who have been delegated the authority to manage their own RPKI objects rather than relying on the RIR to host them

These repositories are the authoritative source of all ROA data.

### Stage 2 — Retrieval and Validation (Middle of Diagram)

Inside each AS, there are **RPKI-validating servers** that fetch and verify the ROA data from the repositories. This happens in two ways:

- **Rsync** — the traditional periodic synchronization method, fetching all objects regularly
- **RRDP (RPKI Repository Delta Protocol) over HTTPS** — a more modern and efficient method that only fetches changes/deltas rather than everything

Once fetched, the validating server performs all the cryptographic checks — verifying digital signatures, checking certificate chains up to the RIR trust anchor, and ensuring resource allocations are consistent at each level of the hierarchy.

The successfully validated objects are called **Validated ROA Payloads (VRPs)** and are stored in an **RPKI Cache** within the AS.

### Stage 3 — Propagation to Routers (Bottom of Diagram)

The cached VRPs are then fed to the actual **BGP routers** via the **RPKI-to-router protocol**. The routers receive this pre-validated data and use it to perform Route Origin Validation (ROV) on incoming BGP announcements.

You can see in the diagram that the routers also have local **ROA Data** stored alongside them, representing this locally cached set of validated records they use for checking announcements.

### Why This Architecture Matters

The key design insight is that **routers themselves do not perform cryptographic operations**. All the heavy cryptographic verification happens in the dedicated RPKI-validating server. The routers simply receive the already-validated VRPs and do straightforward lookups against them. This is important because:

- BGP routers handle enormous volumes of traffic and need to be fast
- Cryptographic verification is computationally expensive
- Separating the validation from the routing keeps the system practical and scalable

However this also means the router is **trusting the RPKI cache** to have done the validation correctly, so the connection between the cache and the router needs to be secured (via TLS or IPsec).


in short,
routers get pre-verified ROAs, then check the prefix and origin ASfrom a BGP announcement against the pre-verified ROA = VRP validate ROA payload

### What Happens Where

**RPKI Validating Server** does all the heavy cryptographic work:

- Fetches ROAs from repositories
- Verifies digital signatures
- Checks certificate chains up to the RIR trust anchor
- Checks resource allocations are consistent at each level
- Produces the **Validated ROA Payloads (VRPs)** — a clean, trusted list of (prefix, max length, authorized AS) records

**BGP Router** does the simple lookup:

- Receives the pre-verified VRPs from the RPKI cache via the RPKI-to-router protocol
- When a BGP announcement arrives, it just checks the announced prefix and origin AS against the VRP list
- No cryptography needed — just a simple match/no-match check
- Produces valid/invalid/unknown result and accepts or drops accordingly

### Why This Separation Makes Sense

Routers handle enormous volumes of BGP traffic constantly. Making them perform full cryptographic verification of every announcement would be:

- Computationally too expensive
- Too slow for real-time routing decisions
- Impractical given router hardware constraints

So the architecture offloads all cryptography to the dedicated validating server, and the router just consumes the already-trusted results.

### The Trade-off

The one downside of this approach is that the router is now **trusting the RPKI cache** rather than verifying things itself. This is why the connection between the RPKI cache and the router must be secured, typically via TLS or IPsec, otherwise an attacker could potentially feed false VRPs to the router and bypass the whole system.


not all bgp hijacks are hiojacks
can be misconfigurauiotn - update prefix, but not roa...


<span style="color:rgb(219, 0, 0)">GBPsec authenticates the whole path (not only teh origin)</span> 

![[Pasted image 20260303174103.png]]

MAIN BGP SEC limitation
Incremental deployment of BGPsec is not possible: all routers on path  
should support BGPsec
as we reach one routes that does not have BGPsec, signature gets dropped and even if we reach another router that does have BGPsec, there is no way to verify

### Takeaways from Exercises


### check paper notes ofor ROV++

understand valley-free concept
understand what v1 v2 an v3 foes and what problems they fix

v1 - has a path preference, if not safe path, drop traffic
v2 - sends black-hole announcement if no safe path, if safe path - prefer that path, send no announcement
v3 - send preventive announcement if safe path exists - receive all traffic and redirect it through the safe path