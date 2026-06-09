
guest lecture by Harald Vranken 

more advanced attacks next lecture
two part lecture

textbook - computer sec and the internet


## Intro to wifi


access point two interfaces
wired and wireless interface

standards  1EEE 802.11 
specifications for wireless LAN mac and physical layer

wireless connectivity for fixed portable and mobile devices within a local area

laptop in train - portable or mobile 

portable vs mobile

move device, move access point (access points change) - mobile device
hand over the connection to a diff access point (mobile cell services)
in wifi case, lose connection, go back to reconnect


wi-fi alliance


radio frequency
different link rate for each wifi-x (wifi6 eg)

2.4 GHz band  
• 11 channels available to users  
• channels are 20 MHz wide (overlap; 5 MHz apart)  
• effectively 3 non-overlapping channels: 1, 6, and 11


picture here

IEEE 802.11
<span style="color:rgb(219, 0, 0)">station</span>  STA - device with wireless network interface
<span style="color:rgb(219, 0, 0)">access point</span> - station that connects to other devices but also to networks
provides wireless access to stations

<span style="color:rgb(219, 0, 0)">infrastructure</span> mode stations connect directly (no access point involved)

<span style="color:rgb(219, 0, 0)">ad-hoc mode</span> stations connect directly

basic service net

extended service set

## wi-fi sec basics

![[Pasted image 20260507141612.png]]

![[Pasted image 20260507141556.png]]

two virtual interfaces - with and with  no authentication

 evil twin attack
 special case of evil twin attack - preferred network list

broken access control measure at API


Hidden network (security by obscurity; access requires knowing SSID)  
– AP continuously sends beacon packets; may hide by not including SSID  
– Easy to eavesdrop on STAs that have SSID  
Filter STAs based on allowlist of MAC addresses at AP  
– STA can easily spoof its MAC address


## wep

wired equivalent privacy wep

wep with open system authentication

wep with shared key authentication

wep weaknesses

IV is sen in plaintext
atttacker can sniff IV

IV is only 4-bit wide - can be easily guessed

web key derived from password - dictionary attacks



---


robust security network RSN

web and open system auth and shared key auth obsolete

RSN security data confidentiality and integrity



4-way handshake


![[Pasted image 20260507145548.png]]

weakness point - mpk can be brute forced


## WPA2 and WPA3


wpa3 based on diffie helman
SAE simultaneous authentication of equals



# Improved Notes

two layers
PHY physical layers handles the actual physical signals
	radio waves for wireless connection (802.11)
	electrical/optical signals for wired connection (802.3)

The access point only operates at layers 1 and 2 (physical and data link). Above that — IP, TCP, application — the protocols are the same end-to-end and the access point doesn't need to touch them.


### Overlapping Radio Frequencies
2.4 GHz band
• 11 channels available to users
• channels are 20 MHz wide (overlap; 5 MHz apart)
• effectively 3 non-overlapping channels: 1, 6, and 11

### Impact of Overlapping Channels

When multiple devices operate on overlapping channels, it can lead to two types of interference:

- Co-Channel Interference (CCI): Occurs when multiple access points (APs) use the same channel. This can slow down the network but usually remains functional.
- Adjacent-Channel Interference (ACI): Happens when channels overlap, causing signals to corrupt each other, leading to data loss and retransmissions.


wifi for local area
celular mobile network for wide area

**cellular** refers to the mobile network technology provided by telecom companies

### IEEE 802.11 terminology
![[Pasted image 20260520120929.png]]


station - device
access point - provides access to distribution system
distributes system
	when you have multiple APs in a building (like at Radboud University), they all need to be connected to each other and to the internet somehow. The DS is that "backbone" connecting them — usually just a wired network of cables and switches.

from book
"Mobile devices such as laptop computers commonly access network resources by con-
necting to an access point over a wireless medium in specific radio frequency (RF) ranges.
The access point itself is typically connected to a physically wired local network, thereby
providing Internet connectivity. Aside from connecting to a cellular network, many mo-
bile phones can connect by WLAN to an access point—to access Internet services more
cost-effectively (if mobile phone cellular service is billed on a usage basis), or more reli-
ably if cellular reception is poor (e.g., indoors)."


"The main components of a WLAN (Fig. 12.1) are
the wireless endpoint or mobile station (STA), access point (AP), and authentication server
(AS). An AP creates logical connections between STA devices and a distribution system
(DS), usually wired to the rest of the network, providing forwarding and routing services.
The AS may be built into the AP in simple designs (e.g., for home networks), often using
no more than a username-password list (we discuss more advanced authentication later)"

