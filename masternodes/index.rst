.. meta::
   :description: Introduction to documentation on how to set up and operate a masternode for the Historia cryptocurrency.
   :keywords: historia, masternodes, hosting, linux, payment, setup, ipfs

.. _masternodes:

===========
Masternodes
===========

Historia is the first cryptocurrency with a focus on a voting platform and 
file storage mechanism (IPFS) paired together to attempt to give an accurate 
consensus-based view of future history. These features are implemented on 
top of a network of masternodes and the IPFS network, which gives rise 
to many exciting features not available on conventional blockchains.

Historia also makes use of the first role based masternode system to allow for
different roles and different rewards depending on the service the masternode
provides to the Historia network.

This documentation focuses on understanding the services masternodes provide to the network, while also including guides on how to run a masternode and IPFS by leveraging either a hosting provider or by setting up and maintaining your own hosting solution. The primary requirement to run a masternode on the Historia network is 5000 HTA. This is known as the collateral, and cannot be spent without interrupting operation of the masternode. The second requirement is the actual server running the Historia masternode software and the IPFS daemon.

**Option 1: Self-operated masternode**

Users with a deeper understanding (or curiosity) about the inner
workings of the Historia network may choose to operate their own masternode
on their own host server. Several steps are required, and the user must
assume responsibility for setting up, securing and maintaining both the
server and collateral. See these pages for information on how to set up
a self-operated masternode.

.. toctree::
   :maxdepth: 1

   understanding.rst
   hosting.rst
   setup-cdmn.rst
   setup-win-cdmn.rst





 
