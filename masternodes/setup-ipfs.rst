.. meta::
   :description: This guide describes how to set up a IPFS for Historia masternode.
   :keywords: historia, guide, masternodes, IPFS
 
.. _ipfs-setup:

==========
Setup IPFS
==========

Running the IPFS daemon is now a required part of the masternode system. A Content Distribution Masternode - Collateral 5000 requires IPFS to run a masternode. You must follow these steps. 

If you are setting up a Windows Masternode, please skip down to the section IPFS For Windows Masternodes. 

IPFS For Linux Masternodes
=============================

Download / Install IPFS Daemon
------------------------------

To run the IPFS Daemon you must install the Go Lang::
   
   sudo apt-get update  
   sudo apt-get install golang-go -y

Next download and install IPFS daemon. Because we have used Ubuntu 16.04 64-bit for our OS, there isn't a package for this version of Ubuntu::

   wget https://dist.ipfs.io/go-ipfs/v0.4.20/go-ipfs_v0.4.20_linux-amd64.tar.gz
   tar xvfz go-ipfs_v0.4.20_linux-amd64.tar.gz  
   sudo mv go-ipfs/ipfs /usr/local/bin/ipfs

Clean up::

   rm -rf go-ipfs/

Initialize IPFS Daemon for Historia
-----------------------------------
Since we will be using IPFS only for Historia, we can safely run the initialization::
   
   ipfs init -p server
   
