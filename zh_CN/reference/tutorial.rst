教程一：从实例开始学习
==================================

在本教程中，我们将通过一个简单的注册表单案例来引导您使用Phalcon创建应用。我们稍后也会阐述框架的基本行为。如果你对Phalcon的自动代码生成工具感兴趣，你可以查看 `开发工具`_ 。

Throughout this first tutorial, we'll walk you through the creation of an application with a simple registration form from the ground up. We will also explain the basic aspects of the framework's behavior. If you are interested in automatic code generation tools for Phalcon, you can check our `developer tools`_.

检查Phalcon是否安装
--------------------------

我们假设你已经成功安装Phalcon。检查 phpinfo() 输出，查找是否有"Phalcon"这一节，又或者执行下面的代码片断：

We'll assume you have Phalcon installed already. Check your phpinfo() output for a section referencing "Phalcon" or execute the code snippet below:

.. code-block:: php

    <?php print_r(get_loaded_extensions()); ?>

执行结果中应该会出现Phaclon扩展信息：

The Phalcon extension should appear as part of the output:

.. code-block:: php

    Array
    (
        [0] => Core
        [1] => libxml
        [2] => filter
        [3] => SPL
        [4] => standard
        [5] => phalcon
        [6] => mysql
        [7] => mysqli
    )

创建项目
------------------

使用本教程最好的方法是按照每个步骤依次进行。你可以在这里获取完整代码，`here <https://github.com/phalcon/tutorial>`_ 。

The best way to use this guide is to follow each step in turn. You can get the complete code `here <https://github.com/phalcon/tutorial>`_.

文件结构
^^^^^^^^^^^^^^

Phalcon对于应用开发并没有特殊的文件结构要求。基于她是松耦合这一事实，当你使用Phalcon进行开发时，你可以使用任何你喜欢的文件结构。

Phalcon does not impose a particular file structure for application development. Due to the fact that it is loosely coupled, you can implement Phalcon powered applications with a file structure you are most comfortable using.

本教程作为学习Phalcon的起点，建议使用以下文件结构：

For the purposes of this tutorial and as a starting point, we suggest the following structure:

.. code-block:: php

    tutorial/
      app/
        controllers/
        models/
        views/
      public/
        css/
        img/
        js/

注意，你并不需要有一个专门的"library"目录来存放Phalcon提供的库。因为她都已经在内存中加载好了，只等你来使用了（译者注： 我已等待了千年，为何良人“不回来”？）。
        
Note that you don't need any "library" directory related to Phalcon. The framework is available in memory, ready for you to use.

漂亮的URL
^^^^^^^^^^^^^^
在本教程中我们将使用漂亮（友好）的URL。友好的URL不仅利于搜索引擎优化（SEO），也方便用户记住。Phalcon支持几乎所有主流 WEB 服务器的重写模块。不过，漂亮的URL对于你的应用来说不是必须的，没有它们也能照样开发。

We'll use pretty (friendly) urls for this tutorial. Friendly URLs are better for SEO as well as they are easy for users to remember. Phalcon supports rewrite modules provided by the most popular web servers. Making your application's URLs friendly is not a requirement and you can just as easy develop without them.

在这个示例中我们将使用Apache的重写模块。让我们在 /.htaccess 文件中创建一对重写规则吧：

In this example we'll use the rewrite module for Apache. Let's create a couple of rewrite rules in the /.htaccess file:

.. code-block:: apacheconf

    #/.htaccess
    <IfModule mod_rewrite.c>
        RewriteEngine on
        RewriteRule  ^$ public/    [L]
        RewriteRule  (.*) public/$1 [L]
    </IfModule>

所有对该项目的请求都会重定向到 public/ 目录下，这使得 public 目录成为实际的网站根目录。这一步是为了确保内部的项目文件对外不可见，以避开安全威胁。
    
All requests to the project will be rewritten to the public/ directory making it the document root. This step ensures that the internal project folders remain hidden from public viewing and thus posing security threats.

重定向的第二步将会检查被请求的文件是否存在，如果文件存在则不会被WEB服务器重定向。

The second set of rules will check if the requested file exists, and if it does it doesn't have to be rewitten by the web server module:

.. code-block:: apacheconf

    #/public/.htaccess
    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php?_url=/$1 [QSA,L]
    </IfModule>

引导程序
^^^^^^^^^
你需要创建的第一个文件是引导文件。该文件非常重要，是整个应用的基础，让你能控制应用的方方面面。在引导文件中你可以实现组件初以及应用行为的初始化。

