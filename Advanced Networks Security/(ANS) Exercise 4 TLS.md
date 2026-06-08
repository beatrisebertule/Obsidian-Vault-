
### Exercisse 1

openssl
this calls the OpenSSL toolkit, a widely used library for working with cryptography, SSL, and TLS.

`s_client`

This tells OpenSSL to act as a **client** connecting to a remote SSL/TLS server.


1. What is the version of the TLS Record layer?
Application Data (encrypted) has TLS1.2

Of the TLS Handshake Protocol?

check with filter `tls.handshake
I see ClientHello and ServerHello with TLS1.3
![[Pasted image 20260408160628.png]]

Why does Wireshark then list this connection as TLS v1.3? Hint: Look at the TLS extensions  in the ClientHello. 

ClientHello - client suppors both v1.2 and v1.3
![[Pasted image 20260408161139.png]]

ServerHello - server supports only v1.3 

![[Pasted image 20260408161301.png]]




3. How many ciphersuites are listed in the ClientHello? Which one did the server agree on?  

ClientHello has 62 cipher suites
![[Pasted image 20260408161611.png]]

ServerHello
`Cipher Suite: TLS_AES_256_GCM_SHA384 (0x1302)




4. Same for the Signature Hash Algorithms.  
ClientHello
![[Pasted image 20260408162345.png]]
ServerHello



5. Same for the Supported Groups. Are there only Elliptic Curves listed?  

ClientHello
![[Pasted image 20260408162938.png]]

ServerHello
![[Pasted image 20260408163631.png]]

4. Do you see the server certificate?  

No, I don't


5. Abort the connection with Ctrl-C or Cmd-C. Rerun the command but force the use of  
TLS 1.2 only by appending the -tls1_2 flag. Is the negotiated ciphersuite the same as  
before?  


Now I see seperate certificate messages

![[Pasted image 20260408170043.png]]

THe cipher suite is different form the one negotiated before
The one before was AES 
TLS_AES_256_GCM_SHA384 is specific to TLSv1.3

