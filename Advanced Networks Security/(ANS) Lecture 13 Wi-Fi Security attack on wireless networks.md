# Summary: Wi-Fi Security 2 — Attacks on Wireless Networks

## Recap: IEEE 802.11 Connection Process

A station (STA) connects to an access point (AP) through several phases: **discovery** (probe request/response), **link-level authentication** (Open System), and **association** (agreeing on security parameters). After this, key establishment occurs via a shared Pairwise Master Key (PMK) — either as a Pre-Shared Key (PSK) in WPA Personal, or derived via 802.1x EAP in WPA Enterprise. The **4-way handshake** then confirms mutual knowledge of the PMK and establishes session keys. Data is then exchanged using a Temporal Key (TK) in TKIP (WPA), CCMP (WPA2), or GCMP (WPA3).

---

## WPA Enterprise & EAP

WPA Enterprise uses **IEEE 802.1x EAP** for authentication. Various inner protocols may be used:

- **EAP-TLS**: Mutual authentication via certificates for both server and client. Secure but key management is complex since every user needs a certificate.
- **EAP-TTLS / EAP-PEAP**: A TLS tunnel is first established (outer authentication, server-only certificate), and then the client authenticates inside this tunnel using any mechanism (inner authentication).

### eduroam

A real-world EAP-PEAP deployment enabling roaming across institutions. Authentication is federated — the visited network routes credentials to the user's home institution via a hierarchy of RADIUS servers. The client authenticates using home institution credentials.

**Security issues with eduroam:**

- Clients must validate the server certificate and connect only to legitimate APs; failing to do so enables man-in-the-middle attacks.
- If outer authentication is broken, inner authentication may not save you: clients using PAP send credentials in plaintext; clients using MSCHAPv2 are still vulnerable to cracking on weak passwords. This can be worse than no encryption at all.

---

## 1. KRACK — Key Reinstallation Attack

Discovered by Mathy Vanhoef in 2017, KRACK exploits the **4-way handshake** in WPA/WPA2/WPA3 by forcing **nonce reuse** in encrypted data frames. The critical insight: when a PTK (session key) is (re-)installed, the nonce/packet counter resets to 1. In stream ciphers, reusing a nonce with the same key reuses the keystream, which enables decryption.

### How it works

The attacker positions themselves as a **Man-in-the-Middle** (MITM) — either by cloning the AP on a different channel, or by selectively jamming Msg4 of the 4-way handshake:

1. The normal 4-way handshake completes from the STA's perspective, and the STA installs PTK and GTK.
2. The attacker **blocks Msg4** from reaching the AP, so the AP retransmits Msg3.
3. The STA accepts Msg3 again and **reinstalls the PTK**, resetting its nonce counter back to 1.
4. The STA then encrypts a new data frame with nonce 1 again — the same nonce it already used.
5. The attacker, who also has a copy of the message encrypted with nonce 1 (from the Msg4 it intercepted), can XOR the two to recover the keystream, and from that **decrypt the data frame**.

### Two scenarios

- **Scenario 1**: The STA accepts retransmitted Msg3 in plaintext even after installing PTK. The attacker controls which nonce value is in the encrypted Msg4, allowing decryption of the corresponding data frame.
- **Scenario 2**: The STA only accepts encrypted Msg3 after PTK installation. The attacker waits for or triggers rekeying, then forwards two retransmitted Msg3s in rapid succession. The STA sends two Msg4s — one with the old PTK and one with the new PTK'. The attacker extracts the keystream for nonce 1 of PTK' from the known Msg4 plaintext, and decrypts the first data frame sent under PTK'.

### Nonce reuse math

Since all WPA/WPA2/WPA3 ciphers use a stream cipher structure:

> A ⊕ Enc¹_TK(A) = keystream¹_TK → keystream¹_TK ⊕ Enc¹_TK(B) = B

So having the plaintext and ciphertext of one message encrypted with a given nonce instantly decrypts any other message encrypted with the same nonce and key.

### Broader Impact
- Affects TKIP, CCMP, and GCMP.
- Some Android/wpa_supplicant versions reinstall an **all-zero key**, making things far worse.
- With TKIP, the MIC key can be recovered, enabling **frame forgery and injection**.
- The 4-way handshake had been formally proven secure, but the model omitted key reinstallation — highlighting the limits of formal verification.

### Countermeasures
- **Install the PTK only once**: ignore retransmitted Msg3 after key installation.
- Or: **do not reset nonces** when reinstalling the current key.

---

## 2. DoS Attacks on SAE (WPA3 Personal)

