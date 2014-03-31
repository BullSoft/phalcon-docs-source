使用依赖注入
==========================
下面这个示例显得有点冗余，但它解释了我们为什么要使用服务容器和依赖注入。首先，假设我们正在开发一个名为SomeComponent的组件。此时，组件完成什么任务并不重要，重要的是组件在连接数据库时有一些外部依赖。

The following example is a bit lengthy, but explains why using a service container and dependency injection. To begin with, let's pretend we are developing a component called SomeComponent. This performs a task that is not important right now. Our component have some dependency that is a connection to a database.

在第一个示例中，我们在组件中创建数据库连接。这么做是不切实际的，我们既不能改变连接参数，也不能改变数据库系统，因为这些早已写死在组件里了。

In this first example, the connection is created inside the component. This approach is impractical, practically we can not change the connection parameters or the type of database system because the component only works as created.

.. code-block:: php

    <?php

    class SomeComponent
    {

        /**
         * The instantiation of the connection is hardcoded inside
         * the component so is difficult to replace it externally
         * or change its behavior
         */
        public function someDbTask()
        {
            $connection = new Connection(array(
                "host" => "localhost",
                "username" => "root",
                "password" => "secret",
                "dbname" => "invo"
            ));

            // ...
        }

    }

    $some = new SomeComponent();
    $some->someDbTask();

为了解决这个问题，我们创建了一个setter，在使用数据库之前从外部注入这个依赖。仅从现在来看，这似乎是一个好的解决方案：
    
To solve this we create a setter that injects the dependency externally before use it. For now, this seems to be a good solution:

.. code-block:: php

    <?php

    class SomeComponent
    {

        protected $_connection;

        /**
         * Sets the connection externally
         */
        public function setConnection($connection)
        {
            $this->_connection = $connection;
        }

        public function someDbTask()
        {
            $connection = $this->_connection;

            // ...
        }

    }

    $some = new SomeComponent();

    //Create the connection
    $connection = new Connection(array(
        "host" => "localhost",
        "username" => "root",
        "password" => "secret",
        "dbname" => "invo"
    ));

    //Inject the connection in the component
    $some->setConnection($connection);

    $some->someDbTask();

现在考虑一下，如果要在应用的不同地方使用该组件，那么我们每次都需要创建这个数据库连接，然后把它传递给组件。全局注册表可以解决这个问题，我们可以在注册表中维持一个数据库连接实例，之后就不用反复创建它了。
    
Now consider that we use this component in different parts of the application, then we will need to create the connection several times before pass it to the component. This could be solved by using some kind of global registry where we obtain the connection instance and not have to create it again and again.

.. code-block:: php

    <?php

    class Registry
    {

        /**
         * Returns the connection
         */
        public static function getConnection()
        {
           return new Connection(array(
                "host" => "localhost",
                "username" => "root",
                "password" => "secret",
                "dbname" => "invo"
            ));
        }

    }

    class SomeComponent
    {

        protected $_connection;

        /**
         * Sets the connection externally
         */
        public function setConnection($connection){
            $this->_connection = $connection;
        }

        public function someDbTask()
        {
            $connection = $this->_connection;

            // ...
        }

    }

    $some = new SomeComponent();

    //Pass the connection defined in the registry
    $some->setConnection(Registry::getConnection());

    $some->someDbTask();

现在想象一下，我们必须在组件内实现两个方法，第一个方法用于创建新的数据库连接，第二个方法使用共享的数据库连接：
    
Now, let's imagine that we must to implement two methods in the component, the first always need to create a new connection and the second always need to use a shared connection:

