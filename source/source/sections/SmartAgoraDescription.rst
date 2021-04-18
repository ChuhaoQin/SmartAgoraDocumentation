.. highlight:: java

.. role:: underline
    :class: underline

************************
SmartAgora Documentation
************************


Introduction
************

This documentation should give an overview of the SmartAgora Android application, if one intends do extend it.


Use case
********


Installation
************
To use this app, one must download and install Android Studio.
You can find the application and how to install it |location_link|.

.. |location_link| raw:: html

   <a href="https://developer.android.com/studio" target="_blank">here</a>

Then one simply imports this project as an Android Studio project.


Usage
*****
A documentation on how to use the Application can be found here: :ref:`Smart Agora Tutorial`.


Overview
********

.. _Figure1:
.. figure::  SmartAgoraDescription/Smartagora\ Diagram.png

Above you see an overview of the most important classes of the :ref:`HomeActivity class`, and how they interact with each other.
Note that a lot of classes are omitted, as the diagram would not be easily understandable with all of them.


Mechanics
*********
This section explains the main mechanics of the application and how they work together.

Upon starting the application, the :Ref:`HomeActivity class` is called, which initialises all visual elements, as well as the GPS component.


The Basics
----------

This application lets the user answer different questions, which then can be sent to different servers.
There are different :ref:`Modes`, which make the questionnaire behave differently.
In some modes, the answers to a question can generate a score, which can vary for every question and the qiven answer for it.
The idea is, that the user should only be able to answer a specific question when he is in the vicinity of a specific checkpoint.


GPS component
-------------

The GPS component is a custom LocationListener.
When the coordinates get updated, the new coordinates, together with the accuracy is saved via sharedPreferences.
After this a sticky gets posted with the following message:

.. code-block:: java

  PreferencesKeys.LOCATION_UPDATED

Further explanation on sticky messages are found here: :ref:`Sticky Events`

This means that on every change of the coordinates, a check whether to start the :ref:`Vicinity Checking` is done.


Sticky Events
-------------
Sticky messages can used to broadcast messages to specific methods and functions, it can also be used to create listeners for specific events.
(Reference: |location_link2|)

.. |location_link2| raw:: html

   <a href="http://greenrobot.org/eventbus/documentation/configuration/sticky-events/" target="_blank">Sticky Events</a>

In order for a method to listen to sticky events, it needs to have the flag

.. code-block:: java

  @Subscribe(sticky = true, threadMode = ThreadMode.MAIN)

To poste a sticky the following code needs to be executed

.. code-block:: java

  EventBus.postSticky( MESSAGE );

In this application, stickies are used in one place, in which gets checked if any questions are loaded and if so, vicinity checking is launched. See more about this here: :ref:`Vicinity Checking`


Vicinity Checking
-----------------
When vicinity checking is launched, all 9 seconds a thread is checking if the phone is in the vicinity of any POI (point of interest).

If the phone is in any POI, the thread opens the corresponding question.
For that, it first figures out which mode the current assignment is in.
Once figured out, it creates an instance of the respective mode class and calls it's call() method.
The mode class then does the rest, explained in the section :ref:`Modes`.


Modes
-----
There are 4 different modes in this application:

+------------+---------------------------------------------------------------------------+
| Mode       | Key Difference                                                            |
+============+===========================================================================+
| Simple     | In this mode, all POI's can be visited in any order                       |
+------------+---------------------------------------------------------------------------+
| Sequence   | In this mode, the POI's need to be visited in a before specified order    |
+------------+---------------------------------------------------------------------------+
| Decision   | In this mode, future question can depend on the answers of past questions |
+------------+---------------------------------------------------------------------------+
| DIASSimple | This mode is the same as Simple, with the addition that all data get also |
|            | sent to the :ref:`DIAS` server                                            |
+------------+---------------------------------------------------------------------------+

Each of the mode classes have a constructor and a method called "call()", which gets called as soon as the phone is in the vicinity of a POI.

The constructor of all classes initialise its elements.
The call method rechecks if the phone is in range of the POI, and if so shows the question to be answered.
The Mode class is also responsible of the progressbar and saving of the given answers.

Finally, if the mode is DIASSimple, the answered values get sent to the :ref:`DIAS` server.

Database
--------
There are is a class called :ref:`RoomDatabaseHandler`, which gets used in many different places to access the database.
The Database is based on |SQLite| and there are several queries prepared to get the following things out of it:

* A list of answered questions
* A list of the ids of the answered questions
* A list of the of answered questions for the current checkpoint.
* The previously given answer for a specific question
* A list of all the answers for a given assignment

Servers
-------
There are different servers connected with this app.

**The Dashboard:**
The dashboard is an online interface to create the assignments used in the application.

**DIAS**
DIAS is a distributed aggregation service.
A documentation about DIAS can be found here. TODO: add link

**Smart Agora**



The parts
*********

In this section, we will take a look at the parts from the diagram :numref:`Figure1`.


HomeActivity class
------------------
This Activity is the heart of this application, it is the activity, which gets launched together with the application.

OnCreate
########

This method initialises all needed layouts and some of the objects for this activity.
The list of initialised objects include:

* The view of the activity
* The the progressbar for an assignment
* The logos used on the screen
* The navigation bar at the bootom
* The actionbar at the top of the application
* The actionbar drawer
* The openStreetMap
* The skipped, answered and unanswered questions for the current assignment.

It also calls the methods :ref:`initServiceConnection`, :ref:`initVariables`, :ref:`initViews`, :ref:`setBottomNavigationView` and :ref:`slideMenuData`.

animateCameraTo
###############

Given that the map is already loaded, this moves the map position to the given latitude/longitude

buildAlertMessageNoGps
######################

This method shows a alertDialog to ask the user to enable gps for this application.

connectAsyncTask
################

This is a class used as a callback for the response from :ref:`DrawCompleteRoute`.
It draws the path on the map of the response of the google api by calling the method :ref:`drawPath`.

DataScientistId
###############

The DataScientistId is used be able to select between different sets of assignments.

decodePoly
##########

The string is an encoded polygon line from Google, this method decodes the string and adds the points into an ArrayList.

DrawCompleteRoute
#################

Draws a route on the map of all start and destionation locations saved in the database, handled in the class :ref:`QuestionsActivity`.
Uses the class :ref:`makeURL` to create a url to query to "https://maps.googleapis.com/maps/api/directions/json". It also adds a callback called :ref:`connectAsyncTask`, which then handles the response.


drawPath
########

Given a json string, containing an array mapped to the the key "routes", this function takes the first element and calls the method :ref:`decodePoly` to decode the containing polygon line.
The polygon line then gets added to open street maps. Also the method sets an onClickListener on the lines, turning them blue if they were grey and the user clicked on it.

The Json string needs to contain the following elements:

.. code-block:: javascript

  {
    routes :
    [
      {
        overview_polyline :
        {
          points : encodedString
        }
      },
    ]
  }

The string "encodedString" is encoded with google maps, the method :ref:`decodePoly` can decode it into a polygon line.


getDataFromDB
#############

Gets data from the database, by calling getDataFromRoomDB of the class :ref:`QuestionHandling`


initVariables
#############

This method initialises all variables for this class.
The list includes elements, which are initialised every time, and others, which are only initialised, when they were uninitialised before.

Only when uninitialised:

* the application context
* the database handler
* the list with all used sensors
* the list with all names of the used sensors
* the hashMap with all sensor readings
* the list with the likertScale answers for the DIAS mode
* the deviceId*
* the :ref:`QuestionHandling` instance
* the list for the Dias checkpoint questions
* the arrayList for the progressBar values
* the "loading..." dialog
* the list of all used :ref:`ProjectModel`
* the JSONObject lists called "tasks" and "tasks1", but only if **both** are uninitialised.

Initialised every time:

* the current activity (to the value 'this')
* the :ref:`ActivityStackManager`
* the numbers of credits (back to the value 0)
* the textView for showing the answered value in DIAS mode
* the list for the AnswerEntity, the AnswerEntity is one of :Ref:`the entities` used for the :ref:`RoomDatabaseHandler`.



initViews
#########

Asks for the GPS permission for this application.

makeURL
#######

Given the start and destination coordinates, this function creates an url drawing a line between the given points, which can be interpreted by open street maps.

manageDataScientistId
#####################

This method shows a window, letting the user set a new DataScientistId.

MapStartPosition
################

This method fisrt checks if the questions are already loaded and loads them if not already done.
Then it initialises the :ref:`StaticModel` with the new filenames of utils and the question list, clears the list with the skipped questions and the queued questions and initialises them with new ArrayLists.
If there is a start and destination point stored in the :ref:`QuestionsActivity`, the databades is reloaded by calling :ref:`getDataFromDB` and the loaded route is then drawn on the map by calling :ref:`DrawCompleteRoute`.
If there is not start and destionation point, but the assignment mode is of Type simple or DIAS_Simple, the database is reloaded anyway.

onActivityResult
################

This method is called as a callback, if an Activity is called, with the intent on receiving a result.
As it seems, this is an artefact, as no activity was started with the corresponding "startActivityForResult()" method.

onBackPressed
#############

This method is called, when the user presses the back button on his phone.
This method closes all open popups or drawers.
If there is no popup or drawer opened, the method closes the application after being called twice in fice seconds.
When closing, it calls the methods :ref:`stopServiceConnection` and finish(), the latter actuallay closes the whole application.

onDestroy
#########

This method is called, when the activity is closed.
It disconnects from the :ref:`DIAS` server.

onEvent
#######

This method is a callback, called by :ref:`Sticky Events`.
It is called when the location changes.
It first checks if the argument is LOCATION_UPDATED (from :ref:`PreferencesKeys`), because this method is only listening on those messages.
Then it draws the vicinity ellipse around the user by calling :ref:`drawMarkerWithEllipse`.
Finally, checks if any questions are loaded and if so, it calls the method :ref:`StartCheckingForQuestionInRadius`

onMapReady
##########

This method is called as a callback, once open street maps is ready and the map is available.
It initialises the map and saves it in a field for later usage.

onPause
#######

This method is called, when another activity is opened.
It stops the sensorListener and the GPS service.

onResume
########

This method is called, when the activity is reopened.
It restarts the GPS service and sensorListener.

onSensorChanged
###############

This method gets called by the Android SensorManager, when a sensor value changes.
It saves the the reading, together with the sensor type, in the sensorReading list.

onAccuracyChanged
#################

This method gets called by the SensorManager, when the accuracy of a sensor changes.
It doesn't do anything.

onStart
#######

This mehthod initialises the sensor service.

onStop
######

This method is called, when the listener is stopped.

placeMarkers
############

Given an index i, it places a marker at the position, where question i is located, given question i exists.
If the position was already visited and the question answered, the marker will be black, otherwise it will be blue.

setBottomNavigationView
#######################

This method sets the navigationView on the bottom of the screen and initialises all the buttons for it, such as:

* | :ref:`DataScientistId` :
  |   This buttons lets the user set a new :ref:`DataScientistId`
* | SelectProject:
  |   This button lets the user select a new project from the list of available ones, connected with the selected :ref:`DataScientistId`
* | GetAllAssignments:
  |   This button lets the user select and load a new assignment of the list of available ones, connected to the selected project.
* | completeAssignment:
  |   This button lets the user finish and commit the current assignment to the server.
* | help:
  |   This button brings the user to the help screen.

setupDrawerContent
##################

Sets up the side menu.

slideMenuData
#############

Sets up the data for the slide menu header and children, the items of the menu are:

* Transport mean
* Survey questions
* Checkpoints
* Visited Points
* Route location

When clicked on an item, a dropdown list expands under the selected item, showing the answer.
The will only be shown, if available. Which means only if an assignment is loaded, and the assignment supports the selected measure.

StartCheckingForQuestionInRadius
################################

This method checks if any questions are loaded and if it is not interrupted.
If everything is fine, it checks if it is in the vicinity of any nearby question.
If so, it creates a class of the corresponding mode (explained in :ref:`Modes`) for the question and calls the method "call".

startRepeatingTask
##################

This method starts a runnable, which puts itself in a callback loop afterwards. It calls the method :ref:`update` in each iteration.

stopRepeatingTask
#################

This method stops the repeating task, launched in the method :ref:`startRepeatingTask`.

update
######

This method gets the latest reading of the current selected sensor.


StaticModel
-----------

The StaticModel class contains static values, shared over the whole project

Some of them are Mutable, some of them are final.

Final fields
############

There are fields for the overall database:

.. list-table::
   :header-rows: 1

   * - Field
     - Type
     - Description
   * - DATABASE_VERSION
     - int
     - contains the current version of the database
   * - DATABASE_NAME
     - string
     - contains the used name for the |SQLite| database

There are fields for the database tables:

.. list-table::
   :header-rows: 1

   * - Field
     - Type
     - Description
   * - TABLE_QUESTIONS_DATA
     - string
     - name for the questions table
   * - TABLE_ANSWERS_DATA
     - string
     - name for the answers table
   * - TABLE_START_AND_DESTINATION
     - string
     - name for the start and destination table
   * - TABLE_SENSOR_DATA
     - string
     - name for the sensor data table

There are fields for the table column names:

.. list-table::
  :header-rows: 1

  * - Field
    - Type
    - Description
    - Used in table
  * - FILESNAME
    - string
    - column name for the file
    - Question, Answer, Start And Destination, Sensor data
  * - ASSIGNMENTSTATE
    - string
    - column name for the assignment state
    - Start And Destination
  * - ID
    - string
    - column name for the id, which is the key of the table
    - Question, Answer, Start And Destination, Sensor data
  * - QUESTIONID
    - string
    - column name for question id
    - Question, Answer, Sensor data
  * - QUESTION
    - string
    - column name for actual question
    - Question, Answer, Sensor data
  * - NOISE_SENSOR
    - string
    - column name for noise sensor reading
    - Question, Sensor data
  * - LIGHT_SENSOR
    - string
    - column name for light sensor reding
    - Question, Sensor data
  * - ACCEL_SENSOR
    - string
    - column name for acceleration sensor reading
    - Question, Sensor data
  * - PROXIMITY_SENSOR
    - string
    - column name for proximity sensor reading
    - Question, Sensor data
  * - GYRO_SENSOR
    - string
    - column name for gyroscope sensor reading
    - Question, Sensor data
  * - LOCATION
    - string
    - column name for location
    - Question, Sensor data
  * - TIME
    - string
    - column name for measured time
    - Question
  * - FREQUENCY
    - string
    - column name for measurement frequency
    - Question, Sensor data
  * - SEQUENCE
    - string
    - column name for question sequence
    - Question
  * - VISIBILITY
    - string
    - column name for question visibility
    - Question
  * - MANDATORY
    - string
    - column name for mandatoriness
    - Question
  * - VICINITY
    - string
    - column name for vicinity
    - Question
  * - TYPE
    - string
    - column name for question type
    - Question
  * - LATITUDE
    - string
    - column name for latitude
    - Question, Answer
  * - LONGITUDE
    - string
    - column name for latitude
    - Question, Answer
  * - OPTIONS
    - string
    - column name for question options
    - Question
  * - ANSWERS
    - string
    - column for given answers
    - Answer
  * - START_LATITUDE
    - string
    - column name for the start latitude of a start-destination line
    - Start and Destination
  * - START_LONGITUDE
    - string
    - column name for the start longitude of a start-destination line
    - Start and Destination
  * - DESTINATION_LATITUDE
    - string
    - column name for the destination latitude of a start-destination line
    - Start and Destination
  * - DESTINATION_LONGITUDE
    - string
    - column name for the destination longitude of a start-destination line
    - Start and Destination
  * - CAR
    - string
    - column name for car
    - Start and Destination
  * - TRAIN
    - string
    - column name for train
    - Start and Destination
  * - CYCLE
    - string
    - column name for cycle
    - Start and Destination
  * - WALKING
    - string
    - column name for walking
    - Start and Destination
  * - MODE
    - string
    - column name for the used transportaion mode
    - Start and Destination
  * - QID
    - string
    - column name for question id used by notifications
    - Question, Answer
  * - STOPNAME
    - string
    - column name for the stop name of a question
    - Question, Answer
  * - DEFAULTCREDIT
    - string
    - the defualt credit given
    - Start and Destination
  * - COMBINATION
    - string
    - column name for the question combinations
    - Question
  * - ASSIGNMENTID
    - string
    - column name for the assignment id for the start-destination line
    - Start and Destination
  * - TIMEATANSWERING
    - string
    - column name for measured time whilst answering the question
    - Answer
  * - TIMEATSENSORING
    - string
    - measured time for given sensor recording
    - Sensor data
  * - CREDIT
    - string
    - column name for the actually earned credit for a given answer
    - Answer

There are also fields for the sensor measuring:

.. list-table::
   :header-rows: 1

   * - Field
     - Type
     - Waiting time between two measurements
     - Measurements per second
   * - LOW_TIME_INTERVAL
     - int
     - 2 seconds
     - ~0.5
   * - MED_TIME_INTERVAL
     - int
     - 250 ms
     - ~4
   * - HIGH_TIME_INTERVAL
     - int
     - 200 ms
     - ~5

Mutable fields
##############

.. list-table::
   :header-rows: 1

   * - Field
     - Type
     - Description
   * - skippedQuestions
     - List<Integer>
     - a list containing all skipped questions
   * - questionsQueue
     - List<Integer>
     - a list containing all queued questions
   * - questionsSensorsQueue
     - List<Integer>
     - | a list in which the last element of shownQuestionQueue is added,
       | sometimes instead of the last element, just the size of shownQuestionQueue is added,
       | the added elements are never used, as only ever values are added
   * - shownQuestionsQueue
     - List<Integer>
     - a list in which all shown questions are added, to keep track how many questions were actually already shown
   * - XMLLoaded
     - boolean
     - shows if the XML is loaded up


Modes classes
-------------

In SmartAgora, there are different modes questions can be in, explained in :ref:`Modes`.
For each of those modes, there is a class with a method called "call", which is used to executed the assignment.
There are the following mode classes:

* SimpleMode
* SequenceMode
* DecisionMode
* SimpleDIASMode

Also, there are two helper classes called :ref:`ModeFunctions` and :ref:`SensorsInfo`, used in all four modes.

all for modes classes have a method called call.

In the call method, it gets checked if the phone is in the vicinity of a POI.

If so, the corresponding questions get shown to the user.
Upon answering, all questions get saved in the database.
Once all questions for an assignment are answered, the questions get pushed to the server, and the :ref:`Vicinity Checking` gets stopped.
In addition to that, in the DIASModeSimple, all answers get pushed to the DIAS server upon answering.

In the following list are classes used in those 4 classes:

* :ref:`ModeFunctions`, for saving answers to the database
* :ref:`SensorsInfo`, for saving sensor measurements to the database
* AnswerThread, for loading answers from the database, AnswerThread is one of the :ref:`DataBaseThreads`
* :ref:`QuestionHandling`, for loading questions from the database
* QuestionThread, for loading questions from the database in a specific sequence (only in SequenceMode), QuestionThread is one of the :ref:`DataBaseThreads`
* |StateProgressBar|, for showing the progress during question answering
* SensorManager, for measuring the sensors
* :ref:`DIAS` Library, for sending values to the DIAS server (only in DIASmode)
* :ref:`StaticModel`, for updating the different question queues

.. |StateProgressBar| raw:: html

  <a href="https://github.com/kofigyan/StateProgressBar" target="_blank">StateProgressBar</a>

ModeFunctions
-------------

This class gets used by all 4 :ref:`Modes classes`, it contains some universally needed functions.

saveAnswer
##########

This method saves the given answer int the |SQLite| answer table, using the :ref:`RoomDatabaseHandler`.

updateStateBar
##############

This method updates the progressbar, according to the amount of questions already answered, compared to the total amount of questions.

getCombinationId
################

Given an id list and a CombinationEntity List, this method returns the index of the latter list, which represents the idList.

showAllAnswers
##############

Sets the progressbar to 100%.
Asks if the user wants to answer the skipped questions. Calls :ref:`showSkippedQuestions` if yes pressed.

If there are unanswered question, the user gets asked to answer them in the correct POI's.

showSkippedQuestions
####################

Shows all skipped questions to answer them again.

checkEndSurvey
##############

Checks if the phone is in the vicinity of the destination point.



SensorsInfo
-----------

This class saves the measured sensor data.

Constructor
###########

Here, all important fields are initialised, such as:

* context, the main activity context
* sensors, list of all used sensors
* sensorNamesList, list of the names of the sensors, in the same order as sensors
* mSensorReading, a hashmap with a mapping between sensor name and measurement, converted to a string, contains the most up to date value for every sensor
* frequency, the frequency used for the sensors.

saveSensorsValue
################

This method gets the index of which sensor used, matched with which value measured.
First, the list of used sensors for the current assignment is loaded. Then, the list of the names of those sensors is remade to match the sensors list.

Then, the frequency is read out and the method :ref:`saveSensorDataAtIntervals` is called with the arguments of the index, the frequency and the measured value.

The list of sensors, as well as the frequency is accessed from the :ref:`QuestionsActivity`, from the list element "assignmentQuestions" at given index.

saveSensorDataAtIntervals
#########################

This method takes a given question index, a timeinterval and a answer value.

It takes the length of the measurement time from the :ref:`QuestionsActivity`'s current guestion, given by the provided index.

it creates a list to store all measured sensor values as json strings and creates a |CountDownTimer| with the provided length and interval length.
With the given frequency, the timer then updates the time, location and noise level (with :ref:`getNoiseLevel`) in the mSensorReading hashMap and backs up all current sensor values to the :ref:`RoomDatabaseHandler`.
It also creates a json of the readings and puts it in the json list evey time.

