https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/

https://gist.github.com/Soarez/9688998

https://marmelab.com/blog/2019/01/23/https-in-development.html

## Create the root certification authority

1) First, a RSA key must be generated:
    openssl genrsa -out dummyRootCA.key 2048

2) Then create the root SSL certificate with that key. You can edit the fields of the certificate in `rootCA.csr.cnf`:
    openssl req -x509 -new -nodes -key dummyRootCA.key -sha256 -days 1048576 -out dummyRootCA.pem -config <( cat dummyRootCA.cnf )

Import the root certificate (dummyRootCA.pem) in your browser, mobile, etc. 

## Create the domain certificate

1) Generate a key pair for the domain: 
    openssl genrsa -out localhost.private.key 2048

2) Generate a certificate signing request for the domain:
    openssl req -new -key localhost.private.key -out localhost.csr -config <( cat localhost.cnf )

You can change certificate data editing localhost.cnf before issuing the signing request. There is a multidomain example in localhost.multidomain.cnf

Alternatively you can perform steps 1 and 2 with a single command: 
    openssl req -new -sha256 -nodes -out localhost.csr -newkey rsa:2048 -keyout localhost.private.key -config <( cat localhost.cnf )


