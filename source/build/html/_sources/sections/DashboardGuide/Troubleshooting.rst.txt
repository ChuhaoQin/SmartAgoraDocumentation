#################
Troubleshooting
#################


Mono Server
***********

If mono is not working as expected, try stopping it with: 

.. code-block:: bash

    sudo pkill -9 mono

Then, restart it again.

Dashboard execution process
******************************

Navigate to /Dashboard/XMLParser/XMLParser and run the following command to see the status of the process.

.. code-block:: bash

    screen –dr process_ID.processname

XAMPP server
**************

Visit the link http://localhost/ or http://localhost/dashboard to configure XAMPP server.

Hive server
*************

Visit the link http://localhost:8080 to check the status of the hive server.