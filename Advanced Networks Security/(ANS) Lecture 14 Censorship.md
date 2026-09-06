

exam notes

wont get question on 1.3

compare few things
explain

TCP sequence number (not counters)

exercises - not question on online quiz
question will focus on how bad guy can leverage CT logs

we learn all domains and subdomains that certificates are issued for
that's the focus from exercises


censorship - deny access to information
supression of speech



## How

### block DNS
drop DNS
return NXDOMAIN

DNSSEC would disallow this to work
this is the wrong asnwer
but how can I get the correct asnwer?

reroute to diff websites

### IP
firewall with blocked IP addresses
drop TCP SYN packets for block IPs
TCP reset attack


### keyword filtering
specific words are filtered out of a nation’s search engines to prevent access to content related to  that term

example - China


### throtling
loads for ever, annoynig

in Kazahstan, intercept TLS (HTTPS)
create a national security certificate
ask ISP to install that certificate

# Surveillance and self-censorship
surveillance lead to self-censorship 
psychological manipulation trick
make people self-censor them


### EU still wants to scan chat communication
for child abuse
but actually - for mass surveilance



### countries that target tool that circumvent censorship - VPNs
target VPNs themselves

how do you block VPNs

even Europe wants to do that
prevent access to VPNs  for a subset of users
for a ban of access of social media for kids
VPNs coul be used to access social media
ban VPNs for kinds


### target researchers

authors become anonymous - publish anonymously
Chinese researchers 

# Great Firewall of China

one of the most sophisticated cesnhrotisp systems
deployed for more than 2 decades
even sold to other governments

actively developed refined on daily based

collateral damage - unintentional damage


## GFC DNS Characteristics
man on the side
pasively lsitens to traffic
does not remove or edit traffic
whenever a blcoked domain gets querued
race with teh actual asnwer

allows effect of GFC not only inside but aslo outside fo chine
if china's dns server are queried

IP adres of some dropbox

### how do they block web traffic - HTTP and HTTPS
how to block access to http website

you know IP of domain

parse, allowed delays
send a reset

![[Pasted image 20260608210923.png]]


ass soon as firewall detectd domain (blcoked)
send reset in both direction
send 3x for redundancy 


### Shadowsocks
evasion technique

used to be on github develope by a chinesse student

encrypted proxy protocol
vairiant of SOCKS5

all packets appear random

1st variant
based on stream cipher
pre-shared key with server

decrypt with pre-shared key
open tcp/udp tunnel



#### shadowsocks fingerprinting

fingerprint hsadowsocks
m=legnt of message, entropy




2nd variant AEAD vairant

GFW uses the length and entropy of the first data packet in each connection to  
identify probable Shadowsocks traffic  
• It then sends probes: partial replays of past legitimate connections, and random  
probes of varied lengths  
57How China Detects and Blocks Shadowsocks, IMC 2020  
• The GFW sends a few of them in each hour to  
make the probes less noticeable and harder  
to fingerprint!  
• Probes to the potential servers trigger design-  
specific and implementation-specific  
reactions that are fingerprintable


## GFW against encryption
any sort of necrypted traffic would be blocked


entropy
looks at number of ones
if ne of ones close to 50% - classifed as random

entropy of how to measure randomness


### Evasion Techniques

domain fronting