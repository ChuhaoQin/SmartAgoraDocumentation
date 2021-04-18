.. default-domain:: csharp



######
Models
######

.. namespace:: HiveServer.Models

***********
HiveModels
***********

AnswersData
============

.. class:: AnswerData

    Following are the properties that a AnswerData will contain.

    Here are references to it's properties:

    * :prop:`id`
    * :prop:`Answer`
    * :prop:`Files_Name`
    * :prop:`Latitude`
    * :prop:`Longitude`
    * :prop:`Question`
    * :prop:`Type`
    * :prop:`TimeAtAnswering`

    .. property:: int id { get; set; }

	   It is the integer type Id of the answer object.
    
    .. property:: string Answer { get; set; }

       It is the answer that a user provides on a given question in a survey.



    .. property:: string Files_Name { get; set; }

	   It is the name of the asset file in which the question is given.

    .. property:: string Latitude { get; set; }

       It is the Latitude value of the question's position.

    .. property:: string Longitude { get; set; }

       It is the longitude value of the question's position.

    .. property:: string Question { get; set; }
 
       It is the Question Text that will be displayed on the survey.

    .. property:: string Type { get; set; }

       It is the type of question. I can be a checkbox, radio, textbox or Likert scale type.

    .. property:: string TimeAtAnswering { get; set; }

       It is the time at which the user answers the given question.


Asset
=====
.. class:: Asset

   Following are the properties that the class Asset will contain.

   Here are references to it's properties:

    * :prop:`Id`
    * :prop:`Name`
    * :prop:`URL`
    * :prop:`Metadata`

   .. property:: string Id { get; set; }

       It is the unique string type Id of an asset that is used to uniquely identify the asset.

   .. property:: string Name { get; set; }

       It contains the name of the asset. It can be a simple, decision, dias simple or a sequence.

   .. property:: string Url { get; set; }

		It is the URL of the asset. It is hard-coded.

   .. property:: Metadata Metadata { get; set; }

        It contains the main parts of the asset. It contains questions, start, destination addresses, etc.


Assignment
==========

.. class:: Assignment

   Following are the properties that the Assignment class contains.

   Here are references to its properties:

    * :prop:`Id`
    * :prop:`User`
    * :prop:`Project`
    * :prop:`Task`
    * :prop:`Asset`
    * :prop:`State`
    * :prop:`SubmittedData`
    * :prop:`SubmittedAnswerData`

   .. property:: string Id { get; set; }

       It is the Id of an assignment. It is the unique string on the :ref:`Hive` Server.

   .. property:: string User { get; set; }

       It is the Id of the associated user that is selected at the time of assignment creation.

   .. property:: string Project { get; set; }

       It is the Id of the project that is selected at the time of the Assignment creation.

   .. property:: string Task { get; set; }

       It is the Id of the Task that is selected at the time of the Assignment creation.

   .. property:: Asset Asset { get; set; }

	   It is the Id of the Asset that is selected at the time of the Assignment creation.

   .. property:: string State { get; set; }

       It indicates the status of an assignment either an assignment is finished or not.

   .. property:: SubmittedData SubmittedData { get; set; }

       It is an object that contains all data that is gotten from the android side on assignment completion and submission.

   .. property:: SubmittedAnswerData SubmittedAnswerData { get; set; }

       It is an object that contains only answer data that a user has submitted on completing a survey.



AssignmentCriteria
===================

.. class:: AssignmentCriteria

   Following is the property that the class AssignmentCriteria contains.

   Here are references to its properties:

    * :prop:`SubmittedData`

   .. property:: SubmittedData SubmittedData { get; set; }

       It is used to contain the data submitted by a user during the survey.



CompletionCriteria
===================

.. class:: CompletionCriteria

   Following are the properties that the class CompletionCriteria contains.

   Here are references to its properties:

    * :prop:`Total`
    * :prop:`Matching`

   .. property:: int Total { get; set; }

       It is the total number of submitted assignments to mark a task as completed.

   .. property:: int Matching { get; set; }

       It is the total number of matched submitted assignments to mark a task as completed.

CompletionCriteria -> Count
===========================

.. class:: Count

   Following are the properties that the class Count contains.

   Here are  references to its properties:

    * :prop:`Assignments`
    * :prop:`finished`
    * :prop:`skipped`
    * :prop:`unfinished`

   .. property:: string Assignments { get; set; }

       It is the total number of assigments related to a specific project or a task etc.

   .. property:: int finished { get; set; }

       It is the total number of finished assigments related to a specific project or task etc.

   .. property:: int skipped { get; set; }

       It is the total number of skipped assigments related to a specific project or task etc.

   .. property:: int unfinished { get; set; }

       It is the total number of unfinished assigments related to a specific project or task etc.

