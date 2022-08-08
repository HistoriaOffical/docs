.. meta::
   :description: This guide describes how to set use the Create system on the Web App or Local Web App
   :keywords: historia, guide, voting keys, setup, Web App, Local Web App

.. _basex-setup:

========================================================================
Using the Create system for submission of a Proposal, Record, or Archive
========================================================================

How to make use of the Create module to submit a new proposal, record, or archive.

Before you begin
================

This guide assumes you have already setup an account on the https://historia.network web application or have already setup your Historia Local Web Application (HLWA). If you haven't done one of those things, please do that before you use this guide.


How to use the Create system (Record or Proposal)
-------------------------------------------------

The Historia Network developers have tried to make this process as simple as possible for the users.

After you have registered your user on Web App (https://historia.network/). Note: If you are using the Historia Local Web App you do not need to register an account. 

Login to the Web App application and go to Create, as highlighted below.

.. figure:: /img/c1.png
   :width: 600px


Click on the Create button, as highlighted below.

.. figure:: /img/c2.png
   :width: 600px


Once you click on Create, the web application will ask you which type of object you would like the create. Please choose one of the options.

.. figure:: /img/cX.png
   :width: 400px
   
The options available:

Proposal - A proposal is a temporary object that lets voters decide the direction of the Historia Network and this application. If a proposal passes, the Historia developers will do our best to follow the will of the voters. All proposals will expire and be deleted from the decentralized network. Things that could be submitted as proposals listed below:

- You have a good idea, are willing to do the work, and want to get paid in HTA.
- Directions to the developers on content
- Ideas on marketing
- Directions on development or new features

Record - A record is a object that if the voters approve, then will be a permanently added to the network. Once added, the record can never be altered, edited, or deleted by anybody, including the developers. It is 100% impossible as the records exist on in a decentralized database that no one person owns or controls. Things that could be submitted as records listed below:

- A current event
- Wiki content
- Useful information that should always be maintained forever.

Archive - An archive is a subset of a record object with the same behavior from a voting perpsective with regards to blockchain locks. When choosing an archive object, this will trigger functionality within the web application to archive an external web page.

If you choose a Proposal or Record object you will be asked if you would like to import a template to work off of, as seen below. 

.. figure:: /img/c4.png
   :width: 400px


It is recommended that you use a template, unless you have enough skill to create a custom page.

Once Create Builder interface loads there is much going on here as seen below

.. figure:: /img/c5.png
   :width: 600px

Key for above screen shot:

1. Type of object you are working on
2. End Cycle Date - This is the date of the next superblock and payment to any passing objects. It's recommended that you submit your object with at much voting time left possible to gain votes.
3. Next Payment Date - How many days left before the next superblock and payment to any passing objects
4. Voting Deadline Date - How many days left that masternodes and voting nodes have to vote on this proposal. The last two days of the cycle nodes can not vote.
5. Currently Passing - This is the total amount of HTA for the objects that have enough votes to pass currently. If too much HTA is allocated to pass in the current cycle, your object may not be paid even if it passes. This field is important to consider when putting in a object.
6. Total Available Budget - This is the total amount of HTA available for objects per month.
7. Name - This is the name that will be seen on the blockchain of your object.
8. Summary - This is the summary that will be seen on the blockchain of your object.
9. Reward Address - This is YOUR address that you want coins to be paid to, if your object has enough votes to pass.
10. Reward Amount Requested - The amount you are requesting if your object passes.
11. Editor - This is where you can edit the content of your object.
12. Editor Toolbar - This is where you can change formatting and create your object. Please get familar with this, as there is much involved here.
13. Save Button - This button allows you to save a draft without submitting your object to the blockchain. Your work it not automatically saved. Save often.
14. Delete Draft - This button will delete this draft.
15. Submit Button - Once you are complete and are ready to submit your object for a vote hit this button. Note, after you hit this button you will not be able to make changes.


After you hit the submit button, you will be asked for payment for your object as seen below.

.. figure:: /img/c6.png
   :width: 400px


After the payment is received your object has been submitted to the network. It may take up to 1 hour for the blockchain to process and sync your object for vote.

.. figure:: /img/c7.png
   :width: 400px


How to use the Create system (Archive)
--------------------------------------
Once you click on Create, the web application will ask you which type of object you would like the create. In this case choose the Archive option.

.. figure:: /img/cX.png
   :width: 400px
   
After choosing the Archive option, it will pull up the Create Builder screen, but slightly different than above. If you have not yet installed the Historia Network Browser Extension you will see the screen below. You can learn how to install the browser extension `here <https://github.com/HistoriaOffical/SingleFileHistoria>`_. 

.. figure:: /img/c10.png
   :width: 400px
   

 

If using the Historia Local Web Application (HLWA) you are ready to import and Archive. If you are using the Historia Network main web application (https://historia.network), you must setup your API Key for the browser extension to be able to import. To setup your API Key, go to https://historia.network/apikeys and click the generate button. It will produce your individual API key as seen below.

.. figure:: /img/c12.png
   :width: 600px
   
After you have setup your API Key in the Historia Network main web application, you must set the API Key in the Historia Network Browser Extension. Right click on the Historia Network Browser Externsion and open the Options menu. This will bring up the settings for the extension as seen below.


.. figure:: /img/c14.png
   :width: 400px  
   
   
Paste in your API Key into the Historia Network API Key field and close the settings tab. Your API Key has now been setup. After installing and have setup your API Key go back to the Archive page. You will see the following prompt when opening up the Archive page.

.. figure:: /img/c9.png
   :width: 400px


When that prompt is shown in either the HLWA or the Historia Network main web application (https://historia.network) open a new tab in your browser of a page that you want to import into the archive page. Right click on the page and go to Historia Network in the dialog menu, either select the "Send Page to Historia Network (Live)" if you want to use the main web application or "Send Page to Historia Network (Local)". If you choose the local version, the HLWA must also be running first.

After selecting the "Send Page to ..." option, go back to the Archive page. Depending on the complexity of the page in question this can take a few seconds to import. View the imported page below.

.. figure:: /img/c16.png
   :width: 400px
   
   
Trouble Shooting
----------------------------------------------

1. Historia Network main web application (https://historia.network) archive page does not import page

   - Regenerate your API Key (https://historia.network/apikeys)
   - Save regenerated API Key in the Historia Network Browser Extension
   - Verify that you are using the "Send Page to Historia Network (Live)" option.
2. HLWA archive page does not import page

   - Restart Browser
   - Restart the HLWA app on your local machine
   - Verify that you are using the "Send Page to Historia Network (Local)" option.

