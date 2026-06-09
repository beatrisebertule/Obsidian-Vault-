
resilience - maintain availability and reliability under disruptions or unexpected events

Internet Infrastructure attack

<span style="color:rgb(219, 0, 0)">BGP hijacking</span>, DDoS, DNS spoofing, IP spoofing
 recap BGP - border gateway protocol


aim of today DDoS
problem - how to distinguish between good or bad traffic

IoT devices - easy to compromise for distribute DoS attacks
botnet of compromised IoT devices

[bgp](https://www.cloudflare.com/en-gb/learning/security/glossary/what-is-bgp/)

caching helps with DDoS attack but not fully
cannot fully trust that we have cached responses

Increase DNS resilience
1. topological diversity
   example 2022 attack on russian ministry of denseness mil.ru was taken down by DDoS
   all three authoritative name server for mil.ru was behind the same /24 network block
   the attack overwhelmed the upstream of the /24 block
   how to increase top div:
   host multiple nameservers in different autonomous system (AS diversity)
   deploy bame server in different router prefix  (routing diversity )
   host name severs with different upstream
   
2.  organizational diversity
	   bad example - trust on company 
	   increase  use multiple DNS service providers
	   provide with different ownership structures (operating in diff regions, using different upstream provides)
	   
   
3.  geographical diversity
		bad example - fire
		increasedeplouy NS in different data centers in different regions
		USE ip ANYCATS ROTUING TO ALLOWE MULIPLE ns TO SHAR ETHE SAME ip BUT RESPOND FROM CLOSEST LOCATION
			partenr up  with 3rd party dns providers
   
4. IP anycast

IP anycast
used to route traffic to the topological nearest available server within a group of servers that share the same IP addresses

the request is routed to the nearest available server
based on network topology
and can change dynamically in response to network conditions


anycat improve resilienc of DNS by allowing multple serervs share the smae IP address
anycats can helpt ot disyrubte traffic avross mulirple servers


in 2021 half of dns namespace relied on anycats
but top 10 anycast organizations were responsible for 92% of somains adapting anycast
goDaddy alone acounted fro half od domains adapting anycast
where is organizational resilience?



single ASN deployment adn single /24 prefix - main reaosn fro domain failing to resolve

anycat deployment helps fro resilience


recap concepts
bgp
anycast


# Improved Notes
### Why DNS Resilience Matters
Because DNS is so central, it is a prime DDoS target. Disrupting DNS can prevent users from reaching websites, email, and almost all other online services. As the lecture starkly puts it: **"If you can stop the DNS, you can effectively stop most internet communication."**


### Strategies for DNS Resilience

The lecture outlines four main pillars:
#### 1. <span style="color:rgb(219, 0, 0)">Topological Diversity</span>
Distributing nameservers across different network locations so no single network failure can take them all down. Methods include:
- **AS Diversity** — hosting nameservers in different Autonomous Systems
- **Routing Diversity** — deploying across different routed IP prefixes
- **Upstream Diversity** — using different upstream ISPs

**Bad example:** In 2022, Russia's mil.ru had all three authoritative nameservers in the same /24 network block. A DDoS attack took down all three simultaneously. The misconfiguration wasn't fixed until 2024.


#### 2. <span style="color:rgb(219, 0, 0)">Organizational Diversity</span>
Using **multiple, independent DNS service providers** — ideally with different ownership, different regions, and different upstream providers. The challenge is coordinating them to avoid conflicting DNS information.

**Bad example:** The 2016 Dyn attack succeeded in knocking out Twitter, Netflix, and Reddit because all of them were relying solely on Dyn. Had they used multiple providers, the impact would have been limited.

#### 3. <span style="color:rgb(219, 0, 0)">Geographical Diversity</span>
Placing nameservers in **physically separate data centers across different regions**. Strategies include deploying across multiple data centers globally and partnering with DNS providers that have distributed infrastructure.

**Bad example:** A 2021 fire at a French cloud services firm (OVH) took millions of websites offline — government portals, banks, shops, and news sites — because they all had their infrastructure concentrated in one physical location.

#### 4 <span style="color:rgb(219, 0, 0)">IP Anycast</span>
A routing technique where **multiple servers share the same IP address**, and incoming traffic is automatically routed to the topologically nearest available server. Routing adapts dynamically to network conditions.

Benefits for DNS:

- Distributes traffic load across many servers
- Reduces response times (nearest server responds)
- Mitigates DDoS impact by spreading the attack across many nodes

**Anycast vs. the other diversity strategies:**

- Anycast uses BGP routing for server selection (faster, dynamic)
- Topological/geographical/organizational diversity relies on resolvers choosing between different IP addresses
- Best practice is to **combine all four approaches**


### Empirical Evidence: DNS Under Attack
Research drawing on nearly two years of attack data found:
- **81% of domains** that failed to resolve had **single-ASN deployments**
- **60% of failures** were traceable to all nameservers sharing a **single /24 prefix**
- Anycast deployments consistently suffered **less disruption** during attacks
- Hosting nameservers across multiple prefixes or ASNs dramatically increased resilience

### Key Takeaways
- DNS is the backbone of internet communication — disrupting it disrupts almost everything
- DDoS attacks on DNS infrastructure can have catastrophic, wide-reaching consequences
- Resilience requires layered, diverse strategies: **topological, organizational, geographical diversity + IP anycast**
- Concentration — whether in one network block, one provider, or one data center — is the enemy of resilience
- Continuous monitoring, testing, and updating of DNS infrastructure is essential