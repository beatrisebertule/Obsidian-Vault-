
### DNS spoofing
(target one person)

false DNS response
not possible over TLS 

at administrative name server level 
<span style="color:rgb(219, 0, 0)">DNS response forgery</span>
race the correct query answer
craft a legitimate DNS response
	spoof IP address (source)
	guess detonation port 2^16 (because source port is randomised)
	guess query ID, another 2^16 number 

**success depend on guessing source port and query ID**

<span style="color:rgb(219, 0, 0)">guess query ID</span>
how can the bad guy learn query ID?
	query resolution server of the victim to query the attacker's zone

know the local resolver
query that resolver to resolve badguy.com
we learn the last ID
works only when query ID is incremental, no longer the case

<span style="color:rgb(219, 0, 0)">guess source port</span>
random source port
NAT may reduce the randomisation (random on local network, not so random on public)
NAT has its own preference for source port numbers

you could have a local resolver behind the NAT

off-pass - no access to local resolver
on path attacker (sits on links - connections)

### DNS cache poisoning
(target many)

atatcker stamds betteen ISP resolver and autorative name server
win the race 
point domain name to a malicious IP, that gets cache don the local resolver

### Defenses
- random source port + random query ID
- limit the issuance of simultaneous DNS queries for similar resources
- choose a random source IP and a random authoritative server  IP address fro each query
- <span style="color:rgb(219, 0, 0)">0x20 encoding</span>
	www.ru.nl -> WwW.rU.NL (randomise capitalization)
	capitalisation request and response has to be the same - another check 
	if the port and ID gets guessed correctly, this still has to match


you can attack glue records, target the authoritative NS


### Confidentiality
QNAME minimzation - each level learns what they only should

### DNS-over-TLS/HTTPS
DNS encryption  - DNS over TLS

DNS over DTLS: DNS over Datagram Transport  Layer Security  
DoT: DNS over TLS  
DoH: DNS over HTTPS
### DNSSEC

no cache poison
authenticate with signatures the response 
NS signs their responses

chain of trust

RRSIG record type - signature of the record

how to know if it's the right key
request hash from server above
hash hast to match the key received

...

