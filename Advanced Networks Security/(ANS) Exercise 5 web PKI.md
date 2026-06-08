run this to connect
`psql -h crt.sh -p 5432 -U guest certwatch

This command is how you **open a connection to the crt.sh PostgreSQL database** using the `psql` command-line tool.

It queries **crt.sh** — a public certificate transparency log database — to find all **SSL/TLS certificates** ever issued for `ru.nl` (Radboud University), showing issuer, validity dates, serial numbers etc. Useful for **recon/security research**.

```
WITH ci AS (
    SELECT min(sub.CERTIFICATE_ID) ID,
           min(sub.ISSUER_CA_ID) ISSUER_CA_ID,
           array_agg(DISTINCT sub.NAME_VALUE) NAME_VALUES,
           x509_commonName(sub.CERTIFICATE) COMMON_NAME,
           x509_notBefore(sub.CERTIFICATE) NOT_BEFORE,
           x509_notAfter(sub.CERTIFICATE) NOT_AFTER,
           encode(x509_serialNumber(sub.CERTIFICATE), 'hex') SERIAL_NUMBER,
           count(sub.CERTIFICATE_ID)::bigint RESULT_COUNT
        FROM (SELECT cai.*
                  FROM certificate_and_identities cai
                  WHERE plainto_tsquery('certwatch', $1) @@ identities(cai.CERTIFICATE)
                      AND cai.NAME_VALUE ILIKE ('%' || 'www.ru.nl' || '%')
                      AND cai.NAME_TYPE = 'san:dNSName' -- dNSName
                  LIMIT 10000
             ) sub
        GROUP BY sub.CERTIFICATE
)
SELECT ci.ISSUER_CA_ID,
        ca.NAME ISSUER_NAME,
        ci.COMMON_NAME,
        array_to_string(ci.NAME_VALUES, chr(10)) NAME_VALUE,
        ci.ID ID,
        le.ENTRY_TIMESTAMP,
        ci.NOT_BEFORE,
        ci.NOT_AFTER,
        ci.SERIAL_NUMBER,
        ci.RESULT_COUNT
    FROM ci
            LEFT JOIN LATERAL (
                SELECT min(ctle.ENTRY_TIMESTAMP) ENTRY_TIMESTAMP
                    FROM ct_log_entry ctle
                    WHERE ctle.CERTIFICATE_ID = ci.ID
            ) le ON TRUE,
         ca
    WHERE ci.ISSUER_CA_ID = ca.ID
    ORDER BY le.ENTRY_TIMESTAMP DESC NULLS LAST;
```

Subdomain enumeration. Let’s look at ru.nl subdomains.  
1. Devise a SQL query to list all unique domain names that ever appeared in logged certifi-  
cates as subdomains of ru.nl. Hint: the certificate and identities table is all you  
need. The database schema is available at https://github.com/crtsh/certwatch_db/  
blob/master/sql/create_schema.sql. How many there are?  

connect to `psql -h crt.sh -p 5432 -U guest certwatch

```
SELECT COUNT(DISTINCT cai.NAME_VALUE)  
FROM certificate_and_identities cai  
WHERE cai.NAME_VALUE ILIKE '%.ru.nl'  
AND cai.NAME_VALUE != 'ru.nl';
```

2. What is the latest subdomain that came into existence according to CT logs? Penetration  
testers and malicious actors might be interested in services behind fresh domains that  
might not yet be fully configured. Once identified, search this domain on https://crt.sh  
to find more interesting things. What do you notice?

Key management. Let’s now dig into the key management at RU. Our goal is to find out for  
how long keys are typically used, since the same key can be “packaged” into multiple certificates.  

3. Reuse the WITH ci AS block given from your web search above and add the following  
column to it:  
encode(digest(x509_publickey(sub.CERTIFICATE), 'sha256'), 'hex') SPKI_HASH,  

Then, use this table ci to aggregate all certificates by SPKI HASH (i.e., GROUP BY), and  
display the earliest associated NotBefore and latest NotAfter dates, add a column that  
computes the lifespan, and order by longest lifespan first.  
Hint 1: The ci table is constructed with a LIMIT 10000, hence your results may not  
include all known certificates. Remove this limit once your query runs smoothly.  


4. About the longest lifespan:  
(a) What is the longest period a key has been valid for?  
(b) How many (pre)certificates include this key? Note that due to the functioning of CT  
logs and Signed Certificate Timestamp (SCT), both precertificates and corresponding  
final certificates could be logged although they represent the same thing and only the  
final one is functional. Attempting to use the “Deduplicate (pre)certificate pairs”  
option will result in significant overhead and will likely timeout.  
(c) Which domains are affected?  


5. About the longest active keys:  
(a) How many keys are still used in certificates valid today that have been used for more  
than 400 days?  
(b) What’s the domain that has been exposed for the longest period of time to potential  
key compromise? Is this key weak from a cryptographic strength perspective?