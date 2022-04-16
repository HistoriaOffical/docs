.. meta::
   :description: This guide describes how to set up the Historia Local Web App
   :keywords: historia, guide, voting nodes, setup,

.. _votingnode-setup:

==========================================
Setup the Historica Local Web Application
==========================================

Setting up a Historica Local Web App requires a basic understanding of how to use a computer, as well as an ability to follow instructions closely. 

Before you begin
================

This guide assumes you have already setup your voting node. You will need:

- Historia Core Wallet v0.17.0.3 or later to store your Historia. You can get this here: https://github.com/HistoriaOffical/historia/releases/
- A masternode or voting node previously setup. You can learn to setup this up here: https://docs.historia.network/en/latest/votingnodes/index.html
- A computer running Windows, MacOS, or Linux. 
- The Historia Local Web Application Software. You can get this here: https://github.com/HistoriaOffical/Historia-Local-Web-Application/releases/

In this guide, we will assume you will be working from a Windows computer. However, alternative steps for using MacOS or Linux will be indicated where necessary.

Voting Node Requirements
------------------------


Setup the Historia Core for Local Web Application Access
========================================================

Open the Historia Core wallet config file, historia.conf. For different operating 
systems, the Historiacore folder and the historia.conf file can be found in the following locations.

+-----------+--------------------------------------------------------+--------------------------------------------+
| Platform  | Path                                                   | Shortcut                                   |
+===========+========================================================+============================================+
| Linux     | /home/yourusername/.historiacore                       | ~/.historiacore                            | 
+-----------+--------------------------------------------------------+--------------------------------------------+
| OSX       | /Macintosh HD/Library/Application Support/HistoriaCore | ~/Library/Application Support/HistoriaCore |
+-----------+--------------------------------------------------------+--------------------------------------------+
| Windows   | C:"&#92;"Users\yourusername\AppData\Roaming\Historia Core    | %APPDATA%\Historia Core                    |
+-----------+--------------------------------------------------------+--------------------------------------------+

Setup RPC access in the config file
-------------------------------------

Once the historia.conf file is open add the following lines to the historia.conf config file::

  rpcuser=<SomeUsername>
  rpcpassword=<SomePassword>
  rpcport=10100
  rpcthreads=8
  rpcallowip=127.0.0.1
  server=1

Save the historia.conf file, and start or restart the Historia Core Client if it is currently running.

Setup the Web Application
=========================

If you haven't already, download the Historia Local Web Application for your platoform. You can get this here: https://github.com/HistoriaOffical/Historia-Local-Web-Application/releases/

Open the Historia-Local-Web-App executable by double clicking on the icon, as seen below.

Once open you will see the address in the command prompt that you need to connect to, as seen below.

Copy this address and paste it into your browser. This will open the web application in your browser. If it is your first time, it will take you to the setup screen, as seen below.

Add your Historia Core Client information that you used in the historia.conf file. and click the "Test Connection to Historia Client" button. If the connection is successful you will see a Sucess message, as seen below.


Add IPFS gateway information with the proper port (ie 443). You can choose any IPFS gateway. Click the "Test Connection to IPFS Server" button. If the connection is successful you will see a Sucess message, as seen below.

Onnce the Historia Core Client and IPFS Gateway are setup, hit the save button to save all settings.

The Historia Local Web Application is now setup. If you want to use the Historia Local Web Application for voting, you have to setup your voting keys. Follow the Web App voting guide. It is the same process. https://docs.historia.network/en/latest/governance/basex.html


Multiple Masternode / Voting Nodes
----------------------------------------------

You can add multiple masternodes or voting nodes and this will vote with all keys properly. The one caveat is that the passphrase you use in the web application must be the same accross all masternodes / voting nodes.
