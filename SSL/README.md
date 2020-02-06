https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/

https://gist.github.com/Soarez/9688998

https://marmelab.com/blog/2019/01/23/https-in-development.html

## Create the root certification authority

First, a RSA key must be generated:
    openssl genrsa -out rootCA.key 2048

Then create the root SSL certificate with that key. You can edit the fields of the certificate in `rootCA.csr.cnf`:
    openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1048576 -out rootCA.pem -config <( cat rootCA.csr.cnf )

Import the root certificate in your browser, mobile, etc. 

## Create the domain certificate

Generate a certificate key for the host:
    openssl req -new -sha256 -nodes -out server.csr -newkey rsa:2048 -keyout localhost.key -config <( cat localhost.csr.cnf )

Generate a certificate signing request (csr) for that host. That's what te root CA will sign. Here is where you specify for wich hosts, etc. 
It will use v3.ext for configuring the csr:

AQUI HACE DOS PASOS EN UNO; REVISAR

    openssl x509 -req -in localhost.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out server.crt -days 500 -sha256 -extfile v3.ext

See advanced_example.v3.ext for an advanced example.

