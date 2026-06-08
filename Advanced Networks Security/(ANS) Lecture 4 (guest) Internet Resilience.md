
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
