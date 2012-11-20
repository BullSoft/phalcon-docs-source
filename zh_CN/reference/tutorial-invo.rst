======================
 教程二：发票应用程序
======================

在教程二中，我们将会介绍一个更完整的应用程序，以更深入地了解Phalcon开发。发票应用程序（以下简称INVO）是我们创建的示例程序之一，它是一个小型网站，允许用户在线申请发票，以及管理客户和产品。你可以在这里检出代码 Github_ 。

In this second tutorial, we'll explain a more complete application in order to deepen the development with Phalcon. INVO is one of the applications we have created as samples. INVO is a small website that allows their users to generate invoices, and do other tasks as manage their customers and products. You can clone its code from Github_.

INVO 使用 `Twitter Bootstrap <http://twitter.github.com/>`_ 作为客户端框架。尽管此应用不会真的产生发票，但作为一个示例程序也能帮助我们了解框架是如何工作的。

Also, INVO was made with `Twitter Bootstrap <http://twitter.github.com/>`_ as client-side framework. Although the application does not generate invoices still it serves as an example to understand how the framework works.

项目结构
========
当你检出项目代码后，你可以看到如下文件结构：

Once you clone the project in your document root you'll see the following structure:

.. code-block:: bash

    invo/
        app/
            app/config/
            app/controllers/
            app/library/
            app/models/
            app/plugins/
            app/views/
        public/
            public/bootstrap/
            public/css/
            public/js/
        schemas/

正如你所知，Phalcon对应用开发没有特殊的文件结构要求。本项目包含一个简单的MVC目录和一个公共文档根目录。
        
As you know, Phalcon does not impose a particular file structure for application development. This project provides a simple MVC structure and a public document root.

如果你在浏览器中打开 http://localhost/invo ，你将会看到这样的画面：

Once you open the application in your browser http://localhost/invo you'll something like this:

.. figure:: ../_static/img/invo-1.png
   :align: center

本应用程序分为两部分：第一部分是前端，这是一个公共部分，访问者可以获取INVO相关的信息并索要联系方式；第二部分是后端，是一个管理区域，注册用户在这里可以管理产品和客户信息。
   
The application is divided in two parts a frontend, that is a public part where visitors can receive information about INVO and request contact information. The second part is the backend, is an administrative area where a registered user can manage his products and customers.

路由规则
========
INVO使用Phalcon内置的标准路由方式。这些路由使用的规则是：/:controller/:action/:params。也就是说URL的第一部分是控制器，第二部分是动作，第三部分是参数列表。

INVO uses the standard route that is builtin with the Router component. This routes matches the following pattern: /:controller/:action/:params. This means that the first part of the url is the controller, the second the action and so on.

例如：/session/register 这个URI将会被路由到SessionController控制器和registerAction动作。

The following route /session/register will execute the controller SessionController and its action registerAction.

系统配置
========
INVO有一个配置文件，用于设置应用程序的通用参数。引导程序（public/index.php）的前几行就读取了配置文件。

INVO has a configuration file which sets general parameters of the application. This file is read in the first lines
of the bootstrap file (public/index.php):

.. code-block:: php

    <?php

    //Read the configuration
    $config = new Phalcon\Config\Adapter\Ini(__DIR__.'/../app/config/config.ini');

:doc:`Phalcon\\Config <config>` 组件使我们能以面向对象的方式操作配置文件。本项目的配置文件内容如下：
    
:doc:`Phalcon\\Config <config>` allows us to manipulate the file in an object oriented way. The configuration file contains the following
settings:

.. code-block:: ini

    [database]
    host     = localhost
    username = root
    password = secret
    name     = invo

    [application]
    controllersDir = /../app/controllers/
    modelsDir      = /../app/models/
    viewsDir       = /../app/views/
    pluginsDir     = /../app/plugins/
    libraryDir     = /../app/library/
    baseUri        = /invo/

    ;[metadata]
    ;adapter = "Apc"
    ;suffix = my-suffix
    ;lifetime = 3600


Phalcon没有定义任何的默认配置。配置节能有效地帮助我们组织各种配置选项。请看上面的配置文件，该文件共有三个配置节。
    
