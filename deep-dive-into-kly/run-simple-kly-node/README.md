# Run simple KLY node

## Node.js installation

Since the core is written on Node.js you should to install it. If you already have installed, we recommend checking the version

```sh
johndoe@klyntar:~$ node -v
v17.0.0
```

{% hint style="info" %}
Make sure you have at least version 17
{% endhint %}

Use official guides to install Node.js for your platform (Windows/Linux/Mac)

{% embed url="https://nodejs.org/en/download" %}

## Go installation

Some parts of KLY is written on Go(for example, PQC schemes), so you need to install it too. Use this guide to install Golang for your platform & architecture

{% embed url="https://go.dev/doc/install" %}

Or, check if you already have Go

```sh
go version
```

## After all

Now, clone the core repository

```sh
git clone https://github.com/KLYN74R/KlyntarCore.git

cd KlyntarCore
```

Finally, run a single build script

```sh
pnpm run build:linux
```

Or

```sh
pnpm run build:windows
```

This script installs all the requirements, compile Go modules and so on.





## Prepare configuration and genesis files



## Run your node