An AP does one specific job: **it connects wireless devices to a wired network**. It's essentially a bridge between the Wi-Fi world and the wired world.

It operates at **layer 2** (MAC layer) — it just forwards frames between the wireless and wired side without thinking about IP addresses or routing.


### Wifi Security

Securing connection between STA 1 and STA 7  
– End-to-end security has to be provided in upper layers  
– Wireless security is about securing wireless link  
(layer 2 communication)

![[Pasted image 20260520122342.png]]

Wireless security measures  
– Confidentiality and integrity by encryption at layer 2  
– Cryptographic key establishment and key management  
– Authentication between STA and AP

**Authentication**  
Methods for (mutual) authentication of STA and AP  
1. <span style="color:rgb(219, 0, 0)">No authentication</span> (‘**Open System**’ authentication)  
2. <span style="color:rgb(219, 0, 0)">Based on pre-shared secret</span> (‘**Shared Key’** authentication, PSK, AES)  
3. <span style="color:rgb(219, 0, 0)">Based on Authentication Server</span> (AS)  

AS authorizes whether STA (station) may access services provided by AP (authenticator),  
based on credentials provided by station  

The **AS** verifies whether the user/device is allowed to join the network

AS either built-in into AP or external

![[Pasted image 20260520122545.png]]


### Standard IEEE 802.1X  
• IEEE 802.1X Port-Based Network Access Control  
• AP allocates two virtual ports for each STA  
	<span style="color:rgb(219, 0, 0)">A</span>: uncontrolled port always enabled; relay messages between STA and AS  
	<span style="color:rgb(219, 0, 0)">B</span>: controlled port  enabled upon successful authentication

virtual ports - **logical gates inside the AP's software** that control what traffic is allowed through. The AP uses them to enforce a simple rule: you can't access the network until you prove who you are.


<span style="color:rgb(219, 0, 0)"><b>Port A — Uncontrolled Port (always open)</b></span>

```
[STA] ----> Port A ----> [AS]
```

- Always enabled, even before authentication
- **Only** lets authentication messages pass through
- Acts as a messenger between your device and the AS
- Your device can talk to the AS but nothing else
- This is how the authentication conversation happens before you're granted access
<span style="color:rgb(219, 0, 0)"><br><b>Port B — Controlled Port (locked until authenticated)</b></span>

```
[STA] ----> Port B ----> LAN services
            (locked)     (internet, network)
```

- Starts locked — no traffic passes
- The **port controller** (the switch symbol in the diagram) controls it
- Only opens after the AS says authentication was successful
- Once open, your device gets full access to LAN services

![[Pasted image 20260520123240.png]]

```
1. STA arrives, wants to connect
         |
2. AP opens Port A only
   STA can reach AS, nothing else
         |
3. Authentication exchange happens
   (EAP messages go through Port A to AS)
         |
4. AS validates credentials
         |
        / \
      YES   NO
      /       \
5a. AS tells    5b. AS tells
    AP: success     AP: failed
         |               |
6a. Port B          6b. Port B
    opens               stays locked
         |
5. STA gets full
   LAN access
```

![[Pasted image 20260520123539.png]]



### Attacks: rouge AP
<span style="color:rgb(219, 0, 0)">Evil twin attack</span>: active attack in which adversary acts as man-in-the-middle (MITM)  
– adversary impersonates AP (‘evil twin’) and induces STA to connect to it,  
while making connection to legitimate AP  
– adversary is able to inspect, modify, and forge any data between STA and legitimate AP  
<span style="color:rgb(219, 0, 0)">KARMA</span>: special case of evil twin attack  
– vulnerable STA broadcasts ‘preferred network list’ (PNL)  
containing SSIDs of APs points to which STA has previously connected  
– malicious AP receives PNL and takes an SSID from PNL


only possible if no mutual authenticstion
WPA2 does nto do mutual auth, just password


SSID stands for **Service Set Identifier**.
It is simply the **name of a Wi-Fi network** that you see when searching for Wi-Fi.

### Broken access control measures at AP  
<span style="color:rgb(219, 0, 0)">Hidden network </span>(security by obscurity; access requires knowing SSID)  
– AP continuously sends beacon packets; may hide by not including SSID (name of network)
– Easy to eavesdrop on STAs that have SSID  

Even though the AP hides its SSID in beacons, the SSID is still transmitted in <span style="color:rgb(219, 0, 0)">other frames</span>Specifically when a device that knows the SSID tries to connect:

An attacker passively listening with a packet sniffer sees this exchange and learns the SSID immediately. They just had to wait for any legitimate device to connect.


<span style="color:rgb(219, 0, 0)">Filter</span> STAs based on allow-list of MAC addresses at AP  
– STA can easily spoof its MAC address

