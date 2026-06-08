
why

how ISP knows that we connect to VPN server?

VPNs
host-to-host
network-to-network
network-to-host

why host-to-host
hosts cant comm directly
firewalls block comm
plaintext traffic (inspected and blocked), we don't want that
attacker could eavesdrop and modify traffic


### Proxying/Tunneling

### HTTP proxy
GET method
carries http traffic for you

the proxy can read and modify traffic

![[Pasted image 20260522154707.png]]

proxy specific headers - proxy connection
w-forwarded-for header ltes teh server knwo that a proxy carries traffic on behalf of soemone

### HTTP Proxy Tunnel
CONNECT method

![[Pasted image 20260522155200.png]]

For HTTPS traffic the proxy cannot read, the client sends a `CONNECT domain.com:443` request to the proxy. The proxy replies `HTTP/1.1 200 Connection Established`, creating a **bi-directional TCP tunnel** through which the client then runs its TLS session directly to the destination. The proxy only sees encrypted bytes after that.
we cant proxy HTTPS traffic 

<span style="color:rgb(219, 0, 0)">the proxy only forwards encrypted traffic and cannot see the contents.</span>

bi-directional TCP connection with TLS on top
then proxy just relays traffic - established TCP connection and continues with TLS

## [HTTP tunneling](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Proxy_servers_and_tunneling#http_tunneling)

Tunneling transmits private network data and protocol information through public network by encapsulating the data. HTTP tunneling is using a protocol of higher level (HTTP) to transport a lower level protocol (TCP).

