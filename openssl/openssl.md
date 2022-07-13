OpenSSL operations
==================

Common things you may need to do with openssl cli utility. 

-   Get a pseudorandom MAC address like string:
 
        # openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/.$//'

-   Generate password:

        $ openssl rand -base64 20 |md5 | head -c10; echo

    Generate self-signed cert:

        $ openssl req -x509 -newkey rsa:4096 -keyout cert.key -out cert.pem -days 365 -nodes -subj "/C=BE/ST=bebebe/L=bebebe/O=bebebe/OU=bebe/CN=my-canonical-name.bebebe.be"

-   Generate new csr:

        $ openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr

-   Check generated csr:

        $ openssl req -text -noout -verify -in CSR.csr

-   Check a private key:

        $ openssl rsa -in privateKey.key -check

-   Check a certificate:

        $ openssl x509 -in certificate.crt -text -noout

-   Generate csr for existing private key:

        $ openssl req -out CSR.csr -key privateKey.key -new

-   Generate a certificate signing request based on an existing certificate:

        $ openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key

-   Check that pkey matches cert:

        $ openssl x509 -noout -modulus -in certificate.crt | openssl md5
        $ openssl rsa -noout -modulus -in privateKey.key | openssl md5
        $ openssl req -noout -modulus -in CSR.csr | openssl md5

-   Convert pem to p12:

        $ openssl pkcs12 -export -in /tmp/key.pem -out /tmp/key.p12

-   Check cert on remote smtp server:

        $ openssl s_client -connect some.mailserver.com:25 -starttls smtp 2>&1|grep notAfter

-   Check cert on remote web server:

        $ openssl s_client -connect some.webserver.com:443 2>/dev/null| openssl x509 -noout -text

-   Generate a pseudo-random hex string:

        $ openssl rand -hex 16

-   Convert PKCS#1 to PKCS#8:

        $ openssl pkcs8 -in old.key -topk8 -nocrypt -out new.p8

-   Check ssl connection:

        $ openssl s_client -connect remote.server.com:443 -CAfile cacert.crt

-   Extract crt from pem:

        $ openssl crl2pkcs7 -nocrl -certfile private.pem | openssl pkcs7 -print_certs -out certificate.crt

-   Extract private key from pem:

        $ openssl pkey -in private.pem -out key.key

-   View crt contents:

        $ openssl x509 -in mycert.crt -text -noout

-   Check for TLS 1.2 support:

        $ openssl s_client -connect google.com:443 -tls1_2

