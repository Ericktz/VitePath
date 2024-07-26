# How to Install Ubuntu Tutorial

In this tutorial, I'll guide you step-by-step on how to install Ubuntu 20.04 on a PC or VPS, and how to connect to it using SSH.

## Requirements

:zap: A personal computer, laptop or VPS   
:zap: Minimum of 4 cores   
:zap: At least 300GB of storage (SSD preferred)   
:zap: A second PC for establishing an SSH session   

_**Pay Attention!: This tutorial will erase all data on the PC or VPS!**_

## PART 1 - Getting Started

In this part, I'll explain how to install Ubuntu on a VPS and connect to it using SSH. As a bonus, we'll secure it with a firewall to prevent all ports from being open.

For this tutorial, I'll assume you're connecting via SSH from a Windows PC or laptop.

### Step 1: Renting a VPS

Depending on your location, there are several options available. For this tutorial, we'll use Contabo as an example.

1. Visit [Contabo VPS](https://contabo.com/en/vps/). Since we'll use this for our node later, select the VPS S option.
2. Choose the term length (I recommend a monthly term for flexibility). Select the region closest to your location.
3. For storage, 200GB shouldn't suffice for a Full Node, you must need afford an extra $1.50, opt for the 400GB package.
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

## PART 2 - Securing Your VPS

In this part, we'll secure your VPS and change the SSH port.

**Be very careful not to disconnect before the tutorial instructs you to, or you may need to reinstall!**

### Step 1: Change the SSH Port

1. Open the SSH configuration file with:  
   ```
   sudo nano /etc/ssh/sshd_config 
2. Find the line:
    ```
    #port 22
3. Change it to:
    ```
    port 2220
4. Save and close the file by pressing `Ctrl + X`, then `Y`, and `Enter`.
5. Restart the SSH service with:
    ```
    sudo systemctl restart sshd
6. Open a new PuTTY window, enter the IP address and new port, and connect again. Accept the security warning and log in. If successful, you can close the old session.

### Step 2: Enabling the Firewall
1. Allow necessary ports and enable the firewall with the following commands:
   ```
   sudo ufw allow 2220/tcp
   sudo ufw allow 41420
   sudo ufw allow 48132
   sudo ufw enable
   sudo ufw status
2. Ensure you can still connect by opening a new PuTTY session. If successful, your VPS is now secure.

## PART 3 - Installing on a PC/laptop
### Requirements

:zap: A PC/laptop for Ubuntu installation (all data will be erased!)  
:zap: An empty USB stick with at least 8GB of storage (will be wiped)  
:zap: A PC/laptop for following the tutorial and connecting via SSH  

### Step 1: Downloading and Preparing the USB Stick
1. Download Ubuntu Server from [Ubuntu Downloads](https://ubuntu.com/download/server#downloads).
2. Select "Manual installation" and click on "Get Ubuntu 20.04 LTS."
3. Download [Rufus](https://rufus.ie/) to prepare the USB stick.
4. Start Rufus, select your USB device, and the Ubuntu ISO you downloaded. Click "Start" and wait for the process to complete.
   
### Step 2: Installing Ubuntu
1. Reboot the target computer with the USB stick inserted. The installer should start automatically.
2. Follow the installation prompts, ensuring to install OpenSSH.
3. For detailed instructions, refer to this [installation guide](https://techguides.yt/guides/how-to-install-ubuntu-server-20-04-lts-from-usb/#3_Install_Ubuntu_Server_2004_LTS).

### Step 3: Enabling the Firewall
1. Once Ubuntu is installed, log in with the username and password you set.

2. Run the following commands to ensure SSH access and enable the firewall:   
   ```
   sudo apt update
   sudo apt upgrade
   ip addr
3. Note the IP address from the `ip addr` command.
4. From another computer, start PuTTY and connect to this IP address with SSH on port 22. Accept the security alert and log in.
5. Enable the firewall with:
   ```
   sudo ufw allow 22/tcp
   sudo ufw allow 41420
   sudo ufw allow 48132
   sudo ufw enable
   sudo ufw status
6. Test your connection with a new PuTTY session. If successful, your PC is now set up with Ubuntu and secured with a firewall.