![[Pasted image 20260408170124.png]]
`TLS-ECDHE_ECDSA_...

6. Where is the client’s ECDH public key? Server’s?  

![[Pasted image 20260408171141.png]]


there are differen messages ofr key shar exhnabge !!!
serverkey echnage and client key exhnage

ServerKeyExchange message
![[Pasted image 20260408173057.png]]

ClientKeyExchange message
![[Pasted image 20260408173244.png]]


7. How many certificates are sent by the server? Why? Check both the output of OpenSSL  
and what you see in the TLS handshake.  

One server certificate
Server certificate
-----BEGIN CERTIFICATE-----
MIIEVTCCAz2gAwIBAgIQGZznLsX+z0MQLswjDgxaEzANBgkqhkiG9w0BAQsFADA7
MQswCQYDVQQGEwJVUzEeMBwGA1UEChMVR29vZ2xlIFRydXN0IFNlcnZpY2VzMQww
CgYDVQQDEwNXUjIwHhcNMjYwMzE2MDgzOTU3WhcNMjYwNjA4MDgzOTU2WjAZMRcw
FQYDVQQDEw53d3cuZ29vZ2xlLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IA
BO5dhUGEOyBWmwLZQnVx5eHgsruLjREDHXUwjU3eaP7Ds/CnhV0I4ckD32YT4s2Y
MojdnMtbBDQM1X1zLczR1j6jggJAMIICPDAOBgNVHQ8BAf8EBAMCB4AwEwYDVR0l
BAwwCgYIKwYBBQUHAwEwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQUe/5vZ7OoACJ5
kDWNhDZc50IKHqQwHwYDVR0jBBgwFoAU3hse7XkV1D43JMMhu+w0OW1CsjAwWAYI
KwYBBQUHAQEETDBKMCEGCCsGAQUFBzABhhVodHRwOi8vby5wa2kuZ29vZy93cjIw
JQYIKwYBBQUHMAKGGWh0dHA6Ly9pLnBraS5nb29nL3dyMi5jcnQwGQYDVR0RBBIw
EIIOd3d3Lmdvb2dsZS5jb20wEwYDVR0gBAwwCjAIBgZngQwBAgEwNgYDVR0fBC8w
LTAroCmgJ4YlaHR0cDovL2MucGtpLmdvb2cvd3IyL0dTeVQxTjRQQnJnLmNybDCC
AQMGCisGAQQB1nkCBAIEgfQEgfEA7wB1AGQRxGykEuyniRyiAi4AvKtPKAfUHjUn
q+r+1QPJfc3wAAABnPYEOwkAAAQDAEYwRAIgX3+Atz1f4oQaWFbSeWprTAQxNEC0
GLlV2629G+UeCUkCIAmdmfgc2WAYJelNNsoI7vI5c412B0YUmN05un9ZqR/lAHYA
lpdkv1VYl633Q4doNwhCd+nwOtX2pPM2bkakPw/KqcYAAAGc9gQ7HAAABAMARzBF
AiAw5lwSwBPzgvNG6psBYkDKchYjVy1vIPViG/vlWCfUmAIhANtX3WbkxQkEsoTS
NZxFwE8F9x/1QPmoOTdLJ/9vqUKXMA0GCSqGSIb3DQEBCwUAA4IBAQB+ypNbpdyC
Fr+26WI592OtWvHr3zNZHeuztbvZ/ecW+cKb98+f751gUnyw+2Ba5/gKH8JTcib+
tmPyjYMMIhEskwczQ3KG41nzPOYKhZw+o+Vcp0/XLcMop5vPVqmQSBqBhQ3k78PL
ppuIqnbB8TohL+ItlmhxzFib7jAuq7ZJ23ZBqc9uO53a6kF+u7a+hDtJRIy8w0tb
3ahHdFCYUY6zf5/J0rk/EetWO02dbEXM3eqvYv7x51RUUvSQajTcfaJg2E/CMS7C
oz6JCKK+jSgpoL59MtcH+6pI2srjM6e/O48Xct2Ts3r9SkVJrKOyFWnBGX/s3amu
G9o6RR6I0TF1
-----END CERTIFICATE-----

![[Pasted image 20260408171045.png]]


8. While OpenSSL is still connected, send the following HTTP request (make sure to end  
with two new lines at the end):

...

### Exercise 2
Inspecting 0-RTT TLS 1.3 with SSLKEYLOGFILE

requests.txt file simply contain an HTTP request

SSLKEYLOGFILE is a feature used by various TLS/SSL libraries and applications, such as  
web browsers like Firefox and Chrome, or tools like curl, to export and log TLS session keys  
to a specified file during encrypted connections. It allows tools like Wireshark to decrypt and  
inspect otherwise encrypted TLS traﬀic.  

1. We are going to run our own TLS server using OpenSSL. First, create a self-signed certificate:  
`openssl req -x509 -newkey rsa:2048 -keyout server.key -out server.crt -days 365 -nodes -subj "/CN=localhost"  

will create files in the directory
server.crt
server.key

2. Write HELLO WORLD in a file named data.txt in the current directory.

3. Create a file with an HTTP request for this file. On Linux/Mac, do:  
write request to the file
`echo -e "GET /data.txt HTTP/1.1\r\nHost: localhost\r\nConnection: close\r\n\r\n" > request.txt  

4. Run the HTTP server with the following command. Be careful, the -WWW parameter makes  
OpenSSL serve content from the current directory. We use this to request for the data.txt  
file we created earlier. Make sure to run the command from a new folder without sensitive  
data. The -early_data parameter enables the server to process TLS 1.3 “early data”,  
which is sent on resumed TLS session on the “first trip” before the server gets a chance to  
say anything. This is called 0-RTT, and enables TLS to be nearly as eﬀicient as plaintext  
connection by avoiding extra round trips.  
`openssl s_server -key server.key -cert server.crt -WWW -accept 127.0.0.1:8443 -tls1_3 -early_data -trace  

<span style="color:rgb(219, 0, 0)">this runs the server </span>

