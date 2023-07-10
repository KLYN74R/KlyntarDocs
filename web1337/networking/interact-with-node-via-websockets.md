# ⚡ Interact with node via websockets

## Create instance



## Generate TLS certificates

Prepare the `req.cnf` file

```ini
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no


#Set your own
[req_distinguished_name]
C = UA
ST = KIEV
L = Kiev,UA
O = KLYNTAR,inc
OU = KLY@CERT
CN = www.localhost.com


[v3_req]
keyUsage = critical, digitalSignature, keyAgreement
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

#Set all you need
[alt_names]
DNS.1 = somedomain.com # set your domain
DNS.2 = *.anotherdomain.xyz # wildcard
IP.1 = 127.0.0.1 # specific interfaces
IP.2 = ::1
```

Then

```sh
openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout cert.key -out cert.pem -config req.cnf -sha256
```

You'll get RSA-4096 private key and appropriate self-signed certificate

```sh
root@cb4d8e656db5:~/CERTS# openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout cert.key -out cert.pem -config req.cnf -sha256
Generating a RSA private key
........................++++
..................++++
writing new private key to 'cert.key'
-----
root@cb4d8e656db5:~/CERTS# ls
cert.key  cert.pem  req.cnf
root@cb4d8e656db5:~/CERTS#
```



### Set on server side(it's node)



### Set on client side(API user, web1337 side)



## Usage
