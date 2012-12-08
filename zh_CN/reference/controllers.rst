控制器
=================
控制器包含很多方法，通常被称为动作。动作是控制中用于处理请求的方法。默认情况下，控制器中的每一个公共方法都对应一个动作，且可以通过URL访问它。动作负责解析请求并做出响应。通常来讲，响应是渲染后的视图，但也有可能是其他形式。

The controllers provide a number of methods that are called actions. Actions are methods on a controller that handle requests. By default all public methods on a controller map to actions and are accessible by a URL. Actions are responsible for interpreting the request and creating the response. Usually responses are in the form of a rendered view, but there are other ways to create responses as well.

例如，你访问URL: http://localhost/blog/posts/show/2012/the-post-title 。Phalcon默认会把该URL分解为以下几部分：

For instance, when you access a URL like this: http://localhost/blog/posts/show/2012/the-post-title Phalcon by default will decompose each part like this:

+------------------------+----------------+
| **网站目录**           | blog           |
+------------------------+----------------+
| **控制器**             | posts          |
+------------------------+----------------+
| **动作**               | show           |
+------------------------+----------------+
| **参数**               | 2012           |
+------------------------+----------------+
| **参数**               | the-post-title |
+------------------------+----------------+

在本例中，PostsController 将会处理该请求。Phalcon不会要求你把控制器放在应用的某个特殊目录下，它们能够被 :doc:`类自动加载器 <loader>` 加载，所以，你几乎可以随意放置你的控制器。

In this case, the PostsController will handle this request. There is no a special location to put controllers in an application, they could be loaded using :doc:`autoloaders <loader>`, so you're free to organize your controllers as you need.

控制器名必须以"Controller"结尾，而动作必须以"Action"结尾。下面有一个控制器的示例：

Controllers must have the suffix "Controller" while actions the suffix "Action". A sample of a controller is as follows:

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function showAction($year, $postTitle)
        {

        }

    }

URI中的其他参数会被定义为动作的参数，你可以轻易地使用本地变量访问他们。控制器可以继承 :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` 类，这不是必须的。但如果你这么做了，就可以在控制器中很容易地访问应用服务。
    
Additional URI parameters are defined as action parameters, so that they can be easily accessed using local variables. A controller can optionally extend :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>`. By doing this, the controller can have easy access to the application services.

派遣环
-------------
派遣环会一直在派遣器中执行，直到没有任何动作需要执行为止。以上示例中只有一个动作被执行。现在，我们将使用"forward"在派遣环中完成更复杂的操作流程，把执行操作转发给另一个控制器/动作。

The dispatch loop will be executed within the Dispatcher until there are no actions left to be executed. In the previous example only one action was executed. Now we'll see how "forward" can provide a more complex flow of operation in the dispatch loop, by forwarding execution to a different controller/action.

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function showAction($year, $postTitle)
        {
            $this->flash->error("You don't have permission to access this area");

            // Forward flow to another action
            $this->dispatcher->forward(array("controllers" => "users", "action" => "signin"));
        }

    }

如果用户没有权限访问某个动作，请求将会被转发给用户控制器的登录动作。
    
If users don't have permissions to access a certain action then will be forwarded to the Users controller, signin action.

.. code-block:: php

    <?php

    class UsersController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function signinAction()
        {

        }

    }

这种"forwards"（译者注：转发）是没有限制的，但不能导致循环引用，否则你的应用程序将会在这里停止。如果派遣环中没有其他动作需要派遣的话，派遣器将会自动调用MVC的视图层（ :doc:`Phalcon\\Mvc\\View <../api/Phalcon_Mvc_View>` ）。
    
There is no limit on the "forwards" you can have in your application, so long as they do not result in circular references, at which point your application will halt. If there are no other actions to be dispatched by the dispatch loop, the dispatcher will automatically invoke the view layer of the MVC which is managed by :doc:`Phalcon\\Mvc\\View <../api/Phalcon_Mvc_View>`.

初始化控制器
------------------------
:doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` 提供了初始化方法，它会在控制器的任何动作执行之前执行。值得注意的是，我们不建议大家在"__construct"方法（译者注：构造器）中执行控制器初始化。

:doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` offers the initialize method, which is executed first, before any action is executed on a controller. The use of the "__construct" method is not recommended.

.. code-block:: php

    <?php

    class PostsController extends \Phalcon\Mvc\Controller
    {

        public $settings;

        public function initialize()
        {
            $this->settings = array(
                "mySetting" => "value"
            );
        }

        public function saveAction()
        {
            if ($this->settings["mySetting"] == "value") {
                //...
            }
        }

    }

注入服务
------------------
如果控制器继承自 :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` 类，它就能轻易地访问应用的服务容器。例如，如果我们注册了一个这样的服务：

If a controller extends :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` then it have easy access to the service container in application. For example, if we have registered a service like this:

.. code-block:: php

    <?php

    $di = new Phalcon\DI();

    $di->set('storage', function(){
        return new Storage('/some/directory');
    });

那么，我们可以以这种方式访问该服务：
    
Then, we can access to that service by this way:

.. code-block:: php

    <?php

    class FilesController extends \Phalcon\Mvc\Controller
    {

        public function saveAction()
        {

            //Injecting the service by just accessing the property with the same name
            $this->storage->save('/some/file');

            //Accessing the service from the DI
            $this->di->get('storage')->save('/some/file');

            //Another way to access the service
            $this->di->getStorage()->save('/some/file');
        }

    }

