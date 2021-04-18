
#####
Views
#####

****
HTML
****


Here all html will be added.

.. default-domain:: js

*************
Javascripting
*************

.. module:: CreateAssetFormView

CreateAssetFormView
===================

   Following are the global variables and methods.

   Here are the references to global variables and methods:

Global Variables
----------------

    * :data:`addedQuestionCount`
    * :data:`assetMode`
    * :data:`boolAssociateQuestion`
    * :data:`combinationCounter`
    * :data:`combinationFound`
    * :data:`editedQuestionId`
    * :data:`lstCombinations`
    * :data:`lstOptionlen`
    * :data:`lstOptions`
    * :data:`lstQuestionDataModel`
    * :data:`lstSensors`
    * :data:`mainModel`
    * :data:`objCombination`
    * :data:`objCombinationId`
    * :data:`objMainModel`
    * :data:`objOption`
    * :data:`objOptionId`
    * :data:`objQuestionId`
    * :data:`objSensorId`
    * :data:`optionCounter`
    * :data:`optionsAdded`
    * :data:`optionsCombinations`
    * :data:`optionsString`
    * :data:`projectIds`
    * :data:`questionAddressList`
    * :data:`questionLocationMarkers`
    * :data:`questionOldLat`
    * :data:`questionOldLng`
    * :data:`questionType`
    * :data:`removeCombination`
    * :data:`selectOptionString`
    * :data:`sequence`
    * :data:`startAndDestinationModal`
    * :data:`updateSts`

   .. data:: addedQuestionCount
   .. data:: assetMode
   .. data:: boolAssociateQuestion
   .. data:: combinationCounter
   .. data:: combinationFound
   .. data:: editedQuestionId
   .. data:: lstCombinations
   .. data:: lstOptionlen
   .. data:: lstOptions
   .. data:: lstQuestionDataModel
   .. data:: lstSensors
   .. data:: mainModel
   .. data:: objCombination
   .. data:: objCombinationId
   .. data:: objMainModel
   .. data:: objOption
   .. data:: objOptionId
   .. data:: objQuestionId
   .. data:: objSensorId
   .. data:: optionCounter
   .. data:: optionsAdded
   .. data:: optionsCombinations
   .. data:: optionsString
   .. data:: projectIds
   .. data:: questionAddressList
   .. data:: questionLocationMarkers
   .. data:: questionOldLat
   .. data:: questionOldLng
   .. data:: questionType
   .. data:: removeCombination
   .. data:: selectOptionString
   .. data:: sequence
   .. data:: startAndDestinationModal
   .. data:: updateSts


