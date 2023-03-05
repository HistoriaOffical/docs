.. meta::
   :description: This guide describes how to set up a Historia masternode. It also describes various options for hosting and different wallets
   :keywords: historia, guide, masternodes, setup,

.. _masternode-setup:

===================================================================
Setup For Linux 
===================================================================

Setting up a masternode requires a basic understanding of Linux and blockchain technology, as well as an ability to follow instructions closely. It also requires regular maintenance and careful security. There are some decisions to be made along the way, and optional extra steps to take for increased security.

Before you begin
================

This guide assumes you are setting up a single masternode for the first
time. You will need:

- 5000 HTA.
- A wallet to store your Historia, currently only Historia Core wallet is supported.
- A Linux server, preferably a Virtual Private Server (VPS).
- IPv4 address
- Your own DNS name


We also assume you will be working from a Windows computer. However, since most of the work is done on your Linux VPS, alternative steps for using macOS or Linux will be indicated where necessary.

Masternode Info
---------------

- Collateral Requirement: 5000 HTA
- Reward: 50% per block - increaes 2.5% every 2 months until 50% per block
- Ports: TCP 10101, TCP 4001, TCP 443, TCP 80
- IPFS Required: Yes
- IPv4 address required
- DNS name

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

Select Ubuntu 18.04 x64 as the server type. We use this LTS release of
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

Add IPv6 for your server. IPv6 isn't required but nice to have.

.. figure:: ../img/6.PNG
   :width: 400px

   Vultr IPv6 Address screen

Vultr will now install your server. This process may take a few minutes.

.. figure:: ../img/Picture5.png
   :width: 400px

   Vultr server installation screen

Click **Manage** when installation is complete and take note of the IPv4
address, username and password.

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
  ufw allow 443/tcp
  ufw allow 80/tcp  
  ufw allow 4001/tcp  
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

Setup Domain Name System (DNS) A Record
=======================================

Historia requires a DNS name set to enabled SSL for your IPFS node that will be setup below. This is beyond the scope of this document, but there is plenty of documentation online on how to do this. Find a cheap DNS registrar and create a A record that points to the IP address of your VPS. Namecheap.com or GoDaddy.com are options for this. This can be any top level domain, such as .xyz ($0.88 annually) or .fun ($1.00 annually), so get this cheapest domain you can get. 

Remember that if you live in a oppressive country, your name will be associated with your DNS record in the global WHOIS database. Some DNS providers such as ionos.com will give a DNS name privacy for free with domain registration. But they would still be required to hand over your domain name information via court order. Another option is using one of the new blockchain DNS systems such as unstoppabledomains.com, and using crypto currency to purchase your domain name. However we have not tested using a blockchain DNS system yet.

Send the collateral from Historia Core Desktop Wallet
=====================================================

A Historia address with a single unspent transaction output (UTXO) of
exactly 5000 HTA is required to operate a Voting Masternode. Once it has been
sent, various keys regarding the transaction must be extracted for later
entry in a configuration file. A masternode can be started from the official 
wallet. This guide will describe the steps for Historia Core.

Option 1: Sending from Historia Core Desktop Wallet
---------------------------------------------------

Open Historia Core wallet and wait for it to synchronize with the network.
It should look like this when ready:

.. figure:: ../img/Picture10.png
   :width: 400px

   Fully synchronized Historia Core wallet

Click **Tools > Debug console** to open the console. Type the following
two commands into the console to generate a new Historia address for the collateral::

  getnewaddress
  HBm4FXgZXdb4NYqx1DJ3h9v9reFqYe9F6L

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
If setting up a Content Distribution Masternode (CDMN), send exactly 5000 HTA in a single transaction to the new address you generated in the previous step. This may be sent from another
wallet, or from funds already held in your current wallet.::

   sendtoaddress <YourPreviouslyGeneratedAddressHere> 5000

Check Transaction
-----------------
Once the transaction is complete, view the transaction in a `blockchain explorer
<http://blockexplorer.historia.network/>`_ by searching for the address. You
will need 15 confirmations before you can start the masternode, but you
can continue with the next step at this point already: installing Historia
Core on your VPS.

