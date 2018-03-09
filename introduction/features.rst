.. _features:

==================
Features
==================

.. _specifications:

Specifications
==================

-  First block mined at 11PM EST, 18th January 2014
-  No premine
-  X11 hashing algorithm
-  2.6 minute block generation time
-  Block reward decreases by 7.14% per year
-  CPU/GPU/ASIC mining
-  Dark Gravity Wave difficulty adjustment algorithm
-  Between 17.74M and 18.92M total coin supply
-  Decentralized masternode network
-  Superior transaction anonymity using PrivateSend
-  Instant transactions using InstantSend
-  Two-tier network using masternodes to form the second tier
-  Decentralized Governance By Blockchain allows masternode owners to
   vote on budget proposals and decisions that affect Dash. Budget
   proposals fund Dash development and come directly from block rewards,
   i.e. Dash development is self-funded by its own blockchain.


.. _masternode_network:

Masternodes
==================

In addition to traditional Proof of Work (PoW) rewards for mining Dash,
users are also rewarded for running and maintaining special servers
called masternodes. Thanks to this innovative two tier network, Dash can
offer innovative features in a trustless and decentralized way. 
Masternodes are used to power PrivateSend, InstantSend, and the 
governance and treasury system. Users are rewarded for running 
masternodes; 45% of the block reward is allocated to pay the masternode 
network.

Masternodes enable the following services:

-  **InstantSend** allows for near-instant transactions. Dash
   InstantSend transactions are fully confirmed within two seconds.
-  **PrivateSend** gives financial privacy by obscuring the source of
   funds on the blockchain.
-  **Governance and Treasury** allows stakeholders in Dash to determine
   the direction of the project and devotes 10% of the block reward to
   development of the project and ecosystem (as of March 2017, our
   annual budget is $6.7 million).
-  **Dash Evolution** will make using cryptocurrency as easy as using
   PayPal.

Masternode owners must have possession of 1000 DASH, which they prove by
signing a message and broadcasting to the network. Those coins can be
moved at any time, but moving them will cause the masternode to fall out
of queue and stop earning rewards. Masternode users are also given 
**voting rights** on proposals. Each masternode has one vote and this 
vote can be used on budget proposals or important decisions that affect 
Dash.

Masternodes cost money and effort to host so they are paid a share of
the block reward as an incentivize. With current masternode numbers
and rewards, masternodes earn approximately a 8% return on 1000 Dash
(which means 6.97 Dash or USD1360 in July 2017) for the year of
2017. This `tool <https://stats.masternode.me/>`_ shows a
live calculation of masternode earnings. These rewards decrease by 7%
each year, but the rising value of Dash should offset these
reductions. As a matter of fact, masternodes were receiving 140 Dash per
month at the beginning of 2016, but this was actually less money than
today: USD600 per month. There is also the possibility for masternodes
to earn money from fees in the future.


.. _privatesend:

PrivateSend
==================

PrivateSend gives you true financial privacy by obscuring the origins of
your funds. All the Dash in your wallet is comprised of different
"inputs", which you can think of as separate, discrete coins. 
PrivateSenduses an innovative process to mix your inputs with the inputs
of two other people, without having your coins ever leave your wallet. 
You retain control of your money at all times.

The PrivateSend process works like this:

#. PrivateSend begins by breaking your transaction inputs down into
   standard denominations. These denominations are 0.01 Dash, 0.1 DASH,
   1 DASH and 10 DASH -- much like the paper money you use every day.
#. Your wallet then sends requests to specially configured software
   nodes on the network, called "masternodes". These masternodes are
   informed then that you are interested in mixing a certain
   denomination. No identifiable information is sent to the masternodes,
   so they never know "who" you are.
#. When two other people send similar messages, indicating that they
   wish to mix the same denomination, a mixing session begins. The
   masternode mixes up the inputs and instructs all three users' wallets
   to pay the now-transformed input back to themselves. Your wallet pays
   that denomination directly to itself, but in a different address
   (called a change address).
#. In order to fully obscure your funds, your wallet must repeat this
   process a number of times with each denomination. Each time the
   process is completed, it's called a "round". Each round of
   PrivateSend makes it exponentially more difficult to determine where
   your funds originated.