Phalcon has no defined any convention settings. Sections help us organize the options as appropriate. In this file there are three sections to use later.

自动加载器
==========
引导程序（public/index.php）的第二部分是类-自动加载器。自动加载器注册了一系列目录，应用程序最终会在这些目录中查找类。

A second part that appears in the boostrap file (public/index.php) is the autoloader. The autoloader registers a set of directories where the application will look for the classes that it eventually will need.

.. code-block:: php

    <?php

    $loader = new \Phalcon\Loader();

    $loader->registerDirs(
        array(
            __DIR__.$config->application->controllersDir,
            __DIR__.$config->application->pluginsDir,
            __DIR__.$config->application->libraryDir,
            __DIR__.$config->application->modelsDir,
        )
    )->register();

注意，这里注册的目录是在配置文件中事先定义好的。viewDir（视图目录）是唯一在配置文件中定义了，但没有被注册的目录。因为该目录中没有类文件，只有html+php普通文件。
    
Note that what has been done is to register the directories that were in the configuration file. The only directory that is not registered is the viewsDir, because it contains no classes but html + php files.

处理请求
========
让我们来看看引导程序的最后一部分，用户请求最终被Phalcon\\Mvc\\Application处理，这个类会初始化和执行应用程序运行所需的各种环境。

Let's go much further, at the end of the file, the request is finally handled by Phalcon\\Mvc\\Application, this class initializes and executes all the necesary to make the application run:

.. code-block:: php

    <?php

    $application = new \Phalcon\Mvc\Application();
    $application->setDI($di);
    echo $application->handle()->getContent();

依赖注入
========
注意看上面这段代码的第二行，$application 变量接受另一个变量 $di 作为参数。那么，变量 $di 扮演着怎样的角色呢？Phalcon是一个高解耦的框架，所以我们需要一个组件把所有东西“粘”在一起，并让它们一起工作。这个组件就是Phalcon\\DI。它既是一个服务容器，也是一个依赖注入容器，实例化应用程序需要的所有组件。

Look at the second line of the code block above, the variable $application is receiving another variable $di. What is the purpose of that variable? Phalcon is a highly decoupled framework, so we need a component that act as glue to make everything work together. That component is Phalcon\\DI. It is a service container that also performs dependency injection, instantiating all components as they are needed by the application.

在容器中注册服务有好几种方法。在INVO项目中，我们使用匿名函数的方式注册服务。幸亏如此，对象都能够延迟实例化，最终能减少应用对资源的消耗。

There are many ways of registering in the container services. In INVO most services have been registered using anonymous functions. Thanks to this the objects are instantiated in a lazy way, reducing the resources needed by the application.

例如，下面的代码注册了会话服务，当应用需要访问会话数据时，匿名函数才会被调用。

For instance, in the following excerpt is registered the session service, the anonymous function will only be called when the application requires access to the session data:

.. code-block:: php

    <?php

    //Start the session the first time when some component request the session service
    $di->set('session', function(){
        $session = new Phalcon\Session\Adapter\Files();
        $session->start();
        return $session;
    });

这里我们可以根据需要选择合适的会话处理器，并做相应的初始化工作。在这里，我们使用"session"注册会话服务。这是一个约定，这将允许框架分辨出服务容器中的活跃服务。
    
Here we have the freedom to change the adapter, perform additional initialization and much more. Note that the service was registered using the name "session". This is a convention that will allow the framework to identify the active service in the service container.

处理一个请求可能会用到很多服务，一个一个注册它们是一件很繁重的工作。鉴于此，Phalcon框架提供了Phalcon\\DI的一个变种，Phalcon\\DI\\FactoryDefault。

A request can use many services, register each service one to one can be a cumbersome task. For this reason, the framework provides a variant of Phalcon\\DI called Phalcon\\DI\\FactoryDefault.

.. code-block:: php

    <?php

    // The FactoryDefault Dependency Injector automatically registers the
    // right services providing a full stack framework
    $di = new \Phalcon\DI\FactoryDefault();

