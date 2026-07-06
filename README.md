# Particl (PART) ElectrumX Server

Particle (PART) electrumx server and electrum wallet are available on community repos, but not officially supported by PART team. Here this repo assumes you have installed Particl fullnode from BasicSwap
(see https://github.com/ShorelineCrypto/basicswap_docker), and now set up electrumx server for personal local use or public electrum server service. 

### Build Docker Image

To run ElectrumX, you must increase your Linux open file descriptors limit (ulimit -n) in docker container. The default Linux limit is usually 1,024, but ElectrumX requires at least 2,500 to handle LevelDB and simultaneous client connections. 
```
 sudo cp daemon.json /etc/docker/
 sudo systemctl restart docker
```

Build docker image:
```
 git clone https://github.com/ShorelineCrypto/electrumx-part.git && cd electrumx-part
 docker build -t electrumx-part .
```

Or pull down a working image from docker hub.

x64 linux:
```
 docker pull shorelinecrypto/electrumx-part:amd64
 docker tag shorelinecrypto/electrumx-part:amd64 electrumx-part:latest
```
arm64 linux:
```
 docker pull shorelinecrypto/electrumx-part:arm64
 docker tag shorelinecrypto/electrumx-part:arm64 electrumx-part:latest
```

### Run a particl full node

Modify the file `particl.conf` with proper rpc username and rpc password and `docker_run_electrumx-part.sh` and other two shell scripts to make sure path and user/password are all good.

Create proper folders:
```
  sudo mkdir -p /opt/electrumx/db-PART
  sudo mkdir -p /opt/electrumx/ssl
  mkdir ~/.particl
  cp particl.conf ~/.particl/
```

Start particl full node:
```
  bash start_part_core.sh
  bash start_part_core_wallet.sh
```

This will start particl full node. Sync the full node to make sure the blockchain height matches PART explorer.


### Setting up certificate

Electrumx server SSL or WSS connection requires a certificate, free or paid. For personal usage, self signed certificate works well as below:

```
  cd /opt/electrumx/ssl
  openssl genrsa -out server.key 2048
  openssl req -new -key server.key -out server.csr
  openssl x509 -req -days 1825 -in server.csr -signkey server.key -out server.crt
  ln -s server.key privkey.pem
  ln -s server.crt fullchain.pem
```

For public service usage, a free or paid third party SSL certificate is required. 

