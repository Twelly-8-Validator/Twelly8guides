```markdown
## Prerequisites

This page will go over setting up your machine to run the `fairyring` binary and `fairyringclient`.

### Install Dependencies

This guide assumes that you are using Ubuntu.

1. **Upgrade your operating system:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install essential packages:**
   ```bash
   sudo apt install git curl tar wget libssl-dev jq build-essential gcc make
   ```

3. **Download and install Go:**
   ```bash
   sudo add-apt-repository ppa:longsleep/golang-backports
   sudo apt update
   sudo apt install golang-go
   ```

4. **Add `/usr/local/go/bin` & `$HOME/go/bin` directories to your `$PATH`:**
   ```bash
   echo "export PATH=\$PATH:/usr/local/go/bin:\$HOME/go/bin" >> $HOME/.profile
   source $HOME/.profile
   ```

5. **Verify Go was installed correctly. Note that the `fairyring` binary requires Go v1.21:**
   ```bash
   go version
   ```

```markdown
## Installation

This tutorial goes over building and installing the `fairyring` binary. This tutorial assumes you completed the prerequisites.

1. **Remove any existing fairyring nodes & clone the repository:**
   ```bash
   cd $HOME
   rm -rf fairyring
   git clone https://github.com/Fairblock/fairyring.git
   cd fairyring
   ```

2. **Checkout to the latest release of fairyring (currently v0.5.0):**
   ```bash
   git checkout v0.5.0
   ```

3. **Install the fairyringd binary:**
   ```bash
   make install
   ```

4. **Verify the installation:**
   ```bash
   fairyringd version
   ```
   The output should show `0.5.0`.

### Upgrading Your Binary

To upgrade your binary, you can install the latest version using the instructions above and restart your node. You can check the latest version [here](https://github.com/Fairblock/fairyring/releases). If you run into any issues, refer to the troubleshooting section.


```markdown
## Running a Testnet

Make sure you have installed the `fairyring` binary prior to following the instructions below.

1. **Initialize the fairyring node:**
   Use `fairyringd` to initialize your node (replace `NODE_NAME` with a name of your choosing):
   ```bash
   fairyringd init NODE_NAME
   ```

2. **If you are unable to get the genesis file from the link above:**
   ```bash
   cp ~/fairyring/networks/testnets/fairyring-testnet-1/genesis.json ~/.fairyring/config/genesis.json
   ```

3. **Set peers in the `$HOME/.fairyring/config/config.toml`:**
   ```bash
   SEEDS=$(cat $HOME/fairyring/networks/testnets/fairyring-testnet-1/peers-node.txt)
   echo $SEEDS
   sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$SEEDS\"/" $HOME/.fairyring/config/config.toml
   ```

4. **Start the node:**
   ```bash
   fairyringd start
   ```

### Next Steps

If you would like to become a validator in the testnet, please follow [this guide](#).

If you run into any issues, please refer to the troubleshooting section.
```

Feel free to copy and paste this into your README file!
Feel free to copy and paste this into your README file!
