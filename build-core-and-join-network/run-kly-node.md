# ☁️ Run KLY node

## Node.js installation

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Since the core is written on Node.js you should to install it. If you already have installed, we recommend checking the version. The recommended version is **v21.4.0**

{% tabs %}
{% tab title="Linux" %}
```bash
johndoe@klyntar:~$ node -v
v21.4.0
```
{% endtab %}

{% tab title="Windows" %}
```sh
C:\Users\JohnDoe>node -v
v21.4.0
```
{% endtab %}
{% endtabs %}

Use official guides to install Node.js for your platform (Windows/Linux/Mac)

{% embed url="https://nodejs.org/en/download" %}

## Go installation

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Some parts of KLY is written on Go(for example, PQC schemes), so you need to install it too. Use this guide to install Golang for your platform & architecture

{% embed url="https://go.dev/doc/install" %}

Or, check if you already have Go

```sh
go version
```

## PNPM install

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

As a package manager for Node.js we use pnpm. To install it globally, run:

```sh
npm install -g pnpm
```

## After all

Now, clone the core repository

```sh
git clone https://github.com/KLYN74R/KlyntarCore.git

cd KlyntarCore
```

Now depending on your OS run the following commands:

### Install dependencies

```sh
pnpm install
```

### Link core to make it available from any location

```sh
npm link
```

### Build Golang addons

Run a simple script

{% tabs %}
{% tab title="Linux" %}
```bash
chmod 700 build_must_have_addons.sh

./build_must_have_addons.sh
```
{% endtab %}

{% tab title="Windows" %}
```sh
build_must_have_addons.bat
```
{% endtab %}
{% endtabs %}

### Build KLY-EVM

{% tabs %}
{% tab title="Linux" %}
```sh
cd KLY_VirtualMachines/kly_evm

pnpm install

chmod 700 build_kly_evm.sh

./build_kly_evm.sh
```
{% endtab %}

{% tab title="Windows" %}
```sh
cd KLY_VirtualMachines\kly_evm

pnpm install

build_kly_evm.bat
```
{% endtab %}
{% endtabs %}

### Return to main directory

```sh
cd ../../

//Linux only
chmod 700 klyn74r.js
```

## Prepare configuration and genesis files

At this stage, the build process ends and the preparation of files for work begins.

Depending on the network you want to connect to, you will need an appropriate genesis file, as well as a set of configurations for your node.

To find the files that suit you, follow the instructions in the block:

{% content-ref url="networks/" %}
[networks](networks/)
{% endcontent-ref %}
