
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




# Improved <span style="color:rgb(219, 0, 0)">notes</span> on VPN

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
can connect to a malicious proxy

....



# IPSec
old standard
separate authentication from confidentiality
authentication header - authenticate the origin of the traffic
	but not confidentiality (not encrypted)

encapsulation - add encryption on top of authentication

transport mode
tunnel mode



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


has two modes:
- TLS mode - does requires additional pre-distributed HMAC keys to all users
		on top of TLS we want HMAC Keys
- static mode - static pre-shared keys

has two channels:
- control channel
- data channel


establish a TLS session (a channel)
don't use the channel for traffic
but use to negotiate keys, used for actual data encryption

![[Pasted image 20260608165626.png]]



(1) Client sends `P_CONTROL_HARD_RESET_CLIENT_V1/2` - "I want a fresh connection"
(2) Server replies `P_CONTROL_HARD_RESET_SERVER_V1/2` - "Acknowledged, let's start"
(3) Client sends `P_ACK_V1` - acknowledges the server's reset (reliability layer)

(4a) Client sends TLS ClientHello **inside** a `P_CONTROL_V1` packet - TLS handshake begins
(4b) Server replies with its TLS handshake message (ServerHello, certificate, etc.)
(4c) Both sides exchange further messages over the **temporary TLS tunnel** 
	this is where key material is derived, either via **nonce exchange** or **TLS Exporter**
		control channel - <span style="color:rgb(219, 0, 0)">use to derive sesh keys</span>

(5) Client sends final `P_ACK_V1` - control channel setup complete
(6) `P_DATA_V1/2` packets flow - the actual VPN tunnel is live
(7) `P_CONTROL_SOFT_RESET` - server initiates a **key renegotiation** (happens periodically without dropping the tunnel)

<span style="color:rgb(219, 0, 0)">the actual VPN tunnel - datat channel - is not over TLS, that would introduce overhead<br>use TLS to simply communicate the keys</span>

#### Control Channel
- Used for **authentication, key exchange, and session management**
- Runs **TLS** inside `P_CONTROL_V1` packets — essentially TLS-within-UDP
- Reliable: has its own **acknowledgement system** (`P_ACK_V1`) because UDP has no built-in delivery guarantees
- Carries the handshake, certificates, and key material negotiation
- Lower throughput, higher importance for security setup

#### Data Channel
- Used for the actual **encrypted VPN traffic** (your browsing, downloads, etc.)
- Uses `P_DATA_V1/2` packets: **header + payload + HMAC tag**
- Keys are derived from what was negotiated in the control channel
- Optimized for **high throughput**, uses symmetric cipher (e.g. AES-GCM) + HMAC for integrity
- Much faster than the control channel — no TLS overhead per packet


`tap` tunnel full ethernet frames, bridges two LANs together, can have broadcast
`tun` tunnel IP packets only (IPv4 and IPv6), faster performance
	not broadcast 

```
Layer 7 Application (HTTP, DNS, SSH...)
Layer 4 Transport (TCP, UDP)
Layer 3 Network (IP addresses, routing)
Layer 2 Data Link (MAC addresses, Ethernet frames)
Layer 1 Physical (actual cables, radio waves)
```


### TUN  Layer 3 tunnel
- The virtual `tun0` interface sits at the **IP layer**
- OpenVPN grabs raw **IP packets** from the OS and tunnels them
- Your OS thinks `tun0` is just another network interface, sends IP packets to it
- Those IP packets get encrypted and sent as P_DATA

```
Your app → TCP segment → IP packet → tun0 → OpenVPN encrypts → P_DATA → internet
```

**Why faster?** Less data per packet — you strip the Ethernet header, only tunnel the IP payload. Also no broadcast traffic to deal with.

**What you lose:** anything that lives purely at Layer 2 — broadcasts, multicasts, MAC-based protocols.


### TAP Layer 2 tunnel
- The virtual `tap0` interface sits at the **Ethernet layer**
- OpenVPN grabs entire **Ethernet frames** — MAC headers and all
- The remote network appears as if you're **physically plugged into it** via a virtual cable
- You can bridge two LANs together as if they were one

