<!-- v 0.2 - imported and fixed from cashu.space documentation sprint 2k24-->

# BE YOUR OWN MINT GUIDE

A collaborative attempt to document the tooling, steps and best practices for self-hosting or deploying a Cashu mint on all possible platforms. This is not a beginner-friednly guide, it assumes that the reader has some proficiency with git, deploying software and have access to an already running a functional Bitcoin node with a Lightning client (for mainnet deployments)

This guide only overviews currently a selection of clients that are available, not every single implementation that is out there. 

If you wish to add a client to the following list then open an Issue on the github repository of this site. 

## DISCLAIMER

This is provided for educational purposes only. Many of the clients listed in this guide are still in active development and carry substantial risk. Please advise that Cashu is in its early development, you should use the protocol only with small amounts of satoshis for testing and your first attempts at running a mint should be done on testnet/signet (not on mainnet Bitcoin) so that you can try it out without risking any satoshis. 

Do your own research and ask questions in the appropriate support channels for the clients listed below.


## General Observations

For all the mint clients that are at your disposal to install, in the majority of cases the most painless option is to follow the Docker installation process for that client. 

**What is Docker?**

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security lets you run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you don't need to rely on what's installed on the host.

Read more about Docker here: https://docs.docker.com/guides/docker-overview


## Mint Client Overview 
 <!-- | :----: | :----: | :----: | :----: | :----: | :----: | :----: | --> 

| Name | Languages | Platforms | Docker? | Latest release | Code | Guide | 
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| Nutshell | Python | All platforms | Yes | 0.15.3 | https://github.com/cashubtc/nutshell | Link to Guide | 
| Moksha | Rust | Platforms | Yes | 0.2.1 | https://github.com/ngutech21/moksha | Link to Guide | 
| gonuts | Go | All Platforms | No | 0.1.0 | https://github.com/elnosh/gonuts | Link to Guide | 
| cashu-rs-mint | Rust, Nix | All Platforms | No | :construction: | https://github.com/thesimplekid/cashu-rs-mint | Link to Guide | 
| nutmix | Go | Platforms | Yes | :construction: | https://github.com/lescuer97/nutmix | Link to Guide | 



## Platforms


### Linux

To complete any installation of a cashu mint on a Linux you will need the following technical proficiencies:
- knowledge of Bash / comfortable in a Terminal setting
- Root access to the machine that will be running the Mint
- Some knowledge of Lightning (node and channel management)

Covered in this guide are the following mainstream Linux distributions: Ubuntu, Debian, ...

### Windows

ipsum lorem

### Mac OSx

ipsum lorem

### Raspberry Pi/ARM

ipsum lorem


## [ Nutshell CLIENT ]

***Nutshell***
Cashu Nutshell is a Chaumian Ecash wallet and mint for Bitcoin Lightning. Cashu Nutshell is the reference implementation in Python.

#### Support channels for this Mint: https://github.com/cashubtc/nutshell/issues

#### Official Documentation: https://github.com/cashubtc/nutshell/blob/main/README.md


### Installing Nutshell on Linux

#### Easy Install

The easiest way to use Cashu is to install the package it via pip:
```bash
pip install cashu
```

To update Cashu, use `pip install cashu -U`.

If you have problems running the command above on Ubuntu, run `sudo apt install -y pip pkg-config` and `pip install wheel`. On macOS, you might have to run `pip install wheel` and `brew install pkg-config`.