CompletionCriteria -> SocialExperimentXML
=========================================

.. class:: SocialExperimentXML

   Following are the properties that the class SocialExperimentXML contains.

   Here are references to its properties

   * :prop:`Data`

   .. property:: string Data { get; set; }


CompletionCompletionCriteria -> submittedData
==============================================
.. class:: submittedData

   Following are the properties that the class submittedData contains.

   Here are  references to its properties:

    * :prop:`Data`

   .. property:: string Data { get; set; }

Metadata
========

.. class:: Metadata

   This is a Metadata class with properties, there is no method.

   Here are  references to its properties:

    * :prop:`record`

   .. property:: Questions record { get; set; }

       It contains the question object that contains question, question location, question type etc.


MetaProperty
============

.. class:: MetaProperty

   This is a MetaProperty class with properties, there is no method.

   Here are  references to its properties:

    * :prop:`Name`
    * :prop:`Type`

   .. property:: string Name { get; set; }

   .. property:: string Type { get; set; }

Project
=======
.. class:: Project

   Following are the methods and properties that a project will contain on creation.

   Here are  references to its properties:

    * :prop:`Id`
    * :prop:`Name`
    * :prop:`Description`
    * :prop:`AssetCount`
    * :prop:`TaskCount`
    * :prop:`UserCount`
    * :prop:`MetaProperties`
    * :prop:`AssignmentCount`

   .. property:: string Id { get; set; }

       It contains the unique Id of the project. It is created automatically using a timestamp so that the newly created project has a unique Id.

   .. property::  string Name { get; set; }

       It is the name of the Project provided by the user at the time of the project creation.

   .. property::  string Description { get; set; }

       It is an optional description of the project. You can skip this one.

   .. property::  int AssetCount { get; set; }

       It contains the total number of Assets associated with the specific project.

   .. property::  int TaskCount { get; set; }

	   It contains the total number of Tasks associated with the specific project.

   .. property::  int UserCount { get; set; }

       It contains the total number of users associated with the specific project.

   .. property::  MetaProperty[] MetaProperties { get; set; }


   .. property::  AssignmentCount AssignmentCount { get; set; }

       It contains the total number of Assignments associated with the specific project. It contains two values: 'Total' for the total assignments including finished or unfinished whereas the 'Finished' for the finished assignments count.

Project -> AssignmentCount
===========================
.. class:: AssignmentCount

   Following are the properties that the class AssignmentCount contains.

   Here are references to its properties:

    * :prop:`Total`
    * :prop:`Finished`

   .. property:: int Total { get; set; }

       It is the total number of assingments associated with a specific project.

   .. property:: int Finished { get; set; }

       It is the count of the finished assignments of a project.

ProjectQuestionModel
====================

.. class:: ProjectQuestionModel

   This is a ProjectQuestionModel class with properties, there is no method.

   Here are  references to its properties:

    * :prop:`ProjectId`
    * :prop:`QuestionsModel`

   .. property:: string ProjectId { get; set; }

       It is the Id of the project with which the question data is associated.

   .. property:: Questions QuestionsModel { get; set; }

       It is the Questions class object that has the questions data.

Record
======

.. class:: Record

   This is a Record class with properties, there is no method.

   Here are  references to its properties:

   	* :prop:`sensors`
	* :prop:`start`
	* :prop:`end`
	* :prop:`step`

   .. property:: string[] sensors { get; set; }

       It is an array of the sensor names. Currently, there are seven sensors.

   .. property:: string start { get; set; }
   .. property:: string end { get; set; }
   .. property:: int step { get; set; }



SubmittedAnswerData
===================

.. class:: SubmittedAnswerData

   This is a SubmittedAnswerData class with properties, there is no method.

   Here are  references to its properties:

    * :prop:`SubmittedData`

   .. property:: string[] List<AnswersData> SubmittedData { get; set; }

       It is the list that contains all the answers of the users corresponding to the questions.



SubmittedData
=============

.. class:: SubmittedData

   This is a SubmittedAnswerData class with properties, there is no method.

   Here are  references to its properties:

    * :prop:`record`

   .. property:: Record record { get; set; }