#. This mixing process happens in the background without any
   intervention on your part. When you wish to make a transaction, your
   funds will already be anonymized. No additional waiting is required.

**IMPORTANT:** Your wallet only contains 1000 of these "change
addresses". Every time a mixing event happens, one of your addresses is
used up. Once enough of them are used, your wallet must create more
addresses. It can only do this, however, if you have automatic backups
enabled. Consequently, users who have backups disabled will also have
PrivateSend disabled.


.. _instantsend:

InstantSend
==================

Traditional decentralized cryptocurrencies must wait for certain period 
of time for enough blocks to pass to ensure that a transaction is both 
irreversible and not an attempt to double-spend money which has already 
been spent elsewhere. This process is time-consuming, and may take 
anywhere from 15 minutes to one hour for the widely accepted number of 
six blocks to accumulate. Other cryptocurrencies achieve faster 
transaction confirmation time by centralizing authority on the network 
to various degrees.

Dash suffers from neither of these limitations thanks to its 
second-layer network of masternodes. Masternodes can be called upon to 
form voting quorums to check whether or not a submitted transaction is 
valid. If it is valid, the masternodes "lock" the inputs for the 
transaction and broadcast this information to the network, effectively 
promising that the transaction will be included in subsequently mined 
blocks and not allowing any other spending of these inputs during the 
confirmation time period.

InstantSend technology will allow for cryptocurrencies such as Dash to 
compete with nearly instantaneous transaction systems such as credit 
cards for point-of-sale situations while not relying on a centralized 
authority. Widespread vendor acceptance of Dash and InstantSend could
revolutionize cryptocurrency by shortening the delay in confirmation of
transactions from as long as an hour (with Bitcoin) to as little as a 
few seconds.

InstantSend was introduced in a whitepaper called `Transaction Locking 
and Masternode Consensus: A Mechanism for Mitigating Double Spending
Attacks <https://github.com/dashpay/docs/raw/master/pdf/Dash%20Whitepaper%20-%20Transaction%20Locking%20and%20Masternode%20Consensus.pdf>`_.

**How Dash 'InstantSend' Protects Merchants from Double Spends**,
Dash Detailed by Amanda B. Johnson, 16 September 2016

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; margin-bottom: 1em; overflow: hidden; max-width: 70%; height: auto;">
        <iframe src="//www.youtube.com/embed/HJx82On8jig" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>


.. _sporks:

Sporks
============================

In response to unforeseen issues with the rollout of the major "RC3"
update in June 2014, the Dash development team created a mechanism by
which updated code is released to the network, but not immediately made
active ("enforced"). This innovation allows for far smoother transitions
than in the traditional hard fork paradigm, as well as the collection of
test data in the live network environment. This process of multi-phased 
forking was originally to be called "soft forking" but the community affectionately dubbed it "the spork" and the name stuck.

New features or versions of Dash undergo extensive testing on testnet
before they are released to the main network. When a new feature or
version of Dash is released on mainnet, communication is sent out to
users informing them of the change and the need for them to update their
clients. Those who update their clients run the new code, but it is not
activated until a sufficient percentage of network participants (usually
80%) reach consensus on running it. In the event of errors occurring
with the new code, the client’s blocks are not rejected by the network
and unintended forks are avoided. Data about the error can then be
collected and forwarded to the development team. Once the development
team is satisfied with the new code’s stability in the mainnet
environment – and once acceptable network consensus is attained –
enforcement of the updated code can be activated remotely by multiple
members of the core development team signing a network message together
with their respective private keys. Should problems arise, the code can
be deactivated in the same manner, without the need for a network-wide
rollback or client update. For technical details on individual sporks,
see
`here <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/128319489/Understanding+Sporks>`_.


.. _x11_hash_algorithm:

X11 Hash Algorithm
==================

