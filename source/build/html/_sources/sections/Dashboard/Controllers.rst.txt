.. default-domain:: csharp

###########
Controllers
###########


*********
XMLParser
*********

.. namespace:: XMLParser.Controllers.XMLParser

XMLParserController
===================
.. class:: XMLParserController

   This is API controller that contains method for communication of the android app and the :ref:`Hive` Server.

   Here are some references to it's methods:

   * :meth:`GenerateHiveCall`
   * :meth:`GenerateXMLFile`
   * :meth:`GetDownloadFile`
   * :meth:`DeleteDownloadedFile`
   * :meth:`GetAllProjects`
   * :meth:`SaveAssignment`
   * :meth:`SaveSensorData`



   .. method:: public void GenerateHiveCall(ProjectQuestionModel projectQuestionModel)

       It is called for the Asset creation on the :ref:`Hive` Server.

   .. method:: public IHttpActionResult GenerateXMLFile(ProjectQuestionModel mainModel)

       It is called for creating an asset file with extension .json for downloading, viewing and analyzing the asset.

   .. method:: public HttpResponseMessage GetDownloadFile()

       It is called to download already created JSON asset file from the server.

   .. method:: public IHttpActionResult DeleteDownloadedFile()

       It is called after GetDownloadFile to delete the json asset file from the server to avoid creating numerous useless files.

   .. method:: public async System.Threading.Tasks.Task<List<HiveServer.Models.Project>> GetAllProjects(Questions mainModel)

       It is called to get all existing projects from the :ref:`Hive` Server.

   .. method:: public IHttpActionResult SaveAssignment(Assignment assignmentModel)

       It is called for saving the submitted assignment on the :ref:`Hive` Server.

   .. method:: public IHttpActionResult SaveSensorData(SensorDataModel sensorDataModel)

       It is called to save sensors information.


.. namespace:: XMLParser.Controllers

************************
AuthenticationController
************************

.. class:: AuthenticationController


**************
HiveController
**************

.. class:: HiveController

   This is a Controller with methods.

   Here are some references to its methods:

   * :meth:`Index`
   * :meth:`GetAllProjects`
   * :meth:`GetProjectsIds`
   * :meth:`GetAssetData`
   * :meth:`GetAssetsIds`
   * :meth:`GetUserData`
   * :meth:`GetUsersIds`
   * :meth:`GetTaskData`
   * :meth:`GetTasksIds`
   * :meth:`CreateAssignment`
   * :meth:`CreateProject`
   * :meth:`CreateTask`

   .. method:: public ActionResult Index()

   .. method:: public void GenerateHiveCall(ProjectQuestionModel projectQuestionModel)

       It is called for an Asset creation on the :ref:`Hive` Server.

   .. method:: public async Task<ActionResult> GetAllProjects(jQueryDataTableParamModel param)

       It is called to get all projects from the :ref:`Hive` Server.

   .. method:: public async Task<ActionResult> GetProjectsIds()

       It is called to get only Project Ids of all projects that exist on the :ref:`Hive` Server.

   .. method:: public async Task<ActionResult> GetAssetData(string projectId, string assetId)

       It accepts project Id and an asset Id and returns the corresponding asset object after getting from the :ref:`Hive` server.

   .. method:: public async Task<ActionResult> GetAssetsIds(string projectId)

       It accepts a project Id and returns Ids of all assets that are associated with given project Id.

   .. method:: public async Task<ActionResult> GetUserData(string projectId, string userId)

       It accepts project ID and a user Id and returns a specific user.

   .. method:: public async Task<ActionResult> GetUsersIds(string projectId)

       It accepts a project Id and returns all user Ids that are associated with given project Id.

   .. method:: public async Task<ActionResult> GetTaskData(string projectId, string taskId)

       It accepts project Id and a task Id and returns specific task object.

   .. method:: public async Task<ActionResult> GetTasksIds(string projectId)

       It accepts project Id and returns all task Ids that are associated with given project Id.

   .. method:: public async Task<ActionResult> CreateAssignment(string projectId, string taskId, string assetId, string userIdsList)

       It is called for the creation of an assignment for single or multiple users.

   .. method:: public async Task<ActionResult> CreateProject(string projectId, string projectName, string projectDesc)

       It is called for the creation of a Project. The project description is an optional.

   .. method:: public async Task<ActionResult> CreateTask(string projectId, string name, string desc, string state, int total, int matching)

       It is called for creating a task.


**************
HomeController
**************