.. code-block:: php

    <?php

    class Registry
    {

        protected static $_connection;

        /**
         * Creates a connection
         */
        protected static function _createConnection()
        {
            return new Connection(array(
                "host" => "localhost",
                "username" => "root",
                "password" => "secret",
                "dbname" => "invo"
            ));
        }

        /**
         * Creates a connection only once and returns it
         */
        public static function getSharedConnection()
        {
            if (self::$_connection===null){
                $connection = self::_createConnection();
                self::$_connection = $connection;
            }
            return self::$_connection;
        }

        /**
         * Always returns a new connection
         */
        public static function getNewConnection()
        {
            return self::_createConnection();
        }

    }

    class SomeComponent
    {

        protected $_connection;

        /**
         * Sets the connection externally
         */
        public function setConnection($connection){
            $this->_connection = $connection;
        }

        /**
         * This method always needs the shared connection
         */
        public function someDbTask()
        {
            $connection = $this->_connection;

            // ...
        }

        /**
         * This method always needs a new connection
         */
        public function someOtherDbTask($connection)
        {

        }

    }

    $some = new SomeComponent();

    //This injects the shared connection
    $some->setConnection(Registry::getSharedConnection());

    $some->someDbTask();

    //Here, we always pass a new connection as parameter
    $some->someOtherDbTask(Registry::getConnection());

目前为止，我们已经见识到依赖注入是如何解决问题的。通过参数传递依赖，而不是在代码里面创建依赖，这么做可以让应用的各部分充分解耦，而且代码可维护性更高。
    
So far we have seen how dependency injection solved our problems. Passing dependencies as arguments instead of creating them internally in the code makes our application more maintainable and decoupled. However to long term, this form of dependency injection have some disadvantages.

如果该组件有很多依赖，我们要创建很多setter来传递依赖，或通过创建构造器通过参数传递，每次使用组件时都要额外创建依赖，使得代码不像预期的那样可维护：

For instance, if the component has many dependencies, we will need to create multiple setter arguments to pass the dependencies or create a constructor that pass them with many arguments, additionally create dependencies before use the component, every time, makes our code not maintainable as we would like:

.. code-block:: php

    <?php

    //Create the dependencies or retrieve them from the registry
    $connection = new Connection();
    $session = new Session();
    $fileSystem = new FileSystem();
    $filter = new Filter();
    $selector = new Selector();

    //Pass them as constructor parameters
    $some = new SomeComponent($connection, $session, $fileSystem, $filter, $selector);

    // ... or using setters

    $some->setConnection($connection);
    $some->setSession($session);
    $some->setFileSystem($fileSystem);
    $some->setFilter($filter);
    $some->setSelector($selector);

想想我们不得不在应用的许多地方创建这些对象。如果不再需要某些依赖，我们不得不找到每个注入该依赖的setter或构造器，并进行逐个修改删除。为了解决这个问题，我们使用全局注册表创建组件。但是，在创建对象之前，它新增了一个抽象层：
    
Think we had to create this object in many parts of our application. If you ever do not require any of the dependencies, we need to go everywhere to remove the parameter in the constructor or the setter where we injected the code. To solve this we return again to a global registry to create the component. However, it adds a new layer of abstraction before creating the object:

.. code-block:: php

    <?php

    class SomeComponent
    {

        // ...

        /**
         * Define a factory method to create SomeComponent instances injecting its dependencies
         */
        public static function factory()
        {

            $connection = new Connection();
            $session = new Session();
            $fileSystem = new FileSystem();
            $filter = new Filter();
            $selector = new Selector();

            return new self($connection, $session, $fileSystem, $filter, $selector);
        }

    }

这一刻，我们仿佛回到了开始的地方，我们又在组件内构建依赖！每一次我们继续寻找方法来解决这个问题。但似乎又会走向黑暗的深渊（糟糕的实践）。
    
One moment, we returned back to the beginning, we are building again the dependencies inside the component! We can move on and find out a way to solve this problem every time. But it seems that time and again we fall back into bad practices.

能找到一种实用又优雅的方式来解决这个问题吗？当然有，答案就是依赖容器。容器扮演全局注册表的角色。依赖容器就像一座桥梁一样帮助我们获得依赖，极大地减小了我们组件的复杂度：

A practical and elegant way to solve these problems is to use a container for dependencies. The containers act as the global registry that we saw earlier. Using the container for dependencies as a bridge to obtain the dependencies allows us to reduce the complexity of our component:

.. code-block:: php

    <?php

    class SomeComponent
    {

        protected $_di;

        public function __construct($di)
        {
            $this->_di = $di;
        }

        public function someDbTask()
        {

            // Get the connection service
            // Always returns a new connection
            $connection = $this->_di->get('db');

        }

        public function someOtherDbTask()
        {

            // Get a shared connection service,
            // this will return the same connection everytime
            $connection = $this->_di->getShared('db');

            //This method also requires a input filtering service
            $filter = $this->_db->get('filter');

        }

    }

    $di = new Phalcon\DI();

    //Register a "db" service in the container
    $di->set('db', function(){
        return new Connection(array(
            "host" => "localhost",
            "username" => "root",
            "password" => "secret",
            "dbname" => "invo"
        ));
    });

    //Register a "filter" service in the container
    $di->set('filter', function(){
        return new Filter();
    });

    //Register a "session" service in the container
    $di->set('session', function(){
        return new Session();
    });

    //Pass the service container as unique parameter
    $some = new SomeComponent($di);

    $some->someTask();

现在，组件便能在需要时轻易访问它所依赖的服务了。如果它不依赖某个服务，该服务甚至都不会初始化，这能极大节省资源。现在组件已经高解耦了。例如，我们可以更改数据库连接的创建方式，它们的行为或其他方面的改变，不会对给件造成任何影响。
    
The component now simply access the service it require when it needs it, if it does not requires a service, that is not even initialized saving resources. The component is now highly decoupled. For example, we can replace the manner in which connections are created, their behavior or any other aspect of them and that would not affect the component.

我们的方法
------------
Phalcon\\DI 是实现依赖注入服务的一个给件，它本身就是一个容器。

Phalcon\\DI is a component that implements Dependency Injection of services and it's itself a container for them.

由于Phalcon是高度解耦的，Phalcon\\DI 是集成框架不同组件必不可少的。开发者也能在应用中使用该组件注入依赖，管理不同类的全局实例。

Since Phalcon is highly decoupled, Phalcon\\DI is essential to integrate the different components of the framework. The developer can also use this component to inject dependencies and manage global instances of the different classes used in the application.

基本上，该组件实现了 `控制反转`_ 模式。使用依赖注入，对象不再使用setter或构造器接收依赖了，而是向依赖注入容器请求服务。这减小了整体复杂度，因为现在在组件中我们只有一种途径获取依赖关系。

Basically, this component implements the `Inversion of Control`_ pattern. Applying this, the objects do not receive their dependencies using setters or constructors, but requesting a service dependency injector. This reduces the overall complexity, since there is only one way to get the required dependencies within a component.

另外，这种模式增加了代码的可测试性，从而使其不易出错。

Additionally, this pattern increases testability in the code, thus making it less prone to errors.

在容器中注册服务
-------------------------------------
服务既可以是框架注册的，也可以是开发者注册的。当组件A依赖于组件B（或其实例）时，它可以从容器中请求组件B，而不是创建组件B的新实例。

Services can be registered by the framework itself or the developer. When a component A requires component B (or an instance of its class) to operate, it can request component B from the container, rather than creating a new instance component B.

这么做给我们带来很多好处：

This way of working gives us many advantages:

* 我们可以使用自己或第三方的组件代替当前组件。
* 我们可以完全控制对象初始化，在传递给组件之前，我们可以随心所欲设置对象。
* 我们可以用结构化和统一的方式获取组件的全局实例。


* We can replace a component by one created by ourselves or a third party one easily.
* We have full control of the object initialization, allowing us to set this objects as you need before delivery them to components.
* We can get global instances of components in a structured and unified way

注册服务的方式不只一种：

Services can be registered in several ways:

.. code-block:: php

    <?php

	//Create the Dependency Injector Container
	$di = new Phalcon\DI();

	//By its class name
	$di->set("request", 'Phalcon\Http\Request');

	//Using an anonymous function, the instance will lazy loaded
	$di->set("request", function(){
	    return new Phalcon\Http\Request();
	});

	//Registering directly an instance
	$di->set("request", new Phalcon\Http\Request());

	//Using an array definition
	$di->set("request", array(
	    "className" => 'Phalcon\Http\Request'
	));

