.. _calm_paas:

---------------------
Platform as a Service
---------------------

.. note::

  Estimated time to complete: **20 MINUTES**

Overview
++++++++

PHP Application Server
++++++++++++++++++++++

In this exercise you will use the CentOS blueprint you created and install Apache + PHP with a demo PHP page. Calm tasks will help with the process of installing the new packages and create your first PaaS blueprint.

#. Within the Calm UI, select |blueprints| **Blueprints** in the left hand toolbar to view and manage Calm blueprints.

#. Select your IaaS blueprint, at the top click **Action > Clone**.

   .. figure:: images/calm_paas_01.png

#. Fill out **Blueprint Name** with *initials*-PaaSPHP

#. Click **Clone**

   .. figure:: images/calm_paas_02.png

#. Click **VM Details >**

#. Fill out the following fields:

   - **Name** - *ApachePHPAHV*
   - **Cloud** - *Nutanix*
   - **Operating System** - *Linux*

   .. figure:: images/calm_paas_03.png

#. Click **VM Configuration >**

#. Fill out the following fields:

   - **VM Name** - @@{DATACENTERS}@@-PHP-@@{calm_time}@@
   - **vCPUs** - *2*

     - Enable runtime (**Runtime** is specified by toggling the **Running Man** icon to Blue)

   - **Cores per vCPU** - *1*
   - **Memory (GiB)** - *4* 
   
     - Enable runtime

   .. figure:: images/calm_paas_04.png

#. Click **Save** at the top

Adding a credential
...................

To configure and install software after the virtual server has been provisioned, a credential objects is required by Calm for connecting to the server and run the tasks.

#. Click **Advanced Options (Optional)**

#. Scroll up and click **Add/Edit credentials**

   .. figure:: images/calm_paas_05.png

#. Click **+ Add Credentials**

#. Add the following credential (**Runtime** is specified by toggling the **Running Man** icon to Blue):

   +------------------------+--------------+-----------------+---------------+-------------+
   | **Credential Name**    | **Username** | **Secret Type** | **Password**  | **Default** |
   +------------------------+--------------------------------+---------------+-------------+
   | CRED_CENTOS            |  centos  (R) |    Password     | nutanix/4u (R)|      X      |
   +------------------------+--------------+-----------------+---------------+-------------+

   .. note::

     **(R)** in the table means *Enable runtime*

   .. figure:: images/calm_paas_06.png

#. Click **Done**

#. Select **Check log-in upon create**

#. Set **Credential** to **CRED_CENTOS**

   .. figure:: images/calm_paas_07.png

#. Click **Save**

Adding automation tasks
.......................

#. Scroll down up to **Packages (Optional)**

   .. figure:: images/calm_paas_08.png

#. Click **Edit** for *Package Install*

   You are now in the *Task Editor* where you can add your automation tasks and order them with the right flow.

#. Click **+ Add Task**

#. Click **+Task1**

#. Fill out the following fields:

   - **Task Name** - UpdateOS
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CRED_CENTOS
   - **Script**

     .. code-block:: bash
   
       sudo yum update -y

     .. figure:: images/calm_paas_09.png

#. Click **Add Task**

#. Click **+Task2**

#. Fill out the following fields:

   - **Task Name** - InstallApachePHP
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CRED_CENTOS
   - **Script**

     .. code-block:: bash
   
       sudo yum install -y httpd php
       sudo systemctl enable httpd.service
       sudo systemctl start httpd.service

     .. figure:: images/calm_paas_10.png

#. Click **Add Task**

#. Click **+Task3**

#. Fill out the following fields:

   - **Task Name** - CreatePHPPage
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CRED_CENTOS
   - **Script**

     .. code-block:: bash
   
       echo '
       @@{PHP_CODE}@@ ' | sudo tee /var/www/html/index.php

     .. note::

       Be aware we are calling a variable, PHP_CODE, that doesn't exist yet

     .. figure:: images/calm_paas_11.png

#. Click **Done**

#. Click **Save**

#. Fix the *warning*

   .. figure:: images/calm_paas_12.png

#. Click **App variables (2)**

#. Delete the variable **PASSWORD**, click the three dots and *Delete*. We will use the credential *CRED_CENTOS* that we have created above.

#. Click **+ Add Variable**

#. Add the following variable:

   +------------------------------------------------------------------------------------+----------------------------+
   |                                                                                    |   **Additional Options**   |
   +------------------------+-------------------+------------+------------+-------------+----------------+-----------+
   | **Variable Name**      |   **Data Type**   | **Value**  | **Secret** | **Runtime** | **Input Type** | **Mark**  |
   +------------------------+-------------------+------------+------------+-------------+----------------+-----------+
   | PHP_CODE               | Multi line String | Code below |            |      X      |     Simple     | Mandatory |
   +------------------------+-------------------+------------+------------+-------------+----------------+-----------+
   
   .. figure:: images/calm_paas_13.png

#. Click **Done**

#. Click **Save**

#. Fix the *warning*

   .. figure:: images/calm_paas_14.png

   If you remember we used the variable @@{PASSWORD}@@ in the *cloud-init* configuration. We need to update that variable with the new credential object.

#. Click **VM Configuration**

#. On **Guest Customization** you have to replace the word *centos* (there are two) with **@@{CRED_CENTOS.username}@@**. Give it a try, start typing **@@{** and you will see a drop-down list will all the available macros. Search for **CRED_CENTOS** and when you continue typing **.**, the properties for that variable will be shown.

   .. figure:: images/calm_paas_15.png

   .. figure:: images/calm_paas_16.png

#. Replace **@@{PASSWORD}@@** with the *secret* property of the **@@{CRED_CENTOS}@@** variable.

   .. figure:: images/calm_paas_17.png

#. To ensure your cloud-init is correct, replace this with the following block of code (this should be the same)

   .. code-block:: bash   
   
     #cloud-config
     users:
       - name: @@{CRED_CENTOS.username}@@
         sudo: ['ALL=(ALL) NOPASSWD:ALL']
     chpasswd:
       list: |
         @@{CRED_CENTOS.username}@@:@@{CRED_CENTOS.secret}@@
       expire: False
     ssh_pwauth: True

#. Click **Save**


Takeaways
+++++++++



.. |bp-icon| image:: ../images/blueprints_icon.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/blueprints.png
