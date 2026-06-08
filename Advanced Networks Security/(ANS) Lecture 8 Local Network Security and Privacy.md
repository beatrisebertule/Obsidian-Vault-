guest lecture by Gunes

optional intern connection
modem

local netw - refers to local area network
private netw - designated IP ranges

localhost - hostnames that refer to this computer 127.0.0.1
loopback used to send packets to itself

shodan
webpage
lists all publicly exposed devices
can publicly connect to them

also
pips technologies

assumption devices running on local are protected?

not directly accessible device
still not safe
smart devices

how can they be reached from outside

port forwarding


the browser acts like a trojan horse
browser runs malicious script which scans and attacks local devices
does not even cross public/private boundary to get access to local devices

http see open ports - can reach local ports
https all port appear to be closed
<span style="color:rgb(219, 0, 0)">blocked by cros policy - cross origin</span>

locahltest.me another name for localhost

<span style="color:rgb(219, 0, 0)">mixed content </span>

---
all open ports give different errors



browser port scanning 

side channels - timing to abuse
error timing to tell if a port id open or not



attack overview
1. malicious script
2. dns rebinding
3. ...

demo to discover and control IoT devices


### what is <span style="color:rgb(219, 0, 0)">DNS rebinding</span>?

etc/host
overwrite what dns does
 ...

core concept - bind victim's IP to attack.com

Ollama
exploits open ports - expose data from "locally" hosted models

use private IP to start local open port scan


### local network misuse for tracking

local network based abuses

ebay - scan open ports to check if device is remotely controlled or not

http request jumps to localhost fixed port
containing fbp cookie
only on android

localhost tracking mitigations
quick fix: block the fixed port 
updates 


LNA - local netw access
ask for user permission



---
cleaner notes

<span style="color:rgb(219, 0, 0)">terminology</span>
Local network: within a limited area such as a home or building
Private network: uses the private IP address space
localhost: Host name referring to this computer
	127.0.0.1 for IPv4 or ::1 for IPv6
Loopback interface/address: Used to send packets to itself


**Implicit trust** — services running locally are assumed to be safe since they're not on the internet, but this assumption is dangerous

Example: Open or unprotected local services (mostly HTTP)
	Smart home devices
	Games, antivirus software, local LLM servers

shodan - webpage that keeps track of all exposed services


### Accessing devices/service running on local network
Access from the websites you visit?
	Your browser runs a malicious script, which scans and attacks local services/devices
	The public/private boundary doesn’t need tobe crossed
Issues (or fixes, what prevents this):
	Mixed content & cross-origin protections

```
You visit malicious (or compromised) website
        ↓
Website loads a JavaScript in your browser
        ↓
Script runs INSIDE your network (your browser is already local)
        ↓
Script scans and attacks local devices (192.168.1.1-255)
        ↓
No public/private boundary ever crossed
```

