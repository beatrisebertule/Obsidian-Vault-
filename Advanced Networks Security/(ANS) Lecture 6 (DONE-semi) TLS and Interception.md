semi - review quizz

<span style="color:rgb(219, 0, 0)">MORE ON PAPER NOTES</span>
### TLS 1.2 

TLS replaced SSL

before u start
you need a certificate from CA

pre-master secret **PMS**
derive MS - master secret

who depends on what

KDF key ..function
HMAC - hashed based auth ..

![[Pasted image 20260323121805.png]]

server private key
if accese can decrypt mesag past curent future

no forawrad secrecy

https://tls12.xargs.org/#server-hello

Let's encrypt
CA that issues certificat for free (before you had ti pay)
fowrda secret - comproimsoon of server long-term key
shoudl not lead to decytpion of past communications

rsa does not aoofer forawrd secrecy

diffie gelman does offer it


ecdhe last e <span style="color:rgb(219, 0, 0)">ephemernal</span> - key used for that session and not otehr session
so long term secert wont impact paste ecnryption essgaes

whistleblower snowden
shaped whocj hcipher suiets are sued - no longer rsa as cannot provide forward secrecyu
### post-Snowden TLS
communication should have forward secrecy
the generated PMS used to derived MS, then used to encrypt communication no longer 


LTS with ECDHE

![[Pasted image 20260323121831.png]]
![[Pasted image 20260323122816.png]]
rsa keye change vs ecdhe key exchange


### Interception
break tls

malicious purpose -> man in teh middle
not malicious reasonj - middle box - middel ni two connections 

### Middle boxes

TLS session splitting
middle box - the certificate of the middlebox is signed by a trusted CA
client needs to be provisioned with the certificate of the middlebox
we have to trust that the middlebox verifies the server's certificate correctly
dangerous if middlebox does not do a proper job

bad TLS session splitting - lenovo

middle boxes break end-to-end encryption

![[Pasted image 20260323135123.png]]

![[Pasted image 20260323135152.png]]

## Middle box-friendly protocols
doe snot split TLS session the same way as middlebox does

### SSL Keylog 

tell the browser to record crypto parameter in a file - SSL keylog file
share this file with a 3rd party - middlebox that needs to decrypt communication
![[Pasted image 20260323164945.png]]


### Sharing DH Private Shares
no ephemeral key
generate one DH key
share the generated non-ephemeral key with the middlebox
![[Pasted image 20260323170018.png]]

the middle box is able to generate the rest of the crypto material given the shared secret key

loss of forward secrecy :()
overall less secure 



### Keyless SSL (Cloudflare)

for evry handshake - please sign my public key Yn
server has to be online

clients don't share private keys with 3rd parties

cloudfrae (SERCER 10% of internet)
-> KEYLESS SSL

setup
CLEINT - CDN - SERVER


let CDN (content delivery network) impersonate server
caches info


![[Pasted image 20260323171405.png]]

https://deepwiki.com/cloudflare/keyless/1.1-introduction-to-keyless-ssl
https://www.cloudflare.com/en-gb/learning/ssl/keyless-ssl/

Keyless SSL is based on the fact that there is only one time when the private key is used during the TLS handshake, which occurs at the beginning of a TLS communication session. Keyless SSL works by splitting the steps of the [TLS handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/) up geographically. A cloud vendor offering keyless SSL moves the private key part of the process to another server, usually a server that the customer keeps on premises.

<span style="color:rgb(219, 0, 0)">how does CDN have derive the shared secret to decrypt traffic?</span>


### Delegated Credentials (for CDN)

no need to contact server for every handshake

server geneartes delicatin credential key pair (public prive)
sen ethe privae dc an dsigend public dc


![[Pasted image 20260323181541.png]]


keyless sssl and delegated credentilas establsih tls connection ebtween teh cleint and cdn
server is now "behind cloudlfare
### TLS1.3
no more RSA key exchange (removed old crypto algorithms)
forward secrecy by default

Removed old/deprecated algorithms  
	No more RSA key exchange, anonymous ciphersuites, broken hash functions  
Keep only a small set of secure algorithms  
Forward secrecy by default  
Reduce the number of round trips for the handshake to one or even zero!  
	Client key share (DHE key) sent already in the ClientHello  
Encrypt part of the handshake  
	Certificates and some options are now encrypted


perspective on future
we don't need to decrypt the traffic to detect malicious network activity 
machine learning can help
guess which webpage visit by patterns on traffic


we also need to worry about metadata



# Reviewed Notes
[[chat summary tls]]
