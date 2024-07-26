# Setting Up the Validator Node
## Update System Packages:

```
sudo apt update && sudo apt upgrade -y
```
## Install Required Dependencies:

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


## Clone the 0G Repository and Build the Binary:

```
git clone https://github.com/0gnetwork/0g.git
cd 0g
make install
```

## Initialize the Node:

```
0gd init <your_moniker> --chain-id=0g-testnet
```

## Configure the Node:

### Set Peers:

Edit the config.toml file:

```
nano ~/.0gd/config/config.toml
```

## Set persistent_peers to the provided list of peers.

### Set Minimum Gas Prices:

Edit the app.toml file:

```
nano ~/.0gd/config/app.toml
```

Set minimum-gas-prices to an appropriate value, e.g., 0.0250g.

## Download Genesis File:

```
wget -O ~/.0gd/config/genesis.json https://path/to/genesis.json
```

## Start the Node:

```
0gd start
```

## Create Validator:

### Once the node is synced, create a validator:

```
0gd tx staking create-validator \
  --amount=1000000u0g \
  --pubkey=$(0gd tendermint show-validator) \
  --moniker=<your_moniker> \
  --chain-id=0g-testnet \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=<your_wallet_name> \
  --gas=auto \
  --fees=250u0g
```