```
Your app → TCP segment → IP packet → Ethernet frame → tap0 → OpenVPN encrypts → P_DATA → internet
```

**What you gain:** full Layer 2 behaviour — broadcasts, multicasts, and protocols that never even reach Layer 3.

### The Security/Hacking angle — why TAP matters
The slide mentions two specific protocols:
#### LLMNR (Link-Local Multicast Name Resolution)
- A **Layer 2 broadcast** protocol — when a Windows machine can't resolve a hostname via DNS, it shouts out to the whole local network: _"hey, does anyone know where 'FILESERVER' is?"_
- Because it's a **broadcast**, it never crosses a router — it stays on the LAN
- With TUN, you'd **never see these** — they die at the router
- With TAP, you're **virtually on the same LAN**, so you receive those broadcasts


<span style="color:rgb(219, 0, 0)">heavy protocol</span>

# WireGuard
proposed around 2017

strip away a lot of things that are optional

core concept - bind public keys and IP address
route traffic to a key (IP address)

protocols are hardcoed - no negotiation of which protocols to use
(TLS has negotiation which protocols to use)
use only:
	Curve25519, 256-bit ChaCha20, 128-bit Poly1305

only peer-to-peer
no client-server configuration


WireGuard doesn't <span style="color:rgb(219, 0, 0)">negotiate</span> algorithms — you get exactly these:

| Purpose                  | Primitive                |
| ------------------------ | ------------------------ |
| Key exchange             | Curve25519 (ECDH)        |
| Symmetric encryption     | ChaCha20-Poly1305 (AEAD) |
| Hashing / key derivation | BLAKE2s                  |
| Handshake MAC            | HMAC-BLAKE2s             |
| Session key derivation   | HKDF                     |

protocols that negotiate ciphers (like TLS) are vulnerable to downgrade attacks where an attacker forces both sides to agree on a weaker cipher


### The Identity Model: Cryptokey Routing

WireGuard's most distinctive design idea is **cryptokey routing**. Instead of usernames, passwords, or certificates, identity is entirely based on Curve25519 public keys. Each peer has a static keypair. The association is simple:

> "This public key is allowed to send/receive traffic claiming to be from these IP prefixes."

Each interface maintains a table like this:

```
Peer Public Key          → AllowedIPs
xTIB...p8Dg             → 10.192.122.3/32, 10.192.124.0/24
TrMv...WXX0             → 10.192.122.4/32, 192.168.0.0/16
gN65...z6EA             → 10.10.10.230/32
```

**On sending:** when a packet is destined for an IP in a peer's AllowedIPs, it gets encrypted and sent to that peer's endpoint

**On receiving:** a decrypted packet is only accepted if its source IP falls within the sending peer's AllowedIPs. This dual enforcement means cryptographic authentication and routing policy are unified — a peer cannot spoof source IPs outside their AllowedIPs range, because the decryption itself enforces the constraint.


<span style="color:rgb(219, 0, 0)">works based on IP ranges</span> 

how do we know the keys?


routing - tun only, route IP only

<span style="color:rgb(219, 0, 0)">no PKI/certificates baked into protocol, simply trusts configured public keys</span>

goal - service does not respond if a guy connects from "outside" of allowed IP range
does not respond - remain stealthy

does not get or give feedback if not authenticated

but
you are harder to discover (censorship context)
public scan of internet - can't map who hosts wireguard node


# The fundamental concept: Cryptokey Routing

This is the most important WireGuard innovation.

A configuration might look like:

```
Peer Public Key: ServerKey
Allowed IPs: 10.0.0.1/32

Peer Public Key: LaptopKey
Allowed IPs: 10.0.0.2/32
```

WireGuard uses this table in **both directions**:

### Sending

When a packet is sent to:

```
Destination = 10.0.0.1
```

WireGuard looks up:

```
10.0.0.1 → ServerKey
```

and encrypts the packet for the server.

### Receiving

When a packet is decrypted from `ServerKey`, WireGuard verifies:

```
Source IP == 10.0.0.1
```

If not, the packet is dropped.

So a peer cannot pretend to be another IP address even after successful decryption.

[[wireguard chat summary]]
