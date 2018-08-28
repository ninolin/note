version: "3"

services:
  chain:
    image: parity/parity:stable
    container_name: parity
    build: .
    ports:
      - "8545:8545"
      - "8546:8546"
      - "30303:30303"
    volumes:
      - /home/ubuntu/.local/share/io.parity.ethereum:/root/.local/share/io.parity.ethereum