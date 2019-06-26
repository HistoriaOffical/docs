.. meta::
   :description: This guide describes how to set up a Historia masternode. It also describes various options for hosting and different wallets
   :keywords: historia, guide, masternodes, setup,

.. _masternode-setup:

===================================================================
Setup For Linux - Content Distribution Masternode - 5000 Collateral
===================================================================

Setting up a masternode requires a basic understanding of Linux and blockchain technology, as well as an ability to follow instructions closely. It also requires regular maintenance and careful security. There are some decisions to be made along the way, and optional extra steps to take for increased security.

Before you begin
================

This guide assumes you are setting up a single masternode for the first
time. You will need:

- 5000 HTA.
- A wallet to store your Historia, although Historia Core wallet is also supported.
- A Linux server, preferably a Virtual Private Server (VPS).
- IPv4
- IPv6

We also assume you will be working from a Windows computer. However, since most of the work is done on your Linux VPS, alternative steps for using macOS or Linux will be indicated where necessary.

Masternode Roles
----------------

Unlike most other masternode coins, Historia makes use of a role based masternode system. Currently there are two roles:
 - Voting Masternode 
 
   - Collateral Requirement: 100 HTA
   - Reward: 10% per block
   - Ports: TCP 10101, TCP 4001
   - IPFS Required: No
   - IPv4 address required
 - Content Distribution Masternode 
 
   - Collateral Requirement: 5000 HTA
   - Reward: 25% per block - increaes 2.5% every 2 months until 50% per block
   - Ports: TCP 10101, TCP 4001, TCP 80, TCP 443
   - IPFS Required: Yes
   - IPv4 and IPv6 address required

In this guide, we will setup a Content Distribution Masternode (CDMN) with collateral of 5000. 

.. _vps-setup:

Set up your VPS
===============

A VPS, more commonly known as a cloud server, is fully functional installation of an operating system (usually Linux) operating within a virtual machine. The virtual machine allows the VPS provider to run multiple systems on one physical server, making it more efficient and much cheaper than having a single operating system running on the “bare metal” of each server. A VPS is ideal for hosting a Historia masternode because they typically offer guaranteed uptime, redundancy in the case of hardware failure and a static IP address that is required to ensure you remain in the masternode payment queue. While running a masternode from home on a desktop computer is technically possible, it will most likely not work reliably because most ISPs allocate dynamic IP addresses to home users.

We will use Vultr hosting as an example of a VPS. First create an account and add credit. Then go to the Servers menu item on the left and click + to add a new server. 

Select a location for your new server on the following screen:


.. figure:: ../img/Picture1.png
   :width: 400px

   Vultr server location selection screen

Select Ubuntu 16.04 x64 as the server type. We use this LTS release of
Ubuntu instead of the latest version because LTS releases are supported
with security updates for 5 years, instead of the usual 9 months.

.. figure:: ../img/Picture2.png
   :width: 400px

   Vultr server type selection screen

Select a server size offering at least 2GB of memory.

.. figure:: ../img/Picture3.png
   :width: 400px

   Vultr server size selection screen

Enter a hostname and label for your server. In this example we will use htamn01 as the hostname.


.. figure:: ../img/Picture4.png
   :width: 400px

   Vultr server hostname & label selection screen

Add IPv6 for your server. 

.. figure:: ../img/6.PNG
   :width: 400px

   Vultr IPv6 Address screen

Vultr will now install your server. This process may take a few minutes.

.. figure:: ../img/Picture5.png
   :width: 400px

   Vultr server installation screen

Click **Manage** when installation is complete and take note of the IPv4
address, IPv6 address (if setting up Content Distribution Masternnode), username and password.

.. figure:: ../img/Picture6.png
   :width: 276px

   Vultr server management screen


Set up your operating system
============================

We will begin by connecting to your newly provisioned server. On
Windows, we will first download an app called PuTTY to connect to the
server. Go to the `PuTTY download page <https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>`_
and select the appropriate MSI installer for your system.
On Mac or Linux you can ssh directly from
the terminal - simply type ``ssh root@<server_ip>`` and enter your
password when prompted.

.. figure:: ../img/Picture7.png
   :width: 400px

   PuTTY download page

Double-click the downloaded file to install PuTTY, then run the app from
your Start menu. Enter the IP address of the server in the **Host Name**
field and click **Open**. You may see a certificate warning, since this
is the first time you are connecting to this server. You can safely
click **Yes** to trust this server in the future.

