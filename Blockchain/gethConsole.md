# Geth Console Command

## Enter Geth Console

```
geth attach ipc:data/geth.ipc
```

* check block number
```
> eth.blockNumber
1169
```
* check all accounts
```
> eth.accounts
["0x93d593512075bf4433bec2444148fc5e39ff400e"]
```
* set coinbase
```
> miner.setEtherbase("0x428f11af39cd9b97dde1f26e3f23ceba6bc1ee30")
true
```
* get coinbase
```
> eth.coinbase
"0x93d593512075bf4433bec2444148fc5e39ff400e"
```
* new account
```
> personal.newAccount("123456789")
"0x3686093d4fc43d6fcc78190801bb1ffe0b16084c"
```
* unlock account
```
personal.unlockAccount("0x93d593512075bf4433bec2444148fc5e39ff400e")
```
* transfer ether
```
eth.sendTransaction({from: "0x93d593512075bf4433bec2444148fc5e39ff400e", to: "0x3686093d4fc43d6fcc78190801bb1ffe0b16084c", value: "50000000000000000000"});
```