它会注册框架组件提供的主要服务。如果我们想要重新定义某些服务，我们可以像上面定义"session"一样。OK，现在我们应该了解变量$di的来龙去脉了。
    
It registers the majority of services with components provided by the framework as standard. If we need to override the definition of some it could be done as above with "session". Now we know the origin of the variable $di.

登录应用
========
登录后我们便可使用后端控制器了。我们是使用逻辑来区分前端控制器和后端控制器的，所有控制器都位于同一目录下。要进入系统，我们必须要有合法的用户名和密码。用户信息存储在"invo"数据库的"users"表中。

Log in will allow us to work on backend controllers. The separation between the controllers of the backend and frontend is only logical. All controllers are located in the same directory. To enter the system, we must have a valid username and password. The users are stored in the table "users" of the database "invo".

在我们登录系统之前，我们需要配置应用的数据库连接，因此我们在服务容器中注册了一个名为"db"的服务。与之前介绍的类-自动加载器注册目录一样，这次我们也是从配置文件中为服务读取选项：

Before we can log in, we need to configure the connection to the database in the application. A service called "db" will be applied to the service container for this information. As with the autoloader, this time we are also taking parameters from the configuration file to configure a service:

.. code-block:: php

    <?php

    // Database connection is created based in the parameters defined in the configuration file
    $di->set('db', function() use ($config) {
        return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
            "host" => $config->database->host,
            "username" => $config->database->username,
            "password" => $config->database->password,
            "dbname" => $config->database->name
        ));
    });

这里，我们返回一个MySQL连接适配器。如果有需要，你可以在这里做额外的操作（如添加日志和分析等），或者换其他数据库适配器，总之，你想怎么样设置都可以。
    
Here we return an instance of the MySQL connection adapter. If needed, you could do extra actions such as adding a logger, a profiler or change the adapter, or setup it as you want.

下面这张简单的表单（app/views/session/index.phtml）会向服务器请求登陆信息。出于简洁性考虑，此处我们省略了一些HTML代码。

Back then, the following simple form (app/views/session/index.phtml) requests the logon information. We've removed some HTML code to make the example more concise:

.. code-block:: html+php

    <?php echo Tag::form('session/start') ?>

        <label for="email">Username/Email</label>
        <?php echo Tag::textField(array("email", "size" => "30")) ?>

        <label for="password">Password</label>
        <?php echo Tag::passwordField(array("password", "size" => "30")) ?>

        <?php echo Tag::submitButton(array('Login')) ?>

    </form>

SessionController::startAction（app/controllers/SessionController.pthml）负责验证用户输入的信息是否在数据库中存在。
    
The SessionController::startAction (app/controllers/SessionController.phtml) have the task of validate the entered data checking for a valid user in the database:

.. code-block:: php

    <?php

    class SessionController extends ControllerBase
    {

        // ...

        private function _registerSession($user)
        {
            $this->session->set('auth', array(
                'id' => $user->id,
                'name' => $user->name
            ));
        }

        public function startAction()
        {
            if ($this->request->isPost()) {

                //Taking the variables sent by POST
                $email = $this->request->getPost('email', 'email');
                $password = $this->request->getPost('password');

                $password = sha1($password);

                //Find for the user in the database
                $user = Users::findFirst("email='$email' AND password='$password' AND active='Y'");
                if ($user != false) {

                    $this->_registerSession($user);

                    $this->flash->success('Welcome '.$user->name);

                    //Forward to the invoices controller if the user is valid
                    return $this->dispatcher->forward(array(
                        'controller' => 'invoices',
                        'action' => 'index'
                    ));
                }

                $this->flash->error('Wrong email/password');
            }

            //Forward to the login form again
            return $this->dispatcher->forward(array(
                'controller' => 'session',
                'action' => 'index'
            ));

        }

    }

注意，在控制器中我们使用了很多公共属性，如：$this->flash, $this->request和$this->session。
这些是之前在依赖注入容器里面定义的服务。当你第一次访问它们时，它们会被注入到控制器中，成为控制器的属性。
    
Note that multiple public attributes are accessed in the controller like: $this->flash, $this->request or $this->session.
These are services defined in dependency injector from earlier. When accessed the first time, they are injected as part of the controller.

