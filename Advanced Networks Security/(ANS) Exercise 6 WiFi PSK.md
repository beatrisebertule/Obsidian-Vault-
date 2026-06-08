### Approach  
You can use any tool you like for this exercise, but we recommend to use aircrack-ng for the  
dictionary attack and wireshark to analyze the traffic.  The first step would be to find out how to use aircrack-ng for the dictionary attack. Search online  for documentation or tutorials. Once you have the PSK, you can configure wireshark to use this key to decrypt the captured traffic. (Again, you can search online to figure out how to do this.)  Once you can see the clear text messages in wireshark, you can search for HTTP GET-requests and find the response with the secret.

- Opens `capture.cap` and extracts the **4-way handshake**
- Takes the **SSID** and each password from `wordlist.txt`
- For each password it:
    - Derives a PMK from `password + SSID`
    - Derives a PTK from the PMK
    - Computes a **MIC** (Message Integrity Code)
    - Compares it to the MIC in the captured handshake
- If MIC matches → **password found!**

crack with a wordlist

```
aircrack-ng xer-data/ex-psk.pcap -w exer-data/x-psk_dictionary
```


![[Pasted image 20260520162709.png]]


<span style="color:rgb(219, 0, 0)">key - pedomellon</span>



I see no secret



add key to decrypt traffic
![[Pasted image 20260520163948.png]]