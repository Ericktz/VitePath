## Running a FullNode walkthrough

Welcome to our comprehensive installation tutorial! In this step-by-step guide, we'll walk you through the entire process of setting up a full node. Whether you're a beginner or an experienced user, this tutorial will provide clear instructions and helpful tips to ensure a smooth and successful installation. Let's get started on building your full node with ease and confidence.

REQUIREMENTS

Hardware:

:small_blue_diamond:A personal computer, mini pc or vps
:small_blue_diamond:4 cores minimum
:small_blue_diamond:4GB RAM
:small_blue_diamond:500GB minimum storage capacity (Ledger currently takes up 195GB)
:small_blue_diamond:Clean install of Ubuntu 20.04LTS (The installation guide for this can be found here: ⁠📝︲installation-tutorial-ubuntu)
:small_blue_diamond:A good Internet connection

Other requirements:

10,000 VITE which will be locked to the address you enter in node_config.json

Such amount of VITE needs to be locked as soon as the node is online to make sure you do not miss out on the rewards for voting. Currently, the daily reward for running a full node with 10,000 VITE locked is around 7 VITE.
FULL NODE INSTALLATION
Part I: Getting started:

To start with, make sure that the computer or a virtual private server (VPS) which you will be using has a Secure Shell Protocol (SSH) access and that Ubuntu 20.04 has already been installed in it.

Step 1:Log into your computer or VPS with SSH. (Logging in with SSH is explained in detail here ⁠📝︲installation-tutorial-ubuntu)

Step 2: Download the node software.
curl -L -O  https://github.com/vitelabs/go-vite/releases/download/v2.13.0/gvite-v2.13.0-linux.tar.gz


Step 3: Unpack the node software:
tar -xzvf gvite-v2.13.0-linux.tar.gz


Step 4: Move the folder to another directory:
mv gvite-v2.13.0-linux vite


Step 5: Go into the folder it should have 3 files (gvite, bootstrap and node_config.json):
cd vite
ls


output of the ls command should show the three mentioned files!

Part II: Making the node config
nano node_config.json


There are a few things we need to change in there,

Change identity to a name so you can easily find it back in your dashboard.
The RPC line should say enabled.
The WS line should say enabled.
The reward address should be the VITE address which is going to hold the 10,000 locked VITE.

For reference, below is an example of a node_config.json. This is the actual node being used by vitaminion#0001. Feel free to copy and use this but remember to change the identity and reward address between the " "
{
  "Identity": "name here",
  "NetID": 1,
  "ListenInterface": "0.0.0.0",
  "Port": 8483,
  "FilePort": 8484,                                                                       
  "MaxPeers": 10,
  "MinPeers": 5,
  "MaxInboundRatio": 2,
  "MaxPendingPeers": 5,
  "BootSeeds": [
    "https://bootnodes.vite.net/bootmainnet.json"
  ],
  "Discover": true,
  "RPCEnabled": true,
  "HttpHost": "0.0.0.0",
  "HttpPort": 48132,
  "WSEnabled": true,
  "WSHost": "0.0.0.0",
  "WSPort": 41420,
  "HttpVirtualHosts": [],
  "IPCEnabled": true,
  "PublicModules": [
    "wallet"
    "ledger",
    "contract",
    "tx",
    "debug",
    "dashboard",
    "subscribe",
    "util",
    "pow",
    "public_onroad",
    "private_onroad",
    "net",
    "pledge",
    "register",
    "vote",
    "mintage",
    "consensusGroup",
    "sbpstats",
    "dex",
    "dextrade",
    "dexfund",
    "private_dex",
    "data"
  ],
  "Miner": false,
  "CoinBase": "",
  "EntropyStorePath": "",
  "EntropyStorePassword": "",
  "LogLevel": "info",
  "OpenPlugins": true,
  "SubscribeEnabled": true,
  "TxDexEnable": true,
  "VmLogAll": true,
  "DashboardTargetURL": "wss://stats.vite.net",
  "RewardAddr": "vite address here",
  "PowServerUrl": "http://127.0.0.1:7076"
}


Save the edited file with control+x then y

Part III: Starting the node for the first time
./bootstrap


To see if the node started successfully, enter the following command:
cat gvite.log


The output should look like this:
t=2018-11-09T17:44:48+0800 lvl=info msg=NodeServer.DataDir:/home/ubuntu/.gvite/maindata module=gvite/node_manager
t=2018-11-09T17:44:48+0800 lvl=info msg=NodeServer.KeyStoreDir:/home/ubuntu/.gvite/maindata/wallet module=gvite/node_manager
Prepare the Node success!!!
Start the Node success!!!



See the next message for part IV
FULL NODE INSTALL
Part IV: Making the node start when the computer/VPS is booted or restarted.

Start with the following command and write down the output you will need it later:
pwd



Output should look like this:
vitaminion@node01:~/vite$ pwd
/home/vitaminion/vite


Part V: Making the install Script

We are going to make the install script so we can turn it into a service. We are using nano, an easy to use text editor built into ubuntu.

Step 1: Start by entering the following command:
nano install.sh


Step 2: Copy and enter the following content into the screen:
#!/bin/bash

set -e

CUR_DIR="output of pwd command here"
CONF_DIR="/etc/vite"
BIN_DIR="/usr/local/vite"
LOG_DIR=$HOME/.gvite

