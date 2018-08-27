# Install private blockchain on Docker with Parity

## Pull parity image and install docker-compose

```
docker pull parity/parity:stable
apt-get install docker-compose
```

## create chain folder

enter ~/.local/share/io.parity.ethereum and put into config.toml, docker-compose.yaml and genesis.toml
```
docker-compose up
```