.. figure:: ../img/Picture8.png
   :width: 320px

   PuTTY security alert when connecting to a new server

You are now connected to your server and should see a terminal
window. Begin by logging in to your server with the user ``root`` and
password supplied by your hosting provider.

.. figure:: ../img/Picture9.png
   :width: 400px

   Password challenge when connecting to your VPS for the first time

You should immediately change the root password and store it in a safe
place for security. You can copy and paste any of the following commands
by selecting them in your browser, pressing **Ctrl + C**, then switching
to the PuTTY window and right-clicking in the window. The text will
paste at the current cursor location::

  passwd root

Enter and confirm a new password (preferably long and randomly
generated). Next we will create a new user with the following command,
replacing ``<username>`` with a username of your choice::

  adduser <username>

You will be prompted for a password. Enter and confirm using a new
password (different to your root password) and store it in a safe place.
You will also see prompts for user information, but this can be left
blank. Once the user has been created, we will add them to the sudo
group so they can perform commands as root::

  usermod -aG sudo <username>

Now, while still as root, we will update the system from the Ubuntu
package repository::

  apt update
  apt upgrade

The system will show a list of upgradable packages. Press **Y** and
**Enter** to install the packages. We will now install a firewall (and
some other packages we will use later), add swap memory and reboot the
server to apply any necessary kernel updates, and then login to our
newly secured environment as the new user::

  apt install ufw python virtualenv git unzip pv

(press **Y** and **Enter** to confirm)

::

  ufw allow ssh/tcp
  ufw limit ssh/tcp
  ufw allow 10101/tcp  
  ufw logging on
  ufw enable

(press **Y** and **Enter** to confirm)

::

  fallocate -l 4G /swapfile
  chmod 600 /swapfile
  mkswap /swapfile
  swapon /swapfile
  nano /etc/fstab

Add the following line at the end of the file (press tab to separate
each word/number), then press **Ctrl + X** to close the editor, then
**Y** and **Enter** save the file.

::

  /swapfile none swap sw 0 0

Finally, in order to prevent brute force password hacking attacks, we
will install fail2ban and disable root login over ssh. These steps are
optional, but highly recommended. Start with fail2ban::

  apt install fail2ban

Create a new configuration file::

  nano /etc/fail2ban/jail.local

And paste in the following configuration::

  [sshd]
  enabled = true
  port = 22
  filter = sshd
  logpath = /var/log/auth.log
  maxretry = 3

Then press **Ctrl + X** to close the editor, then **Y** and **Enter**
save the file. Retart and enable the fail2ban service::

  systemctl restart fail2ban
  systemctl enable fail2ban

Next, open the SSH configuration file to disable root login over SSH::

  nano /etc/ssh/sshd_config

Locate the line that reads ``PermitRootLogin yes`` and set it to
``PermitRootLogin no``. Directly below this, add a line which reads
``AllowUsers <username>``, replacing ``<username>`` with the username
you selected above. Then press **Ctrl + X** to close the editor, then
**Y** and **Enter** save the file.

Then reboot the server::

  reboot now

PuTTY will disconnect when the server reboots.

While this setup includes basic steps to protect your server against attacks, much more can be done. However, since the masternode does not actually store the keys to any Historia, these steps are considered beyond the scope of this guide.

Send the collateral
===================

A Historia address with a single unspent transaction output (UTXO) of
exactly 100 HTA is required to operate a Voting Masternode. Once it has been
sent, various keys regarding the transaction must be extracted for later
entry in a configuration file. A masternode can be started from the official Historia Core wallet. This guide will describe the steps for Historia Core.

Option 1: Sending from Historia Core wallet
-------------------------------------------

Open Historia Core wallet and wait for it to synchronize with the network.
It should look like this when ready:

.. figure:: ../img/Picture10.png
   :width: 400px

   Fully synchronized Historia Core wallet

Click **Tools > Debug console** to open the console. Type the following
two commands into the console to generate a legacy masternode key
and a new Historia address for the collateral::

  masternode genkey
  93PAqQsDjcVdYJHRfQPjsSt5338GCswMnUaSxoCD8J6fiLk4NHL

  getnewaddress
  HBvcjyzWmt9x9QJNVDyxezhxSXcWEDEdsS

