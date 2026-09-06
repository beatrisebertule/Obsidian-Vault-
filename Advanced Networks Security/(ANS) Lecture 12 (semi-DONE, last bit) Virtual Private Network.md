
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



# Routing
split mode 
traffic gets routed through VPN only partially

routing table with two interfaces - regular LAN and VPN interface
![[Pasted image 20260612171533.png]]

#### The Two Interfaces

| Interface     | Address | Purpose                       |
| ------------- | ------- | ----------------------------- |
| 192.168.1.101 | /24     | Physical NIC on local network |
| 10.0.0.3      | /24     | VPN virtual interface         |

#### Key Routes Explained

**`0.0.0.0 / 0.0.0.0` → Gateway 192.168.1.1 (metric 30)**

- The **default route** — all traffic not matching a more specific rule goes through the regular internet gateway (not the VPN). This is the core of _split tunneling_.

**`10.0.0.0 / 255.255.255.0` → On-link, via 10.0.0.3 (metric 5, bolded)**

- Traffic destined for the **10.0.0.x network goes through the VPN**. Low metric (5) = high priority. This is the VPN-specific route — only this subnet is tunneled.

**`10.0.0.3 / 255.255.255.255` and `10.0.0.255 / 255.255.255.255`**

- Host route for the VPN interface's own IP, and its broadcast address. Standard auto-generated entries.

**`192.168.1.0 / 255.255.255.0` → On-link, via 192.168.1.101**

- Local LAN traffic stays on the physical interface, never touches the VPN.

**`127.x.x.x` routes**

- Standard loopback routes, nothing special.

**`224.0.0.0` and `255.255.255.255` routes**

- Multicast and broadcast routes, auto-generated for each interface.

#### Why It's Called "Split Tunneling"

```
Traffic to 10.0.0.x  →  VPN tunnel  →  Corporate network
Traffic to everything else  →  Normal internet (ISP)
```

Without split tunneling, the default route (`0.0.0.0`) would point to the VPN, forcing **all** traffic through it. Here, only the 10.0.0.0/24 subnet is routed through the VPN — hence the traffic is _split_.

### Why VPN Routes Say "On-link"

The VPN creates a **virtual network interface** on your machine. From the OS perspective, `10.0.0.3` is a local interface just like your physical NIC. So traffic destined for `10.0.0.x` is handed directly to that virtual interface — which then **encrypts and tunnels it** to the VPN server. The OS doesn't need a gateway IP because the VPN driver handles the rest invisibly.

```
Your app
   ↓
Packet to 10.0.0.x
   ↓
Routing table → "send to interface 10.0.0.3"
   ↓
VPN driver encrypts it
   ↓
Sends encrypted packet via 192.168.1.1 → internet → VPN server
```

![[Pasted image 20260612174048.png]]
#### The Clever Solution — Split the Default Route in Two

Instead of replacing `0.0.0.0/0`, the VPN adds **two more specific routes** that together cover all IPs:

```
0.0.0.0/1    (0.0.0.0   mask 128.0.0.0)  →  VPN  covers first half of all IPs
128.0.0.0/1  (128.0.0.0 mask 128.0.0.0)  →  VPN  covers second half of all IPs
```

Together these two routes cover **every possible IP address** (0.0.0.0 → 255.255.255.255), but each has a `/1` prefix — **more specific than the default `/0`** — so they always win over the old default route.

```
Old default:  0.0.0.0/0   ← still there but now always LOSES
New routes:   0.0.0.0/1   ← more specific, wins
              128.0.0.0/1 ← more specific, wins
```

The original default route is effectively dead for all normal traffic.

#### The `<gatewayIP>` Route — Answering "Why?"

This is the key entry the slide is highlighting:

```
Destination: <gatewayIP>    Netmask: 255.255.255.255    Gateway: 192.168.1.1    Interface: 192.168.1.101
```

This is a **host route** (mask `255.255.255.255` = single IP) for the VPN server's real public IP address. It says:

