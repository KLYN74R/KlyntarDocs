---
description: Use proxy to interact with KLY network
---

# 🙈 Using proxy

## TOR Proxy

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Probably the most known hidden network is TOR. We already have a simple and lightweight docker image for you to set up your own local proxy:

{% embed url="https://github.com/KLYN74R/KlyntarBaseImages/tree/main/Tor" %}

### Build

```
docker build -t tag .
```

### Or install

```
docker pull klyntar/dev_tor@sha256:9650d231a781cace3c71591faa29fde37ca46c45cdae7379a34e1f2c532c8535
```

### Create container

```
docker run -dtp 5666:5666 --name tor_proxy <image_name>
```

### Start proxy

```
docker exec -ti tor_proxy sudo -u tor tor
```

{% hint style="success" %}
This will run proxy on 0.0.0.0, so you can use this SOCKS proxy via port 5666. If you need,modify `torrc` file and rebuild the image
{% endhint %}



## Usage

Using TOR proxy you have ability to hide your connection to node with no matter if this node in so-called clearnet or runned as hidden service over TOR

So these two snippets are ok:

<mark style="color:red;">**Node in clearnet**</mark>

```javascript
import Web1337 from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    nodeURL:'https://some.example.com:7332/token=blablabla'
    
    // Set to use SOCKS proxy on local port 5666 for YOU => TOR => NODE interaction
    proxyURL:'socks5h://localhost:5666'
});
```

<mark style="color:red;">**Node runned as a TOR hidden service**</mark>

```javascript
import Web1337 from '@klyntar/web1337';


let web1337 = new Web1337({

    symbioteID:'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
    workflowVersion:0,
    
    // v3 address
    nodeURL:'http://klyntarkari3d3tzvsnzqgg7i3gou3l5t2svedzfdlh4cigcw6hwmcqd.onion'
    
    // Set to use SOCKS proxy on local port 5666 to interact with node hidden over TOR
    proxyURL:'socks5h://localhost:5666'

});
```



## I2P

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

I2P can be used both as HTTP and SOCKS proxy



## Other proxies

* SSH
* Proxy services
