.. meta::
   :description: Description of different wallets available to use and spend Historia cryptocurrency
   :keywords: historia, wallet, core, android,  web, 

.. _wallets:

=======
Wallets
=======

Whenever you are storing objects with a market value, security is
necessary. This applies to barter systems as well as economies using
currency as a medium of exchange. While banks store balances on a
private ledger, cryptocurrencies store balances under unique addresses
on a distributed public ledger. The cryptographic private keys to access
the balance stored on each public address are therefore the object of
value in this system. This section of the documentation discusses
different practical methods of keeping these keys safe in wallets, while
remaining useful for day-to-day needs.

.. _historia-core-wallet:

Historia Core Wallet
================

Historia Core Wallet is the full official release of Historia and supports all
Historia features as they are released. Current support includes IPFS, Record and Proposal Storage,
InstantSend and PrivateSend, as well as an RPC console and governance features. 
Historia Core Wallet (sometimes known as the QT wallet) is a professional or heavy wallet which
downloads the full blockchain (several GB in size). The Historia Core Wallet also includes the IPFS record and proposal database,
and can operate as both a full node and masternode on the network. Because of the
requirement to hold a full copy of the blockchain, some time is required
for synchronization when starting the wallet. Once this is done, the
correct balances will be displayed, and the functions of the wallet can
be used. Historia Core Wallet is available for macOS, Linux, and Windows.


Features:

-  IPFS Intregration
-  Record and Proposal Database storage
-  PrivateSend
-  InstantSend
-  Wallet encryption
-  Coin control and fee control
-  QR code generation and address book
-  Masternode commands and voting
-  Automated backup
-  Debug console

Available documentation:

.. toctree::
   :maxdepth: 1

   historiacore/installation.rst
   historiacore/cmd-rpc.rst
   
   Historia Core Wallet