X11 is a widely used hashing algorithm created by Dash core developer
Evan Duffield. X11’s chained hashing algorithm utilizes a sequence of
eleven scientific hashing algorithms for the proof-of-work. This is so
that the processing distribution is fair and coins will be distributed
in much the same way Bitcoin’s were originally. X11 was intended to make
ASICs much more difficult to create, thus giving the currency plenty of
time to develop before mining centralization became a threat. This
approach was largely successful; as of early 2016, ASICs for X11 now
exist and comprise a significant portion of the network hashrate, but
have not resulted in the level of centralization present in Bitcoin.
Information on mining with X11 can be found in the `Mining <mining>`_ 
section of this documentation.

X11 is the name of the chained proof-of-work (**PoW**) algorithm that
was introduced in Dash (launched January 2014 as "Xcoin"). It was 
partially inspired by the chained-hashing approach of Quark, adding
further "depth" and complexity by increasing the number of hashes, yet
it differs from Quark in that the rounds of hashes are determined *a
priori* instead of having some hashes being randomly picked.

The X11 algorithm uses multiple rounds of 11 different hashes (blake,
bmw, groestl, jh, keccak, skein, luffa, cubehash, shavite, simd, echo),
thus making it one of the safest and more sophisticated cryptographic
hashes in use by modern cryptocurrencies. The name X11 is not related to
the open source X11 windowing system common on UNIX-like operating 
systems.

Advantages of X11
-----------------

The increased complexity and sophistication of the chained algorithm
provides enhanced levels of security and less uncertainty for a digital
currency, compared to single-hash PoW solutions that are not protected
against security risks like SPOF (Single Point Of Failure). For example,
a possible but not probable computing breakthrough that "breaks" the
SHA256 hash could jeopardize the entire Bitcoin network until the
network shifts through a hard fork to another cryptographic hash.

In the event of a similar computing breakthrough, a digital currency
using the X11 PoW would continue to function securely unless all 11
hashes were broken simultaneously. Even if some of the 11 hashes were to
prove unreliable, there would be adequate warning for a currency using
X11 to take measures and replace the problematic hashes with other more
reliable hashing algorithms.

Given the speculative nature of digital currencies and their inherent
uncertainties as a new field, the X11 algorithm can provide increased
confidence for its users and potential investors that single-hash
approaches cannot. Chained hashing solutions, like X11, provide
increased safety and longevity for store of wealth purposes, investment
diversification and hedging against risks associated with single-hash
currencies plagued by SPOF (Single Point Of Failure).

Evan Duffield, the creator of Dash and X11 chained-hash, has written on
several occasions that X11 was integrated into Dash not with the
intention to prevent ASIC manufacturers from creating ASICs for X11 in
the future, but rather to provide a similar migratory path that Bitcoin
had (CPUs, GPUs, ASICs).


.. _dark_gravity_wave:

Dark Gravity Wave
==================

**DGW** or *Dark Gravity Wave* is an open source difficulty-adjusting
algorithm for Bitcoin-based cryptocurrencies that was first used in Dash
and has since appeared in other digital currencies. DGW was authored by 
Evan Duffield, the developer and creator of Dash, as a response to a 
time-warp exploit found in *Kimoto's Gravity Well*. In concept, DGW is 
similar to the Kimoto Gravity Well, adjusting the difficulty levels 
every block (instead of every 2016 blocks like Bitcoin) based on 
statistical data from recently found blocks. This makes it possible to 
issue blocks with relatively consistent times, even if the hashing power
experiences high fluctuations, without suffering from the time-warp 
exploit.

- Version 2.0 of DGW was implemented in Dash from block 45,000 onwards 
  in order to completely alleviate the time-warp exploit.

- Version 3.0 was implemented on May 14 of 2014 to further improve 
  difficulty re-targeting with smoother transitions. It also fixes 
  issues with various architectures that had different levels of 
  floating-point accuracy through the use of integers.


.. _emission_rate:

Emission Rate
==================

Cryptocurrencies such as Dash and Bitcoin are created through a
cryptographically difficult process known as mining. Mining involves
repeatedly solving `hash algorithms <x11-hash-algorithm>`_ until a valid
solution for the current `mining difficulty <dark-gravity-wave>`_ is 
discovered. Once discovered, the miner is permitted to create new units 
of the currency. This is known as the block reward. To ensure that the 
currency is not subject to endless inflation, the block reward is 
reduced at regular intervals. Graphing this data results in a curve 
showing total coins in circulation, known as the coin emission rate.

