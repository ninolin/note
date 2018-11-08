# Install POW private chain on Ubuntu 16.04 with Geth

## Install Geth

```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

## Build up blockchain

We have to create a first block for our private blockchain, it's called genesis block. Genesis block is a json file and describe blockchain setting.  

We create a folder named **private-geth** on home folder first.

```
mkdir private-geth
cd private-geth
```

Create a miner account
```
geth --datadir ./data account new

INFO [10-16|14:11:33.028] Maximum peer count                       ETH=25 LES=0 total=25
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat passphrase:
Address: {4d93c3d4e0f89b8461471b67d568c6d1e334ac05}
```

Our folder like this
```
private-geth
    |- data
        |- keystore
```

Use **puppeth** create **genesis**

```
puppeth
+-----------------------------------------------------------+
| Welcome to puppeth, your Ethereum private network manager |
|                                                           |
| This tool lets you create a new Ethereum network down to  |
| the genesis block, bootnodes, miners and ethstats servers |
| without the hassle that it would normally entail.         |
|                                                           |
| Puppeth uses SSH to dial in to remote servers, and builds |
| its network components out of Docker containers using the |
| docker-compose toolset.                                   |
+-----------------------------------------------------------+

Please specify a network name to administer (no spaces or hyphens, please)
> nino_chain
Sweet, you can set this via --network=nino_chain next time!

INFO [10-16|14:12:54.786] Administering Ethereum network           name=nino_chain
WARN [10-16|14:12:54.792] No previous configurations found         path=/home/localadmin/.puppeth/nino_chain

What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2

Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
> 2

How many seconds should blocks take? (default = 15)
> 5

Which accounts are allowed to seal? (mandatory at least one)
> 0x4d93c3d4e0f89b8461471b67d568c6d1e334ac05
> 0x

Which accounts should be pre-funded? (advisable at least one)
> 0x4d93c3d4e0f89b8461471b67d568c6d1e334ac05
> 0x

Specify your chain/network ID if you want an explicit one (default = random)
>
INFO [10-16|14:14:07.142] Configured new genesis block

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2

 1. Modify existing fork rules
 2. Export genesis configuration
 3. Remove genesis configuration
> 2

What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> ^C
```

Our folder like this
```
private-geth
    |- nino_chain.json
    |- data
```

Init our chain
```
geth --datadir data init nino_chain.json
```

## Run

```
geth --datadir ./data --networkid 34567 --unlock ae95875406cf4df8e8645163fe7ef43fbd421b64 --mine --rpc --rpcport 8545 --rpcapi "net,web3, eth, admin, personal" --rpcaddr "0.0.0.0" --password <(echo asdf1234) --rpccorsdomain "*" --nat "any" --identity "230" console
```

