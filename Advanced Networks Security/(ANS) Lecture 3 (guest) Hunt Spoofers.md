Guest Lecture
Hunting Spoofers in R&A Attacks

rooting out the cause and fingerprinting actors

reflection and amplification attack
spoof - falsify source address (response gets sent to the victim)

query big record

directly target authorative name serevrs (not open resolvers)
so ayht ns can see the spoofer

clues in the packet
recognizable packets
fixed quesry ID
packs of 25 packets
funky buffer size o 9000 bytes

all packets had more that 250 and over, so start values was 255
so they travelled at most 6 hops to travel to the detsination
which is very little
hence "very close" in netwrok terms

look at network patterns to identify the network part the spoofer is on

use open resolvers as layer of "indirection"

who does spoofing
anycast relies on BGPs shoretst path to reach point of presence PoP

VERFPLOETER


DETERMINE catchment of anycast sites

amplification honeypots (any tyoe of services taht can be abused my amplification atatck)
deploy ona  aocmbinaitionof anycat an dunicats

use anycats to confitm spoofing
combine with unicast data to narrow down source networks


has to be in netwrosk that can be spoofe (spoofing allowed)
you need qa compromised host (bot)


TTL variability

![[Pasted image 20260222141412.png]]

One of the 2012 attacks used SURFnet’s authoritative name servers as amplifiers <span style="color:rgb(219, 0, 0)">directly</span> 
This cut out “the middle man” (the open resolvers)
This meant we could try to find the spoofer(s)!
The attack had very distinct characteristics (smth that helps to identify DDoS traffic)


In “normal” amplification attacks, attackers use, e.g., open resolvers as a layer of indirection

ping from the anycast address as source
the response to the ping is then sent to the “closest” (in terms of BGP routing) point of presence
All we then still need is a list of addresses to ping