Take note of the masternode private key and collateral address,
since we will need it later. The next step is to secure your wallet (if
you have not already done so). First, encrypt the wallet by selecting
**Settings > Encrypt wallet**. You should use a strong, new password
that you have never used somewhere else. Take note of your password and
store it somewhere safe or you will be permanently locked out of your
wallet and lose access to your funds. Next, back up your wallet file by
selecting **File > Backup Wallet**. Save the file to a secure location
physically separate to your computer, since this will be the only way
you can access our funds if anything happens to your computer.

Content Distribution Masternode (CDMN) - Collateral 5000
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If setting up a Content Distribution Masternode (CDMN), send exactly 5000 HTA in a single transaction to the new address
you generated in the previous step. This may be sent from another
wallet, or from funds already held in your current wallet. 

Check Transaction
-----------------
Once the transaction is complete, view the transaction in a `blockchain explorer
<http://blockexplorer.historia.network/>`_ by searching for the address. You
will need 15 confirmations before you can start the masternode, but you
can continue with the next step at this point already: installing Historia
Core on your VPS.

.. _masternode-setup-install-historiacore:

Install Historia Core
=====================
You MUST use Historia 0.16.3.1, otherwise this process will fail. https://github.com/HistoriaOffical/historia/releases/tag/0.16.3.1

Historia Core is the software behind both the Historia Core GUI wallet and Historia
masternodes. If not displaying a GUI, it runs as a daemon on your VPS
(historiad), controlled by a simple command interface (historia-cli).

Open PuTTY or a console again and connect using the username and
password you just created for your new, non-root user. 

Option 1: Manual installation
-----------------------------

To manually download and install the components of your Historia masternode, visit https://github.com/HistoriaOffical/historia/releases/tag/0.16.3.1 on your computer to find the link to the latest Historia Core wallet.  Right-click on Download TGZ for Historia Core Linux 64 Bit and select Copy link address. Go back to your terminal window and enter the following command, pasting in the address to the latest version of Historia Core by right clicking or pressing Ctrl + V::

  cd /tmp
  wget https://github.com/HistoriaOffical/historia/releases/download/0.16.3.1/historiacore-0.16.3.1-linux64.tar.gz
  
Create a working directory for Historia, extract the compressed archive and
copy the necessary files to the directory::

  mkdir ~/.historiacore
  tar xfvz historiacore-0.16.3.1-linux64.tar.gz  
  cp historiacore-0.16.3.1/bin/historiad ~/.historiacore/  
  cp historiacore-0.16.3.1/bin/historia-cli ~/.historiacore/  
  chmod 777 ~/.historiacore/historia*  


Clean up unneeded files::

  rm historiacore-0.16.3.1-linux64.tar.gz  
  rm -r historiacore-0.16.3.1/

Create a configuration file using the following command::

  nano ~/.historiacore/historia.conf

An editor window will appear. We now need to create a configuration file
specifying several variables. Copy and paste the following text to get
started, then replace the variables specific to your configuration as
follows::


  #----
  rpcuser=XXXXXXXXXXXXX
  rpcpassword=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
  rpcallowip=127.0.0.1
  #----
  listen=1
  server=1
  daemon=1
  maxconnections=64
  #----
  masternode=1
  masternodecollateral=XXXX
  masternodeprivkey=XXXXXXXXXXXXXXXXXXXXXXX
  externalip=XXX.XXX.XXX.XXX
  #----

Replace the fields marked with ``XXXXXXX`` as follows:

- ``rpcuser``: enter any string of numbers or letters, no special
  characters allowed
- ``rpcpassword``: enter any string of numbers or letters, no special
  characters allowed
- ``masternodecollateral``: 100 or 5000 depending on if you are setting up a Voting Masternode or Content Distribution Masternode. For this guide set this to 5000.
- ``masternodeprivkey``: this is the legacy masternode private key you
  generated in the previous step
- ``externalip``: this is the IPv4 address of your VPS

The result should look something like this:

.. figure:: ../img/Picture12.png
   :width: 400px

   Entering key data in historia.conf on the masternode

Press **Ctrl + X** to close the editor and **Y** and **Enter** save the
file. 

Start Historiad Masternode
--------------------------

You can now start running Historia on the masternode to begin
synchronization with the blockchain::

  ~/.historiacore/historiad

You will see a message reading **Historia Core server starting**. We will
now install Sentinel, a piece of software which operates as a watchdog
to communicate to the network that your node is working properly::

  cd ~/.historiacore
  git clone https://github.com/HistoriaOffical/sentinel.git
  cd sentinel
  virtualenv venv
  venv/bin/pip install -r requirements.txt
  venv/bin/python bin/sentinel.py

