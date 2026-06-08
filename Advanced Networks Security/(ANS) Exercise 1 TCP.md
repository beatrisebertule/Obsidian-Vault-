### Exercise 1

1. What’s the sequence numbers on the handshake packets on the first connection? No-  
tice how Wireshark simplifies the analysis by providing both relative numbers, and next  
expected numbers.  

seq = 0


1. Observe the ephemeral source ports used by your OS on all these connections. Any pattern? Is it because the connections originate from the same process? Re-run your script to verify.  

source port always
608**

re-run

 now source port
469**

2. Change the domain and try again. Any difference on port numbers?  

now source
559**

3. Open your browser and visit some websites, then run the script again to check the source ports.

diff for browser visited sites, similar for script


### Exercise 2: Spoofing packets with Scapy

1. Start by retrieving Google server’s IPv4 address using dig or ping -4.  
`216.58.208.110

2. Create the following packet: 

```
from scapy.all import *

packet = IP(dst="172.217.17.206", ttl=128, flags="DF") / TCP(
    sport=2020,
    dport=80,
    flags="S",
    seq=0,
    window=65535
)

send(packet)
```


run with sudo rights


Set dst in the IP() layer with the IP address you fetched earlier. Choose a random source  
port sport (1024–65535) and sequence number seq (0–232) in the TCP() layer.  
Other settings include a realistic Time-to-Live (TTL), the Do Not Fragment flag on the  
IP layer, the SYN flag in the TCP layer as well as a realistic TCP Receive Window.  

3. Check the details of your packet (which is not yet sent) using ls(packet)  

checked

4. To send the packet, several functions can be used. send() simply sends the packet without  
expecting a reply. sr() sends and receives replies with sr1() receiving only 1 reply packet.  
Start a Wireshark packet capture with display filter: ip.addr... (using the Google’s  
IP address you are expecting to see). Then run send(packet) for now. See the packet  
sent in your capture.  


5. Did you get a SYN/ACK? If you retry, change your source port number to avoid Wireshark  
misinterpreting your SYN as a retransmission as well as your sequence number to avoid  
Google thinking you are a bot

yes, sync akc recieved


### Exercise 3
...
how Scapy can be used to **inject a packet into an already-established connection**

This will keep a session open long enough to enter the parameters
```
# google-http.py
import requests

# Create a session
session = requests.Session()

# Define the URL
url = "142.251.209.238"

# Fetch the resource
response = session.get(url)
input("Waiting...")
```


```
# google-rst.py
from scapy.all import *

ip = input("IP Address: ")   # destination IP address
port = input("Source port: ")  # client's port number
seqn = input("Seq: ")
ackn = input("Ack: ")

print("[+] Preparing packet to inject")
ip = IP(dst=ip, ttl=128, flags="DF")
tcp = TCP(sport=int(port), dport=80, flags="A", seq=int(seqn), ack=int(ackn))
# flags="A" means ACK flag

# data - injected data, just a HTTP GET method
data = "GET /?q=test HTTP/1.1\nHost: www.google.com\nAccept: */*\n\n"
packet = ip / tcp / data

ls(packet)

print("[+] Sending packet")
ans, unans = sr(packet)

print("[+] Response: ")
print(ans.show())
```


we run google-http.py first because 
- opens a TCP connection
- keeps it alive
- pauses so you can inject packets manually

the connection is now in ESTABLISHED state, giving you time to:
- sniff seq/ack/port
- run `google-rst.py`


4. Study the meaning of the parameters prompted when running the script. Look at the last  
ACK packet in Wireshark sent by your client to learn the correct parameters, then run  
the script accordingly. Observe the server’s reply. What is missing to get all the expected  
web page?  

5. Modify the script to wait for a while (e.g., input()) then send an RST packet to the server  
to stop the retransmissions after the injected request. Make sure to adjust the sequence  
number. If you send it quickly enough and see retransmissions stop, you injected the RST  
correctly. Is the client disconnected?



notes to myself
either save packet to a python file, and run with sudo python3...

or enter python environment with sudo
`sudo python`
then do 
`from scapy.all import *`
then continue....




quizzz
seq number increment by the number of byte of the payload
wireshark makes it easy
as  we are given seq number and and next expected seq number



recap tcp and how seq and ack numbers grow !!!

[[(quizz) TCP]]