Task
====
.. class:: Task

   Following are the properties that the class Task will contain.

   Here are references to it's properties:

    * :prop:`Id`
    * :prop:`Name`
    * :prop:`Description`
    * :prop:`CurrentState`
    * :prop:`AssignmentCriteria`
    * :prop:`CompletionCriteria`

   .. property:: string Id { get; set; }

       It is the unique Id of the Task created by the :ref:`Hive` Server at the time of task creation request.

   .. property:: string Name { get; set; }

       It is the name of the Task which is provided by the user.

   .. property:: string Description { get; set; }

       It is the optional description of the task.

   .. property:: string CurrentState { get; set; }

       It is the current state of the Task either it is available or not.

   .. property:: AssignmentCriteria AssignmentCriteria { get; set; }

       For details, you can view the AssignmentCriteria class.

   .. property:: CompletionCriteria CompletionCriteria { get; set; }

       For details, you can view the CompletionCriteria class.




User
====
.. class:: User

   This is an User class with properties, there is no method.

   Here are  references to its properties:

	* :prop:`Id`
	* :prop:`Name`
	* :prop:`Email`
	* :prop:`Project`
	* :prop:`ExternalId`

   .. property:: string Id { get; set; }

      It is the Id of the user i.e the unique device Id through which a user is completing an assignment.

   .. property:: string Name { get; set; }

	  It is the name of the user i.e infact the name of the device.

   .. property:: string Email { get; set; }

       It is the e-mail of the user. It is currently hard-coded.

   .. property:: string Project { get; set; }

       It is the project id to which a user is associated(using the android app).

   .. property:: string ExternalId { get; set; }





**************
QuestionModels
**************

.. namespace:: Models.QuestionModels

CombinationModel
================

.. class:: CombinationModel

   Following are the properties that the class CombinationModel contains.

   Here are references to its properties

    * :prop:`id`
    * :prop:`Selected`
    * :prop:`NextQuestion`
    * :prop:`Credits`



   .. property:: int id { get; set; }

       It is an autoincremented Id of the of choices i.e if user select two checkboxes then there will be two ids 1 and 2.

   .. property:: List<SelectionQuestionModel> Selected { get; set; }

       It is the List of the selected checkboxes.

   .. property:: string NextQuestion { get; set; }

       It is the question Id of the next question asked on the base of the selected checkbox in case of decision mode.

   .. property:: string Credits { get; set; }

       It is the credit of the checkbox combination that will be rewarded to the user when user will select combination of checkboxes.


jQueryDataTableParamModel
=========================

.. class:: jQueryDataTableParamModel

   Following are the properties that the class jQueryDataTableParamModel will contain.

   Here are references to its properties

    * :prop:`sEcho`
    * :prop:`sSearch`
    * :prop:`iDisplayLength`
    * :prop:`iDisplayStart`
    * :prop:`iColumns`
    * :prop:`iSortingCols`
    * :prop:`sColumns`

   .. property:: string sEcho { get; set; }

   .. property:: string sSearch { get; set; }

   .. property::  int iDisplayLength { get; set; }

   .. property:: int iDisplayStart { get; set; }

   .. property::  int iColumns { get; set; }

   .. property:: int iSortingCols { get; set; }

   .. property:: string sColumns { get; set; }


OptionModel
===========

.. class:: OptionModel

   Following are the properties that the class OptionModel will contain.

   Here are references to its properties

    * :prop:`id`
    * :prop:`Name`
    * :prop:`NextQuestion`
    * :prop:`Credits`

   .. property:: int id { get; set; }

       It is simply the Id of the option i.e if there are two option then their Ids will be 1 and 2.

   .. property:: string Name { get; set; }

       It is the text of the option i.e. the content that will be visible to the user as the option.

   .. property:: string NextQuestion { get; set; }

       It is the Id of the next question that is associated with the option if the user select option then in case of the decision mode the associated id's question will be asked.

   .. property:: string Credits { get; set; }

       It is the credit of the option that will be rewarded to the user when a user selects an option.


QuestionDataModel
=================