The first file you need to create is the bootstrap file. This file is very important; since it serves as the base of your application, giving you control of all aspects of it. In this file you can implement initialization of components as well as application behavior.

引导文件 public/index.php 文件看起来应该是这样的：

The public/index.php file should look like:

.. code-block:: php

    <?php

    try {

        //Register an autoloader
        $loader = new \Phalcon\Loader();
        $loader->registerDirs(array(
            '../app/controllers/',
            '../app/models/'
        ))->register();

        //Create a DI
        $di = new Phalcon\DI\FactoryDefault();

        //Setting up the view component
        $di->set('view', function(){
            $view = new \Phalcon\Mvc\View();
            $view->setViewsDir('../app/views/');
            return $view;
        });

        //Handle the request
        $application = new \Phalcon\Mvc\Application();
        $application->setDI($di);
        echo $application->handle()->getContent();

    } catch(\Phalcon\Exception $e) {
         echo "PhalconException: ", $e->getMessage();
    }

自动加载
^^^^^^^^^^^
在引导文件的第一部分，我们注册了一个类-自动加载器。它将会用于加载应用程序的控制器类和模型类。例如，为了提升应用的灵活性，我们可能会注册一个或多个控制器目录。在我们的示例中，我们已经使用了 Phalcon\\Loader 组件。

The first part that we find in the boostrap is registering an autoloader. This will be used to load classes as controllers and models in the application. For example we may register one or more directories of controllers increasing the flexibility of the application. In our example we have used the component Phalcon\\Loader.

有了它，我们便可使用不同的策略加载类。不过就目前示例来说，我们选择根据预定义目录这种方式来定位类文件。

With it, we can load classes using various strategies but for this example we have chosen to locate classes based on predefined directories:

.. code-block:: php

    <?php

    $loader = new \Phalcon\Loader();
    $loader->registerDirs(
        array(
            '../app/controllers/',
            '../app/models/'
        )
    )->register();

依赖管理
^^^^^^^^^^^^^^^^^^^^^
使用Phalcon必须要理的一个重要的概念就是 :doc:`依赖注入容器 <di>` 。这个...听起来可能很复杂，但实际上简单又实用。

A very important concept that must be understood when working with Phalcon is its :doc:`dependency injection container <di>`. It may sound complex but is actually very simple and practical.

服务容器就好比一个大箱子，里面存放了应用程序后续要使用的各种服务（全局资源）。每当框架需要某个组件时，都会使用预先约定好的服务名称向容器请求。鉴于Phalcon是一个解耦框架， Phalcon\\DI 扮演着胶水的角色，以促进不同组件透明地协同工作。

A service container is a bag where we globally store the services that our application will use to work. Each time the framework requires a component, will ask the container using a name service agreed. Since Phalcon is a highly decoupled framework, Phalcon\\DI acts as glue facilitating the integration of the different components achieving their work together in a transparent manner.

.. code-block:: php

    <?php

    //Create a DI
    $di = new Phalcon\DI\FactoryDefault();

:doc:`Phalcon\\DI\\FactoryDefault <../api/Phalcon\_DI_FactoryDefault>` 是 Phalcom\\DI 的一个变种。为了让事情变得更简单，它注册了Phalcon框架的大部分组件，所以我们就不必再一个个去注册它们了。即使之后想更换这个工厂服务也不会有任何问题。
    
:doc:`Phalcon\\DI\\FactoryDefault <../api/Phalcon\_DI_FactoryDefault>` is a variant of Phalcon\\DI. To make things easier, it has registered most of the components that come with Phalcon. Thus we should not register them one by one. Later there will be no problem in replacing a factory service.

接下来，我们注册 “视图” 服务，并显示指定框架将在哪个目录下查找视图文件。因为视图不是以类的形式存在，所以我们也无法使用类-自动加载器处理。

In the next part, we register the "view" service indicating the directory where the framework will find the views files. As the views do not correspond to classes, they can not be charged with an autoloader.

注册服务的方法不只一种，但是在本教程中我们将使用匿名函数的方法：

Services can be registered in several ways, but for our tutorial we'll use lambda functions:

.. code-block:: php

    <?php

    //Setting up the view component
    $di->set('view', function(){
        $view = new \Phalcon\Mvc\View();
        $view->setViewsDir('../app/views/');
        return $view;
    });