WPA3 Personal uses **SAE (Simultaneous Authentication of Equals)**, also called Dragonfly, a password-based key establishment protocol. It involves a **commitment exchange** (forcing the adversary to commit to a single password guess) and a **confirmation exchange** (proving the guess is correct). The resulting PMK feeds into the usual 4-way handshake.

### DoS vulnerability

SAE requires expensive elliptic curve cryptography upon receiving a **Commit message**. An attacker can flood the AP with bogus Commits (using spoofed MAC addresses) to exhaust its CPU resources.

**AP defense**: count ongoing SAE handshakes; above a threshold, respond with a rejection containing an **anti-clogging token** (hash of a random secret and the STA's MAC address). The STA must echo the token in its next Commit.

**Why this fails in wireless**: This defense was adapted from IP-based networks, where a spoofed IP address cannot receive the AP's response. In wireless LANs, however, an attacker can trivially eavesdrop on the response (including the anti-clogging token) and re-send Commit messages with the token and a spoofed MAC address. The AP still has to do full elliptic curve work.

**Experimental results** (Raspberry Pi 1 attacker vs. professional AP): with 256-bit elliptic curve, ~70 Commits/second saturate the AP's CPU; with the much harder 521-bit curve, only ~8 Commits/second suffice. The attacker uses far less CPU than the victim.

_(Reference: Dragonblood — Vanhoef & Ronen, IEEE S&P 2020)_

---

## 3. Wireless Networks Overview

Wireless network types by coverage and data rate:

|Type|Coverage|Example Standards|
|---|---|---|
|WBAN|Body-scale|IEEE 802.15.6|
|WPAN|~100 m|IEEE 802.15.4 (ZigBee), Bluetooth|
|WLAN|~250 m|IEEE 802.11 (Wi-Fi)|
|WMAN|~100 km|IEEE 802.16 (WiMAX), LTE|

**Attack surface at different layers:**

- **Datalink layer**: identity theft, MAC spoofing, injection, MITM, DoS flooding.
- **Physical layer**: eavesdropping, DoS via jamming.

Security goals: confidentiality, integrity, availability, authenticity.

---

## 4. Jamming Attacks

Jamming is a physical-layer DoS attack by emitting radio signals to disrupt legitimate transmissions. It affects both receivers (degraded signal quality) and transmitters (channel sensed as busy, preventing transmission via CSMA/CA back-off).

### Jamming types

**By timing:**

- **Constant**: continuously transmits — high power usage, easy to detect.
- **Intermittent/random**: transmits periodically — harder to distinguish from normal congestion.
- **Reactive**: only transmits when a legitimate signal is detected — more power-efficient.

**By signal strength:** constant or adaptive (tuned to the received power at the victim).

**By signal type:**

- **Constant noise**: simple radio interference.
- **Deceptive**: injects valid-looking packets to keep receivers busy.
- **Intelligent**: exploits higher-layer protocol weaknesses (e.g. TCP flooding, Smurf attacks, malware).

### Detection metrics

|Metric|What it measures|Limitation|
|---|---|---|
|**RSS** (Received Signal Strength)|Energy level at receiver|Hard to distinguish reactive jammer from normal CBR traffic; constant/deceptive jammers look like MaxTraffic|
|**CST** (Carrier Sensing Time)|Time transmitter waits for idle channel|Reactive/random jammers look like no jamming; constant/deceptive jammers well-detected|
|**PSR** (Packet Send Ratio)|Fraction of intended packets actually transmitted|Degraded by carrier sensing delays|
|**PDR/PER** (Packet Delivery/Error Rate)|CRC pass/fail ratio|Increased failures indicate jamming|

**Key finding**: no single metric is reliable across all jammer types. RSS alone is insufficient; combining metrics is necessary.

### Jammer comparison

|Jammer|Energy efficiency|Effectiveness|Complexity|Detection signals|
|---|---|---|---|---|
|Constant|Low|High|Low|RSS, PER, CST|
|Intermittent|Low|Adjustable|Low|RSS, PER, CST|
|Reactive|Moderate|High|Moderate|RSS, PER|
|Adaptive|High|High|High|RSS, PER|
|Deceptive/Intelligent|High|High|High|RSS, PER|

### Countermeasures against jamming

- **Channel surfing / frequency hopping**: rapidly switch channels (pseudorandomly or reactively upon detection).
- **Spread spectrum**: spread the signal over a wide frequency band at low power, making it hard to jam without also drowning out a huge spectrum.
- **Spatial retreating**: move legitimate nodes physically away from the jammed area.



# LocalNet Attack
local networks attacker creates a rouge access point and sets the local networks's IP range to one that contains the IP address of a target server which traffic leaks, the traffic to the target server will leak, 