.. class:: QuestionDataModel

   Following are the properties that the class QuestionDataModel contains.

   Here are references to its properties

    * :prop:`id`
    * :prop:`Question`
    * :prop:`Type`
    * :prop:`Latitude`
    * :prop:`Longitude`
    * :prop:`Sensor`
    * :prop:`Time`
    * :prop:`Type`
    * :prop:`Frequency`
    * :prop:`Sequence`
    * :prop:`Visibility`
    * :prop:`Mandatory`
    * :prop:`OPtion`
    * :prop:`Combination`
    * :prop:`Vicinity`

   .. property:: int id { get; set; }

       It is the id of the question i.e if there are two questions then their ids will be 1 and 2 respectively.

   .. property:: string Question { get; set; }

       It is the text of the question that will be visible to the user on an assignment.

   .. property:: string Type { get; set; }

       It is the type of question. It can be any one of the radio, checkbox, textbox or likertscale.

   .. property:: string Latitude { get; set; }

       It is the Latitude value of the question's position on the map.

   .. property:: string Longitude { get; set; }

       It is the Longitude value of the question's position on the map.

   .. property:: List<SensorModel> Sensor { get; set; }

       It is a list of the selected sensors associated with questions.

   .. property:: string Time { get; set; }

       It is the time duration in seconds for which sensor values are recorded.

   .. property:: string Frequency { get; set; }

       It will set how fast a sensor's value will be recorded.

   .. property:: string Sequence { get; set; }

       It is the sequence number of the question in case of sequence mode.

   .. property:: string Visibility { get; set; }

       It is switch to toggle a question's visibility on the map.

   .. property:: string Mandatory { get; set; }

       It is a check on the question either the question is compulsory to answer or not, for completion of the assignment.

   .. property:: List<OptionModel> Option { get; set; }

       It is the list of the options for the answer of the question.

   .. property:: List<CombinationModel> Combination { get; set; }

       It is the list of the checkboxes combination in case if the type of question is checkbox i.e if there are two checkboxes for a question then there are three possible combinations that allow a user to select either first checkbox or second or both.

   .. property:: string Vicinity { get; set; }

       It allows the user to configure the vicinity or localization of the user.

Questions
=========

.. class:: Questions

   Following are the properties that the class Questions contains.

   Here are references to its properties

    * :prop:`StartAndDestinationModel`
    * :prop:`SampleDataModel`

   .. property:: StartAndDestinationModel StartAndDestinationModel { get; set; }

       It conatins the start address, destination address, mode of asset and default credits.

   .. property:: List<QuestionDataModel> SampleDataModel { get; set; }

       It contains the question model.


SelectionQuestionModel
======================

.. class:: Questions

   Following are the properties that the class SelectionQuestionModel contains.

   Here are references to its properties

    * :prop:`id`
    * :prop:`Order`

   .. property:: int id { get; set; }

   .. property:: string Order { get; set; }


SensorDataModel
===============

.. class:: Questions

   Following are the properties that the class SensorDataModel contains.

   Here are references to its properties

    * :prop:`Id`
    * :prop:`Noise`
    * :prop:`Acceleration`
    * :prop:`Light`
    * :prop:`Gyroscope`
    * :prop:`Proximity`
    * :prop:`id`
    * :prop:`Location`
    * :prop:`Frequency`
    * :prop:`Question`
    * :prop:`id`
    * :prop:`QuestionId`
    * :prop:`id`
    * :prop:`AssignmentId`
    * :prop:`id`
    * :prop:`TimeAtSensoring`
    * :prop:`id`
    * :prop:`Order`

   .. property:: int Id { get; set; }

   .. property:: string Noise{ get; set; }

   .. property:: string Acceleration{ get; set; }

   .. property::  string Light{ get; set; }

   .. property:: public string Gyroscope{ get; set; }

   .. property:: string Proximity{ get; set; }

   .. property:: string Location { get; set; }

   .. property:: string Frequency { get; set; }

   .. property:: string Question { get; set; }

   .. property:: string QuestionId { get; set; }

   .. property:: string AssignmentId { get; set; }

   .. property:: string TimeAtSensoring { get; set; }


SensorModel
===========

.. class:: SensorModel

   Following are the properties that the class SensorModel will contain.

   Here are references to its properties

    * :prop:`id`
    * :prop:`Name`

   .. property:: int id { get; set; }

       It is an id of the sensor. There are seven sensors that are being used.

   .. property:: string Name { get; set; }

       It is the name of the sensor.


StartAndDestinationModel
========================

.. class:: StartAndDestinationModel

   Following are the properties that the class SensorModel will contain.

   Here are references to it's properties

    * :prop:`StartLatitude`
    * :prop:`StartLongitude`
    * :prop:`DestinationLatitude`
    * :prop:`DestinationLongitude`
    * :prop:`Mode`
    * :prop:`DefaultCredit`

   .. property:: string StartLatitude { get; set; }

       It is the latitude value of the start address on the map in case of sequence mode.

   .. property:: string StartLongitude { get; set; }

       It is the longitude value of the start address on the map in case of sequence mode.

   .. property:: string DestinationLatitude { get; set; }

       It is the latitude value of the destination address on the map in case of sequence mode.

   .. property:: string DestinationLongitude { get; set; }

       It is the longitude value of the destination address on the map in case of sequence mode.

   .. property:: string Mode { get; set; }

       It is the mode of the asset from the given four modes i.e simple, decision, sequence and dias simple.

   .. property:: string DefaultCredit { get; set; }

       It is the default credit that will be awarded in case if no credits are set explicitly while creating questions.



.. |br| raw:: html

   <br />