When the timer finished, it takes the given answer of the user, together with all json elements of the list, and calls the method :ref:`saveDataToFile` of :ref:`FileHandling`, to save it into a file.

.. |CountDownTimer| raw:: html

  <a href="https://developer.android.com/reference/android/os/CountDownTimer" target="_blank">CountDownTimer</a>

getNoiseLevel
#############

This method is used internally to measure the current noise level of the environment.

RoomDatabaseHandler
-------------------

The RoomDatabaseHandler extends the |RoomDatabase|, which is a little bit more complicated than most java objects.

The idea is, that this handler handles an |SQLite| database. For that the programmer needs to create "data accsess objects", or short DAO (more on them here: :ref:`the DAO's`).
One can create as many DAO's, as one wants, the key is to declare them in the handler. All those DAO's are interfaces, and for every interface, a function needs to be declared in the handler:

.. code-block:: Java

   public abstract <InterfaceName> get<InterfaceName>();

so the return type and everything after "get" in the function name must match the desired DAO.
One can also create migrations.

The class needs to have the following addition before the class declaration:

.. code-block:: Java

   @Database(entities = {<All entity.class objects here>}, version = <version_number>, exportSchema = false)

More on entities here: :ref:`the entities`.

.. |RoomDatabase| raw:: html

  <a href="https://developer.android.com/reference/android/arch/persistence/room/RoomDatabase" target="_blank">RoomDatabase</a

For each table, there is a DAO, an entity and a thread.

the DAO's
#########

All the DAO's need to have the same schema:

There needs to be an insert function with a specific Entity as parameter in the following way:

.. code-block:: Java

   @Insert(onConflict = <chosen strategy>)
   void insert(<entity class> element)

possible strategies are: REPLACE, ROLLBACK, ABORT, FAIL, IGNORE.

For getting values, there are two possible options, the first one is to only return one element of the database, in this case the return type is a list of the specific type in the DB:

.. code-block:: Java

  @Query("SELECT Question.id FROM Answer JOIN Question on (Answer.assignmentId AND Answer.questionId) = (Question.assignmentId AND Question.id) join Assignment on Question.assignmentId = Assignment.id WHERE Assignment.name=:assignmentName ")
  List<Integer> getAnsweredQuestionIds(final String assignmentName);

As seen, the query is a normal SQL querie in a @Query block before the function definition, function parameters can be used for comparison with "=:assignmentName".

The other option is to get multiple rows per element, in this case a list of entities is returned:

.. code-block:: Java

  @Query("SELECT Answer.id,Answer.answer ,Answer.questionId,Answer.assignmentId,Answer.credits,Answer.answeringTime, Answer.aggregateValue FROM Answer JOIN Question on Answer.questionId = Question.id and Answer.assignmentId = Question.assignmentId  join Assignment on Question.assignmentId = Assignment.id WHERE Assignment.name=:assignmentName ")
  List<AnswerEntity> getAnsweredQuestions(final String assignmentName);


the entities
############

The entities define the actual tables.

in the @Entity before the class definition, the table key information is defines, such as foreign keys or indices in the following way:

.. code-block:: Java

   @Entity(
     foreignKeys = {
       @ForeignKey(
         entity = <another entity class>,
         parentcolumns = <columns from other class>,
         childcolumns = <columns in this entity refering to the foreign parent columns>,
         onDelete = <action>,
         onUpdate = <action>
       )
     }

     indices = {
       @Index(value = "sensorId")
     }

   )

The foreign keys are keys used in other tables, that are linked with values in this table.

The possible actions are: CASCADE, NO_ACTION, RESTRICT, SET_DEFAULT, SET_NULL
If no action is defined, NO_ACTION is used.

In the class, all columns need to be defined by creating private fields for each row.

evey element need the following addition:

.. code-block:: Java

   @ColumnInfo(name = <column name as a string>)

the primary key also needs the following:

.. code-block:: Java

  @PrimaryKey(autoGenerate = <if true, the key gets created automatically>)

The fields can also have getter and setter methods, but they are not necessary.

DataBaseThreads
###############

There is a class for every table in the database querying requests for the specific table, the format for every method is the same:

.. code-block:: Java

   public List<normal datatype or database entity> getSomething(<arguments used for querying>) throws ExecutionException, InterruptedException {
      return databaseHandler.getAnswerDao().getSomething(<arguments used for querying>);
   }

ActivityStackManager
--------------------

The ActivityStackManager gets launched with the app.
It is a singleton, which means, all classes calling :ref:`getInstance` get the same instance of the class.

This class is responsible for launching the different activities of this application.

getInstance
###########

This method is statically typed.
it returns the statically saved instance of this class, always returns the same instance.

startMainHomeScreen
###################

Starts up the :ref:`HomeActivity class`.

startLocationService
####################

Starts the :ref:`LocationService`, if not already launched.

returnLocationFlag
##################

Returns true, if the activity is already started.

stopLocationService
###################

Stops the :ref:`LocationService`, if running.


LocationService
---------------

Once started, the LocationService is listening on the gps sensor for location updates and propagates them to the correct places.

onCreate
########

In onCreate, the application context gets set.

onStartCommand
##############

This method gets called once the LocationService was started via the :ref:`ActivityStackManager`.
When this method is called the first time after it was *installed*, the user gets asked for Access to GPS and Network.
Once granted, the LocationUpdatesListener, which is declared in this class, is started.

onDestroy
#########

This method gets called, once the service is stopped and the object gets destroyed (either from the application or the garbage collection).
It stops the LocationUpdatesListener from listening to the GPS sensor.

LocationUpdatesListener.onLocationChanged
#########################################

The LocationUpdatesListener only has one method, which is really used: onLocationChanged(final Location location).
The location then gets saved in Memory.
Then, a sticky gets posted, with the key LOCATION_UPDATED. (Read more about :ref:`Sticky Events`)

This sticky then causes the method :ref:`onEvent` of the HomeActivity to be called.

Splash Activity
---------------

This activity gets launched together with the application.

onCreate
########

In this method, first the bootup logo gets displayed.
Next, the :ref:`ActivityStackManager` gets instanciated and :ref:`startLocationService` gets called.

.. figure::  SmartAgoraDescription/app_icon.png
  :scale: 40%

Afterwards, the :ref:`HomeActivity class` is started, again via the :ref:`ActivityStackManager` with the function :ref:`startMainHomeScreen`.

PreferencesKeys
---------------

The Class contains all keys used to store data via sharedPreferences.

.. list-table::
   :header-rows: 1

   * - Field
     - Description
   * - LATITUDE
     - The latitude of a gps coordinate
   * - LONGITUDE
     - The logitude of a gps
   * - LOCATION_UPDATED
     - Used for the postSticky in :ref:`LocationUpdatesListener.onLocationChanged` and the callee in :ref:`onEvent`.
   * - GPS_OFF
     - Used for the postSticky in :ref:`onStartCommand`, if the GPS provider is offline.
   * - ACCURACY
     - The accuracy of the sampled gps points
   * - DATA_SCIENTIST_ID
     - The stored dataScientist id for distinguishing between different sets of assignments
   * - PROJECT_ID
     - The stored project id for distinguishing between different projects
   * - AUTO_ASSIGNMENT
     - If the assignment should be automatically loaded.
   * - HELP
     - The help text for the current assignment

DeviceUuidFactory
-----------------

