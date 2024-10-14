# ⚡ Interact with node via websockets

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Generate TLS certificate

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

Add the certificate and key to **KLY\_Plugins/certificates**

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now, change the **KLY\_Plugins/dev\_websocket\_api/configs.json**

```json
{
        
    "PORT":9999,
    "HOST":"::",

    "TLS_KEY":"KLY_Plugins/certificates/<NAME>.key",
    "TLS_CERT":"KLY_Plugins/certificates/<NAME>.crt",

    "mTLS":false,
    "mTLS_CA":"",

    "KEEP_ALIVE_INTERVAL":20000,
    "KEEP_ALIVE":true,
    "KEEP_ALIVE_GRACE_PERIOD":10000,
    
    "MAX_FRAME_SIZE":65536,
    "MAX_MSG_SIZE":1049000000

}
```

Finally, modify the node configs. Go to the directory of your symbiote where directories **CONFIGS**, **CHAINDATA**, **GENESIS** located.

Modify the **CONFIGS/common.json** section with **PLUGINS** to enable API over WebSockets:

```json
"PLUGINS":["dev_savitar/server.js","dev_websocket_api/server.js"]
```

### Set on client side(API user, web1337 side)

On client side you just need to add the path to certificate to mark it as reliable



<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Usage
