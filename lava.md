
<h1 align=center> Lava Testnet Node Setup Guide </h1>

This guide will help you manually install a Lava Protocol node and join the testnet.

#### **Step 1: System Preparation**

1. **Verify Requirements:**

|KEY|VALUE|
|:--|:----|
|**OS:**|Ubuntu 20.04 or later.|
|**CPU:**|4 cores minimum.|
|**RAM:**|8 GB or more.|
|**Storage:**|At least 200 GB SSD.|
|**Network:**|Stable connection with 1 Mbps upload/download.|

2. **Install Necessary Packages:**

   Update your system and install the dependencies:

   ```bash
   sudo apt update
   sudo apt install -y unzip logrotate git jq sed wget curl coreutils systemd
   ```

   Create a temporary directory for installation:

   ```bash
   temp_folder=$(mktemp -d) && cd $temp_folder
   ```

#### **Step 2: Install Go**

1. **Download and Install Go:**

   ```bash
   go_package_url="https://go.dev/dl/go1.20.5.linux-amd64.tar.gz"
   wget -q $go_package_url
   sudo tar -C /usr/local -xzf ${go_package_url##*/}
   ```

2. **Set Up Environment:**

   ```bash
   echo "export PATH=\$PATH:/usr/local/go/bin" >>~/.profile
   echo "export PATH=\$PATH:\$(go env GOPATH)/bin" >>~/.profile
   source ~/.profile
   ```

3. **Verify Installation:**

   ```bash
   go version
   echo $PATH
   ```

#### **Step 3: Set Up Local Node**

1. **Download Configuration Files:**

   ```bash
   git clone https://github.com/lavanet/lava-config.git
   cd lava-config/testnet-2
   source setup_config/setup_config.sh
   ```

2. **Configure Node:**

   ```bash
   mkdir -p $lavad_home_folder $lava_config_folder
   cp default_lavad_config_files/* $lava_config_folder
   cp genesis_json/genesis.json $lava_config_folder/genesis.json
   ```

#### **Step 4: Join the Testnet**

1. **Set Up Genesis Binary:**

   ```bash
   lavad_binary_path="$HOME/go/bin/"
   mkdir -p $lavad_binary_path
   wget -O ./lavad "https://github.com/lavanet/lava/releases/download/v0.21.1.2/lavad-v0.21.1.2-linux-amd64"
   chmod +x lavad
   cp lavad /usr/local/bin
   lavad --help
   ```

2. **Create Systemd Service:**

   Create a service to automatically manage the Lava node:

   ```bash
   echo "[Unit]
   Description=Lava Node
   After=network-online.target
   [Service]
   User=$USER
   ExecStart=$(which lavad) start --home=$lavad_home_folder --p2p.seeds $seed_node
   Restart=always
   RestartSec=180
   LimitNOFILE=infinity
   LimitNPROC=infinity
   [Install]
   WantedBy=multi-user.target" > lavad.service

   sudo mv lavad.service /lib/systemd/system/lavad.service
   ```

3. **Start the Service:**

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable lavad.service
   sudo systemctl start lavad
   ```

   Verify the node is running:

   ```bash
   sudo systemctl status lavad
   sudo journalctl -u lavad -f
   ```

#### **Step 5: Upgrading the Node**

If the network requires an upgrade:

1. **Stop the Service:**

   ```bash
   sudo systemctl stop lavad
   ```

2. **Download and Replace Binary:**

   ```bash
   temp_folder=$(mktemp -d) && cd $temp_folder
   upgrade_binary_url="https://github.com/lavanet/lava/releases/download/v0.21.1.2/lavad-v0.21.1.2-linux-amd64"
   wget "$upgrade_binary_url" -q -O $temp_folder/lavad
   chmod +x $temp_folder/lavad
   sudo cp $temp_folder/lavad $(which lavad)
   ```

3. **Restart the Node:**

   ```bash
   sudo systemctl start lavad
   lavad status | jq .SyncInfo.catching_up
   sudo journalctl -u lavad -f
   ```
