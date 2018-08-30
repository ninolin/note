# 建立一個 parity 的 docker

開一個目錄來放設定檔，把 Dockerfile、config.toml、genesis.json 放進來
```
mkdir ~/parity
cd ~/parity
```

下載 ubuntu16:04 docker 並 Dockerfile 產生新的 docker image
```
docker pull ubuntu:16.04
docker build -t parity-docker . 
```

執行 docker image
```
docker run -p 8545:8545 -p 8546:8546 -p 30303:30303 -d parity-docker
```