This class is used to generate a unique user id for the phone.

constructor
###########

In this method, the UUID gets created, it is basically the Android ID.
If the Android ID should not be available, then a random UUID gets created.

getDeviceUuid
#############

Returns the created UUID.

Ellipse
-------

This class is used to create an ellipse and some basic arithmetic operations on it.

getAngle
########

Returns the angle of rotation of the ellipse.

createEllipse
#############

This method creates the ellipse.
It creates 20 points along the ellips in the ellipsePointsList, given the radius in x and y direction and the angle.

IsInEllipse
###########

This method returns true, if the given point is in the ellipse.

drawMarkerWithEllipse
#####################

Draws the ellips on the openStreetMap, which the user sees.

FileHandling
------------

This class is responsible for saving sensor data and answers for questions into a file.

constructor
###########

an instance of a class called SavingDataToFileAsync, which is declared in FileHandling, is instanciated.

saveDataToFile
##############

Given sensor measurements, a question id and the corresponding answer, those information get saved in a file, which name is given by :ref:`utils`.filename

QuestionHandling
----------------

This class handles everything with the assignment questions currently loaded.

constructor
###########

Initialises thie application context, the question number and number of shown questions (to zero) and calls :ref:`initValue`.

initValue
#########

This method initialises a couple of objects used in this class, such as

* The currently used ellipse
* The :ref:`DatabaseHandler`
* A list for all loaded questions
* A list for the questions in order of the route
* A list with all visited points for the current series of questions

getQuestionNumber
#################

Gets the current question number.

setQuestionNumber
#################

Sets the current question number.

setShownQuestion
################

Sets the shownQuestion list to the given list.

getRouteQuestions
#################

Returns the questions in the route order

getmEllipse
###########

Returns the used ellipse.

getDataFromSavedFile
####################

Gets the data from the :ref:`DatabaseHandler` by calling :ref:`getFilesData` and saves them in the SampleDataModel two instances of :ref:`MainModel`, one is called "questions" and one "selectedQuestions".

getDataFromRoomDB
#################

Gets the current assignment and all questions of it by the QuestionThread and AssignmentThread out of the :ref:`DataBaseThreads` and loads them into the QuestionsActivity.

QuestionInEllipseNew
####################

Returns the question of the nearest POI, if one is in range, returns -1 otherwise.

getCurrentQuestionNew
#####################

Returns the current question given by questionNumber.

addQuestionInVisitedPointsNew
#############################

Adds the question, given by the argument qNumber, to the list of visited questions.

isDoneNew
#########

Returns true if the given point was already visited.

utils
-----

This class handles all file storing and loading and some other utility operations.

showFileChooser
###############

Creates a Filechooser dialog on the secreen from a installed file manager.

getDataFromFile
###############

Returns a string, encoded as an XML file, from the given file location.

haveNetworkConnection
#####################

Checks, if there is a working connection to the internet, returns true if so;

getSensorData
#############

Returns the contents of the file given in the static field "filename".

initializeProgressDialog
########################

Initialises the progress dialog used when syncing with the server.

registerSensorListener
######################

Returns a |SensorManager|, with the Accelerometer, Gyroscope, Light and Proximity sensor already registered.

updateRegisterSensorListener
############################

Registers the Accelerometer, Gyroscope, Light and Proximity sensors to the given |SensorManager|.

unregisterSensorListener
########################

Unregisters the Accelerometer, Gyroscope, Light and Proximity sensors from the |SensorManager|.

buildAlertMessageNoGps
######################

Creates and shows an alertDialog, telling the user to enable gps.

makeURL
#######

Given a source, a destionation and a tavel mode, this method creates an url for querying a google API.
The response then contains markers, which can be added the the openStreetMap.

getDiasCheckPointLeaveStatus
############################

Creates and fires a timer, which periodically checks if the user is far enough away of the checkpoint.
Once the minimum distance is reached, a flag is set in the HomeActivity, such that afterwards the device appropriately leaves from the DIAS network.

distance
########

Calculates the distance between two given points.

deg2rad
#######

Converts values from [0, 360) to values from [0, :math:`\pi`).

rad2deg
#######

Converts values from [0, :math:`\pi`) to values from [0, 360).

getUniqeCheckpointId
####################

Creates a |CRC2| hash from the lat and long value.


.. |SensorManager| raw:: html

  <a href="https://developer.android.com/reference/android/hardware/SensorManager" target="_blank">SensorManager</a>

.. |CRC2| raw:: html

  <a href="https://docs.oracle.com/javase/8/docs/api/java/util/zip/CRC32.html" target="_blank">CRC2</a>

DatabaseHandler
---------------

The databaseHandler is used for accessing and retrieving local data.
It uses an |SQLite| database.

The used tables look like the following ones, the keys are underlined.
All words written in caps lock are static fields from :ref:`StaticModel`.


**TABLE_QUESTIONS_DATA**

.. list-table::
   :header-rows: 1

   * - :underline:`ID`
     - QUESTION
     - LATITUDE
     - LONGITUDE
     - TYPE
     - OPTIONS
     - FILESNAME
     - NOISE_SENSOR
     - LIGHT_SENSOR
     - ACCEL_SENSOR
     - GYRO_SENSOR
     - PROXIMITY_SENSOR
     - LOCATION
     - TIME
     - FREQUENCY
     - SEQUENCE
     - VISIBILITY
     - MANDATORY
     - QUESTIONID
     - QID
     - STOPNAME
     - COMBINATION
     - VICINITY
   * -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -
     -


**TABLE_ANSWERS_DATA**

.. list-table::
  :header-rows: 1

  * - :underline:`ID`
    - QUESTION
    - LATITUDE
    - LONGITUDE
    - FILESNAME
    - ANSWERS
    - QUESTIONID
    - CREDIT
    - QID
    - STOPNAME
    - TIMEATANSWERING
  * -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -


**TABLE_START_AND_DESTINATION**

.. list-table::
  :header-rows: 1

  * - :underline:`ID`
    - START_LATITUDE
    - START_LONGITUDE
    - DESTINATION_LATITUDE
    - DESTINATION_LONGITUDE
    - CAR
    - TRAIN
    - CYCLE
    - WALKING
    - FILESNAME
    - VACINITY
    - MODE
    - DEFAULTCREDIT
    - ASSIGNMENTID
    - ASSIGNMENTSTATE
  * -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -


**TABLE_START_AND_DESTINATION**

.. list-table::
  :header-rows: 1

  * - :underline:`ID`
    - FILESNAME
    - ACCEL_SENSOR
    - LIGHT_SENSOR
    - NOISE_SENSOR
    - GYRO_SENSOR
    - PROXIMITY_SENSOR
    - LOCATION
    - FREQUENCY
    - QUESTION
    - QUESTIONID
    - TIMEATANSWERING
  * -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -
    -

onCreate
########

Here, the tables get created.

saveQuestionDataTransition
##########################

Saves the given list of questions into the database.

saveStartAndDestinationData
###########################

Saves the given start and destionation data into the database.

getFilesData
############

Returns all values from TABLE_QUESTIONS_DATA matching the currently open file name.

getFilesName
############

Returns all names of all files stored in the database.

deleteFile
##########

Delete all data from the tables TABLE_START_AND_DESTINATION and TABLE_QUESTIONS_DATA mathing the given filename.


QuestionsActivity
-----------------

