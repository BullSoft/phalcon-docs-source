教程三：创建简单的REST API
======================================
在本教程中，我们将会讲解如何使用不同的HTTP方法（传说中的get,post,put和delect方法）创建简单的 RESTful_ API 应用：

In this tutorial, we will explain how to create a simple application that provides a RESTful_ API using the different HTTP methods:

* GET    - 获取、查询数据
* POST   - 添加数据
* PUT    - 更新数据
* DELETE - 删除数据


* GET to retrieve and search data
* POST to add data
* PUT to update data
* DELETE to delete data

定义API
----------------
我们设计的API中包含以下方法：

The API consists of the following methods:

+--------+----------------------------+----------------------------------------------------------+
| Method |  URL                       | Action                                                   |
+========+============================+==========================================================+
| GET    | /api/robots                | 获取所有机器人                                           |
+--------+----------------------------+----------------------------------------------------------+
| GET    | /api/robots/search/Astro   | 获取名字中包含‘Astro’的机器人                            |
+--------+----------------------------+----------------------------------------------------------+
| GET    | /api/robots/2              | 通过主键查询机器人                                       |
+--------+----------------------------+----------------------------------------------------------+
| POST   | /api/robots                | 添加机器人                                               |
+--------+----------------------------+----------------------------------------------------------+
| PUT    | /api/robots/2              | 根据主键更新机器人                                       |
+--------+----------------------------+----------------------------------------------------------+
| DELETE | /api/robots/2              | 根据主键删除机器人                                       |
+--------+----------------------------+----------------------------------------------------------+

创建应用
------------------------
该应用非常简单，我们甚至不会去实现一个完整的MVC环境。在本教程中，我们使用 :doc:`微型应用 <micro>` 来达到我们的目的。

As the application is so simple, we will not implement any full MVC environment to develop it. In this case, we will use a :doc:`micro application <micro>`
to meet our goal.

以下文件结构就绰绰有余了：

The following file structure is more than enough:

.. code-block:: php

    my-rest-api/
        models/
            Robots.php
        index.php
        .htaccess

首先，我们要创建一个.htaccess文件，把所有URI重定向至index.php文件，内容如下：
        
First, we need an .htaccess file that contains all the rules to rewrite the URIs to the index.php file, that is our application:

.. code-block:: apacheconf

    <IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php?_url=/$1 [QSA,L]
    </IfModule>

然后，创建index.php文件：
    
Then, in the index.php file we create the following:

.. code-block:: php

    <?php

    $app = new \Phalcon\Mvc\Micro();

    //define the routes here

    $app->handle();

现在，我们按照上面定义的API创建路由：
    
Now we will create the routes as we defined above:

.. code-block:: php

    <?php

    $app = new Phalcon\Mvc\Micro();

    //Retrieves all robots
    $app->get('/api/robots', function() {

    });

    //Searches for robots with $name in their name
    $app->get('/api/robots/search/{name}', function($name) {

    });

    //Retrieves robots based on primary key
    $app->get('/api/robots/{id:[0-9]+}', function($id) {

    });

    //Adds a new robot
    $app->post('/api/robots', function() {

    });

    //Updates robots based on primary key
    $app->put('/api/robots/{id:[0-9]+}', function() {

    });

    //Deletes robots based on primary key
    $app->delete('/api/robots/{id:[0-9]+}', function() {

    });

    $app->handle();

每个路由都使用某个HTTP方法定义，其第一个参数是路由规则，第二个参数是处理方法。在本教程中，处理方法是一个匿名函数。路由规则：'/api/robots/{id:[0-9]+}'，明确设置参数"id"必须是数字格式。
    
Each route is defined with a method with the same name as the HTTP method, as first parameter we pass a route pattern, followed by a handler. In this case
the handler is an anonymous function. The following route: '/api/robots/{id:[0-9]+}', by example, explicitly set that the "id" parameter must have a numeric format.

当我们定义的某个路由和请求的URI匹配时，应用将会执行相应的处理方法。

