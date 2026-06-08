DNS queries and respnses

It is _recursive_ because it **follows the DNS hierarchy step by step** until it gets a final answer:

1. Client → local recursive resolver
2. Resolver → **root DNS server** (“Who knows about `.com`?”)
3. Resolver → **TLD server** (`.com`) (“Who knows about `example.com`?”)
4. Resolver → **authoritative server** for `example.com`
5. Resolver → client (returns the IP address)

![[Pasted image 20260204103327.png]]

Local recursive resolvers cache results:
- If another user already asked for `www.example.com`, the resolver may answer **immediately**
- Cached entries respect **TTL (Time To Live)** values
- This improves speed and reduces DNS traffic globally


Another common use for DNS queries is to look up domains in a DNSBL (DNS-based blacklist), which are lists that are commonly  maintained to keep track of IP addresses associated with spammers and malware.  To look up a domain name in a DNSBL, a client might send a DNS A-record query to a special DNS server, such as pbl.spamhaus.org (a ‘‘policy blacklist’’), which corresponds to a list of IP addresses that are not supposed to be making connections to mail servers.

### DNSSEC

<span style="color:rgb(219, 0, 0)">DNSSEC records</span> allow responses from DNS name servers to carry digital signatures, which the local or stub resolver can subsequently verify to ensure that the DNS records were not modified or tampered with. Each DNS server computes a hash (a kind of long checksum) of the RRSET (Resource Record Set) for each set of resource records of the same type, with its private cryptographic keys. Corresponding public keys can be used to verify the signatures on the RRSETs. 

Verifying the signature of an RRSET with the name server’s corresponding  
public key of course requires verifying the authenticity of that server’s public key.  
This verification can be accomplished if the public key of one authoritative name  
server’s public key is signed by the parent name server in the name hierarchy. For  
example, the .edu authoritative name server might sign the public key correspond-  
ing to the chicago.edu authoritative name server, and so forth.

DNSSEC has two resource records relating to public keys: (1) the RRSIG  
record, which corresponds to a signature over the RRSET, signed with the corres-  
ponding authoritative name server’s private key, and (2) the DNSKEY record,  
which is the public key for the corresponding RRSET, which is signed by the par-  
ent’s private key. This hierarchical structure for signatures allows DNSSEC public  
keys for the name server hierarchy to be distributed in band. Only the root-level  
public keys must be distributed out-of-band, and those keys can be distributed in  
the same way that resolvers come to know about the IP addresses of the root name  
servers. Chap. 8 discusses DNSSEC in more detail.


<span style="color:rgb(219, 0, 0)">DNS zones</span> - each zone is associated with one or more name servers.

<span style="color:rgb(219, 0, 0)">Name servers</span> are hosts that  hold the database for the zone. Normally, a zone will have one primary name server, which gets its information from a file on its disk, and one or more secondary name seName serversrvers, which get their information from the primary name server.Name servers