While Dash is based on Bitcoin, it significantly modifies the coin
emission rate to offer a smoother reduction in coin emission over time.
While Bitcoin reduces the coin emission rate by 50% every 4 years, Dash
reduces the emission by one-fourteenth (approx. 7.14%) every 210240
blocks (approx. 383.25 days). It can be seen that reducing the block
reward by a smaller amount each year offers a smoother transition to a
fee-based economy than Bitcoin.


.. figure:: img/coin_emission.jpg

   Bitcoin vs. Dash coin emission rate


Total coin emission
-------------------

`Bitcoin's total coin emission <https://docs.google.com/spreadsheets/d/12tR_9WrY0Hj4AQLoJYj9EDBzfA38XIVLQSOOOVePNm0/pubhtml?gid=0&single=true>`_
can be calculated as the sum of a geometric series, with the total
emission approaching (but never reaching) 21,000,000 BTC. This will
continue until 2140, but the mining reward reduces so quickly that 99%
of all bitcoin will be in circulation by 2036, and 99.9% by 2048.

`Dash's total coin
emission <https://docs.google.com/spreadsheets/d/1JUK4Iy8pjTzQ3Fvc-iV15n2qn19fmiJhnKDDSxebbAA/edit#gid=205877544>`_
is also the sum of a geometric series, but the ultimate total coin
emission is uncertain because it cannot be known how much of the 10%
block reward reserved for budget proposals will actually be allocated,
since this depends on future voting behavior. Dash will continue to emit
coins for approximately 192 years before a full year of mining creates
less than 1 DASH. After 2209 only 14 more dash will be created. The last
dash will take 231 years to be generated, starting in 2246 and ending
when emission completely stops in 2477. Based on these numbers, a
maximum and minimum possible coin supply in the year 2254 can be
calculated to be between:

+-----------------+-----------------------------------+
| 17,742,696 DASH | Assuming zero treasury allocation |
+-----------------+-----------------------------------+
| 18,921,005 DASH | Assuming full treasury allocation |
+-----------------+-----------------------------------+

Block reward allocation
-----------------------

Unlike Bitcoin, which allocates 100% of the block reward to miners, Dash
splits the block reward between the
`miner <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/1146945/Mining>`_,
a deterministically selected
`masternode <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/33947684/Understanding+masternodes>`_,
and the decentralized `budget system <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/8585246/Governance+and+Budget+System>`_.
Dash features superblocks, which appear every 16616 blocks (approx.
30.29 days) and release 10% of the cumulative budget over that 
`budget cycle period <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/19169430/Using+Decentralized+Governance%3A+Proposals%2C+Voting%2C+and+Budgets>`_
to the winning proposals in the budget system. To pay for superblocks,
the block reward for normal blocks over the period is reduced by 10%.
Normal block payments are split equally between miners and masternodes
according to the `payment logic <https://dashpay.atlassian.net/wiki/spaces/DOC/pages/8880184/Payment+Logic>`_,
resulting in an coin reward allocation over a budget cycle as follows:

+-----+----------------------------------------+
| 45% | Mining Reward                          |
+-----+----------------------------------------+
| 45% | Masternode Reward for Proof-of-service |
+-----+----------------------------------------+
| 10% | Decentralized Governance Budget        |
+-----+----------------------------------------+

This documentaiton is based on calculations and posts by moocowmoo.
Please see `this reddit post <https://www.reddit.com/r/dashpay/comments/7fc2on/dash_over_1000_in_a_few_weeks/dqb4pjn/>`_
for more details, or run your own `emission calculations using this
tool <https://repl.it/@moocowmoo/dash-minmax-coin-generation>`_.
See `this site <https://stats.masternode.me>`_ for live data on current
network statistics.

.. _decentralized_governance:

Decentralized Governance
=========================