When a defined route matches the requested URI then the application will execute the corresponding handler.

创建模型
----------------
我们的API提供机器人相关的信息，而这些信息是存储在数据库中的。下面这个模型让我们以面向对象方式访问数据库表。我们已经使用内置的验证器实现了一些业务逻逻和简单地验证。这么做可以确保数据库中的数据满足应用的要求：

Our API provides information about robots, these data are stored in a database. The following model allows us to access that table in an object oriented way.
We have implemented some business rules using built-in validators and simple validations. Doing this will give us the peace of mind that saved data
meet the requirements of our application:

.. code-block:: php

    <?php

    use \Phalcon\Mvc\Model\Message;
    use \Phalcon\Mvc\Model\Validator\InclusionIn;
    use \Phalcon\Mvc\Model\Validator\Uniqueness;

    class Robots extends \Phalcon\Mvc\Model
    {

        public function validation()
        {
            //Type must be: droid, mechanical or virtual
            $this->validate(new InclusionIn(
                array(
                    "field"  => "type",
                    "domain" => array("droid", "mechanical", "virtual")
                )
            ));

            //Robot name must be unique
            $this->validate(new Uniqueness(
                array(
                    "field"   => "name",
                    "message" => "The robot name must be unique"
                )
            ));

            //Year cannot be less than zero
            if ($this->year < 0) {
                $this->appendMessage(new Message("The year cannot be less than zero"));
            }

            //Check if any messages have been produced
            if ($this->validationHasFailed() == true) {
                return false;
            }
        }

    }

现在，我们必须建立数据库连接以供模型使用：
    
Now, we must set up a connection to be used by this model:

.. code-block:: php

    <?php

    $di = new \Phalcon\DI\FactoryDefault();

    //Set up the database service
    $di->set('db', function(){
        return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
            "host" => "localhost",
            "username" => "asimov",
            "password" => "zeroth",
            "dbname" => "robotics"
        ));
    });

    $app = new \Phalcon\Mvc\Micro();

    //Bind the DI to the application
    $app->setDI($di);

获取数据
---------------
我们要实现的第一个“处理方法”是接受GET请求，并返回所有机器人。我们使用PHQL来执行这个简单的查，并以JSON格式返回结果：

The first "handler" that we will implement is which by method GET returns all available robots. Let's use PHQL to perform this simple query returning the results as JSON:

.. code-block:: php

    <?php

    //Retrieves all robots
    $app->get('/api/robots', function() use ($app) {

        $phql = "SELECT * FROM Robots ORDER BY name";
        $robots = $app->modelsManager->executeQuery($phql);

        $data = array();
        foreach($robots as $robot){
            $data[] = array(
                'id' => $robot->id,
                'name' => $robot->name,
            );
        }

        echo json_encode($data);

    });

:doc:`PHQL <phql>` 可以让我们用一种更高层次、面向对象的SQL方言来书写查询。Phalcon将会根据我们使用的数据库系统把它转换成真正的SQL语句。匿名函数中的"use"可以让我们从全局作用域向函数作用域传递变量，以一种无比简单的方式。
    
:doc:`PHQL <phql>`, allow us to write queries using a high level, object oriented SQL dialect that internally
translates to the right SQL statements depending on the database system we are using. The clause "use" in the anonymous function allows
us to pass some variables from global to local scope easily.

通过机器人名称检索的处理方法如下：

The searching by name handler would look like:

.. code-block:: php

    <?php

    //Searches for robots with $name in their name
    $app->get('/api/robots/search/{name}', function($name) use ($app) {

        $phql = "SELECT * FROM Robots WHERE name LIKE :name: ORDER BY name";
        $robots = $app->modelsManager->executeQuery($phql, array(
            'name' => '%'.$name.'%'
        ));

        $data = array();
        foreach($robots as $robot){
            $data[] = array(
                'id' => $robot->id,
                'name' => $robot->name,
            );
        }

        echo json_encode($data);

    });

