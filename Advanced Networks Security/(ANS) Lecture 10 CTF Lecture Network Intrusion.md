
**Network Intrusion Detection rule engineering with Suricata** - Workshop on rule specificity and coverage

prep lecture + CTF
upload logs of copilot

intrusion-detection systems detect attacks against compute systems and 
![[Pasted image 20260423185355.png]]

intrusion detection systems automatic pre-filter for human operators
network traffic is fast - human can't inspect manually 
these systems - narrow down terabytes of traffic data to few cases that the human has to inspect
<span style="color:rgb(219, 0, 0)">based on rule engineering</span>

aim of study - how AI can help to engineer the rules

main focus - detection rule engineering

![[Pasted image 20260423185610.png]]


some basic info on wireshark

# Suricata
![[Pasted image 20260423190139.png]]

streams, identified by
![[Pasted image 20260423191055.png]]

then specify protocol you want to protect

action: use alert in CTF challenges (aim at detecting)
header: which protocol we want to protect, then specify direction of traffic
	the signature will look at packets that originate from mother network (want to protect) got to external networks
option: only 2 mandatory option fields - msg and sid (signature id)


![[Pasted image 20260423191846.png]]
![[Pasted image 20260423191905.png]]


```
alert http any any -> $HOME_NET 80 
(msg:"First rule"; content:"Wget"; sid:1;)
```

`alert` - **generate an alert** when the rule matches
`http` - the rule applies to **HTTP traffic** 
`any any` - source IP and port, means: from **any IP address, any port**

`$HOME_NET 80` - destination:
    - `$HOME_NET` = your internal network (a variable defined elsewhere)
    - `80` = **HTTP port**

`(...)` - rule operations

`content:"Wget"`**
 - this is the key part (highlighted in your image)
- the system looks for the **string `"Wget"` inside the HTTP payload**

`sid:1` - unique rule ID, used to identify this rule


![[Pasted image 20260423193004.png]]

These are keywords used for payload matching and payload transformation in a network inspection/filtering context. Concise explanation of each item:

- content: Matches literal data bytes in the network payload (case-sensitive). You can express bytes as hex like "\x20" for space.
    
- pcre: Uses a PCRE regular expression to match the payload. Bytes may be written as hex escapes (e.g., "\x20").
    
- offset, depth: Modifiers that limit where the search runs:
    
    - offset: start the search at a specific byte index in the payload (absolute).
    - depth: limit how many bytes from that offset are searched.
- distance, within: Modifiers used when matching relative to a previous match:
    
    - distance: minimum number of bytes after the previous match where the next match may begin.
    - within: maximum number of bytes after the previous match to consider.
- isdataat: Checks that there are at least N bytes available at a given position (useful to ensure a full field is present before extracting).
    
- byte_test: Extracts a number of bytes and performs a numeric comparison on that value (e.g., check a length field). The operator applies to the extracted bytes interpreted as an integer; you can specify endian/bit-shift behavior (e.g., bitmask/shift).
    
- byte_extract / bitmask/shift (implied): Operators or transformations applied to extracted bytes to compute the final numeric result (useful when fields are packed or use bit flags).
    
- payload transformation keywords (visual boxes in the image): show examples where payload data is extracted, shifted, masked, then compared or used for further checks.


![[Pasted image 20260423193034.png]]

![[Pasted image 20260424133749.png]]


trigger only when there are 5 alerts within 360 seconds

purpose - limit alerting so you only generate notifications after a rule matches enough times, or at most a set number of times, within a time window

`by_src`— per source IP address, so count triggers per source IP

https://docs.suricata.io/en/suricata-8.0.4/


![[Pasted image 20260424134434.png]]![[Pasted image 20260424134620.png]]

## proxies

- Proxies can simplify detecting complex behavior because many malware samples use a common HTTP User-Agent (UA) string when making Go-http-client or similar proxied outbound requests — that makes them easy to spot.
- Downside: relying on proxies (or their default UA) risks false positives by also detecting legitimate applications that use the same default UA. In other words, proxy-based signatures can catch benign traffic.
    
- When proxies are acceptable:
    
    - Encrypted traffic where inspection isn't possible (e.g., SSH) — proxy heuristics may be one of the few ways to detect anomalous behavior.
    - Very complex behaviors that the detection engine cannot model (e.g., advanced application-layer flows not supported by the engine) — in those edge cases proxies or middleboxes may be the only practical approach.
- Recommendation: avoid using proxies as a primary detection mechanism because of high false-positive risk; use them only when necessary and combine with additional checks (context, other indicators, rate/thresholding, destination reputation) to reduce false alerts.

![[Pasted image 20260424135843.png]]
![[Pasted image 20260424140027.png]]
![[Pasted image 20260424140441.png]]
![[Pasted image 20260424140501.png]]



# CTF Scenarios

## Baby scenario
detect dns request
prevent false positives on benign scenario

## reconnaissance scenario - phpinfo function

![[Pasted image 20260424140752.png]]
![[Pasted image 20260424141014.png]]


## Initial Access - phishing
detect webpage that contains the malicious code that initiates the download
![[Pasted image 20260424141202.png]]
![[Pasted image 20260424141218.png

## C2
![[Pasted image 20260424141325.png]]

TRICKY challege
look at the handshake
as the traffic is encrypted after



### CTF
baby rule
`alert dns $HOME_NET any -> any 53 (msg:"DNS query for detectme.foobar"; dns.query; content:"detectme.foobar"; nocase; sid:1000001;)


rat

`alert tls 10.0.2.15 any -> 23.94.236.147 any (msg:"RAT C2 TLS detected via JA3 fingerprint"; flow:established,to_server; ja3.hash; content:"fc54e0d16d9764783542f0146a98b300"; classtype:trojan-activity; sid:9000008; rev:1;)

worked !!



php

```
alert http $HOME_NET any -> any any (
    msg:"Reconnaissance - phpinfo.php accessed";
    flow:to_server,established;
    http.uri;
    content:"/phpinfo.php";
    nocase;
    sid:6000001;
    rev:1;
)
```

malware
`alert http $HOME_NET any -> any any (msg:"Malware Delivery - Script-initiated executable download"; flow:to_client,established; file_data; content:".exe"; nocase; sid:7000001;)