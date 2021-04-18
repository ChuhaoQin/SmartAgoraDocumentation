###########################
Windows Installation Guide
###########################

Requirements
*******************

- Visual Studio – 2018 or higher
- Elasticsearch (for Hive server) – version 2.4.3 (recommended)
- Hive Server – version 1.0.2 (latest)
- XAMPP server for MySQL

Cloning the Source Code
*************************

- Place the *Go_Workspace* in C:/Users/username/. It contains the source code for Elasticsearch and scripts to run Hive server locally.
- Clone the source code of Dashboard by executing the command:

.. code-block:: bash

   git clone https://github.com/epournaras/SmartAgoraBackend

Installing Hive Server
************************

Follow the instruction |Hive_link| to get details for hive installation and setup. Hive server runs on http://localhost:8080 by default.

.. |Hive_link| raw:: html

  <a target="_blank" href="https://github.com/nytlabs/hive/blob/master/README.md#setup" target="_blank">here</a>

Executing Hive & Elasticsearch locally
****************************************

To run Hive server locally, follow either of the two steps given below:

1. Change the working directory to C:/Users/Username/Go_Workspace/bin and execute the following command in the command prompt:

.. code-block:: bash

  hive-server -index hive -esDomain localhost -esPort=9200 -port 8080 &


2. Manually execute the batch files:
  - /Go_Workspace/Elasticsearch-2.4.3/bin/Elasticsearch.bat
  - /Go_Workspace/bin/hive-server.exe


The following screen appears after successful run of Hive server.

.. _Figure 1:
.. figure:: Windows/Figure_1.png

Visit http://localhost:8080 to check the status. The following screen appears showing successful execution of hive server.

.. _Figure 2:
.. figure:: Windows/Figure_2.png

Installing XAMPP Server
****************************

We need XAMPP server to create the database for Dashboard. We can also go for other alternatives such as Apache, Nginx or MySQL Workbench etc.
XAMPP installation steps are given below:

1. Download XAMPP from https://www.apachefriends.org/index.html.
2. Open the downloaded file and configure XAMPP.

.. _Figure 3:
.. figure:: Windows/Figure_3.png

3. Uncheck "Tomcat", "Perl" and "Fake Sendmail" and click next.

.. _Figure 4:
.. figure:: Windows/Figure_4.png

4. Keep all the default settings. Click finish to start the XAMPP control panel.
5. Start the Apache and MySQL servers from XAMPP control panel.

.. _Figure 5:
.. figure:: Windows/Figure_5.png

6. Visit http://localhost/dashboard/ to see the administration module of XAMPP server.

.. _Figure 6:
.. figure:: Windows/Figure_6.png

Creating the Database
***************************

1. Visit http://localhost/phpmyadmin to open admin module for MySQL in XAMPP.
2. Create a database and update the name of database and its credentials in Dashboard/XMLParser/XMLParser/web.config and Dashboard/XMLParser/socialexperiment.sql files.

.. _Figure 7:
.. figure:: Windows/Figure_7.png

3. Import and execute Dashboard/XMLParser/socialexperiment.sql using phpMyAdmin to create all necessary tables in the Dashboard database.

.. _Figure 8:
.. figure:: Windows/Figure_8.png

4. You will see the following screen after successful execution of the script:

.. _Figure 9:
.. figure:: Windows/Figure_9.png

Installing Visual Studio
****************************

Visual Studio can be downloaded |visualstudio|. It is needed to build the solution file for the Dashboard.

.. |visualstudio| raw:: html

  <a target="_blank" href="https://www.visualstudio.com/downloads/" target="_blank">here</a>

Building the Solution File
**************************

1. Open Visual Studio and select “Open a project or solution”.

.. _Figure 10:
.. figure:: Windows/Figure_10.png

2. Select the solution file Dashboard/XMLParser/XMLParser.sln to open the Dashboard project.
3. Open Dashboard/XMLParser/XMLParser/Web.config and add database credentials for MySQL and host and port addresses for Hive server.
4. Build the solution file with CTRL+Shift+B. The solution file namely XMLParser.sln will be built inside the /Dashboard/XMLParser directory.

.. _Figure 11:
.. figure:: Windows/Figure_11.png

The Dashboard Welcome Screen
************************************

Visit http://localhost:44300 to reach the Login screen.
Sign up to explore the functionality of the Dashboard.

.. _Figure 12:
.. figure:: Windows/Figure_12.png
