.. default-domain:: csharp

######
Client
######

.. namespace:: HiveServer

*******************
HiveServer-> Client
*******************

.. class:: Client

   Following are the properties that a Client class will contain.

   Here are references to it's properties:

    * :prop:`client`
    * :prop:`host`
    * :prop:`port`
    * :prop:`url`

   .. property:: HttpClient client { get; set; }

	   It is HttpClient object for communication with the :ref:`Hive` Server. It is initialized in client constructor.

   .. property:: string host { get; set; }

       It is the base address of the :ref:`Hive` Server. It is initialized in client constructor.


   .. property:: string port { get; set; }

       It is the port number of the :ref:`Hive` Server. It is initialized in client constructor.


   .. property:: string url { get; set; }

       It is the url address of the :ref:`Hive` Server containing host and port number. It is initialized in client constructor.


   Here are references to it's methods:

   * :meth:`Client`
   * :meth:`CreateAsset`
   * :meth:`CreateAssignment`
   * :meth:`CreateProject`
   * :meth:`CreateTask`
   * :meth:`GetAllProjects`
   * :meth:`GetAssetAsync`
   * :meth:`GetAssetData`
   * :meth:`GetTasks`
   * :meth:`GetTasksData`
   * :meth:`GetUserAsync`
   * :meth:`GetUserData`




   .. method:: public Client()

	   It is the constructor of the class. It contains the initialization of the client, host, port and url variables.

   .. method:: public void CreateAsset(string projectId, Models.Asset[] Asset, string userName)

	   It contains the functionality for the creation of Asset on the :ref:`Hive` Server. It also creates an asset file on disk. There is a boolean for checking either the environment is Windows or Linux

   .. method:: public async Task<Models.Assignment> CreateAssignment(string projectId, string taskId, string assetId, string userId)

       It creates Assignment on :ref:`Hive` Server according to the provided parameters that are project Id, task Id, asset Id, and user Id.

   .. method:: public async Task<Models.Project> CreateProject(Models.Project project)

       It is called for creating Project on the :ref:`Hive` Server. It accepts a project object so that it can create a project.

   .. method:: public async Task<Models.Task> CreateTask(string projectId, Models.Task task)

       It is called for creating Task on the :ref:`Hive` Server. Project Id and a task object are provided to it.

   .. method:: public async Task<List<Models.Project>> GetAllProjects()

       It is called to get all the projects from the :ref:`Hive` Server. There are two parameters: 'from' and 'size' to set the project's objects quantity.

   .. method:: public async Task<List<Models.Asset>> GetAssetAsync(string projectId)

	   It accepts a project Id and returns all Assets from the :ref:`Hive` Server that are associated with given project Id.

   .. method:: public async Task<Models.Asset> GetAssetData(string projectId, string assetId)

       It accepts project Id and an assets Id and returns the asset from the :ref:`Hive` Server that contains the provided project and asset Id.

   .. method:: public async Task<List<Models.Task>> GetTasks(string projectId)

	   It accepts a project Id and then retrieves all tasks from :ref:`Hive` Server that are associated with provided project Id.

   .. method:: public async Task<string> GetTasksData(string projectId, string taskId)

       It accepts the project Id and a task Id and returns the task object from :ref:`Hive` Server related to the given project and task Id.

   .. method:: public async Task<List<Models.User>> GetUserAsync(string projectId)

	   It accepts a project Id and returns all users object from :ref:`Hive` Server that are associated with this project.

   .. method:: public async Task<Models.User> GetUserData(string projectId, string userId)

       It accepts a project Id and a user Id and return user object from :ref:`Hive` Server that contain the provided project and user Id.
