.. meta::
   :description: This guide describes how to set up IPFS API server on Windows to use the Historia Local Web Application
   :keywords: historia, guide, masternodes, IPFS
 
.. _ipfs-setup:

========================
Setup IPFS API - Windows
========================

This guide will walk you through how to setup a private IPFS API server on Windows to submit a Proposal, Record, or Archive to use for the Historia Local Web Application.

IPFS API For Windows
=============================

Download / Install IPFS Daemon
------------------------------

Setup IPFS
================
Option 1: Use prebuild package
-----------------
Download and follow instruction from this page: ::

   https://dist.ipfs.io/#go-ipfs



Option 2: Compile from source
-----------------
If want to build from source on Windows take a look at this document for instructions.::

   https://github.com/ipfs/go-ipfs/blob/master/docs/windows.md
   
   / Note : To run the IPFS Daemon you must install the Go Lang


Option 3: Manual from compiled or downloaded files
-----------------
Download or compile IPFS files and put them to a folder , then Ensure the Go and IPFS binaries (found in C:\\Go\\bin or where you installed) are in your Path system environment variables. To check click System, Advanced system settings, Environment Variables... and open Path under System variables:

.. figure:: /masternodes/img/ipfs-folder.png

.. figure:: /masternodes/img/system-variables.png


Initialize IPFS Daemon for Historia
-----------------
Since we will be using IPFS only for Historia, we can safely run the initialization: 
(Run commands on Windows Powershell or Command Prompt as Admin) ::

   ipfs init

Remove Original Bootstap IPFS Nodes and Connect to Historia IPFS Swarm
-----------------
Add Historia IPFS bootstrap nodes, configure our IPFS node, and only connect to the Historia IPFS Swarm.  ::

   ipfs bootstrap add /ip4/202.182.119.4/tcp/4001/ipfs/QmVjkn7yEqb3LTLCpnndHgzczPAPAxxpJ25mNwuuaBtFJD
   ipfs bootstrap add /ip4/149.28.22.65/tcp/4001/ipfs/QmZkRv4qfXvtHot37STR8rJxKg5cDKFnkF5EMh2oP6iBVU
   ipfs bootstrap add /ip4/149.28.247.81/tcp/4001/ipfs/QmcvrQ8LpuMqtjktwXRb7Mm6JMCqVdGz6K7VyQynvWRopH
   ipfs bootstrap add /ip4/45.32.194.49/tcp/4001/ipfs/QmZXbb5gRMrpBVe79d8hxPjMFJYDDo9kxFZvdb7b2UYamj
   ipfs bootstrap add /ip4/45.76.236.45/tcp/4001/ipfs/QmeW8VxxZjhZnjvZmyBqk7TkRxrRgm6aJ1r7JQ51ownAwy
   ipfs bootstrap add /ip4/209.250.233.69/tcp/4001/ipfs/Qma946d7VCm8v2ny5S2wE7sMFKg9ZqBXkkZbZVVxjJViyu


Now when you start IPFS, the IPFS daemon will now connect to the Historia IPFS swarm when started. ::

  ipfs daemon

After running the IPFS daemon, you should now be able to use the default address listed in the console. ::

  API server listening on /ip4/127.0.0.1/tcp/5001
