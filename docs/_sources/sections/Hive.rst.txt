.. highlight:: java

****
Hive
****


Introduction
************

This is a special edition of HIVE software distribution designed for
the use with Smart Agora.
The main differences compared to the original HIVE are:

- the server code has been adapted to Elasticsearch 2.x and refactored
- the Go client API library and command line client have been added
- the data strcuctures and workflow have been adapted to Smart Agora requirements

In the :ref:`SMART AGORA Dashboard`, the hive server is used as a backend.
The hive server itself again depends on some repositories and so on. In the following a short dependecy list shows all needed repositories for running Hive.

* |Hive_link|

  * |Elastigo|

    * |Go-hostpool|
    * |Gou|
  * |Mux|
  * |Context|

All of those dependencies have been already combined in the |Hive-Crowd-Sensing| repository.

Building HIVE
*************

Code Structure
##############

The source code is kept in directory "src". The source
code tree has the following structure:

* src

  * fragata

    * hive

      * client |tab| - |tab| HIVE client API library
      * main

        * command |tab| - |tab| main program for HIVE command line client
        * server |tab| - |tab| main program for HIVE server
      * server |tab| - |tab| HIVE server API library
  * github.com

    * araddon |tab| - |tab| required packages
    * bitly |tab| - |tab| required packages
    * gorilla |tab| - |tab| required packages
    * jacqui |tab| - |tab| required packages

      * elastigo |tab| - |tab| Go interface to Elasticsearch
    * nytlabs

      * hive |tab| - |tab| original NYT version of HIVE server adapted to Elasticsearch 2.x (not used; included for reference only)


Building from the source code
#############################

To build the HIVE software from the source code, you will need the recent version
of Go programming language distribution installed on your computer (refer to
https://golang.org/doc/install for detail.)

Copy the HIVE "src" directory tree to your Go workspace directory (designated
via $GOPATH environment variable) and build HIVE server and command line client
executables as follows (assuming Go workspace directory as you current directory
and sub-directory ./bin available as location of executables):

go build -o ./bin/hive-server fragata/hive/main/server
go build -o ./bin/hive-command fragata/hive/main/command


Quick test:

To run HIVE, you need Elasticsearch 2.x server installed and running.

Starting Hive
*************

Start HIVE server with default parameters:

    ./bin/hive-server

if Elasticsearch is running on the local host and default port and you want
HIVE to listen port 8080 and store data in the Elasticsearch index named
"hive". If you want a different configuration, set respective parameters explicitly
as described in the HIVE server user documentation.

Make "test" your current directory. Specify the command line client environment
variables as described in the command line client user documentation, e.g.:

    export HIVE_HOST=127.0.0.1
    export HIVE_PORT=8080
    export HIVE_COOKIE_DIR=./cookie

Run the command line client with the following commands:

    ../bin/hive-command admin-setup true setup01.json

will reset the database and create a new project with an associated task and
asset as described in "setup01.json";

    ../bin/hive-command create-user basic user01.json

will create a new user with parameters described in "user01.json", make
it a current user and store respective user ID in a cookie file in "./cookie"
sub-directory;

    ../bin/hive-command user-assignment basic record >assign01.json

will generate an assignment for the current user and store it in
"assign.json".

Then simulate the user action of "solving" the assignment by inserting the payload
information into the assignment file; to do this, replace two properties
"State" and "SubmittedData" in the end of "assign.json" as follows:

.. code-block:: json

   code content
    "State": "finished",
    "SubmittedData": {
        "record": {
            "end": "2016-06-01T12:10:00Z",
            "sensors": ["accelerometer"],
            "start": "2016-06-01T12:00:00Z",
            "step": 1000,
            "accelerometer": [
                0.0, 1.0, 0.0, -1.0, 0.0, 1.0, 0.0
            ]
        }
    }

(see "assign_templ.json" as an example).

Submit the updated assignment file "assign.json" to HIVE:

    ../bin/hive-command user-create-assignment basic record assign01.json

You can use adminstrator commands of the command line interface to inspect
entities of your HIVE database (consult command line interface user documentation
for details).


.. |nbspc| unicode:: U+00A0 .. non-breaking space
.. |tab| unicode:: U+00A0 U+00A0 U+00A0 U+00A0 U+00A0 U+00A0 U+00A0 U+00A0 .. tab

.. |Hive_link| raw:: html

  <a target="_blank" href="https://github.com/nytlabs/hive" target="_blank">Hive Server</a>

.. |Elastigo| raw:: html

  <a target="_blank" href="https://github.com/jacqui/elastigo" target="_blank">Elastigo</a>

.. |Gou| raw:: html

  <a target="_blank" href="https://github.com/araddon/gou" target="_blank">Gou</a>

.. |Go-hostpool| raw:: html

  <a target="_blank" href="https://github.com/bitly/go-hostpool" target="_blank">Go-hostpool</a

.. |Mux| raw:: html

  <a target="_blank" href="https://github.com/gorilla/mux" target="_blank">Mux</a>

.. |Context| raw:: html

  <a target="_blank" href="https://github.com/gorilla/context" target="_blank">Context</a>

.. |Hive-Crowd-Sensing| raw:: html

  <a target="_blank" href="https://github.com/epournaras/Hive-Crowd-sensing/tree/master/Go_Workspace%20(Atif)" target="_blank">Hive-Crowd-Sensing</a>