> _"To reach the VPN server itself, use the physical interface and home router — NOT the VPN tunnel."_

This is essential. Without it:

```
VPN encrypted packet → needs to go to VPN server
→ routing table says 0.0.0.0/1 covers that IP
→ gets sent into VPN tunnel
→ needs to go to VPN server again
→ infinite loop 💀
```

With it:

```
VPN encrypted packet → needs to go to VPN server
→ host route matches (most specific, /32)
→ sent via 192.168.1.1 (home router) → physical internet → VPN server ✓
```

full picture
```
Traffic to VPN server IP  →  /32 host route  →  physical NIC  →  internet  →  VPN server
                                                                                    ↓
All other traffic         →  0.0.0.0/1        →  VPN tunnel  →  (decrypted) →  internet
                             128.0.0.0/1
```

# IPv6 LEAKS
traffic that was meant to go over VPN - to VPN network interfaceVPN configurations use dot cosnuider only IPv4 addresses

dns resolution done by client
responds with IPv6
then tries to connect to IPV6
but IPv6 routinhg table is empty
so traffic goes to the default 0.0.0.0 adddress - traffic leak

cna be configrue to prefer IPv6


# Attacks on VPN

#### What the VPN Actually Does

When you connect to a VPN, your device creates a **virtual network interface** (called `tun0` on Linux, or similar). This is a fake network card that exists only in software.

The VPN client then:

1. Takes your original packet (e.g. a request to google.com)
2. **Encrypts** it
3. **Wraps** it in a new packet addressed to the VPN server
4. Sends that wrapped packet out your real physical interface

```
Original packet:   [IP: google.com | TCP | your data]

After VPN wraps it: [IP: VPN server | UDP | ENCRYPTED(IP: google.com | TCP | your data)]
```

This is the **tunnel** — a packet inside a packet.

#### The Core Problem

When a VPN does "full tunneling," it needs to route _all_ your traffic through the VPN interface. But it can't simply replace the default route (`0.0.0.0/0`) with the VPN, because then your device wouldn't even know how to reach the VPN gateway itself. The solution VPNs use is to add two more-specific routes (`0.0.0.0/1` and `128.0.0.0/1`) that together cover all IPs but leave room for a specific route to the VPN gateway going over your physical interface.

This design creates exploitable assumptions.

---

#### Attack 1 — IPv6 Leaks

Most VPNs only set up IPv4 routing rules. If a destination has both an A (IPv4) and AAAA (IPv6) record, the OS prefers IPv6 — and that traffic walks straight out your physical interface, unencrypted, bypassing the VPN entirely.

Even if the target site is IPv4-only, third-party resources embedded in the page (like CDN-hosted JavaScript) may have AAAA records, leaking your browsing context via the `Referer` header.

---

#### Attack 2 — <span style="color:rgb(219, 0, 0)">DNS Leaks & Hijacking</span>

A DNS leak happens when you're connected to a VPN, but your **DNS queries travel outside the tunnel** — going to your ISP's DNS server instead of the VPN's DNS server.

VPN clients that don't lock down DNS are vulnerable to a rogue access point injecting a malicious DNS server via DHCP. Even VPNs that override DNS to something like `8.8.8.8` are vulnerable: an attacker can use DHCP to assign you an IP in the `8.8.8.0/24` subnet, making `8.8.8.8` appear _local_ — so DNS queries go out your physical interface instead of through the VPN.

resolver == gateway IP

Three cases of increasing sophistication:

**Case 1 — Resolver == gateway IP:** If the DNS resolver is the same IP as the local gateway, traffic isn't tunneled. Immediate leak with no attacker needed.

**Case 2 — Resolver on LAN:** If the DNS resolver is a local IP and the VPN is split-tunnel (allowing LAN traffic), DNS goes out locally. Attacker just needs to control or intercept the local resolver.