通过字段"id"检索就更为简单了，在本教程中，如果没有找到相关机器人，我们也会发出通知：    
    
Searching by the field "id" it's quite similar, in this case, we're also notifying if the robot was found or not:

.. code-block:: php

    <?php

    //Retrieves robots based on primary key
    $app->get('/api/robots/{id:[0-9]+}', function($id) use ($app) {

        $phql = "SELECT * FROM Robots WHERE id = :id:";
        $robot = $app->modelsManager->executeQuery($phql, array(
            'id' => $id
        ))->getFirst();

        if ($robot==false) {
            $response = array('status' => 'NOT-FOUND');
        } else {
            $response = array(
                'status' => 'FOUND',
                'data' => array(
                    'id' => $robot->id,
                    'name' => $robot->name
                )
            );
        }

        echo json_encode($response);
    });

插入数据
--------------
从请求的内容中获取数据（JSON字符串），我们同样使用PHQL来插入数据：

Taking the data as a JSON string inserted in the body of the request, we also use PHQL for insertion:

.. code-block:: php

    <?php

    //Adds a new robot
    $app->post('/api/robots', function() use ($app) {

        $robot = json_decode($app->request->getRawBody());

        $phql = "INSERT INTO Robots (name, type, year) VALUES (:name:, :type:, :year:)";

        $status = $app->modelsManager->executeQuery($phql, array(
            'name' => $robot->name,
            'type' => $robot->type,
            'year' => $robot->year
        ));

        //Check if the insertion was successfull
        if($status->success()==true){

            $robot->id = $status->getModel()->id;

            $response = array('status' => 'OK', 'data' => $robot);

        } else {

            //Change the HTTP status
            $this->response->setStatusCode(500, "Internal Error")->sendHeaders();

            //Send errors to the client
            $errors = array();
            foreach ($status->getMessages() as $message) {
                $errors[] = $message->getMessage();
            }

            $response = array('status' => 'ERROR', 'messages' => $errors);

        }

        echo json_encode($response);

    });

更新数据
-------------
数据更新类拟于数据插入。传入的"id"参数表示哪个机器人应该被更新：

The data update is similar to insertion. The "id" passed as parameter indicates what robot must be updated:

.. code-block:: php

    <?php

    //Updates robots based on primary key
    $app->put('/api/robots/{id:[0-9]+}', function($id) use($app) {

        $robot = json_decode($app->request->getRawBody());

        $phql = "UPDATE Robots SET name = :name:, type = :type:, year = :year: WHERE id = :id:";
        $status = $app->modelsManager->executeQuery($phql, array(
            'id' => $id,
            'name' => $robot->name,
            'type' => $robot->type,
            'year' => $robot->year
        ));

        //Check if the insertion was successfull
        if($status->success()==true){

            $response = array('status' => 'OK');

        } else {

            //Change the HTTP status
            $this->response->setStatusCode(500, "Internal Error")->sendHeaders();

            $errors = array();
            foreach ($status->getMessages() as $message) {
                $errors[] = $message->getMessage();
            }

            $response = array('status' => 'ERROR', 'messages' => $errors);

        }

        echo json_encode($response);

    });

删除数据
-------------
数据删除类似于数据检索（按id）。传入的"id"参数表示哪个机器人应该被删除：

The data update is similar to insertion. The "id" passed as parameter indicates what robot must be updated:【译者注：此处英文可能有误！】

.. code-block:: php

    <?php

    //Deletes robots based on primary key
    $app->delete('/api/robots/{id:[0-9]+}', function($id) use ($app) {

        $phql = "DELETE FROM Robots WHERE id = :id:";
        $status = $app->modelsManager->executeQuery($phql, array(
            'id' => $id
        ));
        if($status->success()==true){

            $response = array('status' => 'OK');

        } else {

            //Change the HTTP status
            $this->response->setStatusCode(500, "Internal Error")->sendHeaders();

            $errors = array();
            foreach ($status->getMessages() as $message) {
                $errors[] = $message->getMessage();
            }

            $response = array('status' => 'ERROR', 'messages' => $errors);

        }

        echo json_encode($response);

    });

