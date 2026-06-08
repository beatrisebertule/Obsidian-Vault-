

how does IP identify a network?
we split the network into subnetworks or subnets

we have
a network part and a host part

Addresses with network prefix in CIDR notation:  
223.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 223.1.1.0/24. . .

/8 means that the first 8 bits refre to the network
so all hosts with IP that share the same first 8 bits are within the same subnet
the rest of the bits identify the host



IP broadcast
send an IP packet to **all hosts in a subnet**.
when a packet is sent to 192.168.1.255, **every host in that subnet receives it**

Use cases
- ARP requests (`Who has 192.168.1.5?`)
- DHCP discovery (client asks “any DHCP server?”)

An ARP (Address Resolution Protocol) request is ==a broadcast message sent within a local network (LAN) by a device seeking to map a known IPv4 address to an unknown physical MAC address==


IP anycast
assign the **same IP address to multiple hosts in different locations**, and the network delivers packets to **the nearest one** (according to routing metrics).

How it works
- Configured mostly on **routers and servers**, especially in BGP networks.
- Example: **DNS root servers**
    - Multiple servers worldwide share the same IP
    - Your packet goes to the **closest one** (in latency or routing terms)

An **IP address associated with anycast** is:
> A single IP address that is **assigned to multiple hosts or servers** in different locations, where the network ensures that packets sent to that IP are delivered to **the nearest host** (according to routing metrics like BGP path length or latency).


There’s a few special address ranges/blocks in IPv4. Some examples:  
- 127.0.0.0/8: localhost / loopback addressing  
- 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16: private network addresses (usually assigned via DHCP, or manually)  
- 169.254.0.0/16: link-local addressing (randomly chosen by host, checked using ARP)  
- 224.0.0.0/4: multicast addressing (old class D)