You will see a message reading **historiad not synced with network! Awaiting
full sync before running Sentinel.** Add historiad and sentinel to crontab
to make sure it runs every minute to check on your masternode::

  crontab -e

Choose nano as your editor and enter the following lines at the end of
the file::

  * * * * * cd ~/.historiacore/sentinel && ./venv/bin/python bin/sentinel.py 2>&1 >> sentinel-cron.log
  * * * * * pidof historiad || ~/.historiacore/historiad

Press enter to make sure there is a blank line at the end of the file,
then press **Ctrl + X** to close the editor and **Y** and **Enter** save
the file. We now need to wait for 15 confirmations of the collateral
transaction to complete, and wait for the blockchain to finish
synchronizing on the masternode. You can use the following commands to
monitor progress::

  ~/.historiacore/historia-cli mnsync status

When synchronisation is complete, you should see the following
response::

  {
   "AssetID": 999,
   "AssetName": "MASTERNODE_SYNC_FINISHED",
   "Attempt": 0,
   "IsBlockchainSynced": true,
   "IsMasternodeListSynced": true,
   "IsWinnersListSynced": true,
   "IsSynced": true,
   "IsFailed": false
  }

Continue with the next step to start your masternode.

.. _start-masternode:
Start your masternode
---------------------

Depending on how you sent your masternode collateral, you will need to start your masternode with a command sent by the Historia Core wallet. Before you continue, you must ensure that your 100 or 5000 HTA collateral transaction has at least 15 confirmation, and that historiad is running and fully synchronized with the blockchain on your masternode. See the previous step for details on how to do this. During the startup process, your masternode may pass through the following states:

- ``MASTERNODE_SYNC``: This indicates the data currently being synchronised in the masternode
- ``MASTERNODE_SYNC_FAILED``: Synchronisation could not complete, check your firewall and restart historiad
- ``WATCHDOG_EXPIRED``: Waiting for sentinel to restart, make sure it is entered in crontab
- ``NEW_START_REQUIRED``: Start command must be sent from wallet; check IPFS is running.
- ``PRE_ENABLED``: Waiting for network to recognize started masternode
- ``ENABLED``: Masternode successfully started
- ``IPFS_EXPIRED``: This indictates that IPFS is not running.
- ``EXPIRED``: Masternode has expired. Restart Historiad, restart masternode, check IPFS is running.
If you masternode does not seem to start immediately, do not arbitrarily issue more start commands. Each time you do so, you will reset your position in the payment queue.

Identify the funding transaction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you used an address in Historia Core wallet for your collateral
transaction, you now need to find the txid of the transaction. Click
**Tools > Debug console** and enter the following command::

  masternode outputs

This should return a string of characters similar to this::

  {
  "06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb" : "1",
  }

The first long string is your transaction hash, while the last number is the index. We now need to create a file called masternode.conf for this wallet in order to be able to use it to issue the command to start your masternode on the network.

Open a new text file in Notepad (or TextEdit on macOS, nano on Linux) and enter the following information:

   - ``Label``: Any single word used to identify your masternode, e.g. MN1
   - ``IP and port``: The IP address and port (usually 10101) configured in the Historia.conf file, separated by a colon (:)
   - ``Masternode private key``: This is the result of your masternode genkey command earlier, also the same as configured in the Historia.conf file
   - ``Transaction hash``: The txid we just identified using masternode outputs
   - ``Index``: The index we just identified using masternode outputs
   - ``IPv6 Address``: The public IPv6 address required for Content Distribution Masternode. Set this to the IPv6 address of your VPS.
   - ``IPFS Peer ID``: The public IPFS peer id of your IPFS daemon required for Content Distribution Masternode. Set this to your IPFS peer id you get after setting up IPFS. You get this from :ref:`Setup IPFS <ipfs-setup>`.

Content Distribution Masternode - Collateral 5000
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Enter all of this information on a single line with each item separated by a space, for example::

   MN1 52.14.2.67:10101 XrxSr3fXpX3dZcU7CoiFuFWqeHYw83r28btCFfIHqf6zkMp1PZ4 06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb 0 2001:19f0:7001:6de:5400:1ff:fef3:8735 QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD

Save this file in the historiacore data folder on the PC running the Historia Core wallet using the filename masternode.conf. You may need to enable View hidden items to view this folder. Be sure to select All files if using Notepad so you don’t end up with a .conf.txt file extension by mistake. For different operating systems, the Historiacore folder can be found in the following locations (copy and paste the shortcut text into the Save dialog to find it quickly):