如果你打算使用Phalcon的所有功能，你可以继续阅读框架提供的 :doc:`默认 <di>` 。
    
If you're using Phalcon as a full-stack framework, you can read the services provided :doc:`by default <di>` in the framework.

请求和响应
--------------------
假设框架提供了下面提到的所有注册服务。我们将阐释控制器如何与HTTP环境交互。"request"服务包含 :doc:`Phalcon\\Http\\Request <../api/Phalcon_Http_Request>` 类的一个实例，"response"服务包含 :doc:`Phalcon\\Http\\Response <../api/Phalcon_Http_Response>` 类的一个实例，代表将被发送到客户端的内容。

Assuming that the registered services are provided by the framework. We explain how to interact with the HTTP environment. The "request" service contains an instance of :doc:`Phalcon\\Http\\Request <../api/Phalcon_Http_Request>` and the "response" contains a :doc:`Phalcon\\Http\\Response <../api/Phalcon_Http_Response>` representing what is going to be sent back to the client.

.. code-block:: php

    <?php

    class PostsController extends Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function saveAction()
        {

            // Check if request has made with POST
            if ($this->request->isPost() == true) {
                // Access POST data
                $customerName = $this->request->getPost("name");
                $customerBorn = $this->request->getPost("born");
            }
        }

    }

我们一般不会直接使用响应对象，但它在动作执行之前就已经构建好了，有时候 - 如在派遣事件之后 - 直接访问响应对象是很有用的：
    
The response object is not usually used directly, but is built up before the execution of the action, sometimes - like in an afterDispatch event - it can be useful to access the response directly:

.. code-block:: php

    <?php

    class PostsController extends Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function notFoundAction()
        {
            // Send a HTTP 404 response header
            $this->response->setStatusCode(404, "Not Found");
        }

    }

要了解更多关于HTTP环境的细节，请猛击链接 `请求 <request.html>`_ 和 `响应 <response.html>`_ 。
    
Learn more about the HTTP environment in their dedicated articles `request <request.html>`_ and `response <response.html>`_.

会话数据
------------
会话能帮助我们在请求之间保持持久性数据。你可以从任何控制器访问 :doc:`Phalcon\\Session\\Bag <../api/Phalcon_Session_Bag>` 类，保存需要持久的数据。

Sessions help us maintain persistent data between requests. You could access a :doc:`Phalcon\\Session\\Bag <../api/Phalcon_Session_Bag>` from any controller to encapsulate data that need to be persistent.

.. code-block:: php

    <?php

    class UserController extends Phalcon\Mvc\Controller
    {

        public function indexAction()
        {
            $this->persistent->name = "Michael";
        }

        public function welcomeAction()
        {
            echo "Welcome, ", $this->persistent->name;
        }

    }

控制器服务
-----------------------------
有些服务可能就是控制器，我们可以始终通过服务容器来获取控制器类。从而让我们能以相同的名称注册其他类，以替代控制器类：

Services may act as controllers, controllers classes are always requested from the services container. Accordingly, a controller can be easily replaced by any other class registered with its name:

.. code-block:: php

    <?php

    //Register a controller as a service
    $di->set('IndexController', function() {
        $component = new Component();
        return $component;
    });

创建控制器基类
--------------------------
应用程序的某些功能，如访问控制列表、语言本地化、缓存和模板引擎，在许多控制器中通常是相同的。这种情况下，我们鼓厉你创建“控制器基类”，以确保你的代码 DRY_ (译者注：Don't Repeat Yourself)。控制器基类是一个继承自 :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` 类和封装所有控制器必须的通用功能的简单类。现在，你的控制器只需继承“控制器基类”就可以访问通用功能。

Some application features like access control lists, translation, cache, and template engines are often common to many controllers. In cases like these the creation of a "base controller" is encouraged to ensure your code stays DRY_. A base controller is simply a class that extends the :doc:`Phalcon\\Mvc\\Controller <../api/Phalcon_Mvc_Controller>` and encapsulates the common functionality that all controllers must have. In turn, your controllers extend the "base controller" and have access to the common functionality.

控制器基类可以放置在任何地方，但由于组织公约，我们建议你把它放在控制器目录下，如：apps/controllers/ControllerBase.php。我们也许会直接在引导程序中包含这个文件，又或者使用类自动加载器加载它。

This class could be located anywhere, but for organizational conventions we recommend it to be in the controllers folder, e.g. apps/controllers/ControllerBase.php. We may require this file directly in the bootstrap file or cause to be loaded using any autoloader:

.. code-block:: php

    <?php

    require "../app/controllers/ControllerBase.php";

控制器基类实现了一些通用组件（动作，方法，属性等）：
    
The implementation of common components (actions, methods, properties etc.) resides in this file:

.. code-block:: php

    <?php

    class ControllerBase extends \Phalcon\Mvc\Controller
    {

      /**
       * This action is available for multiple controllers
       */
      public function someAction()
      {

      }

    }

现在，所有其他控制器都继承自ControllerBase，自动获得通用组件（上面讨论的）：
    
Any other controller now inherits from ControllerBase, automatically gaining access to the common components (discussed above):

.. code-block:: php

    <?php

    class UsersController extends ControllerBase
    {

    }

.. _DRY: http://en.wikipedia.org/wiki/Don't_repeat_yourself