You can skip the entire next section about Poetry and jump right to [Using Cashu](#using-cashu).

#### Manual install: Poetry
These steps help you install Python via pyenv and Poetry. If you already have Poetry running on your computer, you can skip this step and jump right to [Install Cashu](#poetry-install-cashu).

#### Poetry: Prerequisites

```bash
# on ubuntu:
sudo apt install -y build-essential pkg-config libffi-dev libpq-dev zlib1g-dev libssl-dev python3-dev libsqlite3-dev ncurses-dev libbz2-dev libreadline-dev lzma-dev

# install python using pyenv
curl https://pyenv.run | bash

# !! follow the instructions of pyenv init to setup pyenv !!
pyenv init

# restart your shell (or source your .rc file), then install python:
pyenv install 3.10.4

# install poetry
curl -sSL https://install.python-poetry.org | python3 -
echo export PATH=\"$HOME/.local/bin:$PATH\" >> ~/.bashrc
source ~/.bashrc
```
#### Poetry: Install Cashu
```bash
# install cashu
git clone https://github.com/cashubtc/nutshell.git cashu
cd cashu
git checkout <latest_tag>
pyenv local 3.10.4
poetry install
```

If you would like to use PostgreSQL as the mint database, use the command `poetry install --extras pgsql`.

#### Poetry: Update Cashu
To update Cashu to the newest version enter
```bash
git pull && poetry install
```
#### Poetry: Using the Nutshell wallet

Cashu should be now installed. To execute the following commands, activate your virtual Poetry environment via

```bash
poetry shell
```

If you don't activate your environment, just prepend `poetry run` to all following commands.

#### Configuration
```bash
mv .env.example .env
# edit .env file
vim .env
```

To use the wallet with the [public test mint](#test-instance), you need to change the appropriate entries in the `.env` file.

#### Test instance
*Warning: this instance is just for demonstration purposes and development only. The satoshis are not real.*

Change the appropriate `.env` file settings to
```bash
MINT_URL=https://testnut.cashu.space
```
### Running the mint
Before you can run your own mint, make sure to enable a Lightning backend in `MINT_BACKEND_BOLT11_SAT` and set `MINT_PRIVATE_KEY` in your `.env` file.
```bash
poetry run mint
```

For testing, you can use Nutshell without a Lightning backend by setting `MINT_BACKEND_BOLT11_SAT=FakeWallet` in the `.env` file.

#### Libraries & Dependencies

...

#### Stack Requirements

...

### Just the commands

```pip install cashu```

```poetry run mint```

#### External Tutorials

...

#### Best Practices 

...





### Installing Nutshell on Windows

***TBC***
....

### Installing  Nutshell on Mac OSx

**TBC**
....

### Installing Nuthshell on Raspberry Pi (ARM)

**TBC**
....




## [ MOKSHA CLIENT ]

***Moksha***
moksha is a cashu library, mint and cli-wallet written in Rust.

#### Support channels for this Mint: https://github.com/ngutech21/moksha/issues

#### Official Documentation: https://github.com/ngutech21/moksha/blob/master/README.md


### Installing Moksha using Docker

#### Docker-compose

docker-compose simplifies the process of running multi-container Docker applications. Here's how you can use it to run the moksha -mint:

1. First, you need to have Docker Compose installed on your machine. If it's not installed, you can download it from the [official Docker website](https://docs.docker.com/compose/install/).

2. Copy the tls.cert and admin.macaroon files from your LND instance into the `./data/mutinynet/` directory.

3. Configure the `LND_GRPC_HOST` environment variable in the `docker-compose.yml` file to point to your LND instance.

4. Run the following command in the same directory as your `docker-compose.yml` file to start the mint and a postgres database:

```bash
docker-compose up -d app database
```

#### Libraries & Dependencies

...

#### Stack Requirements

...

### Just the commands

```docker-compose up -d app database```

#### External Tutorials

...

#### Best Practices 

...






### Installing Moksha from Source (Ubuntu/Debian)

#### Setup rust

```bash
git clone https://github.com/ngutech21/moksha.git
cargo install just typos-cli sqlx-cli grcov wasm-pack wasm-opt
rustup component add llvm-tools-preview
cd moksha
```

#### Install protobuf for your platform

This is needed for the LND backend.

```bash
sudo apt install protobuf-compiler
brew install protobuf
choco install protoc
```

#### Config

```bash
mv .env.example .env
# edit .env file
vim .env
```

#### Run mint (cashu-server)

To run the mint you need to setup a lightning regtest environment like [Polar](https://lightningpolar.com) and a Lnbits or Lnd instance. In Lnbits create a new wallet and copy the admin key into the .env file and set the url to your Lnbits instance. The mint uses PostgreSQL for storing used proofs and pending invoices. The database URL can be configured in the .env file.

```bash
install docker and docker-compose
docker compose up -d
just db-create
just run-mint
```

#### Libraries & Dependencies

...

#### Stack Requirements

...

#### External Tutorials

...

#### Best Practices 

...





### Installing Moksha on Windows

**TBC**
....


### Installing Moksha on Mac OSx

**TBC**
....

### Installing Moksha on Raspberry Pi (ARM)

**TBC**
....


## [ gonuts CLIENT ]

***gonuts***
Cashu wallet and mint implementation in Go.

#### Support channels for this Mint: https://github.com/elnosh/gonuts/issues
 
#### Official Documentation: https://github.com/elnosh/gonuts/blob/main/README.md
 

### Installing gonuts on Linux

With [Go](https://go.dev/doc/install) installed, you can run the following command to install the wallet:

```
go install github.com/elnosh/gonuts/cmd/nutw@latest
```

To setup a mint for the wallet, create a `.env` file at ~/.gonuts/wallet/.env and setup your preferred mint.

#### Using the wallet

#### Check balance

```
nutw balance
```

#### Create a Lightning invoice to receive ecash

```
nutw mint 100
```

This will get an invoice from the mint which you can then pay and use to mint new ecash.

```
invoice: lnbc100n1pja0w9pdqqx...
```

#### Redeem the ecash after paying the invoice

```
nutw mint --invoice lnbc100n1pja0w9pdqqx...
```

#### Send tokens

```
nutw send 21
```

This will generate a Cashu token that looks like this:

```
cashuAeyJ0b2tlbiI6W3sibW...
```

This is the ecash that you can then send to anyone.

#### Run mint

- `cd cmd/mint`
- you'll need to setup a lightning regtest environment with something like [Polar](https://lightningpolar.com/) and fill in the values in the `.env` file

- `go build -v -o mint mint.go`

- `./mint`

#### Libraries & Dependencies

...

#### Stack Requirements

...

#### Just the commands

```go install github.com/elnosh/gonuts/cmd/nutw@latest```

```cd cmd/mint```

```go build -v -o mint mint.go```

```./mint```

#### External Tutorials

...

#### Best Practices 

...



### Installing gonuts on Windows

***TBC***
....

### Installing gonuts on Mac OSx

**TBC**
....

### Installing gonuts on Raspberry Pi (ARM)

**TBC**
....


## [ cashu-rs-mint CLIENT ]

***cashu-rs-mint***
 Cashu mint using CDK 

#### Support channels for this Mint: https://github.com/thesimplekid/cashu-rs-mint/issues
 
#### Official Documentation: https://github.com/thesimplekid/cashu-rs-mint/blob/main/README.md
 

#### Installing cashu-rs-mint using Nix

```
nix develop
```

This will launch a nix shell with a regtest bitcoind node as well as two lightning nodes.

In order to use the node first a channel will need to be opened.

```
  ln1 newaddr
  ln2 newaddr
```

```
  btc sendtoaddress <ln1 bitcoin address> 100
  btc sendtoaddress <ln2 bitcoin address> 100
  btc getnewaddress
  btc generatetoaddress 50 <btc address>
```

Connect ln nodes
```
  ln2 getinfo
  ln1 connect <pubkey of ln1> 127.0.0.1 15352
```

Open a channel from ln1 to ln2
```
  ln1 fundchannel id=<pubkey of ln2> amount=10000000
```

Open a channel from ln2 to ln1
```
  ln1 getinfo
  ln2 fundchannel id=<pubkey of ln1> amount=10000000
```

Generate blocks to confirm channels
```
  btc getnewaddress
  btc generatetoaddress 50 <btc address>
```

Start the mint, by default the mint will use ln1
```
  cargo r
```

#### Libraries & Dependencies

...

#### Stack Requirements

...

#### Just the commands

```
nix develop
```
```
  ln1 newaddr
  ln2 newaddr
```
```
  btc sendtoaddress <ln1 bitcoin address> 100
  btc sendtoaddress <ln2 bitcoin address> 100
  btc getnewaddress
  btc generatetoaddress 50 <btc address>
```
```
  ln2 getinfo
  ln1 connect <pubkey of ln1> 127.0.0.1 15352
```
```
  ln1 fundchannel id=<pubkey of ln2> amount=10000000
```
```
  ln1 getinfo
  ln2 fundchannel id=<pubkey of ln1> amount=10000000
```
```
  btc getnewaddress
  btc generatetoaddress 50 <btc address>
```
```
  cargo r
```


#### External Tutorials

...

#### Best Practices 

...


### Installing cashu-rs-mint on Linux

***TBC***
....



### Installing cashu-rs-mint on Windows

***TBC***
....

### Installing cashu-rs-mint on Mac OSx

**TBC**
....

### Installing cashu-rs-mint on Raspberry Pi (ARM)

**TBC**
....


## [ Nutmix CLIENT ]

***Nutmix***
golang implementation of ecash mint 

#### Support channels for this Mint: https://github.com/lescuer97/nutmix/issues
 
#### Official Documentation: https://github.com/lescuer97/nutmix/blob/master/README.md


#### Installing Nutmix using Docker

This project is thought to be able to be ran on a docker container or locally.

if you want to run the project in docker you will need two things.

You'll need  the correct variables in an `.env` file. Use the env.example file as reference.

You need to make sure to use a strong `POSTGRES_PASSWORD` and make user the username and password are the same in the
`DATABASE_URL`

It's important to set up the variables under `LIGHTNING CONNECTION` for connecting to a node. 

Please use a secure Private for deriving keysets.

if you want to run with docker traefik and you also need to fill variables below `HOSTING` for your domains.

If you have this correctly setup it should be as easy as running a simple docker command on linux:

If you missed and important variable for the mint. The mint should panic and let you know.

``` docker compose up -d ```


#### Libraries & Dependencies

...

#### Stack Requirements

...

#### Just the commands

``` docker compose up -d ```

#### External Tutorials

...

#### Best Practices 

...


### Installing Nutmix on Linux (Development)

If you want to develop for the project I personally run a hybrid setup. I run the mint locally and the db on docker. 

I have a special development docker compose called: `docker-compose-dev.yml`. This is for simpler development without having traefik in the middle.

1. run the database in docker. Please check you have the necessary info in the `.env` file. 

``` docker compose -f docker-compose-dev.yml up db ```

2. Run the mint locally. 

``` # build the project go run cmd/nutmix/*.go ```

#### Libraries & Dependencies

...

#### Stack Requirements

...

#### Just the commands

``` docker compose -f docker-compose-dev.yml up db ```

``` # build the project go run cmd/nutmix/*.go ```

#### External Tutorials

...

#### Best Practices 

...



### Installing Nutmix on Windows

***TBC***
....

### Installing Nutmix on Mac OSx

**TBC**
....

### Installing Nutmix on Raspberry Pi (ARM)

**TBC**
....



## Frequently Asked Questions about Mints


#### Can mints see my IP address?

Yes. Cashu doesn't protect against network-level heuristics per default. Users should take precautions to protect themselves against leaking network metadata by using privacy tools such as Tor.

#### Why can't I receive tokens from one mint into another mint?

Each mint has their own tokens. Therefore, you cannot receive tokens from one mint into another mint.
You can, however, use the tokens from one mint and swap them over Lightning for tokens from another mint. To transfer tokens from one mint to another, use the Multimint Swap functionality.

#### How do I send (swap) tokens from one mint to another?

In your wallet client UI, use the Multimint Swap functionality, which operates via Lightning invoices under the hood.

- Cashu.me: On the Settings tab.
- Nutstash: On the Mint tab.
- eNuts: Go to Options->Mint Management->Funds->Multimint Swap

Note: You must have 2 mints in order to send/swap between mints.

#### Is there a list of mints I can already connect to (mainnet)?

You can use Bitcoin Mints to list, review, and find mints: https://bitcoinmints.com/

Listed below are the default mints in some of the Cashu wallet clients:
- Cashu.me: https://8333.space:3338
- Minibits: https://mint.minibits.cash
- 

#### Can I send and receive tokens to/from the same mint?

Yes, this is a common first test use case. Also, sometimes you might send a token that is not received or redeemed, and you might want to copy the token and receive it back to yourself.

#### How can I run my own mint?

Read this guide, try it out and contact the developer if you have issues or wish to contribute to the projects.

#### Are Cashu Mints interoperable with each other (client diversity)?

Yes.