.. class:: HomeController

   This is a Controller with methods.

   Here are some references to it's methods:

   * :meth:`Index`
   * :meth:`HiveView`
   * :meth:`CreateProjectView`
   * :meth:`ProjectConfiguration`
   * :meth:`ProjectsView`
   * :meth:`AssetsView`
   * :meth:`UsersView`
   * :meth:`TasksView`
   * :meth:`CreateTaskView`
   * :meth:`CreateAssignmentView`
   * :meth:`MyXMLParser`
   * :meth:`Signup`
   * :meth:`Login`
   * :meth:`AssetCreationModeSelection`
   * :meth:`LogOut`
   * :meth:`serverStorageTesting`
   * :meth:`createProject`
   * :meth:`createTask`
   * :meth:`createAssignment`
   * :meth:`CreateTaskView`
   * :meth:`createAsset`
   * :meth:`SaveSession`
   * :meth:`LoginCall`
   * :meth:`AssociateQuestion`
   * :meth:`RegisterCall`
   * :meth:`UploadFileView`
   * :meth:`EditProfile`
   * :meth:`UpdateProfile`
   * :meth:`CreateAssetMapView`
   * :meth:`CreateAssetFormView`


   .. method:: public ActionResult Index()

       It is called very first when a user reaches the site and after validating login information, it will return the user to either home view or on login view.

   .. method:: public ActionResult HiveView()



   .. method:: public ActionResult CreateProjectView()

   .. method:: public ActionResult ProjectConfiguration()

       It is called for returning Project Configuration View On this view, a user can create a project, task, and assignment without leaving the view.

   .. method:: public ActionResult ProjectsView()

       It is called to view the summary of all the projects in a tabular form.

   .. method:: public ActionResult AssetsView()

       It is called for viewing a specific asset after selecting the project and an asset.

   .. method:: public ActionResult UsersView()

       It is called to view specific user after selecting a project and a user.

   .. method:: public ActionResult TasksView()

       It is called to view the specific task after selecting a project and a task.

   .. method:: public ActionResult CreateTaskView()



   .. method:: public ActionResult CreateAssignmentView()



   .. method:: public ActionResult MyXMLParser()



   .. method:: public ActionResult Signup()

       It is called to return the Signup view to a user so that the user can Signup.

   .. method:: public ActionResult Login()

       It is called to return the Login view to provide the login functionality to the user.

   .. method:: public ActionResult LogOut()

       It is called when the session is expired or when the user clicks on logout button.

   .. method:: public ActionResult serverStorageTesting()

       It is called for testing server storage by automatically creating projects, task, asset etc.

   .. method:: public JsonResult createProject(string number)

       It is part of the server storage testing section for the creation of the project.

   .. method:: public JsonResult createTask(string number, string projectId, string state, int total, int matching)

       It is part of the server storage testing section for the creation of the task.

   .. method:: public JsonResult createAssignment(string number, string projectId, string task_id, string asset_ids, string user_id)

       It is part of the server storage testing section for the creation of the Assignment.

   .. method:: public JsonResult createAsset(string number, string data)

       It is part of the server storage testing section for the creation of the Asset.

   .. method:: public ActionResult SaveSession(string sessionValue)

       It is called for saving username/ email in an Http Session variable for checking login status.

   .. method:: public JsonResult LoginCall(string email,string password)

       It is called to verify the given credentials from the MySql DB.

   .. method:: public ActionResult AssociateQuestion()

       It is called to return the view for question association.

   .. method:: public JsonResult RegisterCall(RegisterDTO registerdto)

       It is called to register a user i.e saving user credentials in DB if it does not exists already.

   .. method:: public ActionResult UploadFileView()

       It is called to return the file uploading view so that a user can upload a manual asset file instead of creating an asset on the Map view or Form view.

   .. method:: public ActionResult EditProfile()

       It is called to return the view for editing the profile if a user wants to update his/her information.

   .. method:: public JsonResult UpdateProfile(RegisterDTO registerdto)

       It is called to updated information of the user in MySql DB.

   .. method:: public ActionResult CreateAssetMapView()

       It is called to return the view for creating Asset through Map wizard.

   .. method:: public ActionResult CreateAssetFormView()

       It is called to return the view for asset creation through Form wizard.

*************
XMLController
*************

.. class:: XMLController

   This is a Controller class with methods.

   Here are some references to its methods:

    * :meth:`HiveCall`

   .. method:: public IHttpActionResult HiveCall(string str)


****************
ValuesController
****************

.. class:: ValuesController

   This is a Controller class with methods.

   Here are some references to it's methods:

    * :meth:`Get`
    * :meth:`Get`
    * :meth:`Post`
    * :meth:`Put`
    * :meth:`Delete`

   .. method:: public IEnumerable<string> Get()

   .. method:: public string Get(int id)

   .. method:: public void Post(string value)

   .. method:: public void Put(int id,string value)

   .. method:: public void Delete(int id)
