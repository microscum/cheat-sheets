# Generate a new key
      openssl genrsa -out server.key 2048

# Generate a new CSR
      openssl req -sha256 -new -key server.key -out server.csr

# Check certificate against CA
      openssl verify -verbose -CApath ./CA/ -CAfile ./CA/cacert.pem cert.pem

# Self Signed
      openssl req -new -sha256 -newkey rsa:2048 -days 1095 -nodes -x509 -keyout server.key -out server.pem

# crlf fix
      perl -pi -e 's/\015$//' badcertwithlf.pem

# match keys, certs and requests

      Simply compare the md5 hash of the private key modulus, the certificate modulus, or the CSR modulus and it tells you whether they match or not.
      openssl x509 -noout -modulus -in yoursignedcert.pem | openssl md5
      openssl rsa -noout -modulus -in yourkey.key | openssl md5
      openssl req -noout -modulus -in yourcsrfile.csr | openssl md5
# criar uma CA
      /usr/share/ssl/misc/CA -newca

# Generate a CSR
      /usr/share/ssl/misc/CA.sh -newreq

# Cert -> CSR
      openssl x509 -x509toreq -in server.crt -out server.csr -signkey server.key

# Sign
      /usr/share/ssl/misc/CA.sh -sign

# Decrypt private key (so Apache/nginx won't ask for it)
     openssl rsa -in newkey.pem -out wwwkeyunsecure.pem
     cat wwwkeyunsecure.pem >> /etc/ssl/certs/imapd.pem

# Encrypt private key AES or 3DES
     openssl rsa -in unencrypted.key -aes256 -out encrypted.key
     openssl rsa -in unencrypted.key -des3 -out encrypted.key

# Get some info
     openssl x509 -noout -text -nameopt multiline,utf8 -in certificado.pem
     openssl x509 -noout -text -fingerprint -in cert.pem
     openssl s_client -showcerts -connect www.google.com:443
     openssl req -text -noout -in req.pem

# list P7B
    openssl pkcs7 -in certs.p7b -print_certs -out certs.pem

# PEM -> PFX
      openssl pkcs12 -export -out alvaro.p12 -name "Certificado do Alvaro" -inkey newreq.pem -in newcert.pem -certfile cacert.pem

# PFX -> pem (with key)
      openssl pkcs12 -in ClientAuthCert.pfx -out ClientAuthCertKey.pem -nodes -clcerts

# DER (.crt .cer .der) to PEM
      openssl x509 -inform der -in MYCERT.cer -out MYCERT.pem

# PEM -> DER
     openssl x509 -outform der -in MYCERT.pem -out MYCERT.der
     openssl rsa -in key.pem -outform DER -out keyout.der

# JKS -> P12
      keytool -importkeystore -srckeystore keystore.jks -srcstoretype JKS -deststoretype PKCS12 -destkeystore keystore.p12

# P12 -> JKS
      keytool -importkeystore -srckeystore keystore.p12 -srcstoretype PKCS12 -deststoretype JKS -destkeystore keystore.jks

# Revoke
     openssl ca -revoke CA/newcerts/cert.pem
     openssl ca -gencrl -out CA/crl/ca.crl
     openssl crl -text -noout -in CA/crl/ca.crl
     openssl crl -text -noout -in CA/crl/ca.der -inform der

# Base64 encoding/decoding
     openssl enc -base64 -in myfile -out myfile.b64
     openssl enc -d -base64 -in myfile.b64 -out myfile.decoded
     echo username:passwd | openssl base64
     echo dXNlcm5hbWU6cGFzc3dkCg== | openssl base64 -d

#  Generate a Java keystore and key pair
     keytool -genkey -alias mydomain -keyalg RSA -keysize 2048 -keystore mykeystore.jks

# Generate a certificate signing request (CSR) for an existing Java keystore
     keytool -certreq -alias mydomain -keyalg RSA -file mydomain.csr -keystore mykeystore.jks

# Import a root or intermediate CA certificate to an existing Java keystore
     keytool -import -trustcacerts -alias ca-root -file ca-root.pem -keystore cacerts
     keytool -import -trustcacerts -alias thawte-root -file thawte.crt -keystore keystore.jks

# Generate a keystore and self-signed certificate
     keytool -genkey -keyalg RSA -alias selfsigned -keystore keystore.jks -storepass password -validity 360
     openssl pkcs8 -topk8 -nocrypt -in key.pem -inform PEM -out key.der -outform DER
     openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER

# For L7: intermediate CA1 >>> intermediate CA2 >>> root CA)
     openssl pkcs12 -export -in input.crt -inkey input.key -certfile root.crt -out bundle.p12

# Better DH for nginx/Apache
     openssl dhparam -out dhparam.pem 2048

# Grab a certificate from a server that requires SSL authentication
     openssl s_client -connect sslclientauth.reguly.com:443 -cert alvarows_ssl.pem -key alvarows_ssl.key
     openssl.cnf: subjectAltName="DNS:localhost,IP:127.0.0.1,DNS:roselcdv0001npg,DNS:roselcdv0001npg.local
