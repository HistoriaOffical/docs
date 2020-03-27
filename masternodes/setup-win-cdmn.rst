.. meta::
   :description: This guide describes how to set up a Historia masternode. It also describes various options for hosting and different wallets
   :keywords: historia, guide, masternodes, setup,

.. _masternode-setup:

=====================================================================
Setup For Windows 
=====================================================================


*** This documentation is out of date. We are currently working on an update. ***

Setting up a masternode requires a basic understanding of Windows and blockchain technology, as well as an ability to follow instructions closely. It also requires regular maintenance and careful security. There are some decisions to be made along the way, and optional extra steps to take for increased security.

Before you begin
================

This guide assumes you are setting up a single masternode for the first
time. You will need:

- 5000 HTA (Content Distribution Masternode)
- A wallet to store your Historia, preferably a hardware wallet, although Historia Core wallet is also supported
- A Windows 10 instance.
- Since we are assuming this is a home network, ports need to be publicly open on the Internet. This normally requires Port Forwarding on your router, which is out of scope for this document. Use your favorite search engine to research Port Forwarding.
   - Content Distribution Masternode: 
         - TCP Port 10101
         - TCP Port 8080
         - TCP Port 4001
   - When asked to allow firewall access by Windows on any of the steps below, please select Allow.

Masternode Roles
----------------

Unlike most other masternode coins, Historia makes use of a role based masternode system. Currently there are two roles:
 - Content Distribution Masternode 
 
   - Collateral Requirement: 5000 HTA
   - Reward: 25% per block - increaes 2.5% every 2 months until 50% per block
   - Ports: TCP 10101, TCP 4001, TCP 8080
   - IPFS Required: Yes
   - IPv4 address required

In this guide, we will setup a Content Distribution Masternode (CDMN) with collateral of 5000. 
 
Install Historia Windows Wallet
===============================
You MUST use Historia 0.16.3.2 or later, otherwise this process will fail. https://github.com/HistoriaOffical/historia/releases/

Download the correct Windows Historia setup file from the previous URL. Once downloaded, run the Historia installer and install the Historia wallet. Open the wallet and let the blockchain sync completely.


Send the collateral
===================

A Historia address with a single unspent transaction output (UTXO) of
exactly 5000 HTA is required to operate a masternode depending on the masternode role you choose. Once it has been
sent, various keys regarding the transaction must be extracted for later
entry in a configuration file. A masternode can be started from the official Historia Core wallet. This guide will describe the steps for Historia Core.

Option 1: Sending from Historia Core wallet
-------------------------------------------

Open Historia Core wallet and wait for it to synchronize with the network.
It should look like this when ready:

.. figure:: /img/Picture10.png
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

Next, setup the historia.conf files by selecting Tools > Open Wallet Configuration File.

A text editor window will appear. We now need to create a configuration file specifying several variables. Copy and paste the following text into the Wallet Configuration file, then replace the variables specific to your configuration as follows::


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

Save the historia.conf file in the default location and exit the text editor.::

   C:\Users\<yourusername>\AppData\Roaming\HistoriaCore\ 

Install Sentinel
================

Download and install Sentinel for Windows
https://github.com/HistoriaOffical/sentinel/releases

Open command prompt

Create new sentinel directory in your HistoraCore directory::

   mkdir cd C:\Users\<yourusername>\AppData\Roaming\HistoriaCore\sentinel

And copy sentinel.exe to the newly created sentinel directory::

   cd C:\Users\<yourusername>\AppData\Roaming\HistoriaCore\sentinel\sentinel.exe

Create new file in the sentinel directory named sentinel.conf::

   cd C:\Users\<yourusername>\AppData\Roaming\HistoriaCore\sentinel\

Edit file and paste the following into the sentinel.conf file::
 
   network=mainnet  
   db_name=database/sentinel.db  
   db_driver=sqlite


Setup Task for Sentinel
-----------------------

Run Task Scheduler  

Create Task -> General Tab - Name: Sentinal

.. figure:: ../img/1.PNG
   :width: 400px

Settings:

   - Trigger Tab -> New (Trigger)  
   - Settings -> Repeat Daily  
   - Recur Every: 1 day  
   - Advanced Settings:  
   - Repeat Task Every: 1 Minute (Notice you have to select 5 minutes from the drop down, then edit the 5 to 1)  
For a duration of:Indefinitely  

.. figure:: ../img/2.PNG
   :width: 400px

Settings:

   - Actions Tab -> New (Action)  
   - Program/script -> Browse to::
   
      C:\Users\<yourusername>\AppData\Roaming\HistoriaCore\sentinel\sentinel.exe  

Click Ok  

.. figure:: ../img/3.PNG
   :width: 400px

Settings:

   - Conditions Tab -> Power  
   - Uncheck box for "Start task only if the computer is on AC Power"  

Click Ok  

.. figure:: ../img/4.PNG
   :width: 400px


.. _start-masternode:
Start your masternode
---------------------

Depending on how you sent your masternode collateral, you will need to start your masternode with a command sent by the Historia Core wallet. Before you continue, you must ensure that your 100 HTA collateral transaction has at least 15 confirmation, and that historiad is running and fully synchronized with the blockchain on your masternode. See the previous step for details on how to do this. During the startup process, your masternode may pass through the following states:

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

The first long string is your transaction hash, while the last number is the index. We now need open Tool -> Open Masternode Configure file for this wallet in order to be able to use it to issue the command to start your masternode on the network. 

- ``Label``: Any single word used to identify your masternode, e.g. MN1
- ``IP and port``: The IP address and port (usually 10101) configured in the Historia.conf file, separated by a colon (:)
- ``Masternode private key``: This is the result of your masternode genkey command earlier, also the same as configured in the Historia.conf file
- ``Transaction hash``: The txid we just identified using masternode outputs
- ``Index``: The index we just identified using masternode outputs
- ``IPv6 Address``: The public IPv6 address required for Content Distribution Masternode. Set this to the IPv6 address of your VPS.
- ``IPFS Peer ID``: The public IPFS peer id of your IPFS daemon required for Content Distribution Masternode. Set this to you IPFS peer id you get after setting up IPFS. You get this from :ref:`Setup IPFS <ipfs-setup>`.

Content Distribution Masternode - Collateral 5000
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If Content Distribution Masternode, enter all of this information on a single line with each item separated by a space, for example::

   MN1 52.14.2.67:10101 XrxSr3fXpX3dZcU7CoiFuFWqeHYw83r28btCFfIHqf6zkMp1PZ4 06e38868bb8f9958e34d5155437d009b72dff33fc28874c87fd42e51c0f74fdb 0 2001:19f0:7001:6de:5400:1ff:fef3:8735 QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD

Save this file and close the text editor. It should be saved in:: 

   C:\Users\<yourusername>\AppData\Roaming\HistoriaCore folder.

Shut down and restart Historia Core wallet. Let the Historia Core wallet fully sync. Historia Core will recognize masternode.conf during startup, and is now ready to activate your masternode. Go to **Settings > Unlock Wallet** and enter your wallet passphrase. Then click **Tools > Debug** console again and enter the following command to start your masternode (replace MN1 with the label for your masternode)::

   masternode start-alias MN1


At this point you can go back to your **Tools > Debug** window and monitor your masternode by entering:: 

   masternode status 

You will probably need to wait around 30 minutes as the node passes through the PRE_ENABLED stage and finally reaches ENABLED. Give it some time.
At this point you can safely log out of your server by typing exit. Congratulations! Your masternode is now running.

