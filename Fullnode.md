# Full Node in Vite Network

In the Vite network, nodes are categorized into Full Nodes and Snapshot Block Producer (SBP) aka Super nodes in the scene. This section introduces the installation of a Full Node.

A Full Node is integral to the network, tasked with maintaining a comprehensive copy of the blockchain ledger, facilitating the transmission and receipt of transactions, and validating all network transactions. Additionally, a Full Node can engage in SBP elections and voting processes. It provides external HTTP/WEBSOCKET APIs and features a local command line interface, which can be enabled via configuration settings.

For a step-by-step guide on setting up a Full Node, refer to the installation-tutorial channel.

Should you have any questions regarding this topic, please feel free to reach out in the support chat.

## Installation Tutorial

In this tutorial, I'll guide you step-by-step on how to install Ubuntu 20.04 on a PC or VPS, and how to connect to it using SSH.

### Requirements

:small_blue_diamond: A personal computer, mini PC, or VPS  
:small_blue_diamond: Minimum of 4 cores  
:small_blue_diamond: At least 300GB of storage  
:small_blue_diamond: A second PC for establishing an SSH session  

### Pay Attention!

**This tutorial will erase all data on the PC or VPS!**

## How to Install Ubuntu: Part 1 - Getting Started

In this part, I'll explain how to install Ubuntu on a VPS and connect to it using SSH. As a bonus, we'll secure it with a firewall to prevent all ports from being open.

For this tutorial, I'll assume you're connecting via SSH from a Windows PC or laptop.

### Step 1: Renting a VPS

Depending on your location, there are several options available. For this tutorial, we'll use Contabo as an example.

1. Visit [Contabo VPS](https://contabo.com/en/vps/). Since we'll use this for our node later, select the VPS S option.
2. Choose the term length (I recommend a monthly term for flexibility). Select the region closest to your location.
3. For storage, 200GB shouldn't suffice for a Full Node, you must can afford an extra $1.50, opt for the 400GB package.
4. Select Ubuntu 20.04 as the operating system.
5. Set a strong password for the root user and keep it secure.
6. Fill in your details and complete the payment.

If you already have a VPS, perform a clean install of Ubuntu 20.04 and continue with this tutorial.

### Step 2: Downloading and Starting PuTTY

After a few hours, you'll receive an email with connection instructions. This email will include the IP address of your VPS.

1. Download and install [PuTTY](https://www.putty.org/) on the PC you’ll use to connect to your VPS.
2. Start PuTTY and enter the IP address from the email in the "Hostname (or IP address)" field. Ensure the connection type is set to SSH and the port is 22. Click "Open."

### Step 3: Logging In

1. A security alert will appear because it's the first time connecting to this IP. Accept the server host key.
2. When prompted, type `root` and press Enter.
3. Enter the root password and press Enter again.
4. You should now see the welcome screen, indicating you've successfully logged into your VPS.

## How to Install Ubuntu: Part 2 - Securing Your VPS

In this part, we'll secure your VPS and change the SSH port.

**Be very careful not to disconnect before the tutorial instructs you to, or you may need to reinstall!**

### Step 1: Change the SSH Port

1. Open the SSH configuration file with:  
   ```
   sudo nano /etc/ssh/sshd_config 
3. Find the line:
    ```
    #Port 22
4. Change it to:
    ```
    Port 2220
5. Save and close the file by pressing Ctrl + X, then Y, and Enter.
6. Restart the SSH service with:
    ```
    sudo systemctl restart sshd
7. Open a new PuTTY window, enter the IP address and new port, and connect again. Accept the security warning and log in. If successful, you can close the old session.

Step 2: Enabling the Firewall
Allow necessary ports and enable the firewall with the following commands:


sudo ufw allow 2220/tcp
sudo ufw allow 41420
sudo ufw allow 48132
sudo ufw enable
sudo ufw status
Ensure you can still connect by opening a new PuTTY session. If successful, your VPS is now secure.

How to Install Ubuntu: Part 3 - Installing on a PC or Mini PC
Requirements
:small_blue_diamond: A PC or mini PC for Ubuntu installation (all data will be erased!)
:small_blue_diamond: An empty USB stick with at least 8GB of storage (will be wiped)
:small_blue_diamond: A PC/laptop for following the tutorial and connecting via SSH

Step 1: Downloading and Preparing the USB Stick
Download Ubuntu Server from Ubuntu Downloads.
Select "Manual installation" and click on "Get Ubuntu 20.04 LTS."
Download Rufus to prepare the USB stick.
Start Rufus, select your USB device, and the Ubuntu ISO you downloaded. Click "Start" and wait for the process to complete.
Step 2: Installing Ubuntu
Reboot the target computer with the USB stick inserted. The installer should start automatically.
Follow the installation prompts, ensuring to install OpenSSH.
For detailed instructions, refer to this installation guide.
Step 3: Enabling the Firewall
Once Ubuntu is installed, log in with the username and password you set.

Run the following commands to ensure SSH access and enable the firewall:


sudo apt update
sudo apt upgrade
ip addr
Note the IP address from the ip addr command.

From another computer, start PuTTY and connect to this IP address with SSH on port 22. Accept the security alert and log in.

Enable the firewall with:


sudo ufw allow 22/tcp
sudo ufw allow 41420
sudo ufw allow 48132
sudo ufw enable
sudo ufw status
Test your connection with a new PuTTY session. If successful, your PC is now set up with Ubuntu and secured with a firewall.

