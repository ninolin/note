# Install private blockchain on Ubuntu 16.04 with Geth

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

Create **genesis.json**

```
vi genesis.json
{
  "config": {
        "chainId": 1,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "coinbase" : "0x0000000000000000000000000000000000000000",
    "difficulty" : "0x40000",
    "extraData" : "",
    "gasLimit" : "0xffffffff",
    "nonce" : "0x0000000000000042",
    "mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
    "timestamp" : "0x00",
    "alloc": { }
}
```

Create a folder for save blockchain data
```
mkdir data
```

Our folder like this
```
private-geth
    |- genesis.json
    |- data
```

Excute init cmd in **private-geth** folder.
```
geth --datadir data init genesis.json
```

After init blockchain will see **geth** and **keysotre** folder in private-geth folder. **geth** is save blockchain data and **keysotre** is save account keystore.
```
private-geth
    |- genesis.json
    |- data
        |- geth
        |- keystore
```

Start blockchain and add console parameter let we can use cmd.
```
geth --datadir data/ console
```

## Start mining

We have to use geth cmd to create an account and call him to start mining.

```
> personal.newAccount("123456")
"0x428f11af39cd9b97dde1f26e3f23ceba6bc1ee30" 
> miner.setEtherbase("0x428f11af39cd9b97dde1f26e3f23ceba6bc1ee30")
true
> miner.start()
INFO [07-11|11:20:33.089] Updated mining threads                   threads=0
INFO [07-11|11:20:33.089] Transaction pool price threshold updated price=18000000000
INFO [07-11|11:20:33.090] Starting mining operation
null
> INFO [07-11|11:20:33.090] Commit new mining work                   number=186 txs=0 uncles=0 elapsed=590.209µs
INFO [07-11|11:20:35.959] Successfully sealed new block            number=186 hash=170289…cf78e5
INFO [07-11|11:20:35.960] 🔨 mined potential block                  number=186 hash=170289…cf78e5
INFO [07-11|11:20:35.960] Commit new mining work                   number=187 txs=0 uncles=0 elapsed=175.419µs
INFO [07-11|11:20:36.401] Successfully sealed new block            number=187 hash=026631…b04d21
INFO [07-11|11:20:36.402] 🔨 mined potential block                  number=187 hash=026631…b04d21
INFO [07-11|11:20:36.403] Commit new mining work                   number=188 txs=0 uncles=0 elapsed=175.379µs
INFO [07-11|11:20:36.855] Successfully sealed new block            number=188 hash=2d3559…2c5f58
```

## Run in backgroud

We create a shell script named **startup-geth.sh** to run blockchain in backgroud and add other parameter.
```
#!/bin/bash
screen -dmS geth /usr/bin/geth \
        --datadir data/ \
        --mine \
        --rpc \
        --rpcaddr "0.0.0.0" \
        --rpcport "8545" \
        --rpcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3" \
        --rpccorsdomain "*" \
```

Run **startup-geth.sh**
```
sh startup-geth.sh
```

Use attach to connect console to check blockchain is being run.
```
geth attach ipc:data/geth.ipc
```

geth console to check blockNumber is being more.
```
> eth.blockNumber
1169
> eth.blockNumber
1170
```

use json-rpc to call blockchain
```
curl -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":73}' -H "Content-Type: application/json" -X POST http://localhost:8545

{"jsonrpc":"2.0","id":73,"result":["0x3bf7731993923549274d9a2b37f570c5b3bdede1","0x428f11af39cd9b97dde1f26e3f23ceba6bc1ee30"]}
```