recap notes on DNSSEC bassed on [nice article](https://www.cloudflare.com/en-gb/learning/dns/dnssec/how-dnssec-works/)


<span style="color:rgb(219, 0, 0)">DNSSEX adds crypto signatures to DNS records</span>
These digital signatures are stored in DNS name servers alongside common record types like A, AAAA, MX, CNAME, etc. By checking its associated signature, you can verify that a requested DNS record comes from its authoritative name server and wasn’t altered.

DNSSEC adds a few new DNS record types:
- <span style="color:rgb(219, 0, 0)"><b>RRSIG</b></span> - Contains a cryptographic signature
- <span style="color:rgb(219, 0, 0)"><b>DNSKEY</b></span> - Contains a public signing key
- <span style="color:rgb(219, 0, 0)"><b>DS</b></span> - Contains the hash of a DNSKEY record
- **NSEC** and **NSEC3** - For explicit denial-of-existence of a DNS record
- **CDNSKEY** and **CDS** - For a child zone requesting updates to DS record(s) in the parent zone

#### Zone-Signing Keys
Each zone in DNSSEC has a zone-signing key pair (ZSK): the private portion of the key digitally signs each RRset in the zone, while the public portion verifies the signature. To enable DNSSEC, a zone operator creates digital signatures for each RRset using the private ZSK and stores them in their name server as RRSIG records. This is like saying, “These are my DNS records, they come from my server, and they should look like this.”

However, these RRSIG records are useless unless DNS resolvers have a way of verifying the signatures. The zone operator also needs to make their public ZSK available by adding it to their name server in a DNSKEY record.  
  
When a DNSSEC resolver requests a particular record type (e.g., AAAA), the name server also returns the corresponding RRSIG. The resolver can then pull the DNSKEY record containing the public ZSK from the name server. Together, the RRset, RRSIG, and public ZSK can validate the response.

<span style="color:rgb(219, 0, 0)">If we trust the zone-signing key in the DNSKEY record, we can trust all the records in the zone. But, what if the zone-signing key was compromised? We need a way to validate the public ZSK.</span>
#### Key-Signing Keys

In addition to a zone-signing key, DNSSEC name servers also have a key-signing key (KSK). The KSK validates the DNSKEY record in exactly the same way as our ZSK secured the rest of our RRsets in the previous section: It signs the public ZSK (which is stored in a DNSKEY record), creating an RRSIG for the DNSKEY.

![[Pasted image 20260209120548.png]]

Of course, the DNSKEY RRset and corresponding RRSIG records can be cached, so the DNS name servers aren’t constantly being bombarded with unnecessary requests.  
  
Why do we use separate zone-signing keys and key-signing keys? As we’ll discuss in the next section, it’s difficult to swap out an old or compromised KSK. Changing the ZSK, on the other hand, is much easier. This allows us to use a smaller ZSK without compromising the security of the server, minimizing the amount of data that the server has to send with each response.  
  
We’ve now established trust within our zone, but DNS is a hierarchical system, and zones rarely operate independently. <span style="color:rgb(219, 0, 0)">Complicating things further, the key-signing key is signed by itself, which doesn’t provide any additional trust. We need a way to connect the trust in our zone with its parent zone.</span> 


# Reviewed Notes
[[claude summary]]

![[Pasted image 20260601160122.png]]


`Client → Local Resolver → Root NS → TLD NS → Authoritative NS

Between Client and Local Resolver
- Attacker must be **close to the client** (same LAN, ARP spoof, evil twin Wi-Fi)
- Relatively easy if on same network
- Poisons just **one user's** resolution
- Less common target because impact is limited to one victim

 Between Resolver and Authoritative NS 
- Off-path attacker **doesn't need to be on the same network**
- Just needs to flood forged responses faster than the real NS replies
- If successful, poisons the **resolver's cache** → affects **all users** of that resolver (entire ISP's customers)
- **Highest impact** — one successful attack affects thousands/millions of users
- This is why it's the classic cache poisoning target

Compromised Resolver Itself
- Attacker doesn't spoof at all — they **directly control** the resolver
- Via malware, DNS-changing trojans, or compromising the ISP's resolver
- No race condition needed — just serve wrong answers directly


### DNS Spoofing or DNS cache poisoning
an attacker tricks a resolver into returning a wrong IP

<span style="color:rgb(219, 0, 0)">how to poison DNS cache? forge a DNS response and race with the original one</span>

**DNS Response Forgery:** Since DNS runs over UDP (port 53), an attacker can simply send a forged packet impersonating the authoritative NS IP with a fake answer. It's a **race condition** — the forged response competes with the real one, and whichever arrives first wins. Crucially, this does **not** require a MITM position — an off-path attacker can attempt it.

to forge a response we have to match DNS query
#### DNS Query-Response Matching
For a forged response to be accepted, the attacker must match several fields:
- **Destination IP** — resolver's IP (known)
- **Source IP** — authoritative NS IP (predictable/known)
- **Source port** — DNS uses port 53 (known)
- <span style="color:rgb(219, 0, 0)"><b>Destination port</b></span> — unknown, 16-bit number (ephemeral port the resolver used)
- <span style="color:rgb(219, 0, 0)"><b>Query ID</b></span> — unknown 16-bit number
- **Queried name** — must match the domain being resolved

so we have to guess destination port and query ID
### How to make the guessing harder
randomize

**Query ID randomization:** Old nameservers incremented the Query ID by 1 each time, making it trivially predictable. An attacker who controls a domain (`test.badguy.com`) can trigger a query and observe the ID. Fixed by 2008 for most servers.

**Source port randomization:** Using a random UDP source port for each query is an effective countermeasure — 65535 theoretical ports (minus the reserved <1024). However, **NAT devices** can undermine this: a NAT may serialize or restrict the UDP source ports it maps, dramatically reducing the effective entropy (e.g. mapping many internal random ports to a small sequential external range).


### Defenses against cache poisoning
1. Randomized Query ID + source port
2. Choose random source IP and random authoritative server IP per query
		many domains have more than one authoritative name server
		ask local resolver to randomize which auth ns gets communicated with (do not reach out to the same one all the time), so the attack has to do additional guess on which one to spoof
3. Limit simultaneous DNS queries for similar resources
		related to Kaminsky attack?
4. **0x20 encoding:** Randomize casing of the queried name (e.g. `WwW.Ru.nL` instead of `www.ru.nl`) — the response must echo the exact casing back, making forgery harder
		suppose: `www.ru.nl`contains 8 alphabetic characters.
		each letter can be either upper case or lower case which gives
		`2^8 = 256` possible capitalization patterns
		these are not the same: `WWW.RU.NL, WwW.Ru.NL, wwW.rU.nL`


### DNSSEC
DNS Security Extensions

DNSSEC addresses <span style="color:rgb(219, 0, 0)"><b>integrity and authenticity</b></span> — ensuring DNS responses haven't been tampered with. It does **not** provide confidentiality.

**Core idea — Chain of Trust:** Starting from the Root, each level signs its records with cryptographic keys. The resolver can verify each signature up the chain.

**Two key types per zone:**

- **ZSK (Zone Signing Key)** — signs the actual DNS records (RRSets) in RRSIG records
- **KSK (Key Signing Key)** — signs the DNSKEY record itself (i.e. signs the ZSK)

**How it works:**
1. A DNSSEC-aware client sets the **DO bit = 1** in its query
2. The authoritative NS returns the requested record **plus an RRSIG** (signature over the record set, signed with the ZSK)
3. The resolver fetches the zone's **DNSKEY** (public key) to verify the signature
4. To trust the DNSKEY, the parent zone provides a **DS record** (Delegation Signer = hash of the child's KSK), signed with the parent's ZSK
5. This chains all the way up to the Root, whose KSK is a **trust anchor** hardcoded into resolvers

**DNSSEC Validation Process (for `www.ru.nl`):**

1. Verify RRSIG on Root zone's DNSKEY using Root KSK (trusted anchor)
2. Verify RRSIG on Root zone's DS record using Root ZSK
3. Verify `.nl` KSK hash against DS from Root
4. Verify RRSIG on `.nl` DNSKEY using `.nl` KSK
5. Verify RRSIG on `.nl` DS record using `.nl` ZSK
6. Verify `ru.nl` KSK hash against DS from `.nl`
7. Verify RRSIG on `ru.nl` DNSKEY using `ru.nl` KSK
8. Verify RRSIG on CNAME record (`www.ru.nl`) using `ru.nl` ZSK
9. Verify RRSIG on A record (`wwwproxy.ru.nl`) using `ru.nl` ZSK

RRSIGs are valid for 1–2 weeks and can be cached. Tools like **dnsviz.net** visualize the DNSSEC chain for any domain.



```
parent zone
  └─ DS record  =  hash of child's public KSK
                        ↓ (resolver compares)
child zone
  └─ public KSK  →  verified ✓
        └─ signs → public ZSK  →  verified ✓
                └─ signs → DNS records  →  verified ✓
```

![[Pasted image 20260601182136.png]]

![[Pasted image 20260601182111.png]]

1. Verify the RRSIG on the DNSKEY records from the Root zone using its KSK  (trusted)  
2. Verify the RRSIG on the DS record from the Root zone using its ZSK  
3. Verify the KSK hash of .nl using the DS record from the Root zone  
4. Verify the RRSIG on the DNSKEY records from the .nl zone using its KSK  
5. Verify the RRSIG on the DS record from the .nl zone using its ZSK  
6. Verify the KSK hash of ru.nl using the DS record from the .nl zone  
7. Verify the RRSIG on the DNSKEY records from the ru.nl zone using its KSK  
8. Verify the RRSIG on the CNAME record (www.ru.nl) using the ru.nl zone’s ZSK  
9. Verify the RRSIG on the A record (wwwproxy.ru.nl) using the ru.nl zone’s ZSK


note that root and TLD does not have a signature over NS record, only DS
if NS happens to be tampered, the malicious NS won't have a correct public key that would match with hash, DS record, from parent name server


### QNAME Minimization
Confidentiality Issues
plain DNS leaks information - the full query name is visible to root, also anyone on teh path can see exactly what domains you want to resolve

**QNAME Minimization (RFC 7816):** Each DNS level only receives the minimum information relevant to it. The root only sees `_.com`, the TLD only sees `_.slack.com`, and only the authoritative NS sees the full `stuff.slack.com`. This reduces exposure.

**DNS Encryption solutions:**
- **DNS over DTLS** — DNS over Datagram TLS
- **DoT (DNS over TLS)** — encrypts DNS traffic over TCP with TLS
- **DoH (DNS over HTTPS)** — DNS wrapped inside HTTPS (TCP → TLS → HTTP → DNS), often via CDNs like Cloudflare. Encrypts only the client-to-resolver link; resolver-to-NS links remain unencrypted unless they also use DoT/DoH.


### DNSSEC: Authenticated Denial of Existence (NSEC)

The Problem
DNSSEC must authenticate not only positive answers ("this record exists") but also negative answers:
- **NXDOMAIN**: the queried domain name does not exist.
- **NODATA**: the domain name exists, but the requested RR type does not.

A naive solution would be to sign a statement such as:
> "This domain does not exist."
However, this would be vulnerable to **replay attacks**. An attacker could capture a signed NXDOMAIN response and reuse it to falsely deny the existence of other domain names.

Additionally, generating signatures for every query would require **online signing**, which is inefficient and operationally undesirable.
DNSSEC therefore proves non-existence **indirectly** using **NSEC records**.
### NSEC Records

**NSEC (Next Secure)** records provide authenticated statements about the ordering of names in a DNS zone. The owner names in a zone are sorted canonically. For each existing name `x`, an NSEC record specifies the next existing name `y`.

```text
x NSEC y
```

This means:
> `x` exists, `y` exists, and no owner names exist between them.
Because NSEC records are signed with an RRSIG, resolvers can trust these statements.

### NXDOMAIN Proof

To prove that a queried name `qname` does not exist, the server returns an NSEC record whose interval covers the queried name:
```text
x NSEC y
```

The resolver verifies:
```text
x < qname < y
```
If the queried name falls between `x` and `y`, then it cannot exist in the zone.

### Example
Suppose the zone contains:
```text
apple.example.com
banana.example.com
orange.example.com
```

The server returns:
```text
banana.example.com NSEC orange.example.com
```

For the query:
```text
carrot.example.com
```

Since:
```text
banana < carrot < orange
```
the resolver concludes that `carrot.example.com` does not exist.

### NODATA Proof

Sometimes the queried name exists, but the requested record type does not.
Example:
```text
www.example.com    A
www.example.com    AAAA
```

Query:
```text
MX www.example.com
```

The server returns the NSEC record for `www.example.com`.
The NSEC contains a bitmap of the RR types that exist at that name:
```text
A AAAA RRSIG NSEC
```

Since `MX` is not listed, the resolver has authenticated proof that:
```text
www.example.com exists
but no MX record exists
```

This is a **NODATA** response.

## Replay Attack Analysis

### Case 1: Query Outside the Interval

An attacker replays:
```text
x NSEC y
```

for a name outside the interval.
The resolver checks:
```text
x < qname < y
```
which fails.
**Result:** Validation fails.

### Case 2: Query Inside the Interval
If another queried name also falls within:
```text
x < qname < y
```
the proof remains valid.
This is not a security problem because both names genuinely do not exist.

### Case 3: Query Equals a Boundary
If:
```text
qname = x
```
or
```text
qname = y
```
the NSEC record actually proves that the queried name exists.
Therefore the resolver will not accept an NXDOMAIN interpretation.

## Understanding the Dig Output

Command:
```bash
dig tttt.science.ru.nl NSEC
```

Output:
```text
tttt.science.ru.nl. 1800 IN NSEC 3dboxes-srv.science.ru.nl. TXT RRSIG NSEC
```

This means:
- Owner name (`x`) = `tttt.science.ru.nl`
- Next name (`y`) = `3dboxes-srv.science.ru.nl`

Since an NSEC record exists at `tttt.science.ru.nl`, the domain itself exists.
The record simply states that there are no names between:
```text
tttt.science.ru.nl
```
and
```text
3dboxes-srv.science.ru.nl
```


## How to Observe an NXDOMAIN Proof
Query a likely non-existent name:
```bash
dig random123456.science.ru.nl A +dnssec
```

The response should contain:
```text
status: NXDOMAIN
```
along with one or more NSEC records.

Find the interval:
```text
x NSEC y
```

and verify:
```text
x < random123456.science.ru.nl < y
```
If true, the NSEC record proves that the queried name does not exist.


## Wildcards and Additional NSEC Proofs
If a wildcard exists:
```text
*.science.ru.nl
```

DNSSEC may need to return multiple NSEC records.
The server must prove:
1. The queried name does not exist.
2. The wildcard is the correct source of the response.
Therefore wildcard responses often contain additional NSEC records.

## Zone Walking
A disadvantage of NSEC is that it reveals neighboring domain names. Example:
```text
tttt.science.ru.nl NSEC 3dboxes-srv.science.ru.nl
```

Then:
```bash
dig 3dboxes-srv.science.ru.nl NSEC
```

returns:
```text
3dboxes-srv.science.ru.nl NSEC 45t-srv.science.ru.nl
```

Repeating this process reveals more names in the zone.
This technique is called **Zone Walking**.

## NSEC3
To reduce information leakage caused by zone walking, many DNSSEC deployments use **NSEC3**.
Instead of exposing plaintext domain names, NSEC3 stores hashes of names, making enumeration significantly more difficult.



query examples

```bash
dig @1.1.1.1 foobar.science.ru.nl MX +dnssec +multi

; <<>> DiG 9.18.39-0ubuntu0.24.04.3-Ubuntu <<>> @1.1.1.1 foobar.science.ru.nl MX +dnssec +multi
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21295
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 6, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 1232
;; QUESTION SECTION:
;foobar.science.ru.nl.	IN MX

;; AUTHORITY SECTION:
science
ru.nl.		1800 IN	SOA ns1.science.ru.nl. hostmaster.science.ru.nl. (
				2113717520 ; serial
				3600       ; refresh (1 hour)
				900        ; retry (15 minutes)
				1814400    ; expire (3 weeks)
				1800       ; minimum (30 minutes)
				)
science.ru.nl.		1800 IN	RRSIG SOA 8 3 86400 (
				20260630071102 20260602061102 62436 science.ru.nl.
				YvK2GwnaC0NeiebLWM3b0v2sQnnlMk058u0PZtccY4rh
				6wnTAcm9DVBrU0FOQXcM87FM4vC3hr/5fcgq9vBEXIht
				XSu+kO2v+1d13qd9uj3Sk0HV+dr7t4Z7f0t4+BI+QAQt
				TWro4RLrt7/VAdU6i0MPrBqYKPxcG+K1yQNyt6E= )
folia.science.ru.nl.	1800 IN	NSEC foodwebgm-srv.science.ru.nl. CNAME RRSIG NSEC
folia.science.ru.nl.	1800 IN	RRSIG NSEC 8 4 1800 (
				20260618125456 20260521122529 62436 science.ru.nl.
				QZZpybvUkwie24E7adQNp5lMLF7sNRoMjXrWnHoiaGOx
				mysGUsdpxQS+AWSOYUUHd8LCm1cHa7to9I9PungtlDEr
				29sjoFcs4sbCpG3E/TKafIUy7u1rq4bKqHyAxWTtKTN+
				ID+yid+9oJ2ahIzzZYmv2/N7THUjkRtbKDu8jYw= )
*.science.ru.nl.	1800 IN	NSEC 3dboxes-srv.science.ru.nl. TXT RRSIG NSEC
*.science.ru.nl.	1800 IN	RRSIG NSEC 8 3 1800 (
				20260618125456 20260521122529 62436 science.ru.nl.
				mDlgeUKlicI7K79bkTF9wODYOa3SDXvcuNSI7+HkTwCx
				EYFPRGMHxwIWjDTV5jdycaRLQ8otEEwv1OxsB3am4Mfv
				ulOcIsHTy3OHKC7NKm+vytYnrJ2F8JupXX45J2IVcMXO
				/TdS7ozsVK+ApOUxtI4lo2Qa392wSsF1/vS6V7U= )

;; Query time: 23 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Tue Jun 02 11:01:36 CEST 2026
;; MSG SIZE  rcvd: 723

```

within AUTHORITY section we can see NSEC record that gives us
`folia.science.ru.nl` NSEC `foodwebgm-srv.science.ru.nl`
which means that foobar does not fall between these two CNAMES, hence does not exist
NSEC also give wildcard response


now we ask for a record that does not exist for a domain that does exist
```bash
dig @1.1.1.1 folia.science.ru.nl MX +dnssec +multi

; <<>> DiG 9.18.39-0ubuntu0.24.04.3-Ubuntu <<>> @1.1.1.1 folia.science.ru.nl MX +dnssec +multi
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 33099
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 2, AUTHORITY: 4, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 1232
;; QUESTION SECTION:
;folia.science.ru.nl.	IN MX

;; ANSWER SECTION:
folia.science.ru.nl.	600 IN CNAME mlp01.science.ru.nl.
folia.science.ru.nl.	600 IN RRSIG CNAME 8 4 600 (
				20260618125456 20260521122529 62436 science.ru.nl.
				boBxrXqJY6wUoY2HtQzYJs2vhnUL65/nKqFHSmOWuNHX
				GbK1WRDmafGtnoXWD93uPxAtwJ+R+Fu6C7TXFngZqZLG
				xuf0VmaDjNAqkOHjGX9kPuzRQu+dz6vRidTumnoLMAJV
				xIMzRCn/fcbEHJm2tLJ6r1Af0V+1AR4HJFXP3Ec= )

;; AUTHORITY SECTION:
science.ru.nl.		1800 IN	SOA ns1.science.ru.nl. hostmaster.science.ru.nl. (
				2113717520 ; serial
				3600       ; refresh (1 hour)
				900        ; retry (15 minutes)
				1814400    ; expire (3 weeks)
				1800       ; minimum (30 minutes)
				)
science.ru.nl.		1800 IN	RRSIG SOA 8 3 86400 (
				20260630071102 20260602061102 62436 science.ru.nl.
				YvK2GwnaC0NeiebLWM3b0v2sQnnlMk058u0PZtccY4rh
				6wnTAcm9DVBrU0FOQXcM87FM4vC3hr/5fcgq9vBEXIht
				XSu+kO2v+1d13qd9uj3Sk0HV+dr7t4Z7f0t4+BI+QAQt
				TWro4RLrt7/VAdU6i0MPrBqYKPxcG+K1yQNyt6E= )
mlp01.science.ru.nl.	1800 IN	NSEC mlp01-vif-114-201.science.ru.nl. A HINFO RRSIG NSEC
mlp01.science.ru.nl.	1800 IN	RRSIG NSEC 8 4 1800 (
				20260618131455 20260521122530 62436 science.ru.nl.
				h784/8zk3FQUQFQ6ZrgnbeWZuNNelQK2J9QdAC6ZnKsP
				3J2haOMFgY4M08TiDoTN0PbqRHactShwMsPE6nq7ls9t
				eJF8f/JI4sLo3elqwTfD+l1b+vfiSrm50qv7nFrpXJqu
				wxtV2ew14O4KZGxMbaiNvKeIuCUn7T6Lf1DrvMs= )

;; Query time: 42 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Tue Jun 02 11:03:46 CEST 2026
;; MSG SIZE  rcvd: 691

```

under AUTH section we see
`NSEC mlp01-vif-114-201.science.ru.nl. A HINFO RRSIG NSEC`
these are all resource records that are available (bitmap of RRs that exits), MX is not there

other takeaways

set up your own resolver on localhost
```bash
options {  
	directory "..."; # adjust path, Windows: C:\Program Files\ISC BIND 9\etc  
	listen-on port 53 { 127.0.0.1; }; # only listens on localhost  
	allow-query { localhost; }; # restricts who can query BIND  
	recursion yes; # this server will perform queries recursively  
	# i.e., it is not a simple authoritative NS  
	dnssec-validation no; # for now, does not validate DNSSEC records  
};
```

`dnssec-validation auto` meants that we will finally verify DNSSEC records
will rely on trust anchors to validate the records


fully validate a path use `delv

```bash
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

check key length:
```bash
dig @1.1.1.1 nl. DNSKEY +multi

; <<>> DiG 9.18.39-0ubuntu0.24.04.3-Ubuntu <<>> @1.1.1.1 nl. DNSKEY +multi
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41722
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;nl.			IN DNSKEY

;; ANSWER SECTION:
nl.			71 IN DNSKEY 256 3 13 (
				ArhUNmeRU9NE4fAbORd6DjntZ0vDP3PoQotzRxGg/gdn
				iUCSGp/naJdkcG5/Y9+SVE6f5h/NZCqGy2+Kh/FeEw==
				) ; ZSK; alg = ECDSAP256SHA256 ; key id = 467
nl.			71 IN DNSKEY 256 3 13 (
				K6vIyw43NeqqIe4vXEskU84cusiwHji866W22QpDbFTR
				Qk1h1xJq4qDIpPpWNHZ86mWGvw7W3nD15Lb/wrCAyw==
				) ; ZSK; alg = ECDSAP256SHA256 ; key id = 41543
nl.			71 IN DNSKEY 257 3 13 (
				aeDdFmc/JLPyva7Y4bS2SFbfWmxaiSrnqwgs+D1PKSPS
				ruxIRH+6gHLhJ4XYIzrSaT3uk6rsx3c5jV8U4B8O+g==
				) ; KSK; alg = ECDSAP256SHA256 ; key id = 17153

;; Query time: 44 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Tue Jun 02 12:13:50 CEST 2026
;; MSG SIZE  rcvd: 271

```


all keys use ECDSHA 256
 to query zoen `.nl` type dot at the end `nl.`