**Case 3 — Resolver outside LAN (the clever DHCP attack):** VPN sets DNS to e.g. `8.8.8.8`. Normally this would go through the tunnel. But a rogue AP responds to DHCP first and assigns the victim `8.8.8.2/24` — putting the victim in the same /24 as `8.8.8.8`. The OS now thinks `8.8.8.8` is _local_, routes DNS directly through WiFi. Attacker at `8.8.8.1` intercepts all DNS.


### DNS Hijacking

**Active attack** — someone intercepts your DNS queries and returns **fake answers**.

Now, _how exactly do you hijack DNS?_

#### The attacker needs two things:

1. **Your DNS queries must reach them** (via a leak, or being on-path)
2. **Their answer must arrive before the legitimate one**

#### Concrete methods from the lecture:

**Method A — <span style="color:rgb(219, 0, 0)">Win the DHCP race:</span>**

- Attacker makes themselves your DNS server via a rogue DHCP response
- Now your queries _legitimately_ go to them
- They can answer anything they want

**Method B — <span style="color:rgb(219, 0, 0)">The subnet trick (8.8.8.2/24):</span>**

- Your VPN sets DNS to `8.8.8.8` (should go through tunnel)
- Attacker assigns you IP `8.8.8.2/24` via DHCP
- OS thinks `8.8.8.8` is local → queries go to attacker's gateway `8.8.8.1` instead of through VPN
- Attacker intercepts and responds with fake IPs

---

#### Attack 3 — LocalNet Attack (USENIX 2023)

A rogue AP assigns you an IP whose subnet _contains the IP address of a target server_. For example, if the target is `1.2.3.20`, the attacker gives you `1.2.3.12/24`. Your OS now adds a local route for `1.2.3.0/24` via your physical interface. When you try to reach `1.2.3.20`, the routing table sends it directly out — not through the VPN — because it looks "local." Around 66–87% of tested VPN clients were vulnerable depending on OS.

---

#### Attack 4 — ServerIP Attack (USENIX 2023)

Rather than attacking your traffic, this one hijacks the DNS query your VPN client makes _to find its own gateway_. The attacker on the LAN poisons that DNS response to point to a target server's IP instead. Your VPN client then adds a specific host route for that IP (thinking it's its gateway) via your physical interface. After the VPN connects normally (the attacker transparently forwards VPN traffic to the real gateway), traffic destined for the target server leaks out unencrypted. Over 80% of tested clients were vulnerable on most platforms.

---

#### The Common Thread

All these attacks exploit the fact that VPN clients must carve exceptions in the routing table for traffic that shouldn't go through the tunnel — and a local attacker can manipulate network configuration (via DHCP or DNS spoofing) to make arbitrary traffic fall into those exceptions.

A route like `0.0.0.0/1` means: match any IP address where the **first 1 bit** matches `0`. Since the first bit of `0.0.0.0` is `0`, this matches all IPs that start with a `0` bit — which is the entire range `0.0.0.0` to `127.255.255.255`.

Similarly, `128.0.0.0/1` means: match any IP where the first bit matches `1` (since `128` in binary is `10000000`). That covers `128.0.0.0` to `255.255.255.255`.


### 6. LocalNet Attack (USENIX 2023)

Generalizes the DHCP subnet trick beyond just DNS. The attacker sets the local network's IP range to **contain the IP of any arbitrary target server**.

