## **Question 1** (1 point)
Which DNS record type is used to store IPv6 addresses?

1. MX
2. SOA
<span style="color:rgb(219, 0, 0)">3. AAAA</span>
3. A
## **Question 2** (1 point)
Why is a generic signed "NXDOMAIN" response considered dangerous?
1. It is too large for UDP packets.                                              
2. It does not include a TTL value.                                                
3. It could be used to disrupt all domains at the resolver.                        
<span style="color:rgb(219, 0, 0)">4. It could be replayed by an attacker to deny the existence of other valid names. </span>

## **Question 3** (1 point)
Which one of the following record type exists that could have been queried for tttt.science.ru.nl?

|       |                                                                                                                                                                |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       | "PC" "IP"                                                                                                                                                      |
|       | "v=spf1 include:customers.clickdimensions.com include:spf.protection.outlook.com include:spf-production.science.ru.nl include:spf-test-acc.science.ru.nl -all" |
| wrong | "MS=F0FC8830ACA38F14D803B6E294D582BDA7C7C758"                                                                                                                  |
| this  | v=spf1 -all                                                                                                                                                    |

when u query NSEC tells what records exist - TXT RRSIG and 
then query ttt..... with TXT and you will see v-spf....

## **Question 4** (1 point)
 
How does an NSEC record prove that a specific record type (e.g., MX) does not exist at an existing domain?

|     |                                                                     |
| --- | ------------------------------------------------------------------- |
| x   | By including a bitmap of RR types that actually exist at that name. |
|     | By providing a list of all existing domains in the zone.            |
|     | By returning an error code 404.                                     |
|     | By redirecting the user to a root server.                           |

## **Question 5** (1 point)

What is the purpose of the dnssec-validation auto; setting in a BIND configuration?

|     |                                                                      |
| --- | -------------------------------------------------------------------- |
|     | It disables all security checks to increase speed.                   |
|     | It forces the resolver to use a local private key for all queries.   |
|     | It allows the resolver to sign records on behalf of the client.      |
| x   | It enables validation using default trust anchors to verify records. |
|     |                                                                      |

## **Question 6** (1 point)

Which tool can be used specifically to validate DNSSEC records and show a "fully validated" output?

|      |           |
| ---- | --------- |
| this | delv      |
|      | ping      |
|      | bind9     |
|      | wireshark |

## **Question 7** (1 point)

 

What is the primary function of the dig utility


|   |   |
|---|---|
||To capture network packets.|
||To interact with DNS resolvers and retrieve records.|
||To encrypt local files for DNSSEC.|
||To act as an authoritative name server.|

## **Question 8** (1 point)

 

When investigating the failure of _dnssec-failed.org_, what is the root cause of the validation error?


|   |   |
|---|---|
||The RRSIG record has expired by more than 10 years.|
||The DS record in the parent zone does not match the DNSKEYs provided by the nameservers.|
||The server is physically offline.|
||The resolver does not have the `dnsutils` package installed.|

## **Question 9** (1 point)

 
Which one of the following is a name server for the nl. zone?


|   |   |
|---|---|
||ns3.dns.nl|
||ns.dns.nl|
||ns1.surfnet.nl|
||ns2.dns.nl|

## **Question 10** (1 point)

 


What are the DNSKEY lengths for the nl. zone ?


|       |                          |
| ----- | ------------------------ |
| wrong | RSA 2048 bits, too large |
|       | RSA 512 bits, insecure   |
|       | RSA 1024 bits, secure    |
| wrong | ECDSA 256 bits, secure   |

  
got 8/10