这些服务是共享使用的，无论你在什么地方调用它们，你访问的都是同一个实例。

These services are shared, which means that we will always be accessing the same instance regardless of the place where we invoke them.

例如，我们调用"session"服务，然后把用户ID存储到"auth"变量中：

For instance, here we invoke the "session" service and then we store the user identity in the "auth" variable:

.. code-block:: php

    <?php

    $this->session->set('auth', array(
        'id' => $user->id,
        'name' => $user->name
    ));

后院不能起火
============
后端是私人领域，只有注册用户才能访问。因此有必要验证只有登陆用户才有访问这些控制器的权限。如果你没登陆应用，还想访问产品控制器（私有），你将会碰到如下场景：

The backend is a private area where only registered users have access. Therefore it is necessary to check that only registered users have access to these controllers. If you aren't logged in the application and you try to access by example the products controller (that is private) you'll see a screen like this:

.. figure:: ../_static/img/invo-2.png
   :align: center

每次当有人尝试访问控制器和动作时，应用都会验证该用户所属角色确有权限访问，否则就会显示上面图中的消息并跳转至首页。
   
Every time someone try to access any controller and action, the application verifies that the current role has access to it, otherwise it displays a message like the above and forwards the flow to the home page.

现在，让我们来挖掘一下应用是如何完成权限功能的。首先要要知道的是，有一种组件叫调度器。当路由器组件分析出路由信息后便会通知调度器，然后由调度器负责加载相应的控制器，并执行相应的动作方法。

Now let's find out how the application accomplishes this. The first thing to know is that there is a component called Dispatcher. It is informed about the route found by the component Router. Based on this is responsible for loading the appropriate controller and execute the corresponding action method.

一般来说，调度器是框架自动创建的。在本教程中，在执行请求的动作之前，我们要检验用户是否有权限访问该动作。为了达到这个目的，我们定义了一个匿名函数来替换框架的默认调度器：

Normally, the Dispatcher is created automatically by the framework. In our case, we want to make a special action that is check before executing the required action if the user has access to it or not. To achieve this we replace the component by creating a function defined by us in the bootstrap:

.. code-block:: php

    <?php

    $di->set('dispatcher', function() use ($di) {
        $dispatcher = new Phalcon\Mvc\Dispatcher();
        return $dispatcher;
    });

我们现在可以完全控制此应用的调度器了。框架的许多组件都通过事件允许我们修改它们的内部操作流程。由于依赖注入组件扮演着“胶水”的角色，一个名为事件管理的组件可以帮助我们把某些组件产生的消息传递给其他对象。
    
We now have total control of the Dispatcher used by the application. Now, many components of the framework launch events that allow us to modify the internal flow of operation. As the dependency Injector component acts as glue for components, a new component called EventsManager helps us to bring the events produced by some component to the objects that require them.

事件管理
-----------------
一个事件管理器可以让我们监听一个特定类型的事件。现在我们感兴趣的是“调度”类型，它能筛选出所有由调度器产生的事件：

A EventsManager allows us to attach listeners to a particular type of event. The type that interests us now is "dispatch" that filters all events produced by the Dispatcher:

.. code-block:: php

    <?php

    $di->set('dispatcher', function() use ($di) {

        //Obtain the standard eventsManager from the DI
        $eventsManager = $di->getShared('eventsManager');

        //Instantiate the Security plugin
        $security = new Security($di);

        //Listen for events produced in the dispatcher using the Security plugin
        $eventsManager->attach('dispatch', $security);

        $dispatcher = new Phalcon\Mvc\Dispatcher();

        //Bind the EventsManager to the Dispatcher
        $dispatcher->setEventsManager($eventsManager);

        return $dispatcher;
    });

安全插件是一个位于（app/plugins/Security.php）的类文件。这个类实现了"beforeExecuteRoute"方法，这恰好是调度器产生的某个事件的名称：
    
The Security plugin is a class located at (app/plugins/Security.php). This class implements the method "beforeExecuteRoute". This is the same
name as one of the events produced in the Dispatcher:

.. code-block:: php

    <?php

    class Security extends Phalcon\Mvc\User\Plugin
    {

        // ...

        public function beforeExecuteRoute(Phalcon\Events\Event $event, Phalcon\Mvc\Dispatcher $dispatcher)
        {
            // ...
        }

    }

钩子事件的方法总是接收两个参数：第一个参数包含事件产生的上下文信息；第二个参数是产生事件的对象。插件不应该继承自 Phalcon\Mvc\User\Plugin 类，但是这样能让我们更容易的访问到应用中的其他服务。
    
The hooks events always receive a first paramter that contains contextual information of the event produced and a second that is the
object that produced the event itself. Plugins should not extend the class Phalcon\Mvc\User\Plugin, but by doing it they gain easier access to the services of the application.

现在我们在当前会话中验证用户角色，使用ACL检验他是否有访问权限。如果没有权限，我们将按约定把他带到首页：

Now, we're verifying the role in the current session, check to see if he has access using the ACL list. If he does not have access we redirect him to the home screen as explained:

.. code-block:: php

    <?php

    class Security extends Phalcon\Mvc\User\Plugin
    {

        // ...

        public function beforeExecuteRoute(Phalcon\Events\Event $event, Phalcon\Mvc\Dispatcher $dispatcher)
        {

            //Check whether the "auth" variable exists in session to define the active role
            $auth = $this->session->get('auth');
            if (!$auth) {
                $role = 'Guests';
            } else {
                $role = 'Users';
            }

            //Take the active controller/action from the dispatcher
            $controller = $dispatcher->getControllerName();
            $action = $dispatcher->getActionName();

            //Obtain the ACL list
            $acl = $this->_getAcl();

            //Check if the Role have access to the controller (resource)
            $allowed = $acl->isAllowed($role, $controller, $action);
            if ($allowed != Phalcon\Acl::ALLOW) {

                //If he doesn't have access forward him to the index controller
                $this->flash->error("You don't have access to this module");
                $dispatcher->forward(
                    array(
                        'controller' => 'index',
                        'action' => 'index'
                    )
                );

                //Returning "false" we tell to the dispatcher to stop the current operation
                return false;
            }

        }

    }

制定访问控制列表
---------------------
在前面的示例中，我们使用 $this->_getAcl() 方法获取访问控制列表。这个方法在插件中也实现了。现在我们一步一步介绍如何制定访问控制列表：

In the previous example we obtain the ACL using the method $this->_getAcl(). This method is also implemented in the Plugin.
Now explain step by step how we built the access control list:

.. code-block:: php

    <?php

    //Create the ACL
    $acl = new Phalcon\Acl\Adapter\Memory();

    //The default action is DENY access
    $acl->setDefaultAction(Phalcon\Acl::DENY);

    //Register two roles, Users is registered users
    //and guests are users without a defined identity
    $roles = array(
        'users' => new Phalcon\Acl\Role('Users'),
        'guests' => new Phalcon\Acl\Role('Guests')
    );
    foreach($roles as $role){
        $acl->addRole($role);
    }

现在我们为每个部分定义各自的资源。控制器名称是资源名，它们的动作属于资源的访问入口。
    
Now we define the respective resources of each area. Controller names are resources and their actions are the accesses in
the resources:

.. code-block:: php

    <?php

    //Private area resources (backend)
    $privateResources = array(
        'companies' => array('index', 'search', 'new', 'edit', 'save', 'create', 'delete'),
        'products' => array('index', 'search', 'new', 'edit', 'save', 'create', 'delete'),
        'producttypes' => array('index', 'search', 'new', 'edit', 'save', 'create', 'delete'),
        'invoices' => array('index', 'profile')
    );
    foreach($privateResources as $resource => $actions){
        $acl->addResource(new Phalcon\Acl\Resource($resource), $actions);
    }

    //Public area resources (frontend)
    $publicResources = array(
        'index' => array('index'),
        'about' => array('index'),
        'session' => array('index', 'register', 'start', 'end'),
        'contact' => array('index', 'send')
    );
    foreach($publicResources as $resource => $actions){
        $acl->addResource(new Phalcon\Acl\Resource($resource), $actions);
    }

