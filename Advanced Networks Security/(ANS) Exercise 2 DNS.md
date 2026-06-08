
we setup our own recursive server 
we query @localhost to resolve DNS queries for us
#### Exercise 1

`dig www.ru.nl

dig - use to query DNS (Domain Name System) name servers for information about host addresses, mail exchanges, and other DNS records


`dig SOA www.ru.nl
ask fro start of authority records 

dig +dnssec example.com to see if the if a domain uses dnssec


to see the nameservers  for the nl zone by asking one of the root servers: a.root-servers.net
query
`dig @a.root-servers.net nl. NS`

`@a.root-serevrs.net` root server (we don't ask recursive resolver such as 8.8.8.8)
but we ask root server directly
`nl.` for zone nl

root server know about TLD (top level domain) name servers
hence the answer will contain names of TLD name servers that know are responsible for zone .nl 


response
`nl.     172800  IN  NS  ns1.dns.nl.
`nl.     172800  IN  NS  ns2.dns.nl.
`nl.     172800  IN  NS  ns3.dns.nl.
these name servers are responsible for zone .nl (everything under .nl)

comment for myself
a recursive resolver finds the answer for you
a nameserver owns the answer

ask directly one of the nameservers for ru.nl
`dig @ns1.dns.nl ru.nl. NS`
will give name servers responsible for ru.nl

Then ask one of these nameservers for brightspace.ru.nl.
`dig @ns4.ru.nl brightspace.ru.nl

I get a  record
`CNAME radboud.brightspace.com`

#### Exercise 2

`dig @localhost ru.nl`

gives one record entry
`ANSWER SECTION:
`ru.nl.			568	IN	A	131.174.78.60`

Does science.ru.nl have a nameserver? Issue the correct query.
`dig @localhost science.ru.nl NS`

gives
```
ANSWER SECTION:
science.ru.nl.		86400	IN	NS	ns2.science.ru.nl.
science.ru.nl.		86400	IN	NS	ns3.science.ru.nl.
science.ru.nl.		86400	IN	NS	ns1.science.ru.nl.

ADDITIONAL SECTION:
ns1.science.ru.nl.	86400	IN	A	131.174.224.4
ns2.science.ru.nl.	86400	IN	A	131.174.16.133
ns3.science.ru.nl.	86400	IN	A	131.174.30.34
```

command `dig @ns3.ru.nl science.ru.nl NS
gives the same response
when I query 8.8.8.8 the answer does not give me glue record if IP addresses of the NS

DNSKEYS of science.ru.nl
`dig @localhost science.ru.nl DNSKEY +short`

```
science.ru.nl.		86249 IN DNSKEY	256 3 8 

(
				AwEAAeDP0wuxzGNSpi222TrVspGkOu7NsPQB4/mdnv5x
				+WrXZxv+0YN41qF/UegR8cpNZJy8RzwA7jd80sGwUUAO
				j0bmHdDyNRukXrWWGWbRCCetE7LlZkR/DSbP8a3wi/2W
				VzKGDOl/fUG5D1bu75xapbpw7wcJRPj4/5l9KXy642LX
				) ; ZSK; alg = RSASHA256 ; key id = 50688
science.ru.nl.		86249 IN DNSKEY	257 3 8 (
				AwEAAadcVjwvVz3lU41ceOfen7iYZi4f6b4r3Vt1cDHk
				ud8N/tOYdXOwru5oeYFQ2aLNKlSBskklnBE0m8wdsA7C
				xrngXSUzR4Dt5ID56jtlKlCzVjQDji3j4ijjl404oLTa
				f1nbJ342Ip3FfSc075agS9U8BIewPX5wYvCKZdgqA7Pj
				NwfhefATydL5iiGUt7LrcnI5v04EJ8YcizFHmjPhNqEH
				1Hhxnk3zEoc4dIPJwSAmPLbYa74URl9HwETbWIOBw2uY
				M5zhZIBgvBUXdByTCJY6Wd2zkn976kI7SLSXfHOS/+Zo
				0oQvZvMmtjn21WauUhjW4KNytvTMweqD4Cty6DM=
				) ; KSK; alg = RSASHA256 ; key id = 22537
science.ru.nl.		86249 IN DNSKEY	257 3 8 (
				AwEAAdYLX4NPi4cnX39XpSpylNhKwAfHyBRFRvJDxxOo
				+iv+UKZ9/0Qj5nlugIOuUndtISJMubf3GjCbrn3DAKpB
				D/DvurTAnCgtL7n7axenwH/BzO1ixSOeKx2nj5vkFxRD
				zu+MpBsfugc1scHJkrNCb3jPeMlZQkjcaejz6HVHXfe/
				jgRLSnwXLa5UpQpgwpHUKd5usGtKHPJygonOiq5NtieN
				ip5zLv++GnZmJe/sjHo5rQb4tK//Ac1pD1YrqZvyM+uc
				jAeuLiwU4aMyvKC3qFK9fEgsWSaTg92HA+0vPeEpU19e
				VhPXIMaQG4/iC+c1iEQr6Jr4qCUlnkoLxJG12gc=
				) ; KSK; alg = RSASHA256 ; key id = 20086
science.ru.nl.		86249 IN DNSKEY	256 3 8 (
				AwEAAdeNB/ZagutBiB+woeSwY5Hb7hSkR9hKL3kxnIKV
				Y0HUpVKFhAzhSozN+wMuX+8bwkDBv8x6jMLv1+4ffvDN
				VDXJtGzDZaacm7WmwFh1vjCqIqTqFs8EI+H3er0ki+EZ
				2KCz9dmCW/raECGzaA5oGEUIiw/RVLpEjz0a1Rzcrmol
				) ; ZSK; alg = RSASHA256 ; key id = 7743

```

I see four public keys, two are ZSK, and two are KSK
ZSK is much smaller than KSK

flags, protocol, algorithm
257 3 8

8 is fro RSASHA256 algorithm

13 is ECDSA Curve P-256 with SHA-256

<span style="color:rgb(219, 0, 0)">a value of 256 indicates that the DNSKEY contains a ZSK and<br>a value of 257 indicates a KSK</span> 


What are the DNSKEYs for science.ru.nl? Display long keys on multiple lines using  +multi. What can you say about the key sizes? Notice the some extraneous keys. For calculating the key size, refer to RFC 2537 Section 2 to extract the RSA modulus and measure its length

I converted the key from base64 to bianry and counted the bits
I got 125 bits for ZSK
I got 

According to **RFC 2537 Section 2**, for RSA keys:
The public key field encodes:
    - exponent length
    - exponent
    - modulus (n)


What are the key lengths for the DNSKEYs of the nl. zone? Any comment?
`dig @localhost nl. DNSKEY +multi`
```
;; ANSWER SECTION:
nl.			1582 IN	DNSKEY 256 3 13 (
				37hUmkOyt4LJeZLqM3xvlKK5kt0H8Yz5AuV6i7A+v2kb
				FezJCKrBpDPaiQz9Z10isbuIWEohqwE3ax/oQMH4Hw==
				) ; ZSK; alg = ECDSAP256SHA256 ; key id = 37807
nl.			1582 IN	DNSKEY 257 3 13 (
				aeDdFmc/JLPyva7Y4bS2SFbfWmxaiSrnqwgs+D1PKSPS
				ruxIRH+6gHLhJ4XYIzrSaT3uk6rsx3c5jV8U4B8O+g==
				) ; KSK; alg = ECDSAP256SHA256 ; key id = 17153

```

comments: the keys are much shorter
a different algorithm was used (not RSASHA256 but ECDSAP)

ECDSAP - Elliptic Curve Digital Signature Algorithm
eliptic curves has much much smaller keys

there are also no "extra keys" - just one zone and one key

An RRSIG (Resource Record Signature) is a DNSSEC record containing a digital signature that authenticates the integrity and origin of a specific RRSet (Resource Record Set) within a DNSSEC-enabled zone.


`dnssec-validation auto`
DNSSEC records will be verified

I could not query `dig @localhost ...` so I querried `dig@ns3.science.ru.nl ...`
output
```
;; ANSWER SECTION:
smtp.science.ru.nl.	180 IN A 131.174.16.145
smtp.science.ru.nl.	180 IN A 131.174.16.143
smtp.science.ru.nl.	180 IN A 131.174.30.193
smtp.science.ru.nl.	180 IN RRSIG A 8 4 180 (
				20260324030257 20260224024337 50688 science.ru.nl.
				vAXIuSAanAvM2527aO2jT6/rDLpfTsZhe3Rx1YtsILCb
				3Sg4PyidhR8rco4W6m4DCIhnGEt/XXhp/n/SGMr9n6cJ
				mK9rOuXrLivDGi+utvaDNITyDB+AV252QIv/2EZCQoAK
				XIJKfH2U3+1By39meJ88BAAYX/Zf6biMgLYMQAM= )

;; AUTHORITY SECTION:
science.ru.nl.		86400 IN NS ns1.science.ru.nl.
science.ru.nl.		86400 IN NS ns3.science.ru.nl.
science.ru.nl.		86400 IN NS ns2.science.ru.nl.
science.ru.nl.		86400 IN RRSIG NS 8 3 86400 (
				20260324024318 20260224020442 50688 science.ru.nl.
				kiUt9zjkoj6vPPVl2WItPp2VMHCf2V3XHZpXX8LUmMWM
				ziXgT1UEPtGUAlUAhaljnPc9+ojU196tiETfPJ8uPfiq
				PN7emmHX37liytjuxIoqJGRsNhGug5pqQHm7s7eU4iEc
				+0F7J4gu/jdC3pJ2NTInSnw1E/ZgAGIbdw5PNMY= )

;; ADDITIONAL SECTION:
ns1.science.ru.nl.	86400 IN A 131.174.224.4
ns2.science.ru.nl.	86400 IN A 131.174.16.133
ns3.science.ru.nl.	86400 IN A 131.174.30.34
ns1.science.ru.nl.	86400 IN RRSIG A 8 4 86400 (
				20260324030257 20260224024337 50688 science.ru.nl.
				CKI4DK28Xp3jjBB025Jm26faj/RS2zwQQqjtaudcbXqA
				0k6TNTAQdmbqqid8qh2i3FZWrwiY9IJyu2V2HEkdrvLP
				0WQseDITle8AvCkNkVj8h/DyMOdjYkRzRfwLvz//aB/0
				QK3KmUykSM088Z6fixhZ6ftMN7vqsi/hK1O0lsw= )
ns2.science.ru.nl.	86400 IN RRSIG A 8 4 86400 (
				20260324030257 20260224024337 50688 science.ru.nl.
				FBExaMCrX0GZK/ekpVk+lXG04BcqLxeq1wERdD+28ZaT
				N3Ydjin0wBdSN103C1/r1JKEfEgV6Ryf1t2N/w+fU2Wi
				QxEiVnwvF7GP92sIZxsr+xw6ayNIzfP+T9T+Xt5SQGwu
				qW3xYTIi9oQKrRAPswlwuBuySKx8KFXqnBxGZQI= )
ns3.science.ru.nl.	86400 IN RRSIG A 8 4 86400 (
				20260324030257 20260224024337 50688 science.ru.nl.
				CssgQmJA1o5n6FRTEphH2M4x59HX4s0sRanebL/b/zur
				dAYUfmzbMIZC7j60teBxNJ6107T4cJ+vU7Q8vE9Ue8fK
				jMayxaWaOpGercHN7GxrK7T9udwLoUuXnsfDQ+SR93eT
				Hz+B/iJR5CF2oH2FLaOBMEsvcD0s9DQT6n9ZtL8= )

```


<span style="color:rgb(219, 0, 0)"><b>`+dnssec`</b> = <i>a flag that says: “include DNSSEC-related records (like RRSIG, DS) in the response.”</i> <b>`DNSKEY`</b> = <i>a query type that says: “give me the public cryptographic keys for this zone.”</i></span> 

comments for myself 
```
science.ru.nl. IN DNSKEY 256 ... ← ZSK  
science.ru.nl. IN DNSKEY 257 ... ← KSK  
science.ru.nl. IN RRSIG DNSKEY ... ← signed by KSK
```


I query to get the RRSIG over the resource records:
`dig @ns3.science.ru.nl smtp.science.ru.nl +dnssec +multi`


because top authority signs them, I can verify with the public key of that
hence, I query:
`dig @ns3.ru.nl science.ru.nl DNSKEY +dnssec


comment to myself
DS record contains a cryptographic hash of a child zone's [Key Signing Key (KSK)](https://www.google.com/search?client=ubuntu-sn&channel=fs&q=Key+Signing+Key+%28KSK%29&mstk=AUtExfDa-OmUKPmHAERFhjArnl1wyCZAXN3lAeM1xHLbNRKUCMnwWJKHFkbExUnuk4qxJqHOV_-ROGiXDjZ-1Qgl5z63Tb3Gp9dE8q_UJFf93dakaym3hKVOJxO6015FmJKPGlpziD_eYIx9HyD7-qNTxz0xplZJsNFmNlH8mmhQcJfrXlk&csui=3&ved=2ahUKEwjInZHpqPmSAxVJ2gIHHSM_MTUQgK4QegQIARAD),
allowing resolvers to verify the authenticity of the child zone

visualises teh chain of trust, can see which keys (whichs IDs) to use to verify
https://dnsviz.net/d/smtp.science.ru.nl/dnssec/

check out https://dnssec-debugger.verisignlabs.com/smtp.science.ru.nl
checks teh chain of trust fro you


to verify the chain of trust manually:
```
$ delv +vtrace smtp.science.ru.nl A

;; fetch: smtp.science.ru.nl/A
;; validating smtp.science.ru.nl/A: starting
;; validating smtp.science.ru.nl/A: attempting insecurity proof
;; validating smtp.science.ru.nl/A: checking existence of DS at 'nl'
;; fetch: nl/DS
;; validating smtp.science.ru.nl/A: in fetch_callback_ds
;; validating smtp.science.ru.nl/A: fetch_callback_ds: got failure
;; broken trust chain resolving 'smtp.science.ru.nl/A/IN': 127.0.0.53#53
;; resolution failed: broken trust chain

```


```
$ delv +rtrace smtp.science.ru.nl
;; fetch: smtp.science.ru.nl/A
;; fetch: nl/DS
;; broken trust chain resolving 'smtp.science.ru.nl/A/IN': 127.0.0.53#53
;; resolution failed: broken trust chain

```

to see the details of all the keys involved, run 
`delv +mtrace smtp.science.ru.nl



when we query `dig @localhost dnssec-failed.org` with `dns-validation no
we get a response with no problems

but `dnssec-failed.org` has wrong signatures hence dnssec should fail
wehne we query `dig @1.1.1. dnssec-failed.org`
we get `status: SERVFAIL`

![[Pasted image 20260227113852.png]]

problem - cannot verify DNSKEY with DS (hash)

### Exercise 3
#### Authenticated Denial of Exitsance

we cannot simply sign a message "NXDOMAIN"

NXDOMAIN - queried  name does not exist
NOERROR response with an empty answer section, often called NODATA
	queried name exists but  has no records of the requested typ

because then on-path attackers could simply reply this signed message
to deny the existence of other names
replay attack 

solution
NSEC record
prove non-existence indirectly

The zone’s owner names are ordered canonically.
For each existing name x, an NSEC  record asserts the next existing name y in that order, i.e., there are no owner names strictly between x and y.
Because NSEC records are signed (via RRSIG), they let a resolver validate where the queried name would fall in the zone’s ordered namespace.
This yields two kinds of negative proofs:

<span style="color:rgb(219, 0, 0)">NXDOMAIN</span> (name does not exist). To prove that qname does not exist, the server  
returns an NSEC record for some existing name x whose interval (x, y) covers qname, where y is  
the “next” name listed in the NSEC. If the resolver checks that  

	x < qname < y

in canonical order, then qname cannot be an owner name in the zone.  

<span style="color:rgb(219, 0, 0)">NODATA</span> (type does not exist at an existing name). To prove that qname exists but  
has no RRset of type qtype, the server returns an NSEC record at qname. The NSEC includes a  
bitmap of RR types that do exist at qname. If qtype is absent from the bitmap, the resolver  
has an authenticated proof that the requested type does not exist there.  

A replayed negative proof is only valid for names/types consistent with the signed NSEC  
statements. 

Let’s look at all possible cases:  

1. Query name outside the covered interval. If an attacker replays an NSEC that covers (x, y) to deny a different qname that does not fall in that interval, validation fails because the ordering check x < qname < y does not hold.  

2. Query name inside the covered interval. If an attacker replays the same proof for another name that does fall inside (x, y), the proof remains valid, is validated, and the attacker does not gain any advantage.  

3. Query name equal to an interval boundary. If qname equals x or y, a resolver would not accept this answer because the replayed NSEC records actually prove the existence of the requested domain.  

Overall, NSEC provides an efficient, precomputable, and verifiable way to authenticate neg-  
ative answers. 



Question 1

`dig tttt.science.ru.nl NSEC
ouput
`tttt.science.ru.nl.	1800	IN	NSEC	3dboxes-srv.science.ru.nl. TXT RRSIG NSEC
`
**Owner name (`x`)** = `tttt.science.ru.nl`
**Next domain (`y`)** = `3dboxes-srv.science.ru.nl`

What stands after y?
`dig 3dboxes-srv.science.ru.nl NSEC`
output
`3dboxes-srv.science.ru.nl. 1800	IN	NSEC	45t-srv.science.ru.nl. A RRSIG NSEC


CONFUSION
I DONT UNDERSTAND HOW TO CHECK THAT DOMAIN NAME DOES NOT EXIST




---
sum

check that domain name exists
solution: x < qname < y

check that no data (no resource records) are available for a domain name that does exists
solution: bitmap of RR

`dnssec-validation auto`
a configuration setting in BIND 9 DNS servers that enables automatic DNSSEC validation using built-in root zone trust anchors


#### QUESTIONS FORM QUIZ that I could not answer

question 3 on no domain or no data responses

[[(quizz) DNS]]