5. Start recording on the loopback interface in Wireshark (“Adapter for loopback traﬀic  
capture”). Then, from a different terminal, send this request to your local TLS server:  
`cat request.txt | openssl s_client -connect localhost:8443 -tls1_3 -sess_out session.pem -keylogfile keylog.txt  

This command first lets OpenSSL connect to the server, log session tickets in the session.pem  
file, usable to resume the connection later. Meanwhile, OpenSSL also logs session keys to  
the SSLKEYLOGFILE. Once the connection is established, it sends the HTTP request.  
You should see the HTTP response including the content of data.txt.  

connect to the server
<span style="color:rgb(219, 0, 0)">ession.pem -keylogfile keylog.txt </span>si what keeps a log of the session keys


6. Make sure session.pem contains some material, and inspect the content of keylog.txt.  
Notice the error “Verification error: self-signed certificate” on the client.  

master secret is not part of logged keys within keylog.txt file

7. Configure Wireshark to read the SSLKEYLOGFILE by going to Edit > Preferences...  
> Protocols > TLS > and click on Browse under “(Pre)-Master-Secret log filename” to  
point to your keylog.txt file, then click OK. The encrypted TLS traﬀic should now be  
decrypted and you should be able to see the GET /data.txt request. Examine the rest of  
the decrypted packets.  

<span style="color:rgb(219, 0, 0)">DECRYPTED TRAFFIC</span>

![[Pasted image 20260408180715.png]]

8. Next, let’s send the same request but in the first flight, i.e., the early data. We are going  
to resume the previous session, that is, we want to avoid launching a fresh key exchange  
including certificate validation. We use OpenSSL s_client -early_data for this purpose.  
Run the command:  
openssl s_client -connect localhost:8443 -tls1_3 -sess_in session.pem  
-early_data request.txt -keylogfile keylog.txt  Exercise 3: Parsing and verifying certificates using OpenSSL

9. You may not have noticed a speed difference in your local environment, but if your con-  
nection goes through a high-latency link, e.g., satellite or simply a distant server, there  
would be one. In Wireshark, go to Statistics > Flow Graph. You should see the two TLS  
connections and the messages going back and forth. Analyze the difference between the  
two connections. Note: Although it seems the client is waiting for the New Session Ticket  
messages, it is not necessary before the client can send an HTTP request; it could do so  
right after sending the Finished messagExercise 3: Parsing and verifying certificates using OpenSSL


### Exercise 3: Parsing and verifying certificates using OpenSSL

Using OpenSSL, connect to google.com with the additional -showcerts option to display all  
certificates presented by the server, and copy each of them to separate files (including the  
-----BEGIN CERTIFICATE----- and -----END CERTIFICATE-----).  
1. Manually save each certificate. Parse each of them using
`openssl x509 -in <cert> -noout -text  

`x509` is the OpenSSL tool for working with <b>X.509 certificates</b> (the standard certificate format for TLS/SSL).<br>- It can display, convert, or manage certificates

`noout`
- Prevents OpenSSL from printing the raw PEM-encoded certificate to the terminal.
- Without `-noout`, you would see the `-----BEGIN CERTIFICATE----- ... -----END CERTIFICATE-----` block.
- 
`text` 
- Prints the certificate in **human-readable text format**, including details such as:

output of  `openssl x509 -in cert0.txt -noout -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            19:9c:e7:2e:c5:fe:cf:43:10:2e:cc:23:0e:0c:5a:13
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, O=Google Trust Services, CN=WR2
        Validity
            Not Before: Mar 16 08:39:57 2026 GMT
            Not After : Jun  8 08:39:56 2026 GMT
        Subject: CN=www.google.com
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:ee:5d:85:41:84:3b:20:56:9b:02:d9:42:75:71:
                    e5:e1:e0:b2:bb:8b:8d:11:03:1d:75:30:8d:4d:de:
                    68:fe:c3:b3:f0:a7:85:5d:08:e1:c9:03:df:66:13:
                    e2:cd:98:32:88:dd:9c:cb:5b:04:34:0c:d5:7d:73:
                    2d:cc:d1:d6:3e
                ASN1 OID: prime256v1
                NIST CURVE: P-256
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature
            X509v3 Extended Key Usage: 
                TLS Web Server Authentication
            X509v3 Basic Constraints: critical
                CA:FALSE
            X509v3 Subject Key Identifier: 
                7B:FE:6F:67:B3:A8:00:22:79:90:35:8D:84:36:5C:E7:42:0A:1E:A4
            X509v3 Authority Key Identifier: 
                DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30
            Authority Information Access: 
                OCSP - URI:http://o.pki.goog/wr2
                CA Issuers - URI:http://i.pki.goog/wr2.crt
            X509v3 Subject Alternative Name: 
                DNS:www.google.com
            X509v3 Certificate Policies: 
                Policy: 2.23.140.1.2.1
            X509v3 CRL Distribution Points: 
                Full Name:
                  URI:http://c.pki.goog/wr2/GSyT1N4PBrg.crl

            CT Precertificate SCTs: 
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 64:11:C4:6C:A4:12:EC:A7:89:1C:A2:02:2E:00:BC:AB:
                                4F:28:07:D4:1E:35:27:AB:EA:FE:D5:03:C9:7D:CD:F0
                    Timestamp : Mar 16 09:39:58.345 2026 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:44:02:20:5F:7F:80:B7:3D:5F:E2:84:1A:58:56:D2:
                                79:6A:6B:4C:04:31:34:40:B4:18:B9:55:DB:AD:BD:1B:
                                E5:1E:09:49:02:20:09:9D:99:F8:1C:D9:60:18:25:E9:
                                4D:36:CA:08:EE:F2:39:73:8D:76:07:46:14:98:DD:39:
                                BA:7F:59:A9:1F:E5
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 96:97:64:BF:55:58:97:AD:F7:43:87:68:37:08:42:77:
                                E9:F0:3A:D5:F6:A4:F3:36:6E:46:A4:3F:0F:CA:A9:C6
                    Timestamp : Mar 16 09:39:58.364 2026 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:20:30:E6:5C:12:C0:13:F3:82:F3:46:EA:9B:
                                01:62:40:CA:72:16:23:57:2D:6F:20:F5:62:1B:FB:E5:
                                58:27:D4:98:02:21:00:DB:57:DD:66:E4:C5:09:04:B2:
                                84:D2:35:9C:45:C0:4F:05:F7:1F:F5:40:F9:A8:39:37:
                                4B:27:FF:6F:A9:42:97
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        7e:ca:93:5b:a5:dc:82:16:bf:b6:e9:62:39:f7:63:ad:5a:f1:
        eb:df:33:59:1d:eb:b3:b5:bb:d9:fd:e7:16:f9:c2:9b:f7:cf:
        9f:ef:9d:60:52:7c:b0:fb:60:5a:e7:f8:0a:1f:c2:53:72:26:
        fe:b6:63:f2:8d:83:0c:22:11:2c:93:07:33:43:72:86:e3:59:
        f3:3c:e6:0a:85:9c:3e:a3:e5:5c:a7:4f:d7:2d:c3:28:a7:9b:
        cf:56:a9:90:48:1a:81:85:0d:e4:ef:c3:cb:a6:9b:88:aa:76:
        c1:f1:3a:21:2f:e2:2d:96:68:71:cc:58:9b:ee:30:2e:ab:b6:
        49:db:76:41:a9:cf:6e:3b:9d:da:ea:41:7e:bb:b6:be:84:3b:
        49:44:8c:bc:c3:4b:5b:dd:a8:47:74:50:98:51:8e:b3:7f:9f:
        c9:d2:b9:3f:11:eb:56:3b:4d:9d:6c:45:cc:dd:ea:af:62:fe:
        f1:e7:54:54:52:f4:90:6a:34:dc:7d:a2:60:d8:4f:c2:31:2e:
        c2:a3:3e:89:08:a2:be:8d:28:29:a0:be:7d:32:d7:07:fb:aa:
        48:da:ca:e3:33:a7:bf:3b:8f:17:72:dd:93:b3:7a:fd:4a:45:
        49:ac:a3:b2:15:69:c1:19:7f:ec:dd:a9:ae:1b:da:3a:45:1e:
        88:d1:31:75




1. On the leaf certificate (the one for google.com), note the Authority Key Identifier (AKI).  
In the certificate of the CA identified by the Issuer Name on the leaf certificate, identify  
the Subject Key Identifier (SKI).  

for file `cert0.txt

SKI
`X509v3 Subject Key Identifier: 7B:FE:6F:67:B3:A8:00:22:79:90:35:8D:84:36:5C:E7:42:0A:1E:A4

AKI
`X509v3 Authority Key Identifier: DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30

2. Similarly, on the intermediate CA, match the AKI with the SKI in its parent CA certificate.  
file cert1.txt

`X509v3 Subject Key Identifier: DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30

`X509v3 Authority Key Identifier: E4:AF:2B:26:71:1A:2B:48:27:85:2F:52:66:2C:EF:F0:89:13:71:3E

file cert2.txt
`X509v3 Subject Key Identifier: E4:AF:2B:26:71:1A:2B:48:27:85:2F:52:66:2C:EF:F0:89:13:71:3E

`X509v3 Authority Key Identifier: 60:7B:66:1A:45:0D:97:CA:89:50:2F:7D:04:CD:34:A8:FF:FC:FD:4B



3. The root CA certificate is not included in the connection with the server. The client is  
supposed to have it and trust it. The cURL project distribute Mozilla’s list of CA certifi-  
cates trusted for web purposes at https://curl.se/docs/caextract.html. Download  
the cacert.pem file.  