echo "install config to "$CONF_DIR


sudo mkdir -p $CONF_DIR
sudo cp $CUR_DIR/node_config.json $CONF_DIR
ls  $CONF_DIR/node_config.json

echo "install executable file to "$BIN_DIR
sudo mkdir -p $BIN_DIR
mkdir -p $LOG_DIR
sudo cp $CUR_DIR/gvite $BIN_DIR

echo '#!/bin/bash
exec '$BIN_DIR/gvite' -pprof -config '$CONF_DIR/node_config.json' >> '$LOG_DIR/std.log' 2>&1' | sudo tee $BIN_DIR/gvited > /dev/null

sudo chmod +x $BIN_DIR/gvited

ls  $BIN_DIR/gvite
ls  $BIN_DIR/gvited

echo "config vite service boot."

echo '[Unit]
Description=GVite node service
After=network.target

[Service]
ExecStart='$BIN_DIR/gvited'
Restart=on-failure
User='`whoami`'
LimitCORE=infinity
LimitNOFILE=10240
LimitNPROC=10240

[Install]
WantedBy=multi-user.target' | sudo tee /etc/systemd/system/vite.service>/dev/null

sudo systemctl daemon-reload


Step 3: In the line that reads CUR_DIR="output of pwd command here" change the text inside the two quotation marks (" ") to the output of pwd that you wrote down earlier in Part IV of this tutorial.

Step 4: We are now going to give this file its executable permissions so that we can run it. Do this by entering this command:
sudo chmod +x install.sh


Step 5: Run the install command and enable the service by entering the following these commands:
./install.sh


And:
sudo systemctl enable vite


Part VI: Starting the Node

Next up is starting the node as a service this requires a few commands the first time.

Step 1: Kill the initial started one by entering this command:
pgrep gvite | xargs kill -s 9


Step 2: Wait for a few minutes before running the next command (5 minutes is good):
sudo service vite start


Step 3: Check if the service started correctly by running this command:
sudo service vite status


The output should look something like this:
● vite.service - GVite node service
     Loaded: loaded (/etc/systemd/system/vite.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-03-01 09:23:39 UTC; 1 months 16 days ago
   Main PID: 69893 (gvite)
      Tasks: 17 (limit: 4482)
     Memory: 2.6G
     CGroup: /system.slice/vite.service
             └─69893 /usr/local/vite/gvite -pprof -config /etc/vite/node_config.json

Mar 01 09:23:39 node01 systemd[1]: Started GVite node service.


After which, it will start syncing the ledger. Pay close attention to this and f it might take a while to finish.

On a low-end hardware, this would take a good two to three weeks work to finish. If in a hurry, you may send a DM to @VitaMinion with a request to discuss ledger transfer.

In the last part i will show you how to lock the 10,000 VITE for the full node rewards.
FULL NODE INSTALL
Part VII: Locking VITE for Rewards

This part of the tutorial will show you how to lock the required amount of VITE and is written with the assumption that you are using VITE Wallet installed on your phone.

Step 1: Logging into the ViteX Exchange

:small_blue_diamond:Go to https://x.vite.net/ and click on LOGIN located in the top right corner. If you have used this site before, you need to click UNLOCK to unlock your wallet.

:small_blue_diamond:When the QR code pops up, scan it using the scanner of the Vite Wallet app on your phone.

Once you are logged in your screen should look like this:
FULL NODE INSTALL
Step 2: Navigating the VITE Wallet

On the left side of the page there are a few buttons you can click to explore. From top to bottom:

:small_blue_diamond:The VITE logo takes you to VX Mining and Dividend dashboard.
:small_blue_diamond:The Wallet icon takes you to your wallet where you can see all your assets.
:small_blue_diamond:The Three lines icon takes you to the quota staking and locking section of vitex.
:small_blue_diamond:The Gear icon takes you to the settings page.
:small_blue_diamond:The moon/light bulb icon changes the page to light/dark mode.

Since we are going to use the staking/locking page, we will choose the three lines icon. Your screen should look like this:
Imagen
Step 3: More Wallet Options

In the top menu bar on the page are 6 options explained from left to right:

:small_blue_diamond:The first option where the page opens on is the quota page. This is where you can lock VITE to get quota.

:small_blue_diamond: The second option is the voting page. This is where you can select an SBP to vote for.

:small_blue_diamond: The third option is the Full Node page. This is where you can lock the VITE needed for running a full node. This is the page we are going to choose for locking the 10,000 VITE required for running the full node.

:small_blue_diamond: The fourth option is the SBP page. This is where you can register an SBP.

:small_blue_diamond: The fifth option is the Token Issuance page. This is the page to use for minting a new token on the Vite Network.

:small_blue_diamond: The Sixth option is the Transactions page. All transactions done with your logged in wallet will appear here.

Step 4: Locking the Required Amount of VITE

Go to the Full Node page and fill in the following:

The amount to stake, every node you run requires 10,000 VITE. So for example, if you run 3 nodes the total amount needs to be 30,000 VITE, this amount can be locked in separate amounts. Just make sure you increase by 10,000 VITE at a time.

The locking period is 30 days, after that time you can unlock. But remember, once unlocked you don't get the rewards anymore.

After you have filled in the amount, click on the "Submit To Lock" button and then you are done.
Imagen