在上面的示例中，当框架要获取（用户）请求数据时，它将会向容器请求'request'服务。反过来，容器将会返回所请求服务的一个实例。开发者在需要时可以替换这个组件。
    
In the above example, when the framework needs to access the request data, it will ask for the service identified as ‘request’ in the container. The container in turn will return an instance of the required service. A developer might eventually replace a component when he/she needs.

上面列出来的每种方法都可以设置或注册服务，但它们各有优缺点。具体使用哪种方法取决于开发者和实际需求。

Each of the methods (demonstrated in the above example) used to set/register a service has advantages and disadvantages. It is up to the developer and the particular requirements that will designate which one is used.

通过字符串设置服务是最简单的，但也最缺乏灵活性。通过数组设置服务提供了更多的灵活性，但使得代码看复杂。匿名函数在前两种方法上寻得一个平衡，不仅如此，使用它比我们预期的更具维护性。

Setting a service by a string is simple but lacks flexibility. Setting services using an array offers a lot more flexibility but makes the code more complicated. The lambda function is a good balance between the two but could lead to more maintenance than one would expect.

Phalcon\\DI 为它存储的每个服务提供延迟加载功能。除非开发者选择直接实例化对象然后再存储在容器中，任何通过数组、字符串等方式存储的对象都会被延迟加载，也即当被请求时才会被实例化。

Phalcon\\DI offers lazy loading for every service it stores. Unless the developer chooses to instantiate an object directly and store it in the container, any object stored in it (via array, string etc.) will be lazy loaded i.e. instantiated only when requested.

.. code-block:: php

    <?php

    //Register a service "db" with a class name and its parameters
    $di->set("db", array(
        "className" => "Phalcon\Db\Adapter\Pdo\Mysql",
        "parameters" => array(
              "parameter" => array(
                   "host" => "localhost",
                   "username" => "root",
                   "password" => "secret",
                   "dbname" => "blog"
              )
        )
    ));

    //Using an anonymous function
    $di->set("db", function(){
        return new Phalcon\Db\Adapter\Pdo\Mysql(array(
             "host" => "localhost",
             "username" => "root",
             "password" => "secret",
             "dbname" => "blog"
        ));
    });

上面两种服务注册产生的结果是一样的。但是如果有必要的话，数组定义允许修改服务参数：
    
Both service registrations above produce the same result. The array definition however, allows for alteration of the service parameters if needed:

.. code-block:: php

    <?php

    $di->setParameter("db", 0, array(
        "host" => "localhost",
        "username" => "root",
        "password" => "secret"
    ));

从容器中获得服务只需简单地调用"get"方法。一个新的服务实例将会被返回：
    
Obtaining a service from the container is a matter of simply calling the “get” method. A new instance of the service will be returned:

.. code-block:: php

    <?php $request = $di->get("request");

又或者通过魔术方法调用：
    
Or by calling through the magic method:

.. code-block:: php

    <?php

    $request = $di->getRequest();

Phalcon\\DI 还允许服务重用。如果要获取之前已经实例化的服务，可以使用getShared()方法。 特别是上面提到的 Phaclon\\Http\\Request 服务：
    
Phalcon\\DI also allows for services to be reusable. To get a service previously instantiated the getShared() method can be used. Specifically for the Phalcon\\Http\\Request example shown above:

.. code-block:: php

    <?php

    $request = $di->getShared("request");

在"get"方法中添加数组参数，可以把参数传递给类构造器：
    
Arguments can be passed to the constructor by adding an array parameter to the method "get":

.. code-block:: php

    <?php

    $component = $di->get("MyComponent", array("some-parameter", "other"))

工厂默认DI
------------------
尽管Phalcon的高解耦特性给我们提供了很大的自由度和灵活性，也许我们只想简单地把它当作一个全功能框架。为了实现这一目标，框架提供了 Phalcon\\DI 的一个变种，叫 Phaclon\\DI\\FactoryDefault。这个类会自动地注册框架的相应服务，从而使其成为全功能框架。