The question activity is used as a popup, when the Button "Assignments" in the BottomNavigationView is pressed and the project is loaded from the server.
It shows a list with all available assignments. There is also a button with the title "ADD FILES", which calls the method :ref:`chooseJsonFile`

onCreate
########

Here, the layout is initialised.
It also loads the filelist by calling :ref:`FileListsNew` and asks for storage reading permissions.

chooseJsonFile
##############

Shows a filechooser by calling :ref`showFileChooser`.

onActivityResult
################

This method is called by the application once the fileChooser application returns, either because a file got chosen, or because the action got cancelled.

If a file got chosen, and it does not has the same name as the currently opened file, it gets loaded into the application via :ref:`setJsonToClass`.

setJsonToClass
##############

Opens the file given by the filename and stores it in internally.

showQuestions
#############

Calls :ref:`populateQuestions` to load the questions, then shows the questions on the screen.
When clicking ok, the questions get saved to the database by calling :ref:`saveDataToDB`.
If the button 'cancel' is called, the questions get discarded.

saveDataToDB
############

Saves the given questions into the database via the :ref:`DatabaseHandler`.

getDataFromRoomDB
#################

Gets the selected assignment, the according start and destination entity an the questions of the assignment from the :ref:`DataBaseThreads` and starts a new QuestionsActivity.

populateQuestions
#################

Loads the questions and shows them in the QuestionsActivity popup on the screen.

FileListsNew
############

Gets the list of of assignments from the database via one of the :ref:`DataBaseThreads` and shows them on screen.
Finished assignments are shown with a star at the end of the name.

The models
----------

There are different models, which are used for temporarily storing data, they are explained below.
The :ref:`StaticModel` was explained earlier.

MainModel
#########

This model is used to store the :ref:`StartAndDestinationModel` and a list of instances of the :ref:`QuestionDataModel`

OptionModel
###########

This model is used as an answer option for the :ref:`QuestionDataModel`.
It has the following fields, all of them have getter and setter methods:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - Description
  * - Name
    - String
    - The name of the answer option

ProjectModel
############

This class has metadata for the current project, it contains the following fields:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - Description
  * - name
    - double
    - The name of the project
  * - id
    - String
    - "-&gt; checkbox disable, 1 -&gt; checkbox enable"
  * - selected
    - boolean
    - whether the project is currently selected
  * - autoAssignment
    - boolean
    - If the assignment should be automatically loaded.
  * - help
    - String
    - The help text for the project

QuestionDataModel
#################

This model is used for storing the question data.

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - Description
  * - id
    - String
    - "question id {Attribute}"
  * - Question
    - String
    - The question
  * - Latitude
    - String
    - Lat-location of the question
  * - Longitude
    - String
    - Long-location of the question
  * - Sensor
    - List<SensorModel>
    - A list with the sensor names used in this Question
  * - Time
    - String
    - Time at answering the question
  * - Frequency
    - String
    - The sensor measuring frequency
  * - Sequence
    - String
    - The question sequence
  * - Type
    - String
    - The question type (see :ref:`Modes`)
  * - Visibility
    - String
    - The visibility of the question
  * - Mandatory
    - String
    - whether or not the question is mandatory
  * - Option
    - List<:ref:`OptionModel`>
    - List of answer options with the corresponding credits
  * - StopName
    - String
    - The stop text for the notification
  * - QID
    - String
    - The question id for the notification
  * - Vicinity
    - String
    - The vicinity of the question

SensorModel
###########

This class has a field for the sensor name.

SliderMenuModel
###############

This class has a string for the icon name and an int for the recource id.

StartAndDestinationModel
########################

This class contations a StartAndDestination value pair.

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - Description
  * - StartLatitude
    - String
    - the lat-Position of the start point
  * - StartLongitude
    - String
    - the long-Position of the start point
  * - DestitationLatitude
    - String
    - the lat-Position of the destination point
  * - DestitationLongitude
    - String
    - the long-Position of the destination point
  * - Mode
    - String
    - question mode
  * - DefaultCredit
    - String
    - the default amount of credits received for this question

AppPreferences
--------------

This class is the |SP| handler of the application.

.. |SP| raw:: html

  <a href="https://developer.android.com/reference/android/content/SharedPreferences" target="_blank">SharedPreferences</a>

saveLocation
############

Saves the users current location.

saveAccuracy
############

Saves the current measuring accuracy.

getAccuracy
###########

Retrieves the last saved accuracy.

getLatitude
###########

Gets the last saved latitude from :ref:`saveLocation`.

getLongitude
############

Gets the last saved longitude from :ref:`saveLocation`.

saveDataScientistId
###################

Saves the dataScientistId.

getDataScientistId
##################

Retrieves the saved dataScientistId.

saveHelpForProject
##################

Saves the help message fo the current assignment, which gets shown when pressing on the help button.

getHelp
#######

Retrieves the saved help message.

saveProjectId
#############

Saves the current project id.

getProjectId
############

Retrieves the current project id.

saveAuto
########

Saves, whether the assignment should be automatically be loaded or not.

getAuto
#######

Retrieves the saved automation preference.

NetworkCallbacks
----------------

This class is responsible for all restful API queries with a callback event.
It uses an interface called :ref:`RetrofitInterface` for all GET and POST messages.

getAllProjectsFromServer
########################

Uses the :ref:`getAllProjects` query.
On response, it saves the response in a list of :ref:`ProjectModel` of the :ref:`HomeActivity class`.

getAllTasksFromServer
#####################

Uses the :ref:`getTasks` query, giving it the current project id.
On response, it saves the response in a list of tasks of the :ref:`NetworkCallbacks` static list 'serverTask'.

getAllAssetsFromServer
######################

Uses the :ref:`getAllAssetsfromSpecificProject` query, giving it the current project id.
On response, it saves the response in a list of tasks of the :ref:`NetworkCallbacks` static list 'serverAssets'.

createUserOnServer
##################

Uses the :ref:`createUser` query, giving it the current project id and user.
On response, it shows a toast if not successful.

createTaskOnServer
##################

Uses the :ref:`adminCreateTasks` query, giving it the current project id, task id and the task itself.

createAssignmentOnServer
########################

Uses the :ref:`assignAsset` query, giving it the current project id, task id, assetId and a cookieHeader.
The cookieHeader has the form:

.. code-block:: Java

   <projectId> + "_user_id=" + <userId>

assignAssignmentsToUsers
########################

Uses the :ref:`getAllUsersfromSpecificProject` query, giving it the project id.
On a successful response, adds all users from the response to the :ref:`NetworkCallbacks` static list 'serverUsers' and calls getAssignment for all of them.

getAllAssignmentsFromServer
###########################

Uses the :ref:`getAllAssignmentsfromSpecificProject` query, giving it the project id.
On response, it saves the response in a list of tasks of the :ref:`NetworkCallbacks` static list 'serverAssignments'.

getAssignment
#############

Uses the :ref:`getSpecificAssignment` query, giving it the project id and the assignment id.
If the response code is either 500 (= internal error) or 200 and the response itself is *not* "finished", createAssignmentOnServer is called.
Otherwise, no action is done.

updateAssignmentInTask
######################

Uses the :ref:`userCreateAssignment` query, giving it the project id, the assignment, the task id and the cookie header.
Then calls :ref:`createTaskOnServer`, :ref:`saveAssignmentInFile` and disables the saved assignment in the database.
Sets the assignment submission status to true in the :ref:`utils` class.