测试应用
-----------------------
我们将使用 curl_ 来测试应用中的每个路由，以验证它们的操作是否符合预期：

Using curl_ we'll test every route in our application verifying its proper operation:

获取所有机器人：

Obtain all the robots:

.. code-block:: bash

    curl -i -X GET http://localhost/my-rest-api/api/robots

    HTTP/1.1 200 OK
    Date: Wed, 12 Sep 2012 07:05:13 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 117
    Content-Type: text/html; charset=UTF-8

    [{"id":"1","name":"Robotina"},{"id":"2","name":"Astro Boy"},{"id":"3","name":"Terminator"}]

根据名称检索机器人：
    
Search a robot by its name:

.. code-block:: bash

    curl -i -X GET http://localhost/my-rest-api/api/robots/search/Astro

    HTTP/1.1 200 OK
    Date: Wed, 12 Sep 2012 07:09:23 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 31
    Content-Type: text/html; charset=UTF-8

    [{"id":"2","name":"Astro Boy"}]

根据id获取某个机器人：    
    
Obtain a robot by its id:

.. code-block:: bash

    curl -i -X GET http://localhost/my-rest-api/api/robots/3

    HTTP/1.1 200 OK
    Date: Wed, 12 Sep 2012 07:12:18 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 56
    Content-Type: text/html; charset=UTF-8

    {"status":"FOUND","data":{"id":"3","name":"Terminator"}}

插入一个新机器人：
    
Insert a new robot:

.. code-block:: bash

    curl -i -X POST -d '{"name":"C-3PO","type":"droid","year":1977}' http://localhost/my-rest-api/api/robots

    HTTP/1.1 200 OK
    Date: Wed, 12 Sep 2012 07:15:09 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 75
    Content-Type: text/html; charset=UTF-8

    {"status":"OK","data":{"name":"C-3PO","type":"droid","year":1977,"id":"4"}}

尝试插入一个已经存在的机器人（使用相同的名称）：
    
Try to insert a new robot with the name of an existing robot:

.. code-block:: bash

    curl -i -X POST -d '{"name":"C-3PO","type":"droid","year":1977}' http://localhost/my-rest-api/api/robots

    HTTP/1.1 500 Internal Error
    Date: Wed, 12 Sep 2012 07:18:28 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 63
    Content-Type: text/html; charset=UTF-8

    {"status":"ERROR","messages":["The robot name must be unique"]}

或者使用未知类型更新机器人：
    
Or update a robot with an unknown type:

.. code-block:: bash

    curl -i -X PUT -d '{"name":"ASIMO","type":"humanoid","year":2000}' http://localhost/my-rest-api/api/robots/4

    HTTP/1.1 500 Internal Error
    Date: Wed, 12 Sep 2012 08:48:01 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 104
    Content-Type: text/html; charset=UTF-8

    {"status":"ERROR","messages":["Value of field 'type' must be part of list: droid, mechanical, virtual"]}

最后，删除一个机器人：
    
Finally, delete a robot:

.. code-block:: bash

    curl -i -X DELETE http://localhost/my-rest-api/api/robots/4

    HTTP/1.1 200 OK
    Date: Wed, 12 Sep 2012 08:49:29 GMT
    Server: Apache/2.2.22 (Unix) DAV/2
    Content-Length: 15
    Content-Type: text/html; charset=UTF-8

    {"status":"OK"}

总结
----------
正如上面所描述的一样，使用Phalcon开发Restful API是如此的简单。后续文档中将会详细介绍如何使用“微型应用”和PHQL语言。

As we have seen, develop Restful APIs with Phalcon is easy. Later in the documentation we'll explain in detail how to use micro applications and the PHQL language.


.. _curl : http://en.wikipedia.org/wiki/CURL
.. _RESTful : http://en.wikipedia.org/wiki/Representational_state_transfer
