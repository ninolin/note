FROM ubuntu:16.04
MAINTAINER nino

RUN apt-get update
RUN apt-get -y install openssl libtool autoconf automake uuid-dev build-essential gcc g++ python-software-properties unzip make git libcap2-bin wget vim curl
RUN wget https://get.parity.io -O installParity.sh
RUN sed -i 's/sudo/ /g' installParity.sh
RUN sh /installParity.sh
RUN mkdir -p /root/io.parity.ethereum
ADD config.toml /root/io.parity.ethereum
ADD genesis.json /root/io.parity.ethereum
RUN echo asdf1234 >> /root/io.parity.ethereum/.pw
RUN mkdir -p /root/io.parity.ethereum/keys
RUN mkdir -p /root/io.parity.ethereum/keys/parity-chain
ADD keystore.json /root/io.parity.ethereum/keys/parity-chain

CMD ["parity", "--config", "/root/io.parity.ethereum/config.toml"]