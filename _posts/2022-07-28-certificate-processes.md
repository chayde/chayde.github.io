---
layout: post
title: "OpenSSL Certificate Processes"
date: 2022-07-28 09:10:00 -0400
categories: [documentation]
tags: [homelab, ubuntu, bash, openssl, certificate]
---

# Self Signed Certificate with SANs

## Open SSL Command to run
The following command will generate a self signed certificate that will be valid for 10 years (3560 days), the key to this working is the use of an SSL options file. 
```bash
openssl req -x509 -days 3560 -sha256 -newkey rsa:4096 -nodes -keyout example.key -out example.crt -config v3.cnf
```

## SSL Options - v3.cnf
The contents of an example OpenSSL options file. The SAN entries are under the alt section "[alt_names]"
```bash
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = <REPLACE_WITH_TWO_LETTER_COUNTRY>
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = <REPLACE_WITH_FULL_STATE_NAME>
localityName = Locality Name (eg, city)
localityName_default = <REPLACE_WITH_CITY_NAME>
0.organizationName = Organization Name (eg, company)
0.organizationName_default = <REPLACE_WITH_ORGANIZATIONAL_NAME>
commonName = Common Name
commonName_default = <REPLACE_WITH_CERTIFICATE_CN_FQDN>

[ v3_req ]
# Extensions to add to a certificate request
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
extendedKeyUsage = serverAuth, clientAuth, codeSigning, emailProtection

[alt_names]
DNS.1 = <REPLACE_WITH_CERTIFICATE_CN_FQDN>
DNS.2 = <REPLACE_WITH_SAN1_FQDN>
DNS.3 = <REPLACE_WITH_SAN2_FQDN>
DNS.x = <...>
```



