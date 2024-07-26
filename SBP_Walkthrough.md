# Running a SBP walkthrough

Welcome to the step-by-step guide on how to install an SBP.
The top 100 SBPs with the most votes are eligible with a higher probabilty to produce blocks and receive more mining rewards. The probability of a top 25 SBP producing a block is 23/25, while an SBP ranked between 26th and 100th has a probability of 2/75.

Over a full year, SBPs can earn mining rewards worth 30 million VITE (approximately 3% of the total supply). SBP rewards are divided into two categories:

- **Block-producing rewards:** 50% of the rewards are distributed based on the number of blocks created by each SBP relative to the total number of blocks.
  
- **Staking and voting rewards:** The remaining 50% is distributed based on the amount staked and the number of votes each SBP receives, in proportion to the total amount staked and votes received by the top 100 SBPs.

## Hardware requirements:

:zap: PC, Laptop or VPS  
:zap: 4 cores minimum  
:zap: 8GB RAM  
:zap: 500GB minimum storage capacity (Ledger currently takes up 220GB)  
:zap: Full node installed with the guide for full node - [Check here](FullNode_Walkthrough.md)  
:zap: A stable internet connection  

## Other requirements:

1,000,000 VITE which will be locked to the address that you entered in `node_config.json` when you installed your full node.

The said amount of VITE needs to be locked as soon as the SBP is online to make sure that you do not miss out on the daily rewards for creating blocks and votes. The rewards will depend on the rank of your SBP has and the amount of votes it will have.

## Part I - INSTALLING AN SBP
This step-by-step guide assumes that you have installed the Full Node according to the tutorial provided in ⁠[FullNode Walkthrough](FullNode_Walkthrough.md) If it is installed in another way, these instructions may not apply to your settings.

### Step 1: 
- Start by logging into the Full Node with SSH. A how-to guide can be found [here](Ubuntu_Install.md)
### Step 2: 
- The next thing we need to do is to STOP the Full Node. You can do this by typing the command:
  ```
  sudo service vite stop
  ```
- This command may take some time to complete because it needs to write all open files to the ledger. BE PATIENT.

### Step 3: 
- Go into the folder where the gvite is located. Run this command to do so:
  ```
  cd ~/vite
  ```

### Step 4: 
- The next thing we need to do is to create a wallet for the SBP. This will be used as a block creation address.
- *Remember to replace 123456 with a good/strong password.*
  ```
  ./gvite rpc ~/.gvite/maindata/gvite.ipc wallet_createEntropyFile '["123456"]'
  ```

- The above command should give a similar output to the one shown below. (Note: The VITE address and the mnemonic shown in the output below are for instructional purposes only. Your output will be a different address and mnemonic.) Save the output in a safe place because without it, you will not be able to use the address.
  ```
  {
      "jsonrpc": "2.0",
      "id": 1,
      "result": {
          "mnemonic": "ancient rat fish intact viable flower now rebuild monkey add moral injury banana crash rabbit awful boat broom sphere welcome action exhibit job flavor",
          "primaryAddr": "vite_f1c2d944b1e5b8cbfcd5f90f94a0e877beafeced1f331d9acf",
          "filename": "~/.gvite/maindata/wallet/vite_f1c2d944b1e5b8cbfcd5f90f94a0e877beafeced1f331d9acf"
      }
  }
  ```

- The above output explained:
  - **mnemonic:** The mnemonic phrase of the wallet that you have just created. Remember to save this!
  - **primaryAddr:** Vite address corresponding to the mnemonic phrase.
  - **filename:** The location of the keystore file.

### Step 5: 
- Check if the wallet was created successfully by entering this command:
  ```
  ls ~/.gvite/maindata/wallet/
  ```
  
- Its output should look similar to this:
  ```
  vite_f1c2d944b1e5b8cbfcd5f90f94a0e877beafeced1f331d9acf
  ```
- *Remember: Your address will not be exactly the same as the above example.*

### Step 6: 
- Next, you need to change a few settings in the `node_config.json`
  ```
  sudo nano /etc/vite/node_config.json
  ```

- Change the following:
  ```
        "Miner": true,
        "CoinBase": "0:Your address",
        "EntropyStorePath": "Your address",
        "EntropyStorePassword": "Your password",
  ```

- Your address: The VITE address you created above.
- Your password: The password you used to create the VITE address.

-- **DO NOT USE YOUR REWARD ADDRESS AS A COINBASE ADDRESS.** --


### Step 7: 
- To save your edited file, hit `control+X` then `Y` and the file is saved.

### Step 8: 
- Restart the node by running this command:
  ```
  sudo service vite start
  ```

- After this, you may already close the SSH connection.

## Part II - PREPARING TO REGISTER THE SBP
In this section, you will learn how to register an SBP and lock the required amount of 1,000,000 VITE. This guide is written with the assumption that you are using a VITE Web Wallet and your VITE Mobile app installed on your phone.

### Step 1
- Logging into Vitex Exchange
- Go to https://x.vite.net/ and click on login in the top right corner. If you have used this site before, you will need to click unlock to unlock your wallet. A QR code will pop up.
- Scan the QR code using the VITE Wallet app on your phone by activating its scanner feature located at the top right corner.
- Once you are logged in, your screen should look like this: [imagen](URL)

### Step 2: 
- Navigating the VITE Web wallet, on the left side of the page there are a few buttons you can click to explore. From top to bottom:  
  :zap: The VITE logo takes you to VX Mining and Dividend dashboard.  
  :zap: The Wallet icon takes you to your wallet where you can see all your assets.  
  :zap: The Three lines icon takes you to the quota staking and locking section of ViteX.  
  :zap: The Gear icon takes you to the settings page.  
  :zap: The moon/light bulb icon changes the page to light/dark mode.  

- Since we are going to use the staking/locking page, we will choose the three lines icon. Your screen should look like this: [imagen](URL)

### Step 3: 
- In the top menu bar on the page are 6 more wallet options explained from left to right:

  - The first option where the page opens to is the quota page. This is where you can lock VITE to get quota.
  - The second option is the voting page. This is where you can select an SBP to vote for.
  - The third option is the Full Node page. Here you can lock the VITE for a full node.
  - The fourth option is the SBP page. This is where you go to when locking the 1,000,000 VITE required for running an SBP.
  - The fifth option is the Token Issuance page. This is the page to use for minting a new token on the Vite Network.
  - The sixth option is the Transactions page. All transactions done with your logged-in wallet will appear here.

## PART III - REGISTERING YOUR SBP AND LOCKING THE REQUIRED AMOUNT OF VITE
- Go to the SBP page and fill in the following:

  - **Node name:** This needs to be the name that you set in `node_config.json`
  - **Block Creation Address:** The Coinbase address that you set in `node_config.json`
  
- After which, click on the SUBMIT button.

- *Note: It is important to make sure that before you click SUBMIT, the required amount of 1,000,000 VITE is already in your wallet. This amount of VITE will be locked for a period of approximately 90 days. After such a period, you may already unlock your VITE. However, unlocking your 1,000,000 after the 90-day period will mean that you will no longer be receiving any more rewards.*
