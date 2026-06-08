
Domain Name Server
Primary function of DNS: return IP addresses for queried hostnames

DNS - a database that contains <span style="color:rgb(219, 0, 0)">resource records</span>
Types of resource recors:
- A stands for IPv4
- AAAA stands for IPv6
- NS directs to another name server
...

### Glue Records
If the resource record happens to be NS (name server), glue records specify their IP addresses
![[Pasted image 20260202163825.png]]

### DNS Hierarchy
The top level of the DNS resides in the root zone where all IP addresses and domain names are kept in databases and sorted by top-level domain name, such as .com, .net, .org, etc.

DNS -distributed, hierarchical database (ROOT, TOP LEVEL DOMAIN, AUTHORITATIVE)

Operation (approximation): DNS client wants to know IP for www.ru.nl:  
1. Client queries root servers to find .nl name server  
2. Client queries .nl name server to get ru.nl name server  
3. Client queries ru.nl name server to get IP address(es) for www.ru.nl

There are typically two types of name servers:
<span style="color:rgb(219, 0, 0)">Authoritative name servers</span>
	root server 
	top level domain servers 
	authoritative servers
	
<span style="color:rgb(219, 0, 0)">Resolvers or DNS Caches</span>  cache previously requested records
	example
		DNS cache on your machine →
		DNS server on the local  network (through DHCP or NDP) →
		DNS server of the ISP →  e.g. 1.1.1.1, 8.8.8.8, or 9.9.9.9 →
		authoritative name servers 
	the TTL (time-to-live) of a DNS record indicates for how long it should be cached

The resolution process walks **from cache → authoritative server**

Total number of DNS root servers: 13
DNS uses UDP

<span style="color:rgb(219, 0, 0)">DNS DDoS</span>
Very hard to defend against DDoS (and DNS amplification)


DNS Targets:
Authenticity of responses  
Privacy of requesters

### Privacy of DNS
Your local DNS resolver sees everything you are requesting  
Any MITM (man in the middle) can see what you’re requesting  
DNS turns out to be exploited en masse by intelligence services

### Authenticity of DNS
<span style="color:rgb(219, 0, 0)">DNS spoofing</span>
Probably most obvious DNS attack: send wrong answer  

Send wrong answer to client: hit one target  
Send wrong answer to DNS cache: hit many targets  
Answers contain “validity period” (TTL) 
<span style="color:rgb(219, 0, 0)">It’s possible to poison DNS caches for a pretty long time</span> 

#### Kaminsky attack

The **Kaminsky attack** is a famous DNS cache-poisoning attack that exploits **predictability in DNS transaction IDs** to trick a recursive resolver into caching a **fake DNS record**.

DNS uses a **16-bit transaction ID (<span style="color:rgb(219, 0, 0)">TXID</span>)** to match responses to queries.  
If an attacker can **guess this ID**, they can forge a fake DNS reply that looks legitimate.

When a recursive resolver sends a DNS query, it includes:
- Query name (e.g., `www.bank.com`)
- **Transaction ID** (random 16-bit number)
    
When a response comes back, the resolver accepts it **only if the <span style="color:rgb(219, 0, 0)">TXID</span> matches**.
So the attacker’s goal is simple:

> Send a forged DNS response that arrives **before** the real one and has the **correct <span style="color:rgb(219, 0, 0)">TXID</span>**.


Fixes - randomised TXID

<span style="color:rgb(219, 0, 0)">DNSSEC</span> adds:
- Cryptographic signatures to DNS records
- Verifiable authenticity, not just matching IDs

Even with correct TXID, forged responses are rejected.

The **transaction ID (TXID)** exists so a DNS resolver can **match a response to the correct query**.


more on Kaminsky attack (randomised source port)
A resolver accepts a DNS response if:
- Transaction ID (TXID) matches
- Source IP looks correct
- Source port matches the port used for the query

Originally:
- DNS queries used a **fixed or predictable UDP source port** (often 53)
- Attacker only had to guess the **16-bit TXID**

### Bailiwick Check

In DNS, a server’s **bailiwick** is the part of the DNS namespace it is **authoritative for**.

Example:
- An authoritative server for `example.com`
- Its bailiwick is `example.com` and its subdomains
    
Without bailiwick checks, a resolver might cache **extra records** included in a response, even if the server has **no authority** over them — enabling **DNS cache poisoning**.


### DNS Censorship
DNS Blocking / Filtering
- The resolver refuses to answer queries for certain domains.
DNS Poisoning / Spoofing
- The resolver **returns a fake IP address** for a blocked domain.
...


Solution for confidentiality and integrity:  
end-to-end encrypt & authenticate everything.



### DNS Authenitcation
DNSSE

### DNS Privacy
Run DNS over HTTPS (HTTP over TLS)