Decentralized Governance by Blockchain, or DGBB, is Dash's attempt to
solve two important problems in cryptocurrency: governance and funding.
Governance in a decentralized project is difficult, because by
definition there are no central authorities to make decisions for the
project. In Dash, such decisions are made by the network, that is, by
the owners of masternodes. The DGBB system allows each masternode to
vote once (yes/no/abstain) for each proposal. If a proposal passes, it
can then be implemented (or not) by Dash's developers. A key example is
early in 2016, when Dash's Core Team submitted a proposal to the network
asking whether the blocksize should be increased to 2 MB. Within 24
hours, consensus had been reached to approve this change. Compare this
to Bitcoin, where debate on the blocksize has been raging for nearly
three years.

DGBB also provides a means for Dash to fund its own development. While 
other projects have to depend on donations or premined endowments, Dash 
uses 10% of the block reward to fund its own development. Every time a 
block is mined, 45% of the reward goes to the miner, 45% goes to a 
masternode, and the remaining 10% is not created until the end of the
month. During the month, anybody can make a budget proposal to the
network. If that proposal receives net approval of at least 10% of the
masternode network, then at the end of the month a series of
"superblocks" will be created. At that time, the block rewards that were
not paid out (10% of each block) will be used to fund approved
proposals. The network thus funds itself by reserving 10% of the block
reward for budget projects.

You can read more about Dash governance in the `governance <dgbb>`_ 
section of this documentation.


.. _sentinel:

Sentinel
=========

Sentinel is an autonomous agent for persisting, processing and
automating Dash 12.1 governance objects and tasks, and for expanded
functions in the upcoming Dash V13 release (Evolution). Sentinel is
implemented as a Python application that binds to a local version 12.1
dashd instance on each Dash 12.1 masternode.

A Governance Object (or "govObject") is a generic structure introduced
in Dash Core 12.1 to allow for the creation of Budget Proposals,
Triggers, and Watchdogs. Class inheritance has been utilized to extend 
this generic object into a "Proposal" object to supplant the current 
Dash budget system.


.. epigraph::

   *"The differences with Sentinel are really architectural and not easy/
   interesting to explain to users as they are a bridge from 12.0 
   towards Evo features (but not fully implementing them), and Sentinel 
   was only a part of 12.1 improvements anyway. Pre-Sentinel, governance 
   functions were 'hard wired' into core code. Sentinel abstracts this 
   process because in Evolution there are many Object types from Users 
   to Accounts to Contacts etc, and if we didn't make this change first,
   future changes / improvements in Evolution (e.g. adding a new type of 
   Object) would require changing core code. Now Core is agnostic to 
   types of objects and we can take this forward for user experience and
   not just governance. In terms of documentation, there is no 
   whitepaper specific to Sentinel, but we have various docs for Evo in
   an on-going RFC process which are used as the basis for Evo 
   development.*

   -- Andy Freer, Evolution Developer


.. _fees:

Fees
======

Transactions on the Dash network are recorded in blocks on the
blockchain. The size of each transaction is measured in bytes, but there
is not necessarily a correlation between high value transactions and the
number of bytes required to process the transaction. Instead,
transaction size is affected by how many input and output addresses are
involved, since more data must be written in the block to store this
information. Each new block is generated by a miner, who is paid for
completing the work to generate the block with a block reward. In order
to prevent the network from being filled with spam transactions, the
size of each block is artificially limited. As transaction volume
increases, the space in each block becomes a scarce commodity. Because
miners are not obliged to include any transaction in the blocks they
produce, once blocks are full, a voluntary transaction fee can be
included as an incentive to the miner to process the transaction. Most
wallets include a small fee by default, although many miners will
process transactions even if no fee is included.

The release of Dash 0.12.2.0 and activation of DIP0001 saw a
simultaneous reduction of fees by a factor of 10, while the block size
was increased from 1MB to 2MB to promote continued growth of low-cost
transactions even as the cost of Dash rises. Dash also supports
`InstantSend <instantsend>`_ and `PrivateSend <privatesend>`_
transactions, which operate on a different and mandatory fee schedule. 
The fee schedule for Dash 12.2.x as of December 2017 is as follows:

+----------------------+-----------------+-----------------------------------+
| Transaction type     | Recommended fee | Per unit                          |
+======================+=================+===================================+
| Standard transaction | .00001 DASH     | Per kB of transaction data        |
+----------------------+-----------------+-----------------------------------+
| InstantSend          | .0001 DASH      | Per transaction input             |
+----------------------+-----------------+-----------------------------------+
| PrivateSend          | .001 DASH       | Per 10 rounds of mixing (average) |
+----------------------+-----------------+-----------------------------------+

As an example, a standard and relatively simple transaction on the Dash
network with one input, one output and a possible change address
typically fits in the range of 200 - 400 bytes. Assuming a price of
US$800 per DASH, the fee falls in the range of $0.0015 - $0.0030, or
well under half a cent. Processing the transaction using InstantSend at
the same price would add $0.08 or eight cents to the transaction. These
fees apply regardless of the Dash or dollar value of the transaction
itself.

PrivateSend works by creating denominations of 10, 1, 0.1 and 0.01 DASH
and then mixing these denominations with other users. Creation of the
denominations is charged at the default fee for a standard transaction.
Mixing is free, but to prevent spam attacks, an average of one in ten
mixing transactions are charged a fee of 0.001 DASH. Spending inputs
mixed using PrivateSend incurs the usual standard or InstantSend fees,
but to avoid creating a potentially identifiable change address, the fee
is always rounded up to the lowest possible denomination. This is
typically .01 DASH, so it is important to deduct the fee from the amount
being sent if possible to minimise fees. Combining InstantSend and
PrivateSend may be expensive due to this requirement and the fact that a
PrivateSend transaction may require several inputs, while InstantSend
charges a fee of 0.0001 DASH per input. Always check your fees before
sending a transaction.


.. _evolution:

Evolution
==========

`Dash Evolution <https://www.dash.org/evolution/>`_ is the code
name for a decentralized currency platform built on Dash blockchain
technology. The goal is to provide simple access to the unique features
and benefits of Dash to assist in the creation of decentralized
technology. Dash introduces a tiered network design, which allows users
to do various jobs for the network, along with decentralized API access
and a decentralized file system.

Dash Evolution will be released in stages. Dash Core releases 12.1 
through to 12.3 will lay the groundwork for the decentralized features 
behind the scenes. An alpha version of Evolution is expected in the 
first half of 2018, including a basic implementation of DashDrive, 
Primitives, DAPI and a simple T3 wallet. This will be released together 
with a series of Dash Improvement Proposals (DIPs), followed by a 
testnet phase with most basic functionality, and a first stable release 
in summer 2018.

Included below is our current work on Evolution, that adds many
components such as:

- **DashDrive:** A decentralized shared file system for user data that
  lives on the second tier network
- **DAPI:** A decentralized API which allows third tier users to access
  the network securely
- **DashPay Decentralized Wallets:** These wallets are light clients
  connected to the network via DAPI and run on various platforms
- **Second Tier:** The masternode network, which provides compensated
  infrastructure for the project
- **Budgets:** The second tier is given voting power to allocate funds 
  for specific projects on the network via the budget system
- **Governance:** The second tier is given voting power to govern the
  currency and chart the course the currency takes
- **Quorum Chain:** This feature introduces a permanent stable masternode
  list, which can be used to calculate past and present quorums
- **Social Wallet:** We introduce a social wallet, which allows friends
  lists, grouping of users and shared multisig accounts.

Evolution Previews
------------------

The following videos featuring Dash Founder Evan Duffield and Head of
UI/UX Development Chuck Williams describe the development process and
upcoming features of the Dash Evolution platform.


**Chuck Williams on Evolution**, Dash Conference London, 14 September 2017

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; margin-bottom: 1em; height: 0; overflow: hidden; max-width: 70%; height: auto;">
        <iframe src="//www.youtube.com/embed/b-XL_ddWCwQ" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

**Evan Duffield on the Evolution Roadmap**, Dash Force News, 28 June 
2017

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; margin-bottom: 1em; height: 0; overflow: hidden; max-width: 70%; height: auto;">
        <iframe src="//www.youtube.com/embed/E65QixSRosw" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>