Every network device has a **MAC address** — a supposedly unique hardware identifier
Example: `AA:BB:CC:DD:EE:FF`
The AP maintains an allowlist of MAC addresses that are permitted to connect
Any device not on the list gets rejected

**Why it fails:**
MAC addresses are transmitted in **plaintext in every single Wi-Fi frame** — they have to be, so devices know who is talking to whom. An attacker passively listening sees all MAC addresses of legitimate devices


### Evolution
First era (1997): from no security to Wired Equivalent Privacy (WEP)  
– authentication: Open System or Shared Key authentication (between STAs and AP; no AS)  
– encryption: RC4 stream cipher  
– compromised within few years; crucial to rethinking security  

Second era (2002): Wi-Fi Protected Access (WPA)  
– authentication: WPA-Personal (passphrase, no AS) or WPA-Enterprise (credential-based  
authentication with AS)  
– encryption: Temporal Key Integrity Protocol (TKIP); add-on to WEP  

Third era (2004): WPA2  
– encryption: AES-CCMP block cipher  

Fourth era (2019): WPA3  
– authentication in WPA3-Personal: key exchange by SAE  
– encryption: AES-GCMP block cipher



### WEP (wired equivalent privacy)
<span style="color:rgb(219, 0, 0)">WEP has serious cryptographic weaknesses</span>

It uses:
- A **pre-shared key** (WEP key) known by both the STA and AP
- The **RC4 stream cipher** to encrypt data
- An **Initialization Vector (IV)** to vary the encryption

The keystream in RC4 is generated from combining the **WEP key + IV**

**Why WEP Failed — The Weaknesses**
<span style="color:rgb(219, 0, 0)"><b>1. The IV is too short</b></span>
The IV is only **24 bits** wide, meaning only 2²⁴ = **16,777,216 possible values**. On a busy network this space is exhausted quickly — IVs start repeating. When two messages use the same IV they use the same keystream, which as shown above leaks information.

<span style="color:rgb(219, 0, 0)"><b>2. The IV is sent in plaintext</b></span>
```
Frame: [IV=12345][encrypted data]
```
The attacker can see exactly which IV was used. They collect frames and group them by IV — all frames with the same IV used the same keystream.

<span style="color:rgb(219, 0, 0)"><b>3. Weak keys in RC4</b></span>
About 9,000 specific IV values produce **weak keystreams** that leak information about the WEP key itself. By collecting enough packets with these weak IVs, statistical analysis can recover the WEP key. Tools like Aircrack-ng automate this completely.


IV 24 bits

Before a device (STA) joins the Wi-Fi network, it must prove it knows the WEP key.
The process is a **challenge–response handshake**.

AP sens challenge
STA encrypts challenge 
AP decrypts challenge 

An attacker listening to Wi-Fi traffic can capture BOTH:
- plaintext challenge
- encrypted response
So attacker knows the plaintext and ciphertext

```
ciphertext = plaintext xor keystream
keystream = ciphertext xor plaintext
```

**What attacker gains**
The attacker does NOT know the actual WEP secret key.
BUT they now know:
- the IV used
- the keystream generated for that IV
That is enough to fake authentication

also
40-bit WEP key is too short (exhaustive guessing possible)


after WEP:
WPA (2003): TKIP add-on to WEP  
WPA2 (2004): AES-CCMP instead of RC4  
WPA3 (2018): AES-GCMP



### WPA
quick fix after WEP

WPA introduced:
TKIP (Temporal Key Integrity Protocol)

### WPA2

replaced RC4/TKIP with AES-CCMP
4-way handshake

handshake:
- prove both STA and AP know the secret
- derive fresh session keys

![[Pasted image 20260520143409.png]]

Weakness: PMK may be shared
Weakness: PMK may be brute-forced


### <span style="color:rgb(219, 0, 0)">wekaness of WPA2</span>
PSK has a weakness: if someone captures the 4-way handshake, they can run offline dictionary attacks against the password.

SAE fixes this by using a **Diffie-Hellman-style exchange** where the password is never directly exposed, making offline attacks practically impossible.
### WPA3



- **WPA Personal** → everyone uses the same Wi-Fi password
	pre-shared key
- **WPA Enterprise** → each user has their own login credentials
	authentication server needed

![[Pasted image 20260520144253.png]]


PSK - pre-shared key
SAE - simultaneous authentication equals


### WPA Personel
![[Pasted image 20260520144604.png]]

### WPA Enterprise
![[Pasted image 20260520144622.png]]



<span style="color:rgb(219, 0, 0)">WPA3 has forward secrecy, no offline bruteforce attacks</span> 