.. _masternode-setup-install-historiacore:

Install Historia Core on Ubuntu VPS
===================================
You MUST use Historia v0.17.0.4 or later, otherwise this process will fail. https://github.com/HistoriaOffical/historia/releases/

Historia Core is the software behind both the Historia Core GUI wallet and Historia
masternodes. If not displaying a GUI, it runs as a daemon on your VPS
(historiad), controlled by a simple command interface (historia-cli).

Open PuTTY or a console again and connect using the username and
password you just created for your new, non-root user. 

Option 1: Manual installation of Historia Core on Ubuntu VPS
------------------------------------------------------------

To manually download and install the components of your Historia masternode, visit https://github.com/HistoriaOffical/historia/releases/ on your computer to find the link to the latest Historia Core wallet.  Right-click on Download TGZ for Historia Core Linux 64 Bit and select Copy link address. Go back to your terminal window and enter the following command, pasting in the address to the latest version of Historia Core by right clicking or pressing Ctrl + V::

  cd /tmp
  wget https://github.com/HistoriaOffical/historia/releases/download/0.17.0.4/historiacore-0.17.0.4-x86_64-linux-gnu.tar.gz
  
Create a working directory for Historia, extract the compressed archive and
copy the necessary files to the directory::

  mkdir ~/.historiacore
  tar xfvz historiacore-0.17.0.4-x86_64-linux-gnu.tar.gz
  cp historiacore-0.17.0/bin/historiad ~/.historiacore/  
  cp historiacore-0.17.0/bin/historia-cli ~/.historiacore/  
  chmod 777 ~/.historiacore/historia*  


Clean up unneeded files::

  rm historiacore-0.17.0.4-x86_64-linux-gnu.tar.gz
  rm -r historiacore-0.17.0/

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
   #----
   #masternode=1
   #masternodeblsprivkey=
   #masternodecollateral=5000
   externalip=XXX.XXX.XXX.XXX:10101
   #----

Replace the fields marked with ``XXXXXXX`` as follows:

- ``rpcuser``: enter any string of numbers or letters, no special
  characters allowed
- ``rpcpassword``: enter any string of numbers or letters, no special
  characters allowed
- ``externalip``: this is the IPv4 address of your VPS

Leave the masternode and masternodeblsprivkey fields commented out for now. The result should look something like this:

.. figure:: ../img/Picture12.png
   :width: 400px

   Entering key data in historia.conf on the masternode

Press **Ctrl + X** to close the editor and **Y** and **Enter** save the
file. 

Start Historiad Masternode on Ubuntu VPS
----------------------------------------

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

Continue with the next step to install IPFS required by your masternode. Running the IPFS daemon is now a required part of the masternode system. You must follow these steps. 

Setup IPFS on Ubuntu VPS
========================

Download / Install IPFS Daemon on Ubuntu VPS
--------------------------------------------

To run the IPFS Daemon you must install the Go Lang::

   sudo apt-get update
   sudo apt-get install golang-go -y

Next download and install IPFS daemon. Because we have used Ubuntu 18.04 64-bit for our OS, there isn't a deb package for this version of Ubuntu::

   wget https://dist.ipfs.io/go-ipfs/v0.4.23/go-ipfs_v0.4.23_linux-amd64.tar.gz
   tar xvfz go-ipfs_v0.4.23_linux-amd64.tar.gz
   sudo mv go-ipfs/ipfs /usr/local/bin/ipfs

Clean up::

   rm -rf go-ipfs/
   
Initialize IPFS Daemon for Historia on Ubuntu VPS
-------------------------------------------------
Since we will be using IPFS only for Historia, we can safely run the initialization::
   
   ipfs init -p server
   