The HTTP protocol specifies a request method called [`CONNECT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/CONNECT). It starts two-way communications with the requested resource and can be used to open a tunnel. This is how a client behind an HTTP proxy can access websites using TLS (i.e., HTTPS, port 443). Note, however, that not all proxy servers support the `CONNECT` method or limit it to port 443 only.


compared to middleboxes, proxies dont terminate the TLS connection

### HTTP Proxy features
1. what if proxy accepts only https?
	
	Normally, when using an HTTP proxy, the connection between:
	client -> proxy
	is plain HTTP.
	
	<span style="color:rgb(219, 0, 0)">we have to protect the connection from client to the proxy</span>
	we can establish a TLS connection between client and proxy
	then CONNECT method happens inside the TLS connection (secured with certificate attributed to the proxy)
	proxy returns connection established
	TCP session open
	<span style="color:rgb(219, 0, 0)">TLS over TLS</span>


2. can proxy authenticate REQUESTS?
		use additional header with base64 password (havr HTTPs proxy as we don't want to share such secret in plaintext)
		proxy authenticates client 

3. can we chain proxies?
		yes
		CONNECT through one proxy to another


### SOCKS proxy
proxy whatever, not only http traffic

![[Pasted image 20260522162036.png]]

domain name resolved by proxy
vs locally

<span style="color:rgb(219, 0, 0)">version</span> SOCKS4a appends domain name
so that the proxy does the domain name resolution
benefits - IP anycast, can resolve geographically closer IP address for that domain

With plain SOCKS4:
1. Client must resolve domain name locally first
2. Then send the IP address to proxy

Your local network or ISP can still see DNS queries:
```
DNS request: example.com
```
So even though traffic goes through proxy:
- DNS leaks occur
- privacy is weaker
- censorship can still happen



<span style="color:rgb(219, 0, 0)">version</span> SOCKS5 adds authentication, supports UDP (no oteotherhr proxy tunnel supports that)
you can do name resolution over proxy

now you can stream data and forward DNS queries and more...



### SSH tunnel

![[Pasted image 20260522163825.png]]
used also to forward communication

through port forwarding
	connect to a port on a server that is not exposed
	connect to the server through ssh and then connect to that port

This slide explains **SSH tunneling** (also called **SSH port forwarding**).
The idea is:
> SSH can securely carry other TCP connections inside the encrypted SSH session.

<span style="color:rgb(219, 0, 0)">Inside that encrypted SSH connection, you can send other traffic.</span>
SSH acts like:
> a secure encrypted pipe for TCP connections.

https://www.cloudflare.com/learning/access-management/what-is-ssh/


### 1. Local Port Forwarding (`ssh -L`)
``ssh -L 123:localhost:456 sshserver``
useful because we can connect to port that are open only locally

### 2. Forwarding to another remote host
``ssh -L 123:remoteserver:456 sshserver``
now SSH server relays traffic onward

useful because  
- behind firewall
- inside private networks
- inaccessible directly


![[Pasted image 20260522171318.png]]






### web proxy auto-discovery (wpad)**

Web Proxy Auto-Discovery allows a network to automatically configure clients' proxies. Attack vectors: a rogue DHCP server can send Option 252 pointing to a malicious PAC file; DNS/LLMNR/NetBIOS-NS fallback can be poisoned to serve a rogue PAC. The PAC file is JavaScript executed by the browser, so `return "PROXY attacker-ip:8080"` silently redirects all traffic. This is a stealthy attack, especially dangerous on public hotspots.


if connected to a wrong proxy - could be malicious
rouge AP races DHCP response from ISP and tries to give you an IP address
pre-configrue number of things
activate web proxy auto discovery
all the traffic has to to proxied through attacker's proxy

Attacker includes:
```
DHCP Option 252
```
This special DHCP option specifies:
```
Where the PAC file is located
```
Example:
```
http://evil/wpad.dat
```

victim dowloads wpad.dat
PAC files are JavaScript.

They can say: use proxy evil.attacker.com:8080

2. there's a fallback attack too

mitigation
turn it off


### IPSec
![[Pasted image 20260522172929.png]]

extension to IP protocol
seperate auth and privacy concerns

ipsec in transport mode 
ipsec in tunnel mode- IPsec over IP in tunnel mode

......



### OpenVPN
custom protocol based on TLS

In <span style="color:rgb(219, 0, 0)">TLS mode</span>, a TLS handshake authenticates both endpoints (mutual
authentication) and negotiate the set of session keys
In <span style="color:rgb(219, 0, 0)">Static Key mode</span>, a pre-shared key is used

TLS mode has optional feature that requires the
pre-distribution of separate HMAC keys to all users

![[Pasted image 20260522173928.png]]



### Wirequard

bind IP address and public key

 understand limitation pros and cons
 what could go wring in certain cases

### Routing through VPN & VPN Client security

### ccc


# Improved notes on VPN

[[vpn claude sum]]

# HTTP Proxy
`X-Forwarded-For` so the server knows that a proxy carried traffic on behalf of someone

without a proxy, the client connects directly to the destination
with one, the client sends the full URL to the proxy (e.g., `GET http://domain.com/ HTTP/1.1`), and the proxy forwards a modified request, typically appending `X-Forwarded-For` to reveal the original client IP

proxy can read traffic

# HTTP Proxy Tunnel
`CONNECT`method
for HTTPS traffic, the client sends a `CONNECT domain.com:443` request to the proxy
the proxy replies with `HTTP/1.1 200 Connection Established`, after which a bi-directional TCP tunnel exists through the proxy
the proxy sees only encrypted TLS traffic and cannot inspect it

<span style="color:rgb(219, 0, 0)">compared to middleboxes, proxies dont terminate the TLS connection</span>

# HTTP Proxy Featrures
- The proxy can itself require HTTPS (TLS-wrapped connections)
- Clients can authenticate using `Proxy-Authorization` headers, supporting Basic and NTLM schemes
- Proxy chains are possible by nesting CONNECT requests
- Non-HTTP traffic can be forwarded once the tunnel is established

# SOCKS Proxy
proxy whatever, not only http traffic

<span style="color:rgb(219, 0, 0)">version</span> SOCKS4a appends domain name
so that the proxy does the domain name resolution
benefits - IP anycast, can resolve geographically closer IP address for that domain
benefits - stronger privacy as your ISP does not see what domain you connect to

with plain SOCKS4 client must resolve domain name locally first
then send the IP address to proxy

bad - our local network or ISP can still see DNS queries:
so even though traffic goes through proxy:
- DNS leaks occur
- privacy is weaker
- censorship can still happen

# SSH Tunnel
...

# Web Proxy Auto-Discovery (WPAD)
...



# IPSec



# OpenVPN

OpenVPN is a TLS-based VPN solution tunneling IP subnetworks or virtual Ethernet adapters over a single UDP or TCP port. It uses a proprietary protocol (not standardized as an RFC) with separate control and data channels.

#### Authentication Modes

- **TLS mode:** Mutual authentication via TLS handshake, negotiating session keys
- **Static Key mode:** Uses a pre-shared key
- An optional feature distributes separate HMAC keys to all users for integrity; since the key is secret, it also functions as a secondary authentication layer

#### Protocol Flow
1. Client sends `P_CONTROL_HARD_RESET_CLIENT_V1/2`
2. Server responds with `P_CONTROL_HARD_RESET_SERVER_V1/2`
3. Client acknowledges with `P_ACK_V1`
4. TLS handshake occurs inside `P_CONTROL_V1` packets, including key material exchange (either nonce exchange or TLS Exporter)
5. OpenVPN derives cipher and HMAC keys for both directions
6. Data flows as `P_DATA_V1/2` packets (header + payload + authentication tag)
7. Periodic soft resets renegotiate keys

![[Pasted image 20260607193530.png]]

#### TUN vs. TAP

- **TUN (tun interface):** Layer-3, IP packets only (IPv4 + IPv6); faster, cannot forward broadcast/multicast
- **TAP (tap interface):** Layer-2, full Ethernet frames; bridges two LANs, supports broadcast/multicast including protocols like LLMNR — relevant when tools like Responder need access to broadcast traffic

#### OpenVPN-NL
A hardened variant certified to NLNCSA Level 2, used in classified environments. It strips insecure options and uses mbed TLS instead of OpenSSL, chosen for its smaller, more auditable codebase.