+-----------+--------------------------------------------------------+--------------------------------------------+
| Platform  | Path                                                   | Shortcut                                   |
+===========+========================================================+============================================+
| Linux     | /home/yourusername/.historiacore                       | ~/.historiacore                            | 
+-----------+--------------------------------------------------------+--------------------------------------------+
| OSX       | /Macintosh HD/Library/Application Support/HistoriaCore | ~/Library/Application Support/HistoriaCore |
+-----------+--------------------------------------------------------+--------------------------------------------+
| Windows   | C:\Users\yourusername\AppData\Roaming\Historia Core    | %APPDATA%\Historia Core                    |
+-----------+--------------------------------------------------------+--------------------------------------------+

Now close your text editor and also shut down and restart Historia Core wallet. Historia Core will recognize masternode.conf during startup, and is now ready to activate your masternode. Go to Settings > Unlock Wallet and enter your wallet passphrase. Then click Tools > Debug console again and enter the following command to start your masternode (replace MN1 with the label for your masternode)::

    masternode start-alias MN1


At this point you can go back to your terminal window and monitor your masternode by entering ~/.Historiacore/historia-cli masternode status. You will probably need to wait around 30 minutes as the node passes through the PRE_ENABLED stage and finally reaches ENABLED. Give it some time.

At this point you can safely log out of your server by typing exit. Congratulations! Your masternode is now running.

Upgrade Instructions From 0.16.2
================================
For nodes that already are running version 0.16.2 of the Historia masternode, follow the following instructions to upgrade to the newest version of Historia.

Download New Binaries
---------------------
Download latest version of the linux binaries.::

   cd ~  
   wget https://github.com/HistoriaOffical/historia/releases/download/0.16.3.1/historiacore-0.16.3.1-linux64.tar.gz

Stop Daemon
-----------
Stop Historia daemon. You have to do the next few steps quickly, as there is a cronjob that will restart historiad if it's not up. If it restarts during this process, just run ./historia-cli stop again, then copy over the binaries.::

   cd ~/.historiacore  
   ./historia-cli stop

Install New Binaries and Clean Up
---------------------------------
Extract the compressed archive, copy the necessary files to the directory and set them as executable::
   
   tar xfvz historiacore-0.16.3.1-linux64.tar.gz  
   cp historiacore-0.16.3.1/bin/historiad .historiacore/  
   cp historiacore-0.16.3.1/bin/historia-cli .historiacore/  
   chmod 777 .historiacore/historia*


Clean up unneeded files::
   
   rm historiacore-0.16.3.1-linux64.tar.gz  
   rm -r historiacore-0.16.3.1/

Update Sentinel
---------------
You must upgrade to the newest version of Sentinel as well::

   cd ~/.historiacore/sentinel  
   git pull
   
Update Historia.conf
--------------------
Before we can start the Historiad we must update a few settings in historia.conf. 
Open the historia.conf configuration file using the following command::

  nano ~/.historiacore/historia.conf

An editor window will appear. We now need to update the configuration file
to add the new masternodecollateral directive. A sample config file is below::

  #----
  rpcuser=XXXXXXXXXXXXX
  rpcpassword=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
  rpcallowip=127.0.0.1
  #----
  listen=1
  server=1
  daemon=1
  maxconnections=64
  #----
  masternode=1
  masternodecollateral=XXXX
  masternodeprivkey=XXXXXXXXXXXXXXXXXXXXXXX
  externalip=XXX.XXX.XXX.XXX:10101
  #----

Replace the fields marked with ``XXXXXXX`` as follows:

- ``rpcuser``: enter any string of numbers or letters, no special
  characters allowed
- ``rpcpassword``: enter any string of numbers or letters, no special
  characters allowed
- ``masternodecollateral``: 100 or 5000 depending on if you are setting up a Voting Masternode or Content Distribution Masternode. For this guide set this to 5000.
- ``masternodeprivkey``: this is the legacy masternode private key you
  generated in the previous step
- ``externalip``: this is the IPv4 address of your VPS

The result should look something like this:

.. figure:: ../img/Picture12.png
   :width: 400px

   Entering key data in historia.conf on the masternode

Press **Ctrl + X** to close the editor and **Y** and **Enter** save the
file. 

Start Historia Masternode
-------------------------