Example: target is `1.2.3.4`. Attacker assigns victim `1.2.3.12/24` via DHCP. The OS adds route `1.2.3.0/24 → wlan0`. When the VPN is set up, it adds a route for `1.2.3.4/32 → wlan0` (the VPN gateway's public IP as a more-specific route). Now all traffic to `1.2.3.4` — the target — goes out unencrypted via WiFi, not through the VPN. The attacker intercepts it.

**Findings across OSes (circa 2023):**

- iOS: 100% vulnerable
- macOS: 87.5% vulnerable
- Windows: 66.7% vulnerable
- Linux: 35.7% vulnerable
- Android: 21.4% vulnerable (most secure due to stricter traffic rules)





### 7. ServerIP Attack (USENIX 2023)

More surgical. Instead of using subnet tricks, the attacker **DNS-spoofs the VPN's own gateway domain**.

Steps:

1. Victim looks up `vpn.com` to connect to the VPN
2. Attacker on the LAN intercepts the DNS query and returns `1.2.3.4/32` (the target server's IP) instead of the real VPN gateway `2.2.2.2`
3. VPN client connects to what it thinks is the VPN gateway, but is actually the target server's IP
4. OS adds route `1.2.3.4/32 → wlan0` (specific host route for "VPN gateway")
5. The attacker separately forwards the actual VPN traffic to the real gateway `2.2.2.2` so the VPN appears to work normally
6. All traffic to `1.2.3.4` now leaks outside the tunnel — victim doesn't notice

**Findings:**

- Linux: 88.2% vulnerable
- Windows: 81.8%
- macOS: 80%
- iOS: 80%
- Android: 30% (most secure)

### 8. Traffic Correlation

Even if a VPN leaks nothing, a **passive adversary** watching traffic _around the VPN gateway_ can correlate: traffic entering the gateway from `102.1.2.3` at time T, traffic exiting to `target.com` at time T+ε with matching volume → likely the same user. No need to compromise the VPN provider.

**Mullvad-specific issue (May 2026):** When a user switches between VPN servers, they tend to get the same _relative position_ in each server's exit IP range (e.g. always the 40th percentile IP). A website seeing the user from Server A at `1.1.1.4` then Server B at `2.2.2.104` can fingerprint that it's the same user — even though the raw IPs differ.

---

### 9. VPN Trust / No-Log Claims

Final section questions _where_ the VPN terminates and _what_ the provider does with data. Key examples:

- PureVPN (2017): claimed no-logs but provided logs to the FBI
- Seven VPN providers (2020): found recording user logs despite explicit no-log pledges, with one server exposing names, passwords, emails, and home addresses

The conclusion: **the VPN provider is just moving your trust**, not eliminating it. You now trust the VPN instead of your ISP — and that trust may be misplaced.









### 1. Rogue AP attributes DNS server on the LAN

**Target: split tunnel VPNs**

- Attacker's DHCP response assigns victim a DNS server that is a **local LAN IP** (e.g. `192.168.1.1`)
- Split tunnel VPN says "local LAN traffic bypasses the tunnel" — by design
- So DNS queries to `192.168.1.1` go out through WiFi, **not the tunnel**
- Attacker controls `192.168.1.1` and hijacks the answers

Why only split tunnels? Because full tunnel VPNs route **everything** including LAN traffic through the tunnel, so this doesn't work.

---

### 2. Rogue AP attributes IP range to match VPN-assigned resolver

**The 8.8.8.2/24 trick — target: full tunnel VPNs**

- VPN will set DNS to e.g. `8.8.8.8` (outside LAN, should go through tunnel)
- Attacker assigns victim IP `8.8.8.2/24` via DHCP
- Now victim's routing table says `8.8.8.0/24 → local interface`
- `8.8.8.8` falls inside that subnet → OS sends DNS locally, **never enters tunnel**
- Attacker sitting at gateway `8.8.8.1` intercepts all DNS

The trick is making the **resolver IP appear local** by manipulating the subnet mask.

---

### 3. Rogue AP attributes LAN gateway to match VPN-assigned resolver

**The DHCP renewal trick — from the PETS 2015 paper**

This is subtler. The VPN is already connected and DNS is correctly going through the tunnel to e.g. `209.99.22.53`.

- Attacker sends a **DHCP renewal** (using a very short lease like 1 minute to force this)
- New DHCP offer keeps the same victim IP but changes the **gateway to `209.99.22.53`** — the VPN's own DNS server IP
- OS adds a specific host route: `209.99.22.53 → wlan0` (local interface)
- This more-specific route **overrides** the tunnel route for that IP
- DNS traffic to `209.99.22.53` now exits locally, attacker intercepts it

The difference from attack 2: instead of making the victim _appear to be in the same subnet_ as the resolver, the attacker **injects a specific host route** for the resolver IP pointing out through WiFi.