在引导文件的最后一部分，我们发现了 :doc:`Phalcon\\Mvc\\Application <../api/Phalcon_Mvc_Application>` 组件。它的职责是初始化请求环境、路由来访请求，然后调度请求到任何可发现的动作上。当处理完毕后，它收集所有响应并返回给浏览器。

In the last part of this file, we find :doc:`Phalcon\\Mvc\\Application <../api/Phalcon_Mvc_Application>`. Its purpose is to initialize the request environment, route the incoming request, and then dispatch any discovered actions; it aggregates any responses and returns them when the process is complete.

.. code-block:: php

    <?php

    $application = new \Phalcon\Mvc\Application();
    $application->setDI($di);
    echo $application->handle()->getContent();

正如你所看到的，引导文件不仅简短，而且不用引入额外的文件。我们用不超过30行代码为自己制定了一个灵活的MVC应用。
    
As you can see, the bootstrap file is very short and we do not need to include any additional files. We have set ourselves a flexible MVC application in less than 30 lines of code.

创建控制器
^^^^^^^^^^^^^^^^^^^^^
默认Phalcon会查找名为"Index"的控制器。当请求中没有控制器和动作传入时，它是整个应用的起点。Index 控制器 (app/controllers/IndexController.php) 示例代码如下：

By default Phalcon will look for a controller named "Index". It is the starting point when no controller or action has been passed in the request. The index controller (app/controllers/IndexController.php) looks like:

.. code-block:: php

    <?php

    class IndexController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {
            echo "<h1>Hello!</h1>";
        }

    }

控制器类名必须要以"Controller"结尾，控制器动作必须要以"Action"结尾。如果你从浏览器访问该应用，你应该看到这样的情景：

The controller classes must have the suffix "Controller" and controller actions must have the suffix "Action". If you access the application from your browser, you should see something like this:

.. figure:: ../_static/img/tutorial-1.png
    :align: center

恭喜您，你已经和Phaclon一起在蓝天飞翔了！

Congratulations, you're flying with Phalcon!

向视图发送输出
^^^^^^^^^^^^^^^^^^^^^^^^
虽然有时我们不得不从控制器发送输出到屏幕，但这种方式是不可取的，纯粹的MVC实践者将会证明这一点。视图才是负责发送输出到屏幕的组件，因此所有信息都必须传送给它。Phalcon将会在以 最后执行的控制器命名的 文件夹中查找 以最后执行的动作命名的 视图文件。在本示例中是（app/views/index/index.phtml）：

Sending output on the screen from the controller is at times necessary but not desirable as most purists in the MVC community will attest. Everything must be passed to the view which is responsible for outputting data on screen. Phalcon will look for a view with the same name as the last executed action inside a directory named as the last executed controller. In our case (app/views/index/index.phtml):

.. code-block:: php

    <?php echo "<h1>Hello!</h1>";

现在我们在控制器（app/controllers/IndexController.php）中定义一个空动作：
    
Our controller (app/controllers/IndexController.php) now has an empty action definition:

.. code-block:: php

    <?php

    class IndexController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

    }

浏览器输出应该和之前一样。当动作执行结束时，静态组件 :doc:`Phalcon\\Mvc\\View <../api/Phalcon_Mvc_View>` 会被自动创建。要了解更多关于视图的内容， :doc:`猛击这里 <views>`
    
The browser output should remain the same. The :doc:`Phalcon\\Mvc\\View <../api/Phalcon_Mvc_View>` static component is automatically created when the action execution has ended. Learn more about :doc:`views usage here <views>` .

设计注册表单
^^^^^^^^^^^^^^^^^^^^^^^^
现在，让我们来修改index.phtml视图文件，在文件中添加到新控制器"signup"的链接。目的就是允许用户在我们的应用中注册。

Now we will change the index.phtml view file, to add a link to a new controller named "signup". The goal is to allow users to sign up in our application.

.. code-block:: php

    <?php

    echo "<h1>Hello!</h1>";

    echo Phalcon\Tag::linkTo("signup", "Sign Up Here!");

生成的HTML代码将显示一个链接到新控制器的"A"标签：
    
The generated HTML code displays an "A" html tag linking to a new controller:

.. code-block:: html

    <h1>Hello!</h1> <a href="/test/signup">Sign Up Here!</a>

为了生成这个"A"标签，我们使用 :doc:`\Phalcon\\Tag <../api/Phalcon_Tag>` 类。这是一个工具类，让我们能够以框架约定的形式创建HTML标签。要了解更多关于生成HTML的内容， :doc:`猛击这里 <tags>` 。
    