Although the decoupled character of Phalcon offers us great freedom and flexibility, maybe we just simply want to use it as a full-stack framework. To achieve this, the framework provides a variant of Phalcon\\DI called Phalcon\\DI\\FactoryDefault. This class automatically registers the appropriate services bundled with the framework to act as full-stack.

.. code-block:: php

    <?php $di = new Phalcon\DI\FactoryDefault();

服务名称约定
------------------------
尽管你可以用你想要的名称注册服务。 Phaclon 有一系列服务名称约定，好让你在需要它们的时候能获取正确的服务。

Although you can register services with the names you want. Phalcon has a seriers of service naming conventions that allow it to get the right services when you need it requires them.

+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| Service Name   | Description                                 | Default                                                                                 |
+================+=============================================+=========================================================================================+
| dispatcher     | Controllers Dispatching Service             | :doc:`Phalcon\\Mvc\\Dispatcher <../api/Phalcon_Mvc_Dispatcher>`                         |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| router         | Routing Service                             | :doc:`Phalcon\\Mvc\\Router <../api/Phalcon_Mvc_Router>`                                 |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| url            | URL Generator Service                       | :doc:`Phalcon\\Mvc\\Url <../api/Phalcon_Mvc_Url>`                                       |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| request        | HTTP Request Environment Service            | :doc:`Phalcon\\Http\\Request <../api/Phalcon_Http_Request>`                             |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| response       | HTTP Response Environment Service           | :doc:`Phalcon\\Http\\Response <../api/Phalcon_Http_Response>`                           |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| filter         | Input Filtering Service                     | :doc:`Phalcon\\Filter <../api/Phalcon_Filter>`                                          |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| flash          | Flash Messaging Service                     | :doc:`Phalcon\\Flash\\Direct <../api/Phalcon_Flash_Direct>`                             |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| session        | Session Service                             | :doc:`Phalcon\\Session\\Adapter\\Files <../api/Phalcon_Session_Adapter_Files>`          |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| eventsManager  | Events Management Service                   | :doc:`Phalcon\\Events\\Manager <../api/Phalcon_Events_Manager>`                         |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| db             | Low-Level Database Connection Service       | :doc:`Phalcon\\Db <../api/Phalcon_Db>`                                                  |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| modelsManager  | Models Management Service                   | :doc:`Phalcon\\Mvc\\Model\\Manager <../api/Phalcon_Mvc_Model_Manager>`                  |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+
| modelsMetadata | Models Meta-Data Service                    | :doc:`Phalcon\\Mvc\\Model\\MetaData\\Memory <../api/Phalcon_Mvc_Model_MetaData_Memory>` |
+----------------+---------------------------------------------+-----------------------------------------------------------------------------------------+

通过服务容器实例化类
------------------------------------------------
当你向服务容器请求服务时，如果它无法找到该服务，它会尝试加载相同名称的类。通过这种行为，我们可以通过简单地注册一个相同名称的服务来取代任何类：

When you request a service to the services container, if it can't find out a service with the same name it'll try to load a class with the same name. With this behavior we can replace any class by another simply by registering a service with its name:

.. code-block:: php

    <?php

    //Register a controller as a service
    $di->set('IndexController', function() {
        $component = new Component();
        return $component;
    });

    //Register a controller as a service
    $di->set('MyOtherComponent', function() {
        //Actually returns another component
        $component = new AnotherComponent();
        return $component;
    });

    //Create a instance via the services container
    $myComponent = $di->get('MyOtherComponent');

我们可以利用这一点，总是通过服务容器实例化类，即使你没有把它们注册为服务。DI 将会退化为一个类自动加载器，并最终加载该类。
    
You can take advantage of this, always instantiating your classes via the services container (even if they aren't registered as services). The DI will fallback to a valid autoloader to finally load the class.


.. _`控制反转`: http://en.wikipedia.org/wiki/Inversion_of_control
.. _`Inversion of Control`: http://en.wikipedia.org/wiki/Inversion_of_control
