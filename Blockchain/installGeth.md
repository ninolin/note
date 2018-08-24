# Install private blockchain on Ubuntu 16.04 with Parity

## Install Parity

```
sudo apt-get update
sudo apt-get install openssl libtool autoconf automake uuid-dev build-essential gcc g++ python-software-properties unzip make git libcap2-bin -y
bash <(curl https://get.parity.io -kL)
```

## Create config.toml

在 HOME 目錄下建立 ~/.local/share/io.parity.ethereum 當作 chain 的目錄
在 chain 目錄下建立設定檔 config.toml

```
[parity]
chain = "/home/ubuntu/.local/share/io.parity.ethereum/genesis.json" 
base_path = "$HOME/.local/share/io.parity.ethereum"              
db_path = "$HOME/.local/share/io.parity.ethereum/chains"         
keys_path = "$HOME/.local/share/io.parity.ethereum/keys"

[network]
port = 30303

[rpc]
disable = false
port = 8545
interface = "0.0.0.0"
cors = ["all"]
apis = ["web3", "eth", "pubsub", "net", "parity", "parity_pubsub", "traces", "rpc", "shh", "shh_pubsub"]
hosts = ["none"]

[websockets]
disable = false
port = 8546
interface = "0.0.0.0"
origins = ["none"]
apis = ["web3", "eth", "net", "parity", "traces", "rpc", "secretstore"]
hosts = ["none"]
```

## Create genesis.json

在 chain 目錄下建立創世區塊 genesis.toml
```
{
    "name": "parity-chain",
    "engine": {
        "authorityRound": {
            "params": {
                "stepDuration": "3",
                "validators" : {
                    "list": []
                }
            }
        }
    },
    "params": {
        "gasLimitBoundDivisor": "0x400",
        "maximumExtraDataSize": "0x20",
        "minGasLimit": "0x5208",
        "networkID" : "0x1"
    },
    "genesis": {
        "seal": {
            "authorityRound": {
                "step": "0x0",
                "signature": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
            }
        },
        "difficulty": "0x20000",
        "gasLimit": "0x5B8D80"
    },
    "accounts": {
    }
}
```

## Create new account

建立一個新的用戶
```
parity account new --chain /home/ubuntu/.local/share/io.parity.ethereum/genesis.json --keys-path /home/ubuntu/.local/share/io.parity.ethereum/keys
```

會請你輸入該用戶的密碼
```
Loading config file from /home/ubuntu/.local/share/io.parity.ethereum/config.toml
Please note that password is NOT RECOVERABLE.
Type password: asdf1234
Repeat password: asdf1234
0x511a4b0153f0fdf1ca90c9544152313237a83255
```

## Create password file

在 chain 目錄下建立密碼檔 .pw
```
echo asdf1234 >> .pw
```

## Update config.toml & genesis.json

Update config.toml
```
[parity]
chain = "/home/ubuntu/.local/share/io.parity.ethereum/genesis.json" 
base_path = "$HOME/.local/share/io.parity.ethereum"              
db_path = "$HOME/.local/share/io.parity.ethereum/chains"         
keys_path = "$HOME/.local/share/io.parity.ethereum/keys"

[network]
port = 30303

[mining]
author = "0x511a4b0153f0fdf1ca90c9544152313237a83255"
engine_signer = "0x511a4b0153f0fdf1ca90c9544152313237a83255"

[account]
unlock = ["0x511a4b0153f0fdf1ca90c9544152313237a83255"]
password = ["$HOME/.local/share/io.parity.ethereum/.pw"]
keys_iterations = 10240

[rpc]
disable = false
port = 8545
interface = "0.0.0.0"
cors = ["all"]
apis = ["web3", "eth", "pubsub", "net", "parity", "parity_pubsub", "traces", "rpc", "shh", "shh_pubsub"]
hosts = ["none"]

[websockets]
disable = false
port = 8546
interface = "0.0.0.0"
origins = ["none"]
apis = ["web3", "eth", "net", "parity", "traces", "rpc", "secretstore"]
hosts = ["none"]
```

Update genesis.json
```
{
    "name": "parity-chain",
    "engine": {
        "authorityRound": {
            "params": {
                "stepDuration": "3",
                "validators" : {
                    "list": ["0x511a4b0153f0fdf1ca90c9544152313237a83255"]
                }
            }
        }
    },
    "params": {
        "gasLimitBoundDivisor": "0x400",
        "maximumExtraDataSize": "0x20",
        "minGasLimit": "0x5208",
        "networkID" : "0x1"
    },
    "genesis": {
        "seal": {
            "authorityRound": {
                "step": "0x0",
                "signature": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
            }
        },
        "difficulty": "0x20000",
        "gasLimit": "0x5B8D80"
    },
    "accounts": {
    }
}
```

## Start Mining

```
parity --config config.toml
```