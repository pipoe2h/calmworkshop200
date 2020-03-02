.. _calm_selfservice:

--------------------
Self-Service Catalog
--------------------

Overview
++++++++

With Calm Service Catalog, you define your own catalog of custom blueprints and marketplace applications, and make them available for your organization. Then, end users can quickly discover and deploy IT services using a Self-Service portal.

Custom Blueprints
+++++++++++++++++

.. note::

  This exercise assumes you have a Blueprint available from a previous exercise. :ref:`calm_iaas`

In this exercise you will learn how to manage custom Calm Blueprints within the Calm Self-Service Catalog. As part of the exercise you will publish a custom Blueprint from the Blueprint section, use Marketplace Manager to approve, assign roles and projects, and publish to the Self-Service Portal (a.k.a. Marketplace). Finally you will edit a project environment so your Blueprint can be launched directly from the Marketplace.

Publishing Blueprints
.....................

#. From **Prism Central > Apps**, select |bp-icon| **Blueprints** from the sidebar.

#. Open any **Active** Blueprint by clicking on its **Name**.

   .. figure:: images/calm_self_01.png

#. Click **Publish** on the top right.

   .. figure:: images/calm_self_02.png

#. Provide the following details:

   - **Name** (e.g. Blueprint Name <INITIALS>_CentOS76)
   - **Publish as a** - New Marketplace blueprint
   - **Initial Version** - 1.0.0
   - **Description** - CentOS 7.6 virtual server

#. Click **Submit for Approval**.

Approving Blueprints
....................

#. From **Prism Central > Apps**, select |mktmgr-icon| **Marketplace Manager** from the sidebar.

   .. note:: You must be logged in as a Cluster Admin user to access the Marketplace Manager.

#. Note your Blueprint does not appear in the list of **Marketplace Items**.

#. Select the **Approval Pending** tab.

   .. figure:: images/calm_self_03.png

#. Select your **Pending** Blueprint.

   .. figure:: images/calm_self_04.png

#. Review the available actions:

   - **Reject (X)** - Prevents  Blueprint from being launched or published in the Marketplace. The Blueprint will need to be submitted again after being rejected before it can be published. The Blueprint will remain in the approval pending view for tracking purposes. This can be deleted if required.
   - **Approve (V)** - Approves the Blueprint for publication to the Marketplace.
   - **Delete** - Blueprint gets rejected and removed from the Approval Pending list. It will remain in the Blueprints view.
   - **Launch** - Launches the Blueprint as an application, similar to launching from the Blueprint Editor.

#. Click **Approve**.

#. Once the application has been successfully approved, it will appear under the **Marketplace Blueprints** tabs. Find it and assign the appropriate **Category** and **Project Shared With**.

   .. figure:: images/calm_self_05.png

#. Click **Apply**.

#. Click **Publish**.

#. Verify the Blueprint's **Status** is now shown as **Published**.

   .. figure:: images/calm_self_06.png

#. From **Prism Central > Apps**, select |mkt-icon| **Marketplace** from the sidebar. Verify your Blueprint is available for launching as an application.

   .. figure:: images/calm_self_07.png

Configuring Project Environment
...............................

To launch a Blueprint directly from the Marketplace, we need to ensure our Project has all of the requisite environment details to satisfy the Blueprint.

#. Select |proj-icon| **Projects** from the sidebar.

#. Click the Project **Name** associated with your Blueprint at the time of publishing (e.g. the **JG-Calm** Project that was assigned as **Project Shared With**).

   .. figure:: images/calm_self_08.png

#. Select the **Environment** tab.

#. Near **Credential**, click :fa:`plus-circle` and depending on if you have SSH Keys or not, do *one* of the two following steps to add a new credential. If you want to keep it simple, use the password option:

   **SSH Keys**:
    - **Credential Name** - CENTOS
    - **Username** - centos
    - **Secret** - Key
    - **Key** - Paste in your private key or upload it.
   
   .. note:: Your SSH Key must start with **-----BEGIN RSA PRIVATE KEY-----**.
   
   **Password**:
    - **Credential Name** - CENTOS
    - **Username** - centos
    - **Secret** - Password
    - **Password** - nutanix/4u

   .. figure:: images/calm_self_09.png

#. Under **VM Configuration**

   - Select **Nutanix**
   - Expand **Linux**
   - **VM Name** - add prefix "default" to the name
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory** - 4GiB
   
     .. figure:: images/calm_self_10.png

#. Under **DISKS (1)**

   - **Image** - CentOS_76
   
     .. figure:: images/calm_self_11.png

#. Near **Network Adapters (NICS)**, click :fa:`plus-circle` and select for *NIC 1* your network, ex. **Primary**.

#. Select **CENTOS** for the **Credential**

   .. figure:: images/calm_self_12.png

#. Click **Save**.

Launching Blueprint from the Marketplace
........................................

#. From **Prism Central > Calm**, select |mkt-icon| **Marketplace** from the sidebar.

#. Select the Blueprint published as part of the previous exercise and click **Launch**.

   .. figure:: images/calm_self_13.png

#. Select the **Calm** Project and click **Launch**.

   .. figure:: images/calm_self_14.png

#. Specify a unique **Application Name** (e.g. <INITIALS>-CentOS).

   - Choose your prefer datacenter.
   - Type a password for the centos user.

   .. note::

     To see the configured VM details, expand the VM on **Service Configuration** tab.

   .. figure:: images/calm_self_15.png

#. Click **Create**.

#. Monitor the provisioning of the Blueprint until complete.

   .. figure:: images/calm_self_16.png

Takeaways
+++++++++

- By using pre-seeded Blueprints from the Nutanix Marketplace, users can quickly try out new applications.
- Marketplace Blueprints can be cloned and modified to suit a user's needs. For example, the pre-seeded LAMP Blueprint could be a starting point for a developer looking to swap PHP for a Go application server.
- Marketplace Blueprints can use local disk images or automatically download associated disk images. Users can create their own keys and slipstream them into Blueprints (via cloud-init) to control access.
- Developers can publish Blueprints to the Marketplace for fast and easy consumption by users.
- Blueprints can be launched directly from the Marketplace with no additional configuration from users, delivering a public cloud-like SaaS experience for end users.
- Administrators have control over what Blueprints are published to the Marketplace and which projects have access to published Blueprints.

.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
