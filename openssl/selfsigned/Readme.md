# Use cases

## External CA

[X] Create certificate signed by external CA

[X] Download certificate from SOAP API
[] Use it in Java application ->

## Rest API

[] Develop plugin to implement api rest to download this certificate
[] Integration it in Java application

## Bulk && roles && notification

[] Download muliple certificate
[] Notification
[] Roles and visilbility
[] Define hierarchy between CA/user/roles

## HSM

# SubCA signed with an external CA

## EJBCA

* Add Certication authorities
  * Signed By EXTERNAL CA	
  * CA Serial Number Octet Size: 20
  * Make Certificate request (CSR)
  * Generate CSR response see below
  * Import response (Certicate chain)
* Add End entity
  * Certificate Profile: Profile
  * CA: new CA signed by external

## External CA signing

``` cmd
# https://whatsmychaincert.com/?getacert.com
set CNRoot=GoDaddyRoot
set CN=GoDaddy

# Create CA
openssl req -x509 -newkey rsa:2048 -sha256 -days 365 -nodes -passout pass:"1234" -keyout root.key -out root.crt -subj "/CN=%CNRoot%" 

# Generate rsa
openssl genrsa -aes256 -passout pass:"1234" -out rsa.key 2048

# Autosign
openssl req -x509 -new  -key rsa.key -passin pass:"1234" -sha256 -days 365 -out root.pem -nodes -subj "/CN=%CNRoot%"
openssl x509 -in root.pem -noout -text 

# Sign CSR
openssl x509 -req -in %CN%.csr -days 365 -CA root.crt -CAkey root.key  -CAcreateserial -out %CN%.crt -extensions "usr_cert"
openssl x509 -in %CN%.crt -noout -text

# Generate certificate chains
# -----BEGIN CERTIFICATE-----
# (Your Primary SSL certificate: your_domain_name.crt)
# -----END CERTIFICATE-----
# -----BEGIN CERTIFICATE-----
# (Your Intermediate certificate: DigiCertCA.crt)
# -----END CERTIFICATE-----
# -----BEGIN CERTIFICATE-----
# (Your Root certificate: TrustedRoot.crt)
# -----END CERTIFICATE-----
cat %CN%.crt root.crt   > %CN%Chain.crt

# Verify chain untrusted: sef-signed 
openssl verify -verbose  -CAfile root.crt  %CN%.crt
```

##  Use SOAP

## EJBCA

* Create end user
* Select CA signed by External CA
  * 

## Chrome boomerang

* Install boomerang in chrome to use browser certificate authentication
* Import service *https://localhost/ejbca/ejbcaws/ejbcaws?wsdl*
* create requests (remove ALWAYS SOAPACTION header)


### Examples

* List of CA
```xml
<x:Envelope
    xmlns:x="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:ws="http://ws.protocol.core.ejbca.org/">
    <x:Header/>
    <x:Body>
        <ws:getAvailableCAs></ws:getAvailableCAs>
    </x:Body>
</x:Envelope>
```

* Query for a certifcate associated to a user
```xml
<x:Envelope
    xmlns:x="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:ws="http://ws.protocol.core.ejbca.org/">
    <x:Header/>
    <x:Body>
        <ws:findCerts>
            <arg0>ancv</arg0>
            <arg1>false</arg1>
        </ws:findCerts>
    </x:Body>
</x:Envelope>
```

* Get certifcates list
```xml
<x:Envelope
    xmlns:x="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:ws="http://ws.protocol.core.ejbca.org/">
    <x:Header/>
    <x:Body>
        <ws:getCertificatesByExpirationTime>
            <arg0>3000</arg0>
            <arg1>100</arg1>
        </ws:getCertificatesByExpirationTime>
    </x:Body>
</x:Envelope>
<x:Envelope
    xmlns:x="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:ws="http://ws.protocol.core.ejbca.org/">
    <x:Header/>
    <x:Body>
        <ws:getCertificatesByExpirationTimeAndIssuer>
            <arg0>3000</arg0>
            <arg1>CN=ancv</arg1>
            <arg2>10</arg2>
        </ws:getCertificatesByExpirationTimeAndIssuer>
    </x:Body>
</x:Envelope>
```
