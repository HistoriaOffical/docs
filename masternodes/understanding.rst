.. meta::
   :description: Explanation of how Historia masternodes work in theory and practice to support InstantSend, PrivateSend and governance
   :keywords: historia, masternodes, hosting, linux, payment, instantsend, privatesend, governance, quorum, evolution, bls, 

.. _understanding_masternodes:

=========================
Understanding Masternodes
=========================
Coming soon.


Masternode requirements
=======================

- 100 HTA: A voting masternode requires 100 HTA. This role gives voting rights to the masternode owner and receives a 5%       reward.  Historia can be obtained from exchanges such as Stex.
- 5000 Historia: A IPFS masternode requires 5000 HTA. This role gives voting rights to the masternode owner and receives a     25% reward. IPFS daemon is required to provide for content distribution. Historia can be obtained from exchange such as     Stex.
- A server or VPS running Linux: Most recent guides use Ubuntu 16.04
  LTS. We recommend VPS services such as Vultr and DigitalOcean,
  although any decent provider will do. Generally an instance with low
  to average specifications will do, although performance requirements
  will increase according to this roadmap.
- A dedicated IPv4 and IPv6 address: These usually come with the VPS/server.
- A little time and (heart): Masternodes used to require complex setup,
  but tools such as historiaman now greatly simplify the process.

In addition to the 100 or 5000 Historia held in collateral, masternodes also have
minimum hardware requirements. As of version v0.16.3.0, these requirements
are as follows:

+---------+------------+-------------+
|         | Minimum    | Recommended |
+=========+============+=============+
| CPU     | 1x 1 GHz   | 1x 2 GHz    |
+---------+------------+-------------+
| RAM     | 2 GB       | 4 GB        |
+---------+------------+-------------+
| Disk    | 20 GB      | 40 GB       |
+---------+------------+-------------+
| Network | 400 GB/mth | 1 TB/mth    |
+---------+------------+-------------+

Masternode bandwidth use ranges between 30-100 GB per month and will
grow as the network does.