Remove Original Bootstap IPFS Nodes and Connect to Historia IPFS Swarm on Ubuntu VPS
------------------------------------------------------------------------------------
Add Historia IPFS bootstrap nodes, configure our IPFS node, and only connect to the Historia IPFS Swarm::

   ipfs bootstrap add /ip4/202.182.119.4/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD
   ipfs bootstrap add /ip4/149.28.22.65/tcp/4001/ipfs/QmZkRv4qfXvtHot37STR8rJxKg5cDKFnkF5EMh2oP6iBVU
   ipfs bootstrap add /ip4/149.28.247.81/tcp/4001/ipfs/QmcvrQ8LpuMqtjktwXRb7Mm6JMCqVdGz6K7VyQynvWRopH
   ipfs bootstrap add /ip4/45.32.194.49/tcp/4001/ipfs/QmZXbb5gRMrpBVe79d8hxPjMFJYDDo9kxFZvdb7b2UYamj
   ipfs bootstrap add /ip4/45.76.236.45/tcp/4001/ipfs/QmeW8VxxZjhZnjvZmyBqk7TkRxrRgm6aJ1r7JQ51ownAwy
   ipfs bootstrap add /ip4/209.250.233.69/tcp/4001/ipfs/Qma946d7VCm8v2ny5S2wE7sMFKg9ZqBXkkZbZVVxjJViyu
   
   ipfs config --json Datastore.StorageMax '"50GB"'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Headers '["X-Requested-With", "Access-Control-Expose-Headers", "Range", "Authorization"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Methods '["POST", "GET"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Expose-Headers '["Location", "Ipfs-Hash"]'
   ipfs config --json Gateway.HTTPHeaders.X-Special-Header '["Access-Control-Expose-Headers: Ipfs-Hash"]'
   ipfs config --json Gateway.NoFetch 'false'
   ipfs config --json Swarm.ConnMgr.HighWater '500'
   ipfs config --json Swarm.ConnMgr.LowWater '200'
   
Now when you start IPFS, the IPFS daemon will now connect to the Historia IPFS swarm when started.

Create IPFS Service To Restart on Reboot or Crash on Ubuntu VPS
---------------------------------------------------------------
Next, create a service for IPFS to restart on reboot or crash. Create a new service file::
   
   sudo nano  /etc/systemd/system/ipfs.service

Copy and past the below config and save the ipfs.service file. Add the username that Historia runs under to "User=". Most likely this is the user that you have created when setting up the OS.

.. parsed-literal::


   [Unit]
   Description=ipfs.service
   After=network.target
  
   [Service]
   Type=simple
   Restart=always
   RestartSec=1
   StartLimitInterval=0
   User=<YOURUSERNAME>
   ExecStart=/usr/local/bin/ipfs daemon
   
   [Install]
   WantedBy=multi-user.target
      

Start IPFS Daemon for Historia on Ubuntu VPS
--------------------------------------------
Start the IPFS service::

   systemctl start ipfs
   
Enable the IPFS service to start on reboot::

   systemctl enable ipfs

Check the IPFS service is running::

   systemctl status ipfs

Get IPFS Peer ID on Ubuntu VPS
------------------------------
Historia needs the IPFS ID generated by the IPFS initialization command in masternode registration command below. Run this command and save the ID value for later::

   ipfs id

Result::
 
   {
      **"ID": "QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",** // THIS IS YOUR IPFS PEER ID, HISTORIA WILL NEED THIS
      "PublicKey": "CAASpgIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDGKc55NxrimIWjWIFK6J9Kgj0caCwzGbNCZ4xphSww4j3gsPe1puLhkQHoQpvB7BeDXMdsuIFEfknBjHsZTxRM66X/ZhODyv+wwuQs92FJ2Lb6n/HB/fqsjvkPYQeSNe+T1Djjc2OYzuZkTZwCNrY9hGUEbEq6O1DeqMHWRN1Gy0fu31QyL6mjVq804udm0sQlO3Cey8hmChTBH+GCw1sTNlUlEQy88FPMSjq6j/qGfHRO1bA/trYLTsjIEMLI+xi/HtVzrOg6n+/kQopjWLCGy19IXn/ZVzOZuJhpqBYAkVnUd1b9na5ND/3iN5VTdO6biK+NQ8hH/DEi4sb8wMqpAgMBAAE=",
      "Addresses": [
         "/ip4/127.0.0.1/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
         "/ip4/<youripv4address>/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
      ],
      "AgentVersion": "go-ipfs/0.4.21/8ca278f45",
      "ProtocolVersion": "ipfs/0.1.0"
   }
   
Check IPFS is connected to Historia Swarm on Ubuntu VPS
-------------------------------------------------------
To verify that IPFS is connect to the correct swarm::

   ipfs swarm peers
   
Output::

   /ip4/149.28.22.65/tcp/4001/ipfs/QmZkRv4qfXvtHot37STR8rJxKg5cDKFnkF5EMh2oP6iBVU
   /ip4/149.28.247.81/tcp/4001/ipfs/QmcvrQ8LpuMqtjktwXRb7Mm6JMCqVdGz6K7VyQynvWRopH
   /ip4/202.182.119.4/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD
   /ip4/45.32.194.49/tcp/4001/ipfs/QmZXbb5gRMrpBVe79d8hxPjMFJYDDo9kxFZvdb7b2UYamj
   /ip4/45.76.236.45/tcp/4001/ipfs/QmeW8VxxZjhZnjvZmyBqk7TkRxrRgm6aJ1r7JQ51ownAwy

You will see at least these peers and many more.

Nginx Web Proxy on Ubuntu VPS
-----------------------------

After setting up IPFS, Nginx proxy and a DNS entry is needed to be setup::
   
   sudo apt-get install nginx  
 
Go to the ip address of your VPS in a web browser to verify that Nginix is running.


Install SSL Certificate on Ubuntu VPS
-------------------------------------
In this example we will be using the free SSL certificate service Let's Encrypt to create and install our SSL certificate. First we must install the Let's Encrypt Certbot. Note YOU MUST keep TCP 80 open to get and maintain a valid SSL certificate::

   sudo snap install core; sudo snap refresh core
   sudo snap install --classic certbot
   sudo ln -s /snap/bin/certbot /usr/bin/certbot

Next we need to prepare Nginx configuration file for Let's Encrypt Certbot. If you're using the default configuration file /etc/nginx/sites-available/default open it with a text editor such as nano and find the server_name directive. Replace the underscore, _, with your own domain name(s)::

   sudo nano /etc/nginx/sites-available/default
   
After editing the configuration file, the server_name directive should look as follows. In this example, we assume that your domain is example.com and that you're requesting a certificate for example.com. Make sure to use your own domain name here::

   server_name example.com;

Save the file and restart Nginx::

   systemctl restart nginx
   
The following command will obtain a certificate for you. Edit your Nginx configuration to use it, and reload Nginx.::

   sudo certbot --nginx
   
If the setup process has gone correctly, you can now go to your domain name in a browser and it will be protected by an SSL certification. However we are not done yet.

Lets finish this process and setup Nginix to point to the IPFS daemon that is running on your masternode. If you're using the default configuration file /etc/nginx/sites-available/default open it with a text editor such as nano again.::

   sudo nano /etc/nginx/sites-available/default
   
Change your nginx configuration file to look something like this::

   server {
   root /var/www/html;
   server_name example.com; #Your domain name should already be set here
   
   #BEGIN IPFS SETTINGS#
   location / {
      proxy_pass http://127.0.0.1:8080;
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
      proxy_set_header X-Forwarded-For $remote_addr;
      allow all;
    }
    #END IPFS SETTINGS#

   listen [::]:443 ssl ipv6only=on; # managed by Certbot
   listen 443 ssl; # managed by Certbot
   ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
   ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
   include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
   ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

   }
   server {
      if ($host = exmaple.com) {
         return 301 https://$host$request_uri;
      } # managed by Certbot
      
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name example.com;
    return 404; # managed by Certbot
   }

Save the file and restart Nginx::

   systemctl restart nginx
   
Congratulations! You now have finished setup for IPFS. You can now test out the IPFS Nginx proxy combination by opening the following in your browser:: 

   https://<yourdomainname>/ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv/readme

If you see the IPFS help message, you have successful setup your IPFS Nginx proxy. You can now proceed to installing your Historia masternode.

Continue with the next step to construct the ProTx transaction required to enable your masternode. 

.. _register-masternode:

Register your masternode
========================

DIP003 introduced several changes to how a masternode is set up and
operated. These changes and the three keys required for the different
masternode roles are described briefly under :ref:`dip3-changes` in this
documentation.

Option 1: Registering from Historia Core Desktop Wallet
-------------------------------------------------------

Identify the funding transaction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you used an address in Historia Core wallet for your collateral
transaction, you now need to find the txid of the transaction. Click
**Tools > Debug console** and enter the following command::

  masternode outputs

This should return a string of characters similar to the following::

  {
  "16347a28f4e5edf39f4dceac60e2327931a25fdee1fb4b94b63eeacf0d5879e3" : "1",
  }

The first long string is your ``collateralHash``, while the last number
is the ``collateralIndex``. 


.. _bls-generation:

Generate a BLS key pair on Historia Core Desktop Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A public/private BLS key pair is required to operate a masternode. The
private key is specified on the masternode itself, and allows it to be
included in the deterministic masternode list once a provider
registration transaction with the corresponding public key has been
created.

If you are using a hosting service, they may provide you with their
public key, and you can skip this step. If you are hosting your own
masternode or have agreed to provide your host with the BLS private key,
generate a BLS public/private keypair in Historia Core by clicking **Tools >
Debug console** and entering the following command::

  bls generate

  {
    "secret": "395555d67d884364f9e37e7e1b29536519b74af2e5ff7b62122e62c2fffab35e",
    "public": "99f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670e"
  }

**These keys are NOT stored by the wallet and must be kept secure,
similar to the value provided in the past by the** ``masternode genkey``
**command.**

Add the private key to your masternode configuration on Ubuntu VPS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The public key will be used in following steps. The private key must be
entered in the ``historia.conf`` file on the masternode. This allows the
masternode to watch the blockchain for relevant Pro*Tx transactions, and
will cause it to start serving as a masternode when the signed ProRegTx
is broadcast by the owner (final step below). Log in to your masternode
using ``ssh`` or PuTTY and edit the configuration file as follows::

  nano ~/.historiacore/historia.conf

The editor appears with the existing masternode configuration. Add or
uncomment these lines in the file, replacing the key with your BLS
private key generated above::

  masternode=1
  masternodecollateral=5000
  masternodeblsprivkey=395555d67d884364f9e37e7e1b29536519b74af2e5ff7b62122e62c2fffab35e

Press enter to make sure there is a blank line at the end of the file,
then press **Ctrl + X** to close the editor and **Y** and **Enter** save
the file. We now need to restart the masternode for this change to take
effect. Enter the following commands, waiting a few seconds in between
to give Historia Core time to shut down::

  ~/.historiacore/historia-cli stop
  sleep 15
  ~/.historiacore/historiad

We will now prepare the transaction used to register the masternode on
the network.

Prepare a ProRegTx transaction on Historia Core Desktop Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A pair of BLS keys for the operator were already generated above, and
the private key was entered on the masternode. The public key is used in
this transaction as the ``operatorPubKey``.

First, we need to get a new, unused address from the wallet to serve as
the **owner key address** (``ownerKeyAddr``). This is not the same as
the collateral address holding 5000 Historia. Generate a new address as
follows::

  getnewaddress

  HTGfMbCy2X65th3L78JVyqpzhu6p1fbSC6

This address can also be used as the **voting key address**
(``votingKeyAddr``). Alternatively, you can specify an address provided
to you by your chosen voting delegate, or simply generate a new voting
key address as follows::

  getnewaddress

  HDsy8GUnsdFKWrRHB8WbD4oaLvETDZ9scY

Then either generate or choose an existing address to receive the
**owner's masternode payouts** (``payoutAddress``). It is also possible
to use an address external to the wallet::

  getnewaddress

  HEAjS5DJ9cjprZvk3t1eeq7jn2dhZztfDJ

You can also optionally generate and fund another address as the
**transaction fee source** (``feeSourceAddress``). If you selected an
external payout address, you must specify a fee source address. Either
the payout address or fee source address must have enough balance to pay
the transaction fee, so send a few coins here, or the final ``register_submit`` transaction will
fail.::

   getnewaddress
   
   HQyqm7srzV7nYhGLjuzTzjBs452suStCQW

Send a few coins to the fee source address.::

   sendtoaddress HQyqm7srzV7nYhGLjuzTzjBs452suStCQW 5

The private keys to the owner and fee source addresses must exist in the
wallet submitting the transaction to the network. If your wallet is
protected by a password, it must now be unlocked to perform the
following commands. Unlock your wallet for 5 minutes::

  walletpassphrase yourSecretPassword 300

We will now prepare an unsigned ProRegTx special transaction using the
``protx 
`` command. This command has the following
syntax::

  protx register_prepare collateralHash collateralIndex ipAndPort ownerKeyAddr operatorPubKey votingKeyAddr operatorReward payoutAddress ipfsPeerId identity feeSourceAddress

Open a text editor such as notepad to prepare this command. Replace each
argument to the command as follows:

- ``collateralHash``: The txid of the 5000 Historia collateral funding 
  transaction
- ``collateralIndex``: The output index of the 5000 Historia funding 
  transaction
- ``ipAndPort``: Masternode IP address and port, in the format 
  ``x.x.x.x:yyyy``
- ``ownerKeyAddr``: The new Historia address generated above for the 
  owner/voting address
- ``operatorPubKey``: The BLS public key generated above (or provided 
  by your hosting service)
- ``votingKeyAddr``: The new Historia address generated above, or the 
  address of a delegate, used for proposal voting
- ``operatorReward``: The percentage of the block reward allocated to 
  the operator as payment
- ``payoutAddress``: A new or existing Historia address to receive the 
  owner's masternode rewards
- ``ipfsPeerId``: The public IPFS ID of your IPFS daemon required from the above IPFS setup.
- ``identity``: This is the domain name that you previously have registered and tested above 	https://<yourdomainname>/ipfs/QmS4ustL54uo8FzR9455qaxZwuMiUhyvMcX9Ba8nUH4uVv/readme
- ``feeSourceAddress``: An (optional) address used to fund ProTx fee. 
  ``payoutAddress`` will be used if not specified.

Example (remove line breaks if copying)::

  protx register_prepare 
    16347a28f4e5edf39f4dceac60e2327931a25fdee1fb4b94b63eeacf0d5879e3 
    1 
    45.76.230.239:10101 
    HTGfMbCy2X65th3L78JVyqpzhu6p1fbSC6 
    99f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670e 
    HDsy8GUnsdFKWrRHB8WbD4oaLvETDZ9scY 
    0 
    HEAjS5DJ9cjprZvk3t1eeq7jn2dhZztfDJ
    QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD
    masternode1.historia.network
    HQyqm7srzV7nYhGLjuzTzjBs452suStCQW

After you have created the protx register_prepare command copy and paste this into the Historia Core Desktop Wallet debug dialog box that you have been working on from your desktop.

Output::

  {
    "tx": "030001000175c9d23c2710798ef0788e6a4d609460586a20e91a15f2097f56fc6e007c4f8e0000000000feffffff01a1949800000000001976a91434b09363474b14d02739a327fe76e6ea12deecad88ac00000000d1010000000000e379580dcfea3eb6944bfbe1de5fa2317932e260acce4d9ff3ede5f4287a34160100000000000000000000000000ffff2d4ce6ef4e1fd47babdb9092489c82426623299dde76b9c72d9799f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670ed1935246865dce1accce6c8691c8466bd67ebf1200001976a914fef33f56f709ba6b08d073932f925afedaa3700488acfdb281e134504145b5f8c7bd7b47fd241f3b7ea1f97ebf382249f601a0187f5300",
    "collateralAddress": "HBm4FXgZXdb4NYqx1DJ3h9v9reFqYe9F6L",
    "signMessage": "HEAjS5DJ9cjprZvk3t1eeq7jn2dhZztfDJ|0|HTGfMbCy2X65th3L78JVyqpzhu6p1fbSC6|HDsy8GUnsdFKWrRHB8WbD4oaLvETDZ9scY|ad5f82257bd00a5a1cb5da1a44a6eb8899cf096d3748d68b8ea6d6b10046a28e"
  }

Next we will use the ``collateralAddress`` and ``signMessage`` fields to
sign the transaction, and the output of the ``tx`` field to submit the
transaction.

Sign the ProRegTx transaction on Historia Core Desktop Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We will now sign the content of the ``signMessage`` field using the
private key for the collateral address as specified in
``collateralAddress``. Note that no internet connection is required for
this step, meaning that the wallet can remain disconnected from the
internet in cold storage to sign the message. In this example we will
again use Historia Core, but it is equally possible to use the signing
function of a hardware wallet. The command takes the following syntax::

  signmessage collateralAddress signMessage

Example::

  signmessage HBm4FXgZXdb4NYqx1DJ3h9v9reFqYe9F6L "HEAjS5DJ9cjprZvk3t1eeq7jn2dhZztfDJ|0|HTGfMbCy2X65th3L78JVyqpzhu6p1fbSC6|HDsy8GUnsdFKWrRHB8WbD4oaLvETDZ9scY|ad5f82257bd00a5a1cb5da1a44a6eb8899cf096d3748d68b8ea6d6b10046a28e"

Output::

  II8JvEBMj6I3Ws8wqxh0bXVds6Ny+7h5HAQhqmd5r/0lWBCpsxMJHJT3KBcZ23oUZtsa6gjgISf+a8GzJg1BfEg=


Submit the signed message on Historia Core Desktop Wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We will now submit the ProRegTx special transaction to the blockchain to
register the masternode. This command must be sent from a Historia Core
wallet holding a balance on either the ``feeSourceAddress`` or
``payoutAddress``, since a standard transaction fee is involved. The
command takes the following syntax::

  protx register_submit tx sig

Where: 

- ``tx``: The serialized transaction previously returned in the ``tx`` 
  output field from the ``protx register_prepare`` command
- ``sig``: The message signed with the collateral key from the 
  ``signmessage`` command

Example::

  protx register_submit 030001000175c9d23c2710798ef0788e6a4d609460586a20e91a15f2097f56fc6e007c4f8e0000000000feffffff01a1949800000000001976a91434b09363474b14d02739a327fe76e6ea12deecad88ac00000000d1010000000000e379580dcfea3eb6944bfbe1de5fa2317932e260acce4d9ff3ede5f4287a34160100000000000000000000000000ffff2d4ce6ef4e1fd47babdb9092489c82426623299dde76b9c72d9799f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670ed1935246865dce1accce6c8691c8466bd67ebf1200001976a914fef33f56f709ba6b08d073932f925afedaa3700488acfdb281e134504145b5f8c7bd7b47fd241f3b7ea1f97ebf382249f601a0187f5300 II8JvEBMj6I3Ws8wqxh0bXVds6Ny+7h5HAQhqmd5r/0lWBCpsxMJHJT3KBcZ23oUZtsa6gjgISf+a8GzJg1BfEg=

Output::

  aba8c22f8992d78fd4ff0c94cb19a5c30e62e7587ee43d5285296a4e6e5af062

Your masternode is now registered and will appear on the Deterministic
Masternode List after the transaction is mined to a block. You can view
this list on the **Masternodes -> DIP3 Masternodes** tab of the Historia
Core wallet, or in the console using the command ``protx list valid``,
where the txid of the final ``protx register_submit`` transaction
identifies your masternode.

For support please come ask questions on the support channel in the Historia 
Discord.