You can now start running Historia on the masternode to begin synchronization with the blockchain::
  
  ~/.historiacore/historiad

Start your masternode
^^^^^^^^^^^^^^^^^^^^^

Check that masternode is in sync::

   ~/.historiacore/historia-cli mnsync status

When synchronisation is complete, you should see the following response::

   {  
      "AssetID": 999,  
      "AssetName": "MASTERNODE_SYNC_FINISHED",  
      "Attempt": 0,  
      "IsBlockchainSynced": true,  
      "IsMasternodeListSynced": true,  
      "IsWinnersListSynced": true,  
      "IsSynced": true,  
      "IsFailed": false  
   }  

Once masternode is in sync, restart masternode::

   masternode start-alias MN1

Check that you are on correct version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Check that version number::

   ~/.historiacore/historia-cli getinfo

Version should be set to 160301

Protocol should be to 70212::

   {  
      "version": 160301,  
      "protocolversion": 70212,  
      "walletversion": 61000,  
      "balance": 0.00000000,  
      "privatesend_balance": 0.00000000,  
      "blocks": 25900,  
      "timeoffset": 0,  
      "connections": 5,  
      "proxy": "",  
      "difficulty": 0.0007275013747428129,  
      "testnet": false,  
      "keypoololdest": 1540240263,  
      "keypoolsize": 1000,  
      "paytxfee": 0.00000000,  
      "relayfee": 0.00001000,  
      "errors": ""  
   }
   
Update masternode.conf
----------------------
Next, open the masternode.conf text file that you previously created, in Notepad (or TextEdit on macOS, nano on Linux). We have to update the masternode.conf file to use the new masternode parameters:

   - ``Label``: Any single word used to identify your masternode, e.g. MN1
   - ``IP and port``: The IP address and port (usually 10101) configured in the Historia.conf file, separated by a colon (:)
   - ``Masternode private key``: This is the result of your masternode genkey command earlier, also the same as configured in the Historia.conf file
   - ``Transaction hash``: The txid we just identified using masternode outputs
   - ``Index``: The index we just identified using masternode outputs
   - ``IPv6 Address``: The public IPv6 address required for Content Distribution Masternode. Set this to the IPv6 address of your VPS.
   - ``IPFS Peer ID``: The public IPFS peer id of your IPFS daemon required for Content Distribution Masternode. Set this to you IPFS peer id you get after setting up IPFS. You get this from :ref:`Setup IPFS <ipfs-setup>`.

Content Distribution Masternode - Collateral 5000
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Enter all of this information on a single line with each item separated by a space, for example::

   MN1 52.14.2.67:10101 XrxSr3fXpX3dZcU7CoiFuFWqeHYw83r28btCFfIHqf6zkMp1PZ4 06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb 0 2001:19f0:7001:6de:5400:1ff:fef3:8735 QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD
Save this file in the historiacore data folder on the PC running the Historia Core wallet using the filename masternode.conf. You may need to enable View hidden items to view this folder. Be sure to select All files if using Notepad so you don’t end up with a .conf.txt file extension by mistake. For different operating systems, the Historiacore folder can be found in the following locations (copy and paste the shortcut text into the Save dialog to find it quickly):

+-----------+--------------------------------------------------------+--------------------------------------------+
| Platform  | Path                                                   | Shortcut                                   |
+===========+========================================================+============================================+
| Linux     | /home/yourusername/.historiacore                       | ~/.historiacore                            | 
+-----------+--------------------------------------------------------+--------------------------------------------+
| OSX       | /Macintosh HD/Library/Application Support/HistoriaCore | ~/Library/Application Support/HistoriaCore |
+-----------+--------------------------------------------------------+--------------------------------------------+
| Windows   | C:\Users\yourusername\AppData\Roaming\Historia Core    | %APPDATA%\Historia Core                    |
+-----------+--------------------------------------------------------+--------------------------------------------+

Now close your text editor and also shut down and restart Historia Core wallet. Historia Core will recognize masternode.conf during startup, and is now ready to activate your masternode. Go to Settings > Unlock Wallet and enter your wallet passphrase. Then click Tools > Debug console again and enter the following command to start your masternode (replace MN1 with the label for your masternode)::

    masternode start-alias mn1

At this point you can go back to your terminal window and monitor your masternode by entering ~/.Historiacore/historia-cli masternode status. You will probably need to wait around 30 minutes as the node passes through the PRE_ENABLED stage and finally reaches ENABLED. Give it some time.

Your masternode is now running. 