getSpecificTask
###############

Uses the :ref:`getSpecificTask` query, giving it the project id and the task id.
Upon response, it creates a new task, sets the different fields, creates a AssignmentSubmittedData (a subclass of :ref:`Assignment`) and sets the answers and sensor readings.
Then calls :ref:`updateAssignmentInTask` with the new objects as parameters.

AssignmentSubmission
####################

Uses the :ref:`getAllAssignmentsfromSpecificProject` query, giving it the project id.
Upon response it iterates through all received assignments and saves the corresponding, given answers into the database.
Also calls :ref:`getSpecificTask` for each task.

saveAssignmentInFile
####################

Saves the given assignment into a file with the name givenm by the asset name.

saveDataToRoomDatabase
######################

Saves the data of all assignments of the current project into the RoomDatabase with the :ref:`RoomDatabaseHandler`.

RetrofitInterface
-----------------

The RetrofitInterface is used for specifying the queries with the restful servers.
It uses |Retrofit| to serialize and create the messages.
All messagaes hav type POST or GET.

The queries are defined in an interface class.
The @GET/@POST parameters define the website path and the functions return values of type Call<ResponseObject>.
In the following, there are all GET and POST queries.


getAllProjects
##############

.. code-block:: Java

  @GET("/admin/projects?from=0&size=3000")
  Call<NetworkModels.ProjectModel> getAllProjects();

getTasks
########

.. code-block:: Java

  @GET("/projects/{projectId}/tasks")
  Call<NetworkModels.TaskModel> getTasks(@Path(value = "projectId", encoded = true) String projectId);

getAllAssetsFromSpecificProject
###############################

.. code-block:: Java

  @GET("/admin/projects/{projectId}/assets")
  Call<NetworkModels.AssetModel> getAllAssetsfromSpecificProject(@Path(value = "projectId", encoded = true) String projectId);

createUser
##########

.. code-block:: Java

  @POST("/projects/{projectId}/user")
  Call<NetworkModels.UserModel> createUser(@Body User u, @Path(value = "projectId", encoded = true) String projectId);

adminCreateTasks
################

.. code-block:: Java

  @POST("/admin/projects/{projectId}/tasks/{taskId}")
  Call<NetworkModels.SingleTaskModel> adminCreateTasks(@Body Task t, @Path(value = "projectId", encoded = true) String projectId, @Path(value = "taskId", encoded = true) String taskId);

assignAsset
###########

.. code-block:: Java

  @GET(" /projects/{projectId}/tasks/{taskId}/assets/{assetId}/assignments")
  Call<NetworkModels.AssignmentModel> assignAsset(@Path(value = "projectId", encoded = true) String projectId, @Path(value = "taskId", encoded = true) String taskId, @Path(value = "assetId", encoded = true) String assertId, @Header("Cookie") String userId);

getAllUsersFromSpecificProject
##############################

.. code-block:: Java

  @GET("/admin/projects/{projectId}/users")
  Call<NetworkModels.UserModel> getAllUsersfromSpecificProject(@Path(value = "projectId", encoded = true) String projectId);

getAllAssignmentsFromSpecificProject
####################################

.. code-block:: Java

  @GET("/admin/projects/{projectId}/assignments?from=0&size=3000")
  Call<NetworkModels.AssignmentModel> getAllAssignmentsfromSpecificProject(@Path(value = "projectId", encoded = true) String projectId);

getSpecificAssignment
#####################

.. code-block:: Java

  @GET(" /projects/{projectId}/assignments/{assignmentId}")
  Call<NetworkModels.SpecificAssignmentModel> getSpecificAssignment(@Path(value = "projectId", encoded = true) String projectId, @Path(value = "assignmentId", encoded = true) String assignmentId);


userCreateAssignment
####################

.. code-block:: Java

  @POST("/projects/{projectId}/tasks/{taskId}/assignments")
  Call<NetworkModels.AssignmentModel> userCreateAssignment(@Path(value = "projectId", encoded = true) String projectId, @Body Assignment assignment, @Path(value = "taskId", encoded = true) String taskId, @Header("Cookie") String userId);

getSpecificTask
###############

.. code-block:: Java

  @GET("/projects/{projectId}/tasks/{taskId}")
  Call<NetworkModels.SingleTaskModel> getSpecificTask(@Path(value = "projectId", encoded = true) String projectId, @Path(value = "taskId", encoded = true) String taskId);

HiveServerModels
----------------

The hive server models are used in the :ref:`NetworkCallbacks` as the returned objects for the server queries.
They need to have a specific layout.
All elementes need to have getter and setter methods, the variable scope is private and all variables need to have the followoing annotation:

.. code-block:: Java

   @SerializedName("<serialized name for variable her>")

This identifier makes it possible for the |Retrofit| library to assign values from the received Json object to the models.
In the following, the fields of all models are specified.

Asset
#####

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - String
    - Id
    - The id for the asset
  * - project
    - String
    - Project
    - The project for this asset
  * - name
    - String
    - Name
    - The asset name
  * - metadata
    - Metadata
    - Metadata
    - An internally defined class, more on it below this list
  * - counts
    - Counts
    - Counts
    - An internally defined class, more on it below this list

**Metadata**

Metadata has the same restrictions and rules as the other :ref:`HiveServerModels`.
It contains a value of type Record with the serialised name "record".

**Counts**

Counts is another internally defined model of :ref:`Asset`.
It contains the following 2 elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - unfinished
    - int
    - unfinished
    - The amount of unfinished assignments
  * - finished
    - int
    - finished
    - The amount of finished assignments

**Record**

Record is another internally defined model of :ref:`Asset`.
It contains two elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - startAndDestinationModel
    - StartAndDestination
    - StartAndDestinationModel
    - The start and destination model for the hive server, specified futher down.
  * - sampleDataModel
    - List<SampleDataModel>
    - SampleDataModel
    - A list with all data for a question, SampleDataModel is specified further below.

**StartAndDestination**

Record is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - startLatitude
    - String
    - StartLatitude
    - The latitude of the starting point of a start and destination model.
  * - startLongitude
    - String
    - StartLongitude
    - The longitude of the starting point of a start and destination model.
  * - destinationLatitude
    - String
    - DestinationLatitude
    - The latitude of the destination point of a start and destination model.
  * - destinationLongitude
    - String
    - DestinationLongitude
    - The longitude of the destination point of a start and destination model.
  * - mode
    - String
    - Mode
    - The transportation mode.
  * - defaultCredit
    - String
    - DefaultCredit
    - The deafult credit for this route.

**SampleDataModel**

SampleDataModel is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - int
    - id
    - The question id
  * - question
    - String
    - Question
    - The question itself
  * - type
    - String
    - Type
    - The question type
  * - latitude
    - String
    - Latitude
    - The latitude of the question on the map
  * - longitude
    - String
    - Longitude
    - The longitude of the question on the map
  * - Sensor
    - List<Sensor>
    - Sensor
    - The list of all used sensors. The Sensor class is defined further below
  * - time
    - String
    - Time
    - Time of answering the question
  * - frequency
    - String
    - Frequency
    - Sensor measuring frequency
  * - sequence
    - String
    - Sequence
    - Question sequence
  * - visibility
    - String
    - Visibility
    - Visibility of question
  * - mandatory
    - String
    - Mandatory
    - Whether question is mandataory or not
  * - Option
    - List<Option>
    - Option
    - A list of available options, the Option class is defined further below.
  * - combination
    - List<Combination>
    - Combination
    - A list of combinations, the Combination class is defined further below.
  * - vicinity
    - String
    - Vicinity
    - The vicinity of the question