To generate the tag we use the class :doc:`\Phalcon\\Tag <../api/Phalcon_Tag>`. This is a utility class that allows us to build HTML tags with framework conventions in mind. A more detailed article regarding HTML generation can be :doc:`found here <tags>`

.. figure:: ../_static/img/tutorial-2.png
	:align: center

下面是Signup控制器（app/controllers/SignupController.php）：

Here is the controller Signup (app/controllers/SignupController.php):

.. code-block:: php

    <?php

    class SignupController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

    }

空的index动作不会传递任何信息到定义表单的视图中：    
    
The empty index action gives the clean pass to a view with the form definition:

.. code-block:: html+php

    <?php use Phalcon\Tag; ?>

    <h2>Sign using this form</h2>

    <?php echo Tag::form("signup/register"); ?>

     <p>
        <label for="name">Name</label>
        <?php echo Tag::textField("name") ?>
     </p>

     <p>
        <label for="name">E-Mail</label>
        <?php echo Tag::textField("email") ?>
     </p>

     <p>
        <?php echo Tag::submitButton("Register") ?>
     </p>

    </form>


在浏览器中查看该表单，应该是这个样子：
    
Viewing the form in your browser will show something like this:

.. figure:: ../_static/img/tutorial-3.png
	:align: center

:doc:`Phalcon\\Tag <../api/Phalcon_Tag>` 组件还提供了一些有效构建表单元素的方法。
    
:doc:`Phalcon\\Tag <../api/Phalcon_Tag>` also provides useful methods to build form elements.

Phalcon\\Tag::form 方法目前只接受一个参数：应用中某 控制器/动作 的相对URI。

The Phalcon\\Tag::form method receives only one parameter for instance, a relative uri to a controller/action in the application.

通过点击"Send"按钮，你将会看到框架抛出的异常，提示我们在"signup"控制器中缺少"register"动作。这个异常是 public/index.php 文件抛出的：

By clicking the "Send" button, you will notice an exception thrown from the framework, indicating that we are missing the "register" action in the controller "signup". This exception is thrown by our public/index.php file:

    PhalconException: Action "register" was not found on controller "signup"

只要实现了这个方法，异常就会消失：

Implementing that method will remove the exception:

.. code-block:: php

    <?php

    class SignupController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function registerAction()
        {

        }

    }

当再次点击"Send"按钮的时候，你将会看到一个空白页面。用户输入的姓名和邮箱应该被储存在数据库中。按照MVC的纲领，为了写出纯粹干净的面向对象，数据库交互应该在模型中完成。
    
If you click the "Send" button again, you will see a blank page. The name and email input provided by the user should be stored in a database. According to MVC guidelines, database interactions must be done through models so as to ensure clean object oriented code.

创建模型
^^^^^^^^^^^^^^^^
Phalcon给PHP带来了第一个C语言级的ORM。这并没有增加开发复杂度，反而大大简化了这个过程。

Phalcon brings the first ORM for PHP entirely written in C-language. Instead of increasing the complexity of development, it simplifies it.

在创建第一个模型之前，我们需要一个数据库-表和它关联。下面我们定义一个简单的表来存储用户信息：

Before creating our first model, we need a database table to map it to. A simple table to store registered users can be defined like this:

.. code-block:: sql

    CREATE TABLE `users` (
      `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
      `name` varchar(70) NOT NULL,
      `email` varchar(70) NOT NULL,
      PRIMARY KEY (`id`)
    );

模型文件应该放在 app/models 目录下。关联"users"表的模型类：
 
A model should be located in the app/models directory. The model mapping to "users" table:

.. code-block:: php

    <?php

    class Users extends \Phalcon\Mvc\Model
    {

    }

设置数据库连接
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
为了使用数据库连接，以及后面通过模型访问数据，我们需要在引导程序中指定它。数据库连接不过是我们应用的另一个服务罢了，设置好后也能被其他组件使用：

In order to be able to use a database connection and subsequently access data through our models, we need to specify it in our bootstrap process. A database connection is just another service that our application has that can be used for sereral components:

.. code-block:: php

    <?php

    try {

        //Register an autoloader
        $loader = new \Phalcon\Loader();
        $loader->registerDirs(array(
            '../app/controllers/',
            '../app/models/'
        ))->register();

        //Create a DI
        $di = new Phalcon\DI\FactoryDefault();

        //Set the database service
        $di->set('db', function(){
            return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
                "host" => "localhost",
                "username" => "root",
                "password" => "secret",
                "dbname" => "test_db"
            ));
        });

        //Setting up the view component
        $di->set('view', function(){
            $view = new \Phalcon\Mvc\View();
            $view->setViewsDir('../app/views/');
            return $view;
        });

        //Handle the request
        $application = new \Phalcon\Mvc\Application();
        $application->setDI($di);
        echo $application->handle()->getContent();

    } catch(\Phalcon\Exception $e) {
         echo "PhalconException: ", $e->getMessage();
    }

如果设置了正确的数据库参数，我们的应用便能和模型一起工作了。
    
With the correct database parameters, our models are ready to work and interact with the rest of the application.

使用模型存储数据
^^^^^^^^^^^^^^^^^^^^^^^^^
接收来自表单的数据，然后下一步把它们存储到数据库-表中。

Receiving data from the form and storing them in the table is the next step.

.. code-block:: php

    <?php

    class SignupController extends \Phalcon\Mvc\Controller
    {

        public function indexAction()
        {

        }

        public function registerAction()
        {

            //Request variables from html form
            $name = $this->request->getPost("name", "string");
            $email = $this->request->getPost("email", "email");

            $user = new Users();
            $user->name = $name;
            $user->email = $email;

            //Store and check for errors
            if ($user->save() == true) {
                echo "Thanks for register!";
            } else {
                echo "Sorry, the following problems were generated: ";
                foreach ($user->getMessages() as $message) {
                    echo $message->getMessage(), "<br/>";
                }
            }
        }

    }

我们永远不能信任用户输入。对于用户输入的变量，我们必须有相应的过滤器适用于它们，对其内容进行 :doc:`过滤/审查 <filter>` 。这样能让我们的应用更安全，因为它能避免一些常见的攻击，如SQL注入等。
    
We can never trust data sent from a user. Variables passed into our application, from user input, need to have a filter applied to them so as to :doc:`validate/sanizite <filter>` their contents. This makes the application more secure because it avoids common attacks like SQL injections.

在本教程中，我们对“姓名”这个变量使用“字符串”过滤器，以保证用户的输入中不会有任何危险字符。 :doc:`Phalcon\\Filter <../api/Phalcon_Filter>` 组件使得这一切变得简单易用，因为它是通过依赖容器注入到 getPost 方法中。

In our tutorial we apply the filter "string" to the "name" variable to ensure that user did not sent us any malicious characters. The component :doc:`Phalcon\\Filter <../api/Phalcon_Filter>` makes this task trivial, since it is injected from the dependency container into the getPost call.

然后我们实例化Users类，它会关联到某条用户记录。类的公有属性会关联到users表中某条记录的所有字段。创建一条新记录，并为其字段设置相应的值，然后调用save()方法，便会在数据库中存储该记录。save()方法会给我们返回一个布尔值，以表明数据存储过程成功与否。

We then instantiate the Users class, which corresponds to a User record. The class public properties map to the fields of the record in the users table. Setting the relevant values in the new record and calling save() will store the data in the database for that record. The save() method returns a boolean value which informs us on whether the storing of the data was successful or not.

对于非空（必填）字段，会对其自动进行非空过滤。如果有必填字段没填，我们便会得到如下提示：

Additional validation happens automatically on fields that are not null (required). If we don't type any of the required fields our screen will look like this:

.. figure:: ../_static/img/tutorial-4.png
	:align: center

总结
----------
这是一个非常简单的教程，如你所见，使用Phalcon创建应用是如此的简单。Phalcon作为一个扩展存在，并不会增加开发复杂度，更不会影响现有功能。我们邀请您继续阅读手册的其他部分，这样你便能发现Phalcon提供的其他功能！

This is a very simple tutorial and as you can see, it's easy to start building an application using Phalcon. The fact that Phalcon is an extension on your web server has not interfered with the ease of development or features available. We invite you to continue reading the manual so that you can discover additional features1 offered by Phalcon!

示例应用程序
-------------------
下面提供了一些基于Phaclon的应用，它们是你学习时更完善的示例：

The following Phalcon powered applications are also available, providing more complete examples:

* `INVO application`_: Invoice generation application. Allows for management of products, companies, product types. etc.
* `PHP Alternative website`_: Multilingual and advanced routing application.

.. _开发工具: tools
.. _developer tools: tools
.. _INVO application: http://blog.phalconphp.com/post/20928554661/invo-a-sample-application
.. _PHP Alternative website: http://blog.phalconphp.com/post/24622423072/sample-application-php-alternative-site

