# Setting Up the Storage Node
## Update System Packages:

```
sudo apt update && sudo apt upgrade -y
```

### Install Required Dependencies:

```
sudo apt install -y build-essential git curl jq
```
## Install Go:

```
wget https://golang.org/dl/go1.19.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.19.1.linux-amd64.tar.gz
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
source ~/.profile
```

## Clone the 0G Storage Node Repository and Build the Binary:

```
git clone https://github.com/0gnetwork/0g-storage.git
cd 0g-storage
make install
```

## Initialize the Storage Node:

```
0g-storage init <your_moniker> --chain-id=0g-testnet
```

## Configure the Node:

### Set Peers:

Edit the config.toml file:

```
nano ~/.0g-storage/config/config.toml
```
Set persistent_peers to the provided list of peers.

### Set Storage Paths:

Edit the app.toml file:

```
nano ~/.0g-storage/config/app.toml
```

Set the storage-path to the desired location on your server.

## Download Genesis File:

```
wget -O ~/.0g-storage/config/genesis.json https://path/to/genesis.json
```
## Start the Storage Node:

```
0g-storage start
```
