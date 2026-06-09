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



# Improved Notes

<span style="color:rgb(219, 0, 0)">terminology</span>
Local network: within a limited area such as a home or building
Private network: uses the private IP address space
localhost: Host name referring to this computer
	127.0.0.1 for IPv4 or ::1 for IPv6
Loopback interface/address: Used to send packets to itself


**Implicit trust** — <span style="color:rgb(219, 0, 0)">services running locally are assumed to be safe since they're not on the internet, but this assumption is dangerous</span>

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



# DNS rebinding
#### First, How Normal DNS Works

When you visit `bank.com`:

```
Your browser → asks DNS server → "what IP is bank.com?"
DNS server   → replies         → "it's 93.184.216.34"
Browser      → connects to       93.184.216.34
```

The browser **caches** this answer for a while (based on TTL — Time To Live, set by the domain owner). TTL is basically "how long should you remember this answer before asking again?"

```
TTL = 3600  → cache for 1 hour
TTL = 60    → cache for 1 minute
TTL = 1     → cache for 1 second → re-ask DNS almost immediately
```

---

#### The Core Trick

DNS rebinding exploits two things together:

1. **You can set TTL very low** — forcing the browser to re-ask DNS frequently
2. **SOP trusts domain names, not IP addresses** — so if the IP changes, SOP doesn't notice

---

#### Step by Step Attack

**Setup phase** — attacker controls:

- A malicious website: `evil.com`
- Their own DNS server for `evil.com`
- This DNS server can return **different answers** each time it's asked

---

**Step 1 — Victim visits evil.com**

```
Browser:     "what IP is evil.com?"
Attacker DNS: "it's 5.5.5.5 (my real server)"  TTL=1 second
Browser:      connects to 5.5.5.5
              loads the malicious page + JavaScript
```

---

**Step 2 — Page loads, JS starts running**

The JS sits and waits 1-2 seconds — just long enough for the TTL to expire and the cached DNS answer to be discarded.

---

**Step 3 — DNS record gets swapped**

Attacker changes the DNS record:

```
evil.com → was: 5.5.5.5 (attacker server)
evil.com → now: 192.168.1.1 (victim's router)
```

---

**Step 4 — JS makes a new request to evil.com**

javascript

```javascript
fetch("http://evil.com/admin")  // JS thinks this is still evil.com
```

Browser thinks: "I need to connect to evil.com, let me check DNS again" (TTL expired)

```
Browser:      "what IP is evil.com now?"
Attacker DNS: "it's 192.168.1.1"
Browser:      connects to 192.168.1.1
```

---

**Step 5 — SOP check passes, attack succeeds**

```
SOP asks:  "is this request going to evil.com?"
Browser:   "yes, the domain is evil.com"      ✅ same origin
SOP:       "allowed — here's the response"

Reality:   the response came from the router at 192.168.1.1
```

The JS can now **read the router's admin page** — login credentials, settings, everything.

---

#### Visual Timeline

```
Time 0s:   victim visits evil.com
           DNS: evil.com → 5.5.5.5 ✓  (TTL=1s)
           malicious JS loads and starts running

Time 1s:   TTL expires, DNS cache cleared

           Attacker swaps DNS:
           evil.com → 192.168.1.1

Time 2s:   JS fires request to "evil.com"
           browser re-resolves DNS
           DNS: evil.com → 192.168.1.1

           Browser connects to router
           SOP check: domain is evil.com ✅
           JS reads router response freely

Time 3s+:  attacker reads/controls router
           via JS on the page
```

---

#### Why the Browser Can't Easily Detect This

The browser has no way to know whether a DNS change is:

- **Legitimate** — a website moving servers, load balancing, CDN switching
- **Malicious** — rebinding to a local device

DNS changes happen all the time for legitimate reasons. The browser can't tell the difference — it just follows DNS faithfully.

---

#### What Makes It Especially Dangerous

- **No malware installed** — pure abuse of standard browser + DNS behaviour
- **Works on any OS** — Windows, Mac, Linux, Android, iOS
- **Works against any local device** — router, smart TV, NAS, thermostat, printer
- **User sees nothing** — just a normal webpage visit
- **Bypasses firewall** — browser is already inside the network perimeter

---

#### Partial Defenses That Exist

|Defense|How it helps|Limitation|
|---|---|---|
|DNS pinning|Browser keeps original IP even after TTL expires|Not consistently implemented|
|Private IP blocking|Browser refuses to resolve public domains to private IPs|Chrome implementing this (Local Network Access)|
|Router login|Requires password for admin panel|Many people never change defaults|
|HTTPS on local devices|Certificate mismatch would alert user|Almost no local devices do this|

The **Local Network Access (LNA)** permission model mentioned in the lecture is the most promising fix — requiring explicit user permission before any page can connect to local network addresses, regardless of what domain it claims to be.