Methods
-------

    * :func:`on("change", "#mainQuestionSelection", function(){...})`
    * :func:`on("change", "#questionType", function(){...})`
    * :func:`on("click", "#addButton", function(){...})`)`
    * :func:`on("click", "#btnSaveAssociateQuestion", function(){...})`
    * :func:`on("click", ".classRemoveOption", function (){...})`
    * :func:`on('click', "#deleteQuestionId", function (){...})`
    * :func:`on('click', "#editQuestionId", function (){...})`
    * :func:`on('keyup', '.optionsCredits', function (event){...})`
    * :func:`ready(function (){...})`
    * :func:`AddCombination()`
    * :func:`AddOption()`
    * :func:`addQuestionInList(qId)`
    * :func:`associateQuestion()`
    * :func:`checkCombinations(event)`
    * :func:`customException(m)`
    * :func:`DeleteDownloadedFileCall()`
    * :func:`download(filename, text)`
    * :func:`downloadFile()`
    * :func:`function (str)`
    * :func:`GenerateHiveCall()`
    * :func:`GenerateXMLCall()`
    * :func:`GetDownloadedFileCall()`
    * :func:`gettingSelectedcombinationLabel(lstSelectedCombination)`
    * :func:`hideGoogleMap()`
    * :func:`hideLoading()`
    * :func:`LoadInitialValues()`
    * :func:`LoadProjects()`
    * :func:`LoadQuestionById(questionId)`
    * :func:`LoadQuestions(lstQuestions)`
    * :func:`LoadSensors()`
    * :func:`optionAddRemoveAndCreditHtml(optCount)`
    * :func:`optionAddRemoveButtonHtml(rmAddButtonCount)`
    * :func:`placeCombination(k, isSelected, creditVal)`
    * :func:`questionOptionHtml(questionCount)`
    * :func:`refreshPage()`
    * :func:`resetFileUplaodView()`
    * :func:`resetOptionSection()`
    * :func:`saveProjectIdAndAssetMode()`
    * :func:`saveStartAndDestinationData()`
    * :func:`saveValuesOnNextButton()`
    * :func:`setQuestionStep()`
    * :func:`setStartAndDestinationStep()`
    * :func:`showLoading()`
    * :func:`showUploadButton()`
    * :func:`submitForHive(event)`
    * :func:`submitFormevent(event)`
    * :func:`uploadFile()`



   .. function:: on("change", "#mainQuestionSelection", function (){...})
   .. function:: on("change", "#questionType", function (){...})
   .. function:: on("click", "#addButton", function (){...})
   .. function:: on("click", "#btnSaveAssociateQuestion", function (){...})
   .. function:: on("click", ".classRemoveOption", function (){...})
   .. function:: on('click', "#deleteQuestionId", function (){...})
   .. function:: on('click', "#editQuestionId", function (){...})
   .. function:: on('keyup', '.optionsCredits', function (event){...})
   .. function:: ready(function (){...})
   .. function:: AddCombination()
   .. function:: AddOption()
   .. function:: addQuestionInList(qId)
   .. function:: associateQuestion()
   .. function:: checkCombinations(event)
   .. function:: customException(m)
   .. function:: DeleteDownloadedFileCall()
   .. function:: download(filename, text)
   .. function:: downloadFile()
   .. function:: function (str)
   .. function:: GenerateHiveCall()
   .. function:: GenerateXMLCall()
   .. function:: GetDownloadedFileCall()
   .. function:: gettingSelectedcombinationLabel(lstSelectedCombination)
   .. function:: hideGoogleMap()
   .. function:: hideLoading()
   .. function:: LoadInitialValues()
   .. function:: LoadProjects()
   .. function:: LoadQuestionById(questionId)
   .. function:: LoadQuestions(lstQuestions)
   .. function:: LoadSensors()
   .. function:: optionAddRemoveAndCreditHtml(optCount)
   .. function:: optionAddRemoveButtonHtml(rmAddButtonCount)
   .. function:: placeCombination(k, isSelected, creditVal)
   .. function:: questionOptionHtml(questionCount)
   .. function:: refreshPage()
   .. function:: resetFileUplaodView()
   .. function:: resetOptionSection()
   .. function:: saveProjectIdAndAssetMode()
   .. function:: saveStartAndDestinationData()
   .. function:: saveValuesOnNextButton()
   .. function:: setQuestionStep()
   .. function:: setStartAndDestinationStep()
   .. function:: showLoading()
   .. function:: showUploadButton()
   .. function:: submitForHive(event)
   .. function:: submitFormevent(event)
   .. function:: uploadFile()


.. module:: CreateAssetMapView

CreateAssetMapView
==================


   Following are the global variables and methods.

   Here are references to global variables and methods:

Global Variables
----------------

    * :data:`addedQuestionCount`
    * :data:`assetMode`
    * :data:`boolAssociateQuestion`
    * :data:`combinationCounter`
    * :data:`combinationFound`
    * :data:`editedQuestionId`
    * :data:`lstCombinations`
    * :data:`lstOptionlen`
    * :data:`lstOptions`
    * :data:`lstQuestionDataModel`
    * :data:`lstSensors`
    * :data:`mainModel`
    * :data:`objCombination`
    * :data:`objCombinationId`
    * :data:`objMainModel`
    * :data:`objOption`
    * :data:`objOptionId`
    * :data:`objQuestionId`
    * :data:`objSensorId`
    * :data:`optionCounter`
    * :data:`optionsAdded`
    * :data:`optionsCombinations`
    * :data:`optionsString`
    * :data:`projectIds`
    * :data:`questionAddressList`
    * :data:`questionLocationMarkers`
    * :data:`questionOldLat`
    * :data:`questionOldLng`
    * :data:`questionType`
    * :data:`removeCombination`
    * :data:`selectOptionString`
    * :data:`sequence`
    * :data:`startAndDestinationModal`
    * :data:`updateSts`

   .. data:: addedQuestionCount
   .. data:: assetMode
   .. data:: boolAssociateQuestion
   .. data:: combinationCounter
   .. data:: combinationFound
   .. data:: editedQuestionId
   .. data:: lstCombinations
   .. data:: lstOptionlen
   .. data:: lstOptions
   .. data:: lstQuestionDataModel
   .. data:: lstSensors
   .. data:: mainModel
   .. data:: objCombination
   .. data:: objCombinationId
   .. data:: objMainModel
   .. data:: objOption
   .. data:: objOptionId
   .. data:: objQuestionId
   .. data:: objSensorId
   .. data:: optionCounter
   .. data:: optionsAdded
   .. data:: optionsCombinations
   .. data:: optionsString
   .. data:: projectIds
   .. data:: questionAddressList
   .. data:: questionLocationMarkers
   .. data:: questionOldLat
   .. data:: questionOldLng
   .. data:: questionType
   .. data:: removeCombination
   .. data:: selectOptionString
   .. data:: sequence
   .. data:: startAndDestinationModal
   .. data:: updateSts


Methods
-------

    * :func:`on("change", "#mainQuestionSelection", function (){...})`
    * :func:`on("change", "#questionType", function (){...})`
    * :func:`on("click", "#addButton", function (){...})`
    * :func:`on("click", "#btnSaveAssociateQuestion", function (){...})`
    * :func:`on("click", ".classRemoveOption", function (){...})`
    * :func:`on('click', "#deleteQuestionId", function (){...})`
    * :func:`on('click', "#editQuestionId", function (){...})`
    * :func:`on('keyup', '.optionsCredits', function (event){...})`
    * :func:`ready(function (){...})`
    * :func:`AddCombination()`
    * :func:`AddOption()`
    * :func:`addQuestionInList(qId)`
    * :func:`associateQuestion()`
    * :func:`checkCombinations(event)`
    * :func:`customException(m)`
    * :func:`DeleteDownloadedFileCall()`
    * :func:`downloadFile()`
    * :func:`function (str)`
    * :func:`function saveProjectIdAndAssetMode()`
    * :func:`function saveStartAndDestinationData()`
    * :func:`function setQuestionStep()`
    * :func:`function setStartAndDestinationStep()`
    * :func:`GenerateHiveCall()`
    * :func:`GenerateXMLCall()`
    * :func:`GetDownloadedFileCall()`
    * :func:`gettingSelectedcombinationLabel(lstSelectedCombination)`
    * :func:`hideLoading()`
    * :func:`LoadInitialValues()`
    * :func:`LoadProjects`
    * :func:`LoadQuestionById(questionId)`
    * :func:`LoadQuestions(lstQuestions)`
    * :func:`LoadSensors()`
    * :func:`optionAddRemoveAndCreditHtml(optCount)`
    * :func:`optionAddRemoveButtonHtml(rmAddButtonCount)`
    * :func:`placeCombination(k, isSelected, creditVal)`
    * :func:`questionOptionHtml(questionCount)`
    * :func:`refreshPage()`
    * :func:`resetFileUplaodView()`
    * :func:`resetOptionSection()`
    * :func:`showLoading()`
    * :func:`showUploadButton()`
    * :func:`submitForHive(event)()`
    * :func:`submitFormevent(event)()`
    * :func:`uploadFile()`





   .. function:: on("change", "#mainQuestionSelection", function (){...})
   .. function:: on("change", "#questionType", function (){...})
   .. function:: on("click", "#addButton", function (){...})
   .. function:: on("click", "#btnSaveAssociateQuestion", function (){...})
   .. function:: on("click", ".classRemoveOption", function (){...})
   .. function:: on('click', "#deleteQuestionId", function (){...})
   .. function:: on('click', "#editQuestionId", function (){...})
   .. function:: on('keyup', '.optionsCredits', function (event){...})
   .. function:: ready(function (){...})
   .. function:: AddCombination()
   .. function:: AddOption()
   .. function:: addQuestionInList(qId)
   .. function:: associateQuestion()
   .. function:: checkCombinations(event)
   .. function:: customException(m)
   .. function:: DeleteDownloadedFileCall()
   .. function:: downloadFile()
   .. function:: function (str)
   .. function:: function saveProjectIdAndAssetMode()
   .. function:: function saveStartAndDestinationData()
   .. function:: function setQuestionStep()
   .. function:: function setStartAndDestinationStep()
   .. function:: GenerateHiveCall()
   .. function:: GenerateXMLCall()
   .. function:: GetDownloadedFileCall()
   .. function:: gettingSelectedcombinationLabel(lstSelectedCombination)
   .. function:: hideLoading()
   .. function:: LoadInitialValues()
   .. function:: LoadProjects
   .. function:: LoadQuestionById(questionId)
   .. function:: LoadQuestions(lstQuestions)
   .. function:: LoadSensors()
   .. function:: optionAddRemoveAndCreditHtml(optCount)
   .. function:: optionAddRemoveButtonHtml(rmAddButtonCount)
   .. function:: placeCombination(k, isSelected, creditVal)
   .. function:: questionOptionHtml(questionCount)
   .. function:: refreshPage()
   .. function:: resetFileUplaodView()
   .. function:: resetOptionSection()
   .. function:: showLoading()
   .. function:: showUploadButton()
   .. function:: submitForHive(event)()
   .. function:: submitFormevent(event)()
   .. function:: uploadFile()


.. module:: CreateAssetMapView

ProjectConfigurations
=====================

   Here are references to global variables and methods:

Methods
-------

    * :func:`ready(function (){...})`
    * :func:`assignment_loadAssets(projectId)`
    * :func:`assignment_loadTasks(projectId)`
    * :func:`assignment_loadUsers(projectId)`
    * :func:`assignment_resetAssets()`
    * :func:`assignment_resetProjects()`
    * :func:`assignment_resetTasks()`
    * :func:`assignment_resetUsers()`
    * :func:`clearProjectCreationForm()`
    * :func:`clearTaskCreationForm()`
    * :func:`loadProjects()`
    * :func:`resetAll()`

   .. function:: ready(function ()
   .. function:: assignment_loadAssets(projectId)
   .. function:: assignment_loadTasks(projectId)
   .. function:: assignment_loadUsers(projectId)
   .. function:: assignment_resetAssets()
   .. function:: assignment_resetProjects(
   .. function:: assignment_resetTasks(
   .. function:: assignment_resetUsers()
   .. function:: clearProjectCreationForm()
   .. function:: clearTaskCreationForm(
   .. function:: loadProjects()
   .. function:: resetAll()
