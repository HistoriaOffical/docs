.. meta::
   :description: This guide describes how to set your voting keys on BaseX
   :keywords: historia, guide, voting keys, setup, Web App

.. _basex-setup:

==========================================
Using the Web App for Voting
==========================================

Setting up Web App with your voting keys requires a basic understanding of how to use Historia GUI Wallet and a website.

https://historia.network/

Before you begin
================

This guide assumes you are setting up your voting keys on Web App. This assumes that you have already setup your masternode or voting node previously. If you have not setup your masternode or voting node, please do that before you use this guide.


Get Your Voting Private Key
---------------------------

The Historia developers have tried to make this process as simple as possible for the users. On the wallet you have setup your masternode or voting node from, click **Tools > Debug console**. At the console run the following commmand: ::

   masternode status

The output will look something like this: ::

  "proTxHash": "1656038a406384d508ebee5e1fe1fb48c5560391cd3f6b162f65575fa4c1f95a",
  "collateralHash": "849bce016be646c7844587981cab8a3f64148434d20c637a3d8c4a0cc579f056",
  "collateralIndex": 1,
  "collateralAddress": "HVWWqYBK8bjZHrMXsz88HQxxcteqxPV7Ea",
  "operatorReward": 0,
  "state": {
    "service": "209.250.233.69:10101",
    "registeredHeight": 279634,
    "lastPaidHeight": 298601,
    "PoSePenalty": 0,
    "PoSeRevivedHeight": -1,
    "PoSeBanHeight": -1,
    "revocationReason": 0,
    "ownerAddress": "HPpiVzbXKtqioARFPyijdk2QaJiiFm6Xoy",
    "votingAddress": "HTTM4QbcGiWAWtcJB9g22meoYupHnErcyo",
    "ipfsPeerID": "Qma946d7VCm8v2ny5S2wE7sMFKg9ZqBXkkZbZVVxjJViyu",
    "identity": "mn1.historia.network",
    "payoutAddress": "HPi1Q925T7BjHhWndWTh5w3xvZqw5AZ7oi",
    "pubKeyOperator": "1098e4f499307c4931d6d01438efe811d670226eb1e715e4ed91e78124b32c6dcd636540dc980e0cdc7272d3406ef5cb"

Find your **votingAddress**, **collateralHash**, **collateralIndex**. These will be needed to register your masternode in Web App. In this example it is: ::

    "votingAddress": "HTTM4QbcGiWAWtcJB9g22meoYupHnErcyo",
    "collateralHash": "849bce016be646c7844587981cab8a3f64148434d20c637a3d8c4a0cc579f056",
    "collateralIndex": 1,
    
This next command is important to understand what exactly is happening. Normally you should NEVER run the dumpprivkey command and give the output to a third party. But in the one instance this is safe. There should not be any coins associated with this address. The votingAddress private key is required to use BaseX for voting. During the setup of your private key into Web App you will encrypt this within the Web App application and even if the database is hacked, private voting keys will not be lost.
Alternatively you can just use the HistoriaCore GUI Wallet to vote.

Using the previous **votingAddress** as an example we can now get the private key of the voting address in the debug console type in. Please use your own **votingAddress** key below: ::

   dumprivkey HTTM4QbcGiWAWtcJB9g22meoYupHnErcyo

Output: ::

   cSh24QcuT<REMOVED FOR PRIVACY>

Add your voting address private key to Web App
----------------------------------------------

After you have registered your user on Web App (https://historia.network/) 

Login to the Web App application and go to Masternodes->My Masternode, as seen below.

.. figure:: /img/bx1.png
   :width: 400px

Add your masternode details. Identity, Collateral Transaction Hash, and Collateral Index is required, as seen below. It should be noted that Identity is just for your own records and it doesn't matter the value.

.. figure:: /img/bx2.png
   :width: 400px
   
Your masternode or voting node is now registered in the web application, however you must now uploaded your voting private key. Click the Update button for the masternode or voting node that you want to add the voting private key to, as seen below.

.. figure:: /img/bx3.png
   :width: 400px
   

Next enter in the your current password for the web application into the Password field. This will encrypt your voting private key preventing anyone else from using it. Copy your **votingAddress** private key in to the "Voting Address Private Key" field. Then click the Update button, as seen below.


.. figure:: /img/bx4.png
   :width: 400px


You masternode or voting node has now been registered, your voting key has now been stored, and you can now use this web application to vote on future proposals or records.