**Option**

Option is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - int
    - id
    - The question id
  * - name
    - String
    - Name
    - Text of the textbox option
  * - nextQuestion
    - String
    - NextQuestion
    - The question which follows after this one when this option is selected.
  * - credits
    - String
    - Credits
    - The credits for this option when selected

**Sensor**

Sensor is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - int
    - id
    - The question id
  * - name
    - String
    - Name
    - The sensor name

**Combination**

Combination is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - int
    - id
    - Question id
  * - selected
    - List<Order>
    - Selected
    - A list with the selected combination. Order is defined further below.
  * - nextQuestion
    - String
    - NextQuestion
    - The next question following after this one.
  * - credits
    - String
    - Credits
    - the credits for this question

**Order**

Order is another internally defined model of :ref:`Asset`.
It contains the following elements:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - order
    - String
    - Order
    - the order itself
  * - id
    - int
    - id
    - the question id

Assignment
##########

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - assignmentid
    - String
    - id
    - The id of the assignment
  * - projectName
    - String
    - Project
    - The name of the project
  * - task
    - String
    - Task
    - The task name
  * - userName
    - String
    - User
    - the username
  * - state
    - String
    - State
    - The current state of the assignment
  * - asset
    - :ref:`Asset`
    - Asset
    - The corresponding asset.
  * - submittedData
    - AssignmentSubmittedData
    - SubmittedData
    - The submitted data for this assignment, AssignmentSubmittedData is defined below.

**AssignmentSubmittedData**

AssignmentSubmittedData is an internally defined model of :ref:`Assignment`.
It contains the following fields:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - answer
    - List<Answer>
    - SubmittedData
    - The list of all submitted answers, Answer is defined below.
  * - sensorreadings
    - List<Map<String, List<SensorReading>>>
    - SensorData
    - A list with all readings from the sensors, SensorReading is defined futher below.

**Answer**

Answer is an internally defined model of :ref:`Assignment`.
It contains the following fields:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - int
    - Id
    - The corresponding question id
  * - answer
    - String
    - Answer
    - The given answer
  * - files_name
    - String
    - Files_name
    - The name for the file
  * - latitude
    - String
    - Latitude
    - The latitude of the phone when answering
  * - longitude
    - String
    - Longitude
    - The longitude of the phone when answering
  * - question
    - String
    - Question
    - The question, to which the answer was given
  * - type
    - String
    - Type
    - The question type
  * - timeAtAnswering
    - String
    - TimeAtAnswering
    - The measured time when the question was ansered

**SensorReading**

SensorReading is an internally defined model of :ref:`Assignment`.
It contains the following fields:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - String
    - Id
    - The project id
  * - assignmentId
    - String
    - AssignmentId
    - The assignment id
  * - question
    - String
    - Question
    - The corresponding question
  * - questionId
    - String
    - QuestionId
    - The id of the corresponding question
  * - timeAtSensoring
    - String
    - TimeAtSensoring
    - The time measured, when the sonsor measurement was taken
  * - answerValue
    - String
    - Answer Value
    - The answer, which was given to this question
  * - acceleration
    - String
    - Acceleration
    - The measured acceleration
  * - frequency
    - String
    - Frequency
    - The measurement frequency
  * - gyroscope
    - String
    - Gyroscope
    - The measured gyroscope value
  * - light
    - String
    - Light
    - The measured light value
  * - location
    - String
    - Location
    - Then measured location when answering the question
  * - noise
    - String
    - Noise
    - The measured noise level when answering the question
  * - proximity
    - String
    - Proximity
    - The measured porximity level when ansering the phone

Project
#######

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - String
    - Id
    - The project id
  * - name
    - String
    - Name
    - The project name
  * - description
    - String
    - Description
    - The project description
  * - assetCount
    - String
    - AssetCount
    - The amount of assets for this project
  * - taskCount
    - String
    - TaskCount
    - The amount of tasks for this project
  * - userCount
    - String
    - UserCount
    - The amount of users, which have access to this task
  * - assignmentCount
    - AssignmentCount
    - AssignmentCount
    - The amount of assignments for this project, AssignmentCount is defined below.

**AssignmentCount**

AssignmentCount is an internally defined model of :ref:`Assignment`.
It contains the following fields:

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - total
    - String
    - Total
    - The total amount of assignments
  * - finished
    - String
    - Finished
    - The amount of finished assignments
  * - unfinished
    - String
    - Unfinished
    - The amount of unfinished assignments

Task
####

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - String
    - Id
    - The task id
  * - project
    - String
    - Project
    - The corresponding project
  * - name
    - String
    - Name
    - The name of the task
  * - description
    - String
    - Description
    - The description of the task
  * - currentState
    - String
    - CurrentState
    - The current state of the task
  * - assignmentCriteria
    - AssignmentCriteria
    - AssignmentCriteria
    - The criteria for the assignment, AssignmentCriteria is defined below
  * - completionCriteria
    - CompletionCriteria
    - CompletionCriteria
    - The criteria for completing the task, CompletionCriteria is defined further below

**AssignmentCriteria**

AssignmentCriteria is an internally defined model of :ref:`Task`.
It contains one field: submittedData of the type SubmittedData defined further below.

**CompletionCriteria**

CompletionCriteria is an internally defined model of :ref:`Task`.
It contains two int fields: total and matching, defining how many criteria there are in total and how many of them are matching.

**SubmittedData**

SubmittedData is an internally defined model of :ref:`Task`.
It contains one field: assignments of the type Map<String, Assignment>, which stores all submitted assignments.

User
####

.. list-table::
  :header-rows: 1

  * - Name
    - Type
    - SerializedName
    - Description
  * - id
    - String
    - Id
    - The user id
  * - name
    - String
    - Name
    - The username
  * - email
    - String
    - Email
    - The email address of the user
  * - project
    - String
    - Project
    - The corresponding project
  * - externalId
    - String
    - ExternalId
    - The external id of the user

NetworkModels
-------------

The network models, like :ref:`HiveServerModels`, are used in the :ref:`NetworkCallbacks` as the returned objects for the server queries.
In the following, the models are defined, all of them have just one field.

**UserModel**

Contains a list of type :ref:`User` with the serialised name "Users".


**ProjectModel**

Contains a list of type :ref:`Project` with the serialised name "Projects".


**TaskModel**

Contains a List of type :ref:`Task` with the serialised name "Tasks".

**SingleTaskModel**

Contains a single field of type :ref:`Task` with the serialised name "Task".

**SpecificAssignmentModel**

Contains a single field of type :ref:`Assignment` with the serialised name "Assignment".

**AssignmentModel**

Contains a list of elements of type :ref:`Assignment` with the serialised name "Assignments".

**AssetModel**

Contains a list of elements of type :ref:`Asset` with the serialised name "Assets".


.. |Retrofit| raw:: html

  <a href="https://square.github.io/retrofit/" target="_blank">Retrofit</a>

.. |SQLite| raw:: html

  <a href="https://developer.android.com/reference/android/database/sqlite/package-summary.html" target="_blank">SQLite</a>
