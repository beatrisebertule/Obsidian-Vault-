
BGP
bored gateway protocol 

The internet is networks of networks, broke up into hundreds of thousand networks
called <span style="color:rgb(219, 0, 0)">autonomous systems (AS)</span>

each of these networks are a large pool of routers

ASes typically belong to Internet service providers (ISPs) or other large organizations, such as tech companies, universities, government agencies, and scientific institutions. Each AS wishing to exchange [routing](https://www.cloudflare.com/learning/network-layer/what-is-routing/) information must have a registered autonomous system number (ASN). Internet Assigned Numbers Authority (IANA) assigns ASNs to Regional Internet Registries (RIRs), which then assigns them to ISPs and networks. ASNs are 16 bit numbers between one and 65534 and 32 bit numbers between 131072 and 4294967294. As of 2018, there are approximately 64,000 ASNs in use worldwide. These ASNs are only required for external BGP.


external BGP
routes are exchanged and traffic is transmitted over the Internet using external BGP (eBGP)

internal BGP
autonomous systems can also use an internal version of BGP to route through their internal networks, which is known as internal BGP (iBGP)


<span style="color:rgb(219, 0, 0)">BGP hijacking</span>
can happen accidentaly 

#### Network layer
IP (Internet Protocol): getting data to the right host
Internet Control Message Protocol: error reporting et al.  
Routing Protocols: for filling forwarding tables 


The internet protocol moves packets between (network) interfaces of  hosts.  
Almost every node (=interface) on the internet has an IPv4 address


A **network interface of a host** is:
The component that allows a device (host) to connect to a network and send/receive packets.
A **host** = any device with an IP address