4. Search in cacert.pem for the Common Name of the issuer from the highest sub-CA deliv-  
ered on the connection to google.com. The file will list the “Common Name” or in some  
cases other preferred names for the CA. Again, match the AKI in the sub-CA with the  
SKI in this certificate.  



5. Validate the leaf certificate using OpenSSL, by giving it all intermediate certificates via  
the -untrusted options, and the cacert.pem as a list of trusted CAs:  
`openssl verify -untrusted [...] -untrusted [...] -CAfile cacert.pem -show_chain [...]  

input
`openssl verify -untrusted cert1.txt -untrusted cert2.txt -CAfile cacert.pem -show_chain cert0.txt

output
```
cert0.txt: OK
Chain:
depth=0: CN=www.google.com (untrusted)
depth=1: C=US, O=Google Trust Services, CN=WR2 (untrusted)
depth=2: C=US, O=Google Trust Services LLC, CN=GTS Root R1
```

<span style="color:rgb(219, 0, 0)">ok means successful verification</span>

Is leaf certificate on google the same as for
Yes becasue google.com has subject alternativ names
which one of them is  `admob-cn.com

certificate has multiple domain names listed in its **Subject Alternative Name (SAN)** extension

openssl x509 on admob-cn.com leaf certificate

`X509v3 Subject Alternative Name: DNS:*.google.com,......


To verify that the certificate is the same run command
`openssl verify -untrusted elsecert1.txt -untrusted elsecert2.txt -CAfile cacert.pem -show_chain cert0.txt

where we pass the leaf certificate of google.com rather than `admob-cn.com

we can also visually inspect whether the leaf certificates are the same by connecting to the domain with openssl with -showcert flag (look for exact command above)


---


### `openssl verify`

- This is the OpenSSL tool used to **verify a certificate chain**.
- It checks that a given certificate (`cert0.txt`) is valid and trusted.
- Validity means:
    - Each certificate in the chain is correctly signed by its issuer.
    - The chain ends in a **trusted root CA**.


### `-untrusted cert1.txt -untrusted cert2.txt`

- These provide **intermediate certificates** (not root CAs) that may be needed to build the chain.
- OpenSSL will use them to connect the **leaf certificate** (`cert0.txt`) to a trusted root in `cacert.pem`.
- Order does **not** matter, but each intermediate should be included.

> Example in your chain:
> 
> - `cert0.txt` → leaf certificate (google.com)
> - `cert1.txt` → intermediate 1
> - `cert2.txt` → intermediate 2


###  `-CAfile cacert.pem`

- This is a file containing **trusted root certificates**, in PEM format.
- OpenSSL uses it as the **trust anchor**.
- Usually `cacert.pem` comes from Mozilla or your system.

6. Although recent versions of OpenSSL try to use the OS list of trusted CAs, you can pass  
your own list of CAs to validate certificates during a connection by adding -CAfile to the  
s_client command. You can see the result at the top of the output.







### **Subject Key Identifier (SKI)**

- Found in a **certificate** (usually a CA certificate).
- It is a **short identifier derived from the certificate’s public key**.
- Purpose: to uniquely identify the certificate’s public key, so other certificates can reference it.
- Think of it like: **“This is the fingerprint of my public key.”**

**Example (from a CA certificate):**

X509v3 Subject Key Identifier:   
    12:34:56:78:AB:CD:EF:01:23:45:67:89:AB:CD:EF:01:23:45:67:89

---

### **Authority Key Identifier (AKI)**

- Found in a **leaf certificate** (like `www.google.com`).
- Points to the **SKI of the CA that issued this certificate**.
- Purpose: to tell clients, **“This certificate was signed by the CA whose key has this identifier.”**
- Think of it like: **“I was signed by the CA with this key fingerprint.”**

**Example (from your Google leaf certificate):**

X509v3 Authority Key Identifier:   
    DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30



leaf - google.com has authority key id which will be the subject key id of the next certificate in the chain of trust

cert0 has 

X509v3 Authority Key Identifier: 
                DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30

cert1 has 
X509v3 Subject Key Identifier: 
                DE:1B:1E:ED:79:15:D4:3E:37:24:C3:21:BB:EC:34:39:6D:42:B2:30



<span style="color:rgb(219, 0, 0)">subject points to itself<br>authority points to next one</span> 