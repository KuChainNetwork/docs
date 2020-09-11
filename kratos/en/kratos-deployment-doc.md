# Kratos deployment doc



## Use Docker

#### Install docker on ubuntu, see [Docker install](https://docs.docker.com/engine/install/ubuntu/)  

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose -y
# if current use is not root，use the cmd to join docker group
sudo usermod -aG docker ${USER}
```

### Launch new chain with docker

#### docker-compose launch new chain

###### [docker-compose.yml file](https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/)

```docker-compose
curl -O 
https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/raw/d5bd7bc22232325c3c5b2db5df914127b3f436de/docker-compose-new-chain.yml
```

###### bash script

```bash
docker-compose -f docker-compose-new-chain.yml up -d 
```

#### Use docker-compose to join a existed chain

###### [docker-compose.yml file](https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/)

```docker-compose
curl -O 
https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/raw/d5bd7bc22232325c3c5b2db5df914127b3f436de/docker-compose-join-chain.yml
```

###### bash script

```bash
docker-compose -f docker-compose-join-chain.yml  up -d
```

###### 

#### Use docker-compose start REST  lite-node server

###### [docker-compose.yml file](https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/)

```docker-compose
curl -O 
https://gist.githubusercontent.com/cain42/e4b014a3fd21c84f935bf368177c02f2/raw/d5bd7bc22232325c3c5b2db5df914127b3f436de/docker-compose-with-lite-node.yml
```

###### bash script

```bash
docker-compose -f docker-compose-with-lite-node.yml up -d
```

###### 

> Noted： you should change the ip props `<input seed node ip>` to  existed seed node IP address before you run `docker-compose up -d`

***

## Deploy without docker

#### Support OS:Linux、Unix，Ubuntu(recommend)

### Deploy with Ubuntu：

#### check update

```bash
sudo apt-get update
```

#### install update

```bash
sudo apt-get upgrade -y
```

#### install git 

```bash
sudo apt-get install git -y
```

#### install C Compile environment

```bash
sudo apt-get install build-essential -y
```

#### install curl

```bash
sudo apt-get install curl -y
```

#### clone Kratos repo

```bash
git clone https://github.com/KuChainNetwork/kratos.git
```

#### 

#### config Golang compile tools

```bash
curl -O https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
sudo tar -xvf go1.14.4.linux-amd64.tar.gz -C /opt/
sudo mkdir /opt/gopath
sudo chmod 0777 /opt/gopath
echo "export GOROOT=/opt/go" >> ~/.profile
echo "export GOPATH=/opt/gopath" >> ~/.profile
echo "export PATH=\$PATH:\$GOROOT/bin:\$GOPATH/bin" >> ~/.profile
source ~/.profile
```

#### Compile Kratos source code

```bash
cd kratos
go mod vendor
make -e CORE_SYMBOL=kts -e MAIN_SYMBOL=kratos
```

#### Scripts summary

```bash
#!/usr/bin/env bash

sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install git -y
sudo apt-get install build-essential -y
sudo apt-get install curl -y
curl -O https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
sudo tar -xvf go1.14.4.linux-amd64.tar.gz -C /opt/
rm go1.14.4.linux-amd64.tar.gz
sudo mkdir /opt/gopath
sudo chmod 0777 /opt/gopath
echo "export GOROOT=/opt/go" >> ~/.profile
echo "export GOPATH=/opt/gopath" >> ~/.profile
echo "export PATH=\$PATH:\$GOROOT/bin:\$GOPATH/bin" >> ~/.profile
source ~/.profile
git clone https://github.com/KuChainNetwork/kratos.git && cd kratos && make -e CORE_SYMBOL=kts -e MAIN_SYMBOL=kratos

```

***

#### Generate new chain

genesis is the first validator

```bash
CHAIN_ID="kratos"
PARAMS=""
PARAMSCLI="--keyring-backend test"
ktscli ${PARAMSCLI} keys add "kratos" > kratos.info 2>&1
VALKEY=$(ktscli ${PARAMSCLI} keys show kratos -a)
ktscli ${PARAMSCLI} keys add "test" > test.info 2>&1
TESTVALKEY=$(ktscli ${PARAMSCLI} keys show test -a)
ktsd init ${PARAMS} --chain-id=${CHAIN_ID} ${CHAIN_ID}
ktsd ${PARAMS} genesis add-account kratos ${VALKEY}
ktsd ${PARAMS} genesis add-account testacc1 ${TESTVALKEY}
ktsd ${PARAMS} genesis add-account testacc2 ${TESTVALKEY}
ktsd ${PARAMS} genesis add-address ${VALKEY}
ktsd ${PARAMS} genesis add-coin "1000000000000000000000000000000000000000kratos/kts" "main token"
ktsd ${PARAMS} genesis add-account-coin ${VALKEY} "100000000000000000000000kratos/kts"
ktsd ${PARAMS} genesis add-account-coin kratos "100000000000000000000000kratos/kts"
ktsd ${PARAMS} gentx ${VALKEY} --keyring-backend test --name kratos
ktsd ${PARAMS} collect-gentxs
ktsd start --log_level "*:debug" --trace
```

***

#### Join existed chain

Init the chain，CHAIN_ID must equal with existed chain

```bash
ktsd init --chain-id=kratos kratos
```

Use the [kratos genesis.json](https://gist.githubusercontent.com/cain42/a0f469b2d1a84858ff9cfa10f58910c7/raw/bc8c90cb5494adba316cf10345eea1fe3ed7b2f1/genesis.json) file instead of your node genesis.json file，default saved in `HOME/.ktsd/config` folder

change config.toml file，(also saved in `$HOME/.ktsd/config` folder).
change persistent_peers = "" to `persistent_peers = "tendermint_node_id@ip:port"`
change private_peer_ids = "" to `private_peer_ids = "tendermint_node_id"`
you can use` sed` cmd to replace text.

```bash
sed -i "s/persistent_peers = \"\"/persistent_peers = \"bdd5221e39d741b7cd22806a259b1ab36a5cb94a@121.89.216.155:26656\"/g" "${HOME}/.ktsd/config/config.toml"
sed -i "s/private_peer_ids = \"\"/private_peer_ids = \"bdd5221e39d741b7cd22806a259b1ab36a5cb94a\"/g" "${HOME}/.ktsd/config/config.toml"
```

- bdd5221e39d741b7cd22806a259b1ab36a5cb94a@121.89.216.155:26656
- e7f57878a7068792372eea5985fe26e4f164ec5c@121.89.223.62:26656
- e0d4ca5aefdd2b348c981b4b1cc215b32bbe4ed9@121.89.210.210:26656

`ip` is one of the Kratos node's  public IP address 
`port` is the Tendermint P2P RPC port，default is 26656 

start node and join the Kratos network.

```bash
ktsd start
```
