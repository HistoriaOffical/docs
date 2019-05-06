.. meta::
   :description: The upgrade to Historia 0.13.0 involves changes to signature formats as defined in DIP3. This documentation highlights the upgrade steps and progress.
   :keywords: historia, cryptocurrency, masternode, miners, pools, exchanges, wallets, maintenance, dip3, upgrade, deterministic, dmt

.. include:: <isopub.txt>

.. _dip3-dev-upgrade:

=============================
Historia 0.13 Upgrade Information
=============================

**Official binaries are available at** https://github.com/HistoriaOffical/historia/releases/tag/v0.13.2.0

Historia Core v0.13.0 is intended to serve as the foundation for Historia
Evolution, a broadly scoped update to our network intended to facilitate
and enable new payment methods and technologies. Please see the `Historia
Core v0.13.0 Product Brief <https://blog.historia.network/product-brief-historia-core-release-v0-13-0-5d7fddffb7ef>`__ 
for an overview of new features, and complete this `3 question survey <https://goo.gl/forms/3BEo2MQYsZT5rwig1>`__
to help us assess the impact of this upgrade. The upgrade will take 
place in phases, as shown in the following diagram:

.. image:: img/013-upgrade-procedure.png


Installation notes
==================

Historia Core v0.13.0.0 will automatically activate `DIP002
<https://github.com/HistoriaOffical/dips/blob/master/dip-0002.md>`__, `DIP003
<https://github.com/HistoriaOffical/dips/blob/master/dip-0003.md>`__ and `DIP004
<https://github.com/HistoriaOffical/dips/blob/master/dip-0004.md>`__ once 80% of
the network has upgraded. Mining pools must mine an upgraded block and
pay an upgraded masternode to successfully signal the upgrade in a
block, and 80% of blocks in a window must signal in order to lock in the
upgrade. Please confirm the status of network uptake before proceeding
as the upgrade path differs slightly depending on on your timing.

- Activation status can be tracked in the image below or at `this site 
  <http://178.254.23.111/~pub/Historia/Historia_Info.html>`__.

.. figure:: http://178.254.23.111/~pub/13_adoption.png

   Historia v0.13.0.0 adoption by miners

- Masternode upgrade status can be tracked at `Historia Ninja <https://www.historianinja.pl/masternodes.html>`__

- If you are updating to Historia Core v0.13.0.0 **prior** to this 80%
  threshold  you should be able to simply shut down the daemon and
  replace it with  the updated binary.

- If you are updating to Historia Core v0.13.0.0 **after** this 80%
  threshold is reached please note that you will need to re-index the
  chainstate using the “-reindex-chainstate” command.

Dependencies
============

Please note that Historia Core v0.13.0.0 requires a one-time upgrade to all
related software and libraries to ensure continued compatibility.

- `DIP002: Special Transactions <https://github.com/HistoriaOffical/dips/blob/master/dip-0002.md#compatibility>`__ 
  contains more information on backwards compatibility.


- Please refer to the `Transaction Type Integration Guide <https://github.com/HistoriaOffical/docs/raw/master/binary/merchants/Integration-Resources-Historia-v0.13.0-Transaction-Types.pdf>`__ 
  for information on the implementation of Special Transactions and for
  examples of this new format.

- Contact the `Support Desk <https://support.historia.network/en/support/home>`__ 
  with any compatibility questions or for help upgrading.

Please see the official `Release Notes <https://github.com/HistoriaOffical/historia/blob/v0.13.0.0/doc/release-notes.md#rpc-changes>`__ 
for a complete listing of RPC improvements, in summary:

- **Mining:** ``getBlockTemplate`` now returns an array for masternode 
  payments instead of a single object.

- **InstantSend**: instantlock status is now included in
  ``getrawmempool``,   ``getmempoolancestors``, 
  ``getmempooldescendants``, ``getmempoolentry``, ``getrawtransaction``, 
  ``decoderawtransaction``, ``gettransaction``, ``listtransactions``, 
  ``listsinceblock``.

Libraries and APIs
==================

The following lists the current upgrade status for libraries:

+------------------------------------------------------------+---------------------+-----------+
| Name                                                       | Platform            | Upgraded? |
+============================================================+=====================+===========+
| `HistoriaJ <https://github.com/HashEngineering/historiaj>`__       | Android             |           |
+------------------------------------------------------------+---------------------+-----------+
| `Historia-Sync <https://github.com/historiaevo/historiasync-iOS/>`__   | iOS                 | |check|   |
+------------------------------------------------------------+---------------------+-----------+
| `Historiacore-Lib <https://github.com/historiaevo/historiacore-lib>`__ | JavaScript / NodeJS | |check|   |
+------------------------------------------------------------+---------------------+-----------+
| `NBitcoin <https://github.com/MetacoSA/NBitcoin>`__        | .Net                | |check|   |
+------------------------------------------------------------+---------------------+-----------+
| `Bitcoin-PHP <https://github.com/Bit-Wasp/bitcoin-php>`__  | PHP                 |           |
+------------------------------------------------------------+---------------------+-----------+
| `PyCoin <https://github.com/DeltaEngine/pycoin>`__         | Python              | |check|   |
+------------------------------------------------------------+---------------------+-----------+
 
.. The following lists the current upgrade status for APIs