现在ACL知道了应用中所有的控制器和它们的动作。"Users"角色既能访问前端资源又能访问后端资源，而"Guests"角色只能访问公共资源：
    
The ACL now have knowledge of the existing controllers and their related actions. The role "Users" have access to all the resources of both the frontend and the backend. The role "Guests" only have access to the public area:

.. code-block:: php

    <?php

    //Grant access to public areas to both users and guests
    foreach ($roles as $role) {
        foreach ($publicResources as $resource => $actions) {
            $acl->allow($role->getName(), $resource, '*');
        }
    }

    //Grant access to private area only to role Users
    foreach ($privateResources as $resource => $actions) {
        foreach ($actions as $action) {
            $acl->allow('Users', $resource, $action);
        }
    }

万岁！访问控制列表现在完成了。
    
Hooray!, the ACL is now complete.

用户组件
===============
应用中的所有UI元素和样式风格大部分使用Twitter Boostrap完成。有些元素，如导航条随着应用状态的不同而改变。例如：当用户登陆后，右上角区域的“登陆/注册”链接会变成“注销”链接。

All the UI elements and visual style of the application has been achieved mostly through Twitter Boostrap. Some elements, such as the navigation bar change according to the state of the application. For example, in the upper right corner, the link "Log in / Sign Up" changes to "Log out" if a user is logged into the application.

应用的这部分是在"Element"组件（app/library/Elements.php）中实现的。

This part of the application is implemented in the component "Elements" (app/library/Elements.php).

.. code-block:: php

    <?php

    class Elements extends Phalcon\Mvc\User\Component
    {

        public function getMenu()
        {
            //...
        }

        public function getTabs()
        {
            //...
        }

    }

该类继承自 Phalcon\Mvc\User\Component，然而此处继承不是强制的，但是这样做会让我们更容易地访问应用中的其他服务。现在，我们在依赖注入容器中注册这个类：
    
This class extends the Phalcon\Mvc\User\Component, it is not imposed to extend a component with this class, but if it helps to more quickly access the application services. Now, we register this class in the Dependency Injector Container:

.. code-block:: php

    <?php

    //Register an user component
    $di->set('elements', function(){
        return new Elements();
    });

和控制器、插件或组件一样，在视图中你可以使用服务名作为属性访问在容器中注册过的服务：
    
As controllers, plugins or components within a view also can access the services registered in the container just accessing an attribute by name:

.. code-block:: html+php

    <div class="navbar navbar-fixed-top">
        <div class="navbar-inner">
            <div class="container">
                <a class="btn btn-navbar" data-toggle="collapse" data-target=".nav-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </a>
                <a class="brand" href="#">INVO</a>
                <?php echo $this->elements->getMenu() ?>
            </div>
        </div>
    </div>

    <div class="container">
        <?php echo $this->getContent() ?>
        <hr>
        <footer>
            <p>&copy; Company 2012</p>
        </footer>
    </div>

上面这段代码中，我们重点关注的是:
    
The important part is:

.. code-block:: html+php

    <?php echo $this->elements->getMenu() ?>

使用CRUD
=====================
大部分数据操作（公司信息、产品以及产品类型），基于基础、通用的 CRUD_ （创建、读取、更新和删除）开发。完整的CRUD包含以下文件：

Most options that manipulate data (companies, products and types of products), were developed using a basic and common CRUD_ (Create, Read, Update and Delete). Each CRUD contains the following files:

.. code-block:: bash

    invo/
        app/
            app/controllers/
                ProductsController.php
            app/models/
                Products.php
            app/views/
                products/
                    edit.phtml
                    index.phtml
                    new.phtml
                    search.phtml

每个控制器都包含以下动作：                    
                    
Each controller have the following actions:

.. code-block:: php

    <?php

    class ProductsController extends ControllerBase
    {

        /**
         * The start action, it shows the "search" view
         */
        public function indexAction()
        {
            //...
        }

        /**
         * Execute the "search" based on the criteria sent from the "index"
         * Returning a paginator for the results
         */
        public function searchAction()
        {
            //...
        }

        /**
         * Shows the view to create a "new" product
         */
        public function newAction()
        {
            //...
        }

        /**
         * Shows the view to "edit" an existing product
         */
        public function editAction()
        {
            //...
        }

        /**
         * Creates a product based on the data entered in the "new" action
         */
        public function createAction()
        {
            //...
        }

        /**
         * Updates a product based on the data entered in the "edit" action
         */
        public function saveAction()
        {
            //...
        }

        /**
         * Deletes an existing product
         */
        public function deleteAction($id)
        {
            //...
        }

    }

搜索表单
---------------
Every CRUD starts with a search form. This form shows each field that has the table (products), allowing the user to create a search criteria from any field.
The "products" table has a relationship to the table "products_types". In this case we previously query the records in this table in order to facilitate the search by that field:

.. code-block:: php

    <?php

    /**
     * The start action, it shows the "search" view
     */
    public function indexAction()
    {
        $this->persistent->searchParams = null;
        $this->view->setVar("productTypes", ProductTypes::find());
    }

All the "product types" are queried and passed to the view as a local variable "productTypes". Then in the view (app/views/index.phtml) we show a "select" tag
filled with those results:

.. code-block:: php

    <?php

    <div>
        <label for="product_types_id">Product Type</label>
        <?php echo Tag::select(array("product_types_id", $productTypes, "using" => array("id", "name"), "useDummy" => true)) ?>
    </div>

Note that the $productTypes contains the data neccesary to fill the SELECT tag with Phalcon\\Tag::select. Once the form is submitted, it will
execute the action "search" in the controller who will perform the search based on the data entered by the user.

Performing a Search
-------------------
The action "search" has a dual behavior. When accessed via POST, it performs a search based on the data sent from the form.
But when accessed via GET it moves the current page in the paginator. To differentiate one from the other HTTP method,
we check it using the :doc:`Request <request>` component:

.. code-block:: php

    <?php

    /**
     * Execute the "search" based on the criteria sent from the "index"
     * Returning a paginator for the results
     */
    public function searchAction()
    {

        if ($this->request->isPost()) {
            //create the query conditions
        } else {
            //paginate using the existing conditions
        }

        //...

    }

With the help of :doc:`Phalcon\\Mvc\\Model\\Criteria <../api/Phalcon_Mvc_Model_Criteria>`, we can create the search conditions
intelligently based on the data types and values sent from the form:

.. code-block:: php

    <?php

    $query = Criteria::fromInput($this->di, "Products", $_POST);

This method verifies which values are different from "" (empty string) and null and takes them into account to create the query:
If the data type of a field is text or similar (char, varchar, text, etc.) it will use a "like" operator to filter the results.
If the data type is not text or similar, it'll use the operator "=".

Additionally, "Criteria" ignores all the $_POST variables that do not match any field in the table. Also, values ​​are automatically escaped
using "bound parameters".

Now, we store the produced params in the controller's session bag:

.. code-block:: php

    <?php

    $this->persistent->searchParams = $query->getParams();

A session bag, is a special attribute of a controller that persists between requests. When accesed, this attribute injects
a :doc:`Phalcon\\Session\\Bag <../api/Phalcon_Session_Bag>` service, that's independent in each controller.

Then, based on the built params we perform the query:

.. code-block:: php

    <?php

    $products = Products::find($parameters);
    if (count($products) == 0) {
        $this->flash->notice("The search did not found any products");
        return $this->forward("products/index");
    }

If the search doesn't return any product, we forward the user to the index action again. Let's pretend the
search returned results, then we create a paginator to navigate easily through them:

.. code-block:: php

    <?php

    $paginator = new Phalcon\Paginator\Adapter\Model(array(
        "data" => $products,    //Data to paginate
        "limit" => 5,           //Rows per page
        "page" => $numberPage   //Active page
    ));

    //Get active page in the paginator
    $page = $paginator->getPaginate();

Finally we pass the returned page to view:

.. code-block:: php

    <?php

    $this->view->setVar("page", $page);

In the view (app/views/products/search.phtml), we traverse the results corresponding to the current page:

.. code-block:: html+php

    <?php foreach($page->items as $product){ ?>
        <tr>
            <td><?= $product->id ?></td>
            <td><?= $product->getProductTypes()->name ?></td>
            <td><?= $product->name ?></td>
            <td><?= $product->price ?></td>
            <td><?= $product->active ?></td>
            <td><?= Tag::linkTo("products/edit/".$product->id, 'Edit') ?></td>
            <td><?= Tag::linkTo("products/delete/".$product->id, 'Delete') ?></td>
        </tr>
    <?php } ?>

Creating and Updating Records
-----------------------------
Now let's see how the CRUD creates and updates records. From the "new" and "edit" views the data entered by the user
are sent to the actions "create" and "save" that perform actions of "create" and "update" products respectively.

In the creation case, we recover the data sent and assign them to a new "products" instance:

.. code-block:: php

    <?php

    /**
     * Creates a product based on the data entered in the "new" action
     */
    public function createAction()
    {

        $products = new Products();
        $products->id = $request->getPost("id", "int");
        $products->product_types_id = $request->getPost("product_types_id", "int");
        $products->name = $request->getPost("name", "striptags");
        $products->price = $request->getPost("price", "double");
        $products->active = $request->getPost("active");

        //...

    }

Data is filtered before being assigned to the object. When saving we'll know whether the data conforms to the business rules
and validations implemented in the model Products:

.. code-block:: php

    <?php

    /**
     * Creates a product based on the data entered in the "new" action
     */
    public function createAction()
    {

        //...

        if (!$products->save()) {

            //The store failed, the following messages were produced
            foreach ($products->getMessages() as $message) {
                $this->flash->error((string) $message);
            }
            return $this->forward("products/new");

        } else {
            $this->flash->success("Product was created successfully");
            return $this->forward("products/index");
        }

    }

Now in the case of product updating, first we must present to the user the data currently in the edited record:

.. code-block:: php

    <?php

    /**
     * Shows the view to "edit" an existing product
     */
    public function editAction($id)
    {

        //...

        $product = Products::findFirst("id = '$id'");

        Tag::displayTo("id", $product->id);
        Tag::displayTo("product_types_id", $product->product_types_id);
        Tag::displayTo("name", $product->name);
        Tag::displayTo("price", $product->price);
        Tag::displayTo("active", $product->active);

    }

The Tag::displayTo helper sets a default value in the form on the attribute with the same name. Thanks to this, the user can change any value and then
sent it back to the database through to the "save" action:

.. code-block:: php

    <?php

    /**
     * Updates a product based on the data entered in the "edit" action
     */
    public function saveAction()
    {

        //...

        //Find the product to update
        $id = $request->getPost("id", "int");
        $products = Products::findFirst("id='$id'");
        if ($products == false) {
            $this->flash->error("products does not exist ".$id);
            return $this->forward("products/index");
        }

        //... assign the values to the object and store it

    }

Changing the Title Dynamically
==============================
When you browse between one option and another will see that the title changes dynamically indicating where we are currently working.
This is achieved in each controller initializer:

.. code-block:: php

    <?php

    class ProductsController extends ControllerBase
    {

        public function initialize()
        {
            //Set the document title
            Tag::setTitle('Manage your product types');
            parent::initialize();
        }

        //...

    }

Note, that the method parent::initialize() is also called, it adds more data to the title:

.. code-block:: php

    <?php

    class ControllerBase extends Phalcon\Mvc\Controller
    {

        protected function initialize()
        {
            //Prepend the application name to the title
            Phalcon\Tag::prependTitle('INVO | ');
        }

        //...
    }

Finally, the title is printed in the main view (app/views/index.phtml):

.. code-block:: html+php

    <?php use Phalcon\Tag as Tag ?>
    <!DOCTYPE html>
    <html>
        <head>
            <?php echo Tag::getTitle() ?>
        </head>
        <!-- ... -->
    </html>

Conclusion
==========
This tutorial covers many more aspects of building applications with Phalcon, hope you have served to learn more and get more out of the framework.

.. _Github: https://github.com/phalcon/invo
.. _CRUD: http://en.wikipedia.org/wiki/Create,_read,_update_and_delete