Remove Original Bootstap IPFS Nodes and Connect to Historia IPFS Swarm
----------------------------------------------------------------------
IPFS can be bandwidth hungry, so we want to remove the IPFS bootstrap nodes, configure our IPFS node, and only connect to the Historia IPFS Swarm::

   ipfs bootstrap rm --all
   ipfs bootstrap add /ip4/144.202.100.201/tcp/4001/ipfs/QmbQYYMcALCHpkjN4opjDog6VUGct3dsxeREpmMMwjcJFM
   ipfs bootstrap add /ip6/2001:19f0:ac01:1771:5400:1ff:feb0:9db0/tcp/4001/ipfs/QmbQYYMcALCHpkjN4opjDog6VUGct3dsxeREpmMMwjcJFM
   ipfs bootstrap add /ip4/144.202.100.201/tcp/4001/ipfs/QmbQYYMcALCHpkjN4opjDog6VUGct3dsxeREpmMMwjcJFM
   ipfs bootstrap add /ip6/2001:19f0:ac01:1771:5400:1ff:feb0:9db0/tcp/4001/ipfs/QmbQYYMcALCHpkjN4opjDog6VUGct3dsxeREpmMMwjcJFM
   ipfs bootstrap add /ip4/140.82.34.25/tcp/4001/ipfs/QmSJYoShGbUkHx1jZMWLrtmczhDEq22KtjzdfrihZ9Wcmf
   ipfs bootstrap add /ip6/2001:19f0:6c01:a12:5400:1ff:feb0:9db5/tcp/4001/ipfs/QmSJYoShGbUkHx1jZMWLrtmczhDEq22KtjzdfrihZ9Wcmf
   ipfs bootstrap add /ip6/2001:19f0:4400:7566:5400:1ff:feb0:9dbc/tcp/4001/ipfs/QmfAbbuYcq5TgWQmq69JdBX66wzimRttfD7iRcEa9tUsTx
   ipfs bootstrap add /ip4/149.28.132.246/tcp/4001/ipfs/QmfAbbuYcq5TgWQmq69JdBX66wzimRttfD7iRcEa9tUsTx
   ipfs bootstrap add /ip4/149.28.180.79/tcp/4001/ipfs/QmUAB2jSqKsJui5pTjRnDFDP8ir7bJfxHMjBcqodSzLUB2
   ipfs bootstrap add /ip6/2001:19f0:5801:1ad7:5400:1ff:feb0:9dca/tcp/4001/ipfs/QmUAB2jSqKsJui5pTjRnDFDP8ir7bJfxHMjBcqodSzLUB2
   ipfs bootstrap add /ip4/45.77.25.230/tcp/4001/ipfs/QmW5cPiykFxFr8FEsGtkFYhrh66AscDNKNbt65iCLoj4pa
   ipfs bootstrap add /ip6/2001:19f0:7001:3e10:5400:1ff:feb0:9e5b/tcp/4001/ipfs/QmW5cPiykFxFr8FEsGtkFYhrh66AscDNKNbt65iCLoj4pa


   ipfs config --json Datastore.StorageMax '"50GB"'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Headers '["X-Requested-With", "Access-Control-Expose-Headers", "Range", "Authorization"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Methods '["POST", "GET"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Allow-Origin '["*"]'
   ipfs config --json Gateway.HTTPHeaders.Access-Control-Expose-Headers '["Location", "Ipfs-Hash"]'
   ipfs config --json Gateway.HTTPHeaders.X-Special-Header '["Access-Control-Expose-Headers: Ipfs-Hash"]'
   ipfs config --json Gateway.NoFetch 'false'
   ipfs config --json Swarm.ConnMgr.HighWater '500'
   ipfs config --json Swarm.ConnMgr.LowWater '200'
   
Unfortunately we can't use ipfs config to set up the gateway addresses::

   nano ~/.ipfs/config
   
Look for the line::
   
    "Gateway": "/ip4/127.0.0.1/tcp/8080"
    
Change the Gateway line to look like::

    "Gateway": [
      "/ip4/0.0.0.0/tcp/8080",
      "/ip6/::/tcp/8080"
    ],

Now when you start IPFS, the IPFS daemon will now connect to the Historia IPFS swarm when started.

You can view a valid sample config file here::



Create IPFS Service To Restart on Reboot or Crash
-------------------------------------------------
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
      

Start IPFS Daemon for Historia
------------------------------
Start the IPFS service::

   systemctl start ipfs
   
Enable the IPFS service to start on reboot::

   systemctl enable ipfs

Check the IPFS service is running::

   systemctl status ipfs

Get IPFS Peer ID
----------------
Historia need the IPFS ID generated by the IPFS initialization command in the masternode.conf file. Run this command and save the ID value for when you edit your masternode.conf::

   ipfs id

Result::
 
   {
      **"ID": "QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",** // THIS IS YOUR IPFS PEER ID, HISTORIA WILL NEED THIS
      "PublicKey": "CAASpgIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDGKc55NxrimIWjWIFK6J9Kgj0caCwzGbNCZ4xphSww4j3gsPe1puLhkQHoQpvB7BeDXMdsuIFEfknBjHsZTxRM66X/ZhODyv+wwuQs92FJ2Lb6n/HB/fqsjvkPYQeSNe+T1Djjc2OYzuZkTZwCNrY9hGUEbEq6O1DeqMHWRN1Gy0fu31QyL6mjVq804udm0sQlO3Cey8hmChTBH+GCw1sTNlUlEQy88FPMSjq6j/qGfHRO1bA/trYLTsjIEMLI+xi/HtVzrOg6n+/kQopjWLCGy19IXn/ZVzOZuJhpqBYAkVnUd1b9na5ND/3iN5VTdO6biK+NQ8hH/DEi4sb8wMqpAgMBAAE=",
      "Addresses": [
         "/ip4/127.0.0.1/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
         "/ip4/<youripv4address>/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
      ],
      "AgentVersion": "go-ipfs/0.4.20/",
      "ProtocolVersion": "ipfs/0.1.0"
   }

Setup Domain Name System (DNS) A Record
---------------------------------------

Historia requires a DNS name set to enabled SSL for your IPFS node. This is beyond the scope of this document, but there is plenty of documentation online on how to do this. Find a cheap DNS registrar and create a A record that points to the IP address of your VPS. Namecheap or GoDaddy are options for this. This can be any top level domain, such as .network or .info, so get this cheapest domain you can get.


Nginx Web Proxy 
---------------

After setting up IPFS, Nginx proxy and a DNS entry is needed to be setup::
   
   sudo apt-get install nginx  
 
Go to the ip address of your VPS in a web browser to verify that Nginix is running.


Install SSL Certificate
-----------------------
In this example we will be using the free SSL certificate service Let's Encrypt to create and install our SSL certificate. First we must install the Let's Encrypt Certbot::

   add-apt-repository ppa:certbot/certbot
   apt-get update
   apt-get -y install python-certbot-nginx

Next we need to prepare Nginx configuration file for Let's Encrypt Certbot. If you're using the default configuration file /etc/nginx/sites-available/default open it with a text editor such as nano and find the server_name directive. Replace the underscore, _, with your own domain name(s)::

   nano /etc/nginx/sites-available/default
   
After editing the configuration file, the server_name directive should look as follows. In this example, we assume that your domain is example.com and that you're requesting a certificate for example.com and www.example.com. Make sure to use your own domain name here::

   server_name example.com www.example.com;

Save the file and restart Nginx::

   systemctl restart nginx
   
The following command will obtain a certificate for you. Edit your Nginx configuration to use it, and reload Nginx.::

   certbot --nginx -d example.com -d www.example.com
   
If Certbot can obtain an SSL certificate, it will ask how you would like to configure your HTTPS settings. Please choose option 2 to redirect who visit your IPFS node over an unsecured connection.::

   Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
   -------------------------------------------------------------------------------
   1: No redirect - Make no further changes to the webserver configuration.
   2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
   new sites, or if you're confident your site works on HTTPS. You can undo this
   change by editing your web server's configuration.
   -------------------------------------------------------------------------------
   Select the appropriate number [1-2] then [enter] (press 'c' to cancel):

If the setup process has gone correctly, you can now go to your domain name in a browser and it will be protected by an SSL certification. However we are not done yet.

Lets finish this process and setup Nginix to point to the IPFS daemon that is running on your masternode. If you're using the default configuration file /etc/nginx/sites-available/default open it with a text editor such as nano again.::

   nano /etc/nginx/sites-available/default
   
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

Save the file and restart Nginx::

   systemctl restart nginx
   
Congratulations! You now have finished setup for IPFS. You can now proceed to installing your Historia masternode.

Get IPFS Peer ID
================
Open another command prompt. Historia needs the IPFS ID generated by the IPFS initialization command in the masternode.conf file. Run this command and save the ID value for when you edit your masternode.conf::

   ipfs id

Result::
 
   {
      "ID": "QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",  // THIS IS YOUR IPFS PEER ID, HISTORIA WILL NEED THIS
      "PublicKey": "CAASpgIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDGKc55NxrimIWjWIFK6J9Kgj0caCwzGbNCZ4xphSww4j3gsPe1puLhkQHoQpvB7BeDXMdsuIFEfknBjHsZTxRM66X/ZhODyv+wwuQs92FJ2Lb6n/HB/fqsjvkPYQeSNe+T1Djjc2OYzuZkTZwCNrY9hGUEbEq6O1DeqMHWRN1Gy0fu31QyL6mjVq804udm0sQlO3Cey8hmChTBH+GCw1sTNlUlEQy88FPMSjq6j/qGfHRO1bA/trYLTsjIEMLI+xi/HtVzrOg6n+/kQopjWLCGy19IXn/ZVzOZuJhpqBYAkVnUd1b9na5ND/3iN5VTdO6biK+NQ8hH/DEi4sb8wMqpAgMBAAE=",
      "Addresses": [
         "/ip4/127.0.0.1/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
         "/ip4/<youripv4address>/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
         "/ip6/::1/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD",
         "/ip6/<youripv6address>/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD"
      ],
      "AgentVersion": "go-ipfs/0.4.20/",
      "ProtocolVersion": "ipfs/0.1.0"
   }
