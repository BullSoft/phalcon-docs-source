模型
===================
模型代表应用程序的信息（数据）和操作数据的规则。模型主要用于管理与相关数据库表交互的规则。在大多数情况下，库中的每张表都会关联应用中的一个模型。应用程序的大部分业务逻辑都会由模型来完成。

A model represents the information (data) of the application and the rules to manipulate that data. Models are primarily used for managing the rules of interaction with a corresponding database table. In most cases, each table in your database will correspond to one model in your application. The bulk of your application's business logic will be concentrated in the models.

:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 是所有模型的基类。它屏蔽数据库细节，提供基本的CRUD（译者注：创建、浏览、更新和删除）功能，高级查询功能，以及关系模型和其他服务的能力。 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 让你无需使用SQL语句，因为它会把方法动态地转换成相应的数据库操作。

:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` is the base for all models in a Phalcon application. It provides database independence, basic CRUD functionality, advanced finding capabilities, and the ability to relate models to one another, among other services. :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` avoids the need of having to use SQL statements because it translates methods dynamically to the respective database engine operations.

.. highlights::

    模型是工作在数据库之上的抽象层。如果你要直接操纵数据库，请查看 :doc:`Phalcon\\Db <../api/Phalcon_Db>` 组件文档。

.. highlights::

    Models are intended to work on a database high layer of abstraction. If you need to work with databases at a lower level check out the :doc:`Phalcon\\Db <../api/Phalcon_Db>` component documentation.

创建模型
---------------
模型是继承自 :doc:`Phalcon\\Mvc\\Model_Base <../api/Phalcon_Mvc_Model>` 的类。它必须被放置在模型目录下。模型文件中必须包含一个类，类名应该以驼峰形式命名：

A model is a class that extends from :doc:`Phalcon\\Mvc\\Model_Base <../api/Phalcon_Mvc_Model>`. It must be placed in the models directory. A model file must contain a single class; its class name should be in camel case notation:


.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

    }

上面的代码演示了"Robots"模型的实现。注意到Robots类继承自 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 类。:doc:`Phalcon\\Mvc\\Model_Base <../api/Phalcon_Mvc_Model>` 类给继承自它的模型提供大量功能，包括基本的数据库CRUD(创建，浏览，更新，删除）操作，数据验证，另外还有高级查询和关系查型功能。
    
The above example shows the implementation of the "Robots" model. Note that the class Robots inherits from :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`. :doc:`Phalcon\\Mvc\\Model_Base <../api/Phalcon_Mvc_Model>` provides a great deal of functionality to models that inherit it, including basic database CRUD (Create, Read, Update, Destroy) operations, data validation, as well as sophisticated search support and the ability to relate multiple models with each other.

默认情况下，"Robots"模型将会指向数据库表"robots"。如果你想手动给 模型-表 映射关系指定另一个名字，你可以使用 getSource() 方法：

By default model "Robots" will refer to the table "robots". If you want to manually specify another name for the mapping table, you can use the setSource() method:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function getSource()
        {
            return "the_robots";
        }

    }

现在，Robots模型映射到"the_robots"表。initialize()方法让我们可以在模型建立的时候自定义行为，如：关联到不同的表。单次请求中，initialize()方法只会被调用一次。
    
The model Robots now maps to "the_robots" table. The initialize() method aids in setting up the model with a custom behavior i.e. a different table. The initialize() method is only called once during the request.

模型与命名空间
--------------------
命名空间能有效避免类名冲突问题。在本例中，命名空间是必需的，我们使用getSource()方法指出关联的表名（译者注：当有两个同样名称的模型时，我们就不知所措了）：

Namespaces can be used to avoid class name collision. In this case it is necessary to indicate the name of the related table using getSource:

.. code-block:: php

    <?php

    namespace Store\Toys;

    class Robots extends \Phalcon\Mvc\Model
    {

        public function getSource()
        {
            return "robots";
        }

    }

理解记录和对象
--------------------------------
每个模型的实例都代表数据库表中的一条记录。通过对象属性你能轻易地访问表记录的数据。例如：表"robots"有如下记录：

Every instance of a model represents a row in the table. You can easily access record data by reading object properties. For example, for a table "robots" with the records:

.. code-block:: sql

    mysql> select * from robots;
    +----+------------+------------+------+
    | id | name       | type       | year |
    +----+------------+------------+------+
    |  1 | Robotina   | mechanical | 1972 |
    |  2 | Astro Boy  | mechanical | 1952 |
    |  3 | Terminator | cyborg     | 2029 |
    +----+------------+------------+------+
    3 rows in set (0.00 sec)

你可以根据主键查找某条记录，然后把它的名称打印出来：    
    
You could find a certain record by its primary key and then print its name:

.. code-block:: php

    <?php

    // Find record with id = 3
    $robot = Robots::findFirst(3);

    // Prints "Terminator"
    echo $robot->name;

当记录被读到内存中后，你可以对它的数据做一些修改，并保存这些更改：
    
Once the record is in memory, you can make modifications to its data and then save changes:

.. code-block:: php

    <?php

    $robot = Robots::findFirst(3);
    $robot->name = "RoboCop";
    $robot->save();

正如你所看到的，我们根本不需要使用原生的SQL语句。:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 组件 为Web应用提供了数据库的高度抽象。
    
As you can see, there is no need to use raw SQL statements. :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` provides high database abstraction for web applications.

查询记录
---------------
:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 也为查询记录提供了方法。下面的示例将为你演示如何从模型中查询一条或多条记录：

:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` also offers several methods for querying records. The following examples will show you how to query one or more records from a model:

.. code-block:: php

    <?php

    // How many robots are there?
    $robots = Robots::find();
    echo "There are ", count($robots), "\n";

    // How many mechanical robots are there?
    $robots = Robots::find("type = 'mechanical'");
    echo "There are ", count($robots), "\n";

    // Get and print virtual robots ordered by name
    $robots = Robots::find(array("type = 'virtual'", "order" => "name"));
    foreach ($robots as $robot) {
        echo $robot->name, "\n";
    }

    // Get first 100 virtual robots ordered by name
    $robots = Robots::find(array("type = 'virtual'", "order" => "name", "limit" => 100));
    foreach ($robots as $robot) {
       echo $robot->name, "\n";
    }

你也可以使用findFirst()方法获取符合查询条件的一条记录：
    
You could also use the findFirst() method to get only the first record matching the given criteria:

.. code-block:: php

    <?php

    // What's the first robot in robots table?
    $robot = Robots::findFirst();
    echo "The robot name is ", $robot->name, "\n";

    // What's the first mechanical robot in robots table?
    $robot = Robots::findFirst("type = 'mechanical'");
    echo "The first mechanical robot name is ", $robot->name, "\n";

    // Get first virtual robot ordered by name
    $robot = Robots::findFirst(array("type = 'virtual'", "order" => "name"));
    echo "The first virtual robot name is ", $robot->name, "\n";

find()和findFirst()方法都接受关联数组作为参数，以指定查询条件：
    
Both find() and findFirst() methods accept an associative array specifying the search criteria:

.. code-block:: php

    <?php

    $robot = Robots::findFirst(
        array(
            "type = 'virtual'",
            "order" => "name DESC",
            "limit" => 30
        )
    );

    $robots = Robots::find(
        array(
            "conditions" => "type = ?1",
            "bind"       => array(1 => "virtual")
        )
    );

可用的查询选项如下：
    
The available query options are:

+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| Parameter   | Description                                                                                                                                                                                  | Example                                                      |
+=============+==============================================================================================================================================================================================+==============================================================+
| conditions  | Search conditions for the find operation. Is used to extract only those records that fulfill a specified criterion. By default Phalcon_model assumes the first parameter are the conditions. | "conditions" => "name LIKE 'steve%'"                         |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| bind        | Bind is used together with options, by replacing placeholders and escaping values thus increasing security                                                                                   | "bind" => array("status" => "A", "type" => "some-time")      |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| order       | Is used to sort the resultset. Use one or more fields separated by commas.                                                                                                                   | "order" => "name DESC, status"                               |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| limit       | Limit the results of the query to results to certain range                                                                                                                                   | "limit" => 10                                                |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| group       | Allows to collect data across multiple records and group the results by one or more columns                                                                                                  | "group" => "name, status"                                    |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| for_update  | With this option, :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` reads the latest available data, setting exclusive locks on each row it reads                                        | "for_update" => true                                         |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| shared_lock | With this option, :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` reads the latest available data, setting shared locks on each row it reads                                           | "shared_lock" => true                                        |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
| cache       | Cache the resulset, reducing the continuous access to the relational system                                                                                                                  | "cache" => array("lifetime" => 3600, "key" => "my-find-key") |
+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+

如果你喜欢，你还可以使用面向对象方式构建查询，而不是使用数组作为参数：

If you prefer, there is also available a way to to create queries in an object oriented way, instead of using an array of parameters:

.. code-block:: php

    <?php

    $robots = Robots::query()
        ->where("type = :type:")
        ->bind(array("type" => "mechanical"))
        ->order("name")
        ->execute();

静态方法query()返回一个 :doc:`Phalcon\\Mvc\\Model\\Criteria <../api/Phalcon_Mvc_Model_Criteria>` 对象，（译者注：什么？类名这么长？），不是很友好，除非你使用IDE的自动完成功能。
        
The static method query() returns a :doc:`Phalcon\\Mvc\\Model\\Criteria <../api/Phalcon_Mvc_Model_Criteria>` object that is friendly with IDE autocompleters.

Phalcon还允许你使用一种更高层次的、面向对象的类SQL语言查询记录，我们称之为 :doc:`PHQL <phql>` 。

Phalcon also offers you the possibility to query records using a high level, object oriented, SQL-like language called :doc:`PHQL <phql>`.

模型结果集
^^^^^^^^^^^^^^^^
findFirst()会直接返回被调用模型类的一个实例（当然如果有数据返回的话），而find()方法会返回一个 :doc:`Phalcon\\Mvc\\Model\\Resultset\\Simple <../api/Phalcon_Mvc_Model_Resultset_Simple>` 对象。它是一个封装了结果集应有的所有功能的对象，如：遍历、查询指定记录以及计数等。这种对象的功能比标准数组强大很多。而 :doc:`Phalcon\\Mvc\\Model\\Resultset <../api/Phalcon_Mvc_Model_Resultset>` 的一个最棒的特性是，任何时候内存中只有结果集的一条记录。这在处理大数据量时能帮助我们有效控制内存。

While findFirst() returns directly an instance of the called class (when there is data to be returned), the find() method returns a :doc:`Phalcon\\Mvc\\Model\\Resultset\\Simple <../api/Phalcon_Mvc_Model_Resultset_Simple>`. This is an object that encapsulates all the functionality a resultset has like traversing, seeking specific records, counting, etc. These objects are more powerful than standard arrays. One of the greatest features of the :doc:`Phalcon\\Mvc\\Model\\Resultset <../api/Phalcon_Mvc_Model_Resultset>` is that at any time there is only one record in memory. This greatly helps in memory management especially when working with large amounts of data.

.. code-block:: php

    <?php

    // Get all robots
    $robots = Robots::find();

    // Traversing with a foreach
    foreach ($robots as $robot) {
        echo $robot->name, "\n";
    }

    // Traversing with a while
    $robots->rewind();
    while ($robots->valid()) {
        $robot = $robots->current();
        echo $robot->name, "\n";
        $robots->next();
    }

    // Count the resultset
    echo count($robots);

    // Alternative way to count the resultset
    echo $robots->count();

    // Move the internal cursor to the third robot
    $robots->seek(2);
    $robot = $robots->current()

    // Access a robot by its position in the resultset
    $robot = $robots[5];

    // Check if there is a record in certain position
    if (isset($robots[3]) {
       $robot = $robots[3];
    }

    // Get the first record in the resultset
    $robot = robots->getFirst();

    // Get the last record
    $robot = robots->getLast();

Phalcon的结果集模拟滚动游标的特性，你可以根据某行的位置获取该行记录，也可以搜寻结果集使其内部指针指向某个位置。注意，有些数据库系统不支持滚动游标，为了获取请求位置的记录，我们不得不重新执行查询以重置游标到初始位置。类似地，如果一个记录集被遍历了多次，那么查询也会被执行相同的次数。
    
Phalcon resulsets emulates scrollable cursors, you can get any row just by accessing its position, or seeking the internal pointer to a certain position. Note that some database systems don't support scrollable cursors, this forces to re-execute the query in order to rewind the cursor to the beginning and obtain the record at the requested position. Similarly, if a resultset is traversed several times, the query must be executed the same number of times.

因为在内存中存储大量的查询结果会消耗很多资源，我们以32条记录作为一块，每次从数据库获取一块作为结果集，以减少某些场景下重复执行查询的次数。

Because store large query results in memory can consume many resources, resultsets are obtained from the database in chunks of 32 rows chunks reducing the need to re-execute the request in several cases.

注意，结果集可以被序列化后存储在缓存系统中。 :doc:`Phalcon\\Cache <cache>` 能够完成这个任务。但是，序列化数据会导致 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 从数据库获取所有（译者注：符合条件的）数据并保存在一个数组中，这个过程会消耗更多的内存。

Note that resultsets can be serialized and stored in a a cache backend. :doc:`Phalcon\\Cache <cache>` can help with that task. However, serializing data causes :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` to retrieve all the data from the database in an array, thus consuming more memory while this process takes place.

.. code-block:: php

    <?php

    // Query all records from model parts
    $parts = Parts::find();

    // Store the resultset into a file
    file_put_contents("cache.txt", serialize($parts));

    // Get parts from file
    $parts = unserialize(file_get_contents("cache.txt"));

    // Traverse the parts
    foreach ($parts as $part) {
       echo $part->id;
    }

绑定参数
^^^^^^^^^^^^^^^^^^
:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 也支持绑定参数。尽管使用绑定参数有一些性能损失，但我们仍然鼓励你使用这种方式，这能消除你的代码受到SQL注入攻击的可能性。绑定参数支持字符串和数字占位符（译者注：也即有名称的占位符和无名称的占位符）。绑定参数可以简单地实现，如下所示：

Bound parameters are also supported in :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`. Although there is a minimal performance impact by using bound parameters, you are encouraged to use this methodology so as to eliminate the possibility of your code being subject to SQL injection attacks. Both string and integer placeholders are supported. Binding parameters can simply be achieved as follows:

.. code-block:: php

    <?php

    // Query robots binding parameters with string placeholders
    $conditions = "name = :name: AND type = :type:";
    $parameters = array("name" => "Robotina", "type" => "maid");
    $robots     = Robots::find(array($conditions, "bind" => $parameters));

    // Query robots binding parameters with integer placeholders
    $conditions = "name = ?1 AND type = ?2";
    $parameters = array(1 => "Robotina", 2 => "maid");
    $robots     = Robots::find(array($conditions, "bind" => $parameters));

    // Query robots binding parameters with both string and integer placeholders
    $conditions = "name = :name: AND type = ?1";
    $parameters = array("name" => "Robotina", 1 => "maid");
    $robots     = Robots::find(array($conditions, "bind" => $parameters));

当使用数字占位符时，你必须定义它们为整型，如：1或2 。而"1"或"2"会被当作字符串而非数字，如果出现这种情况，占位符便不能正确地被替换。
    
When using numeric placeholders, you will need to define them as integers i.e. 1 or 2. In this case "1" or "2" are considered strings and not numbers, so the placeholder could not be successfully replaced.

字符串会自动被 PDO_ 转义。这个功能会受数据库连接字符集的影响，因此建议在连接参数或数据库配置中定义正确的字符集，错误的字符集在存储或获取数据时可能会产生不良影响。

Strings are automatically escaped using PDO_. This function takes into account the connection charset, so its recommended to define the correct charset in the connection parameters or in the database configuration, as a wrong charset will produce undesired effects when storing or retrieving data.

绑定参数适用于所有查询方法，如：find()和findFirst()，也包括统计方法，如：count(), sum(), average() 等。

Bound parameters are available for all query methods such as find() and findFirst() but also the calculation methods like count(), sum(), average() etc.

关系模型
----------------------------
一共有四种关系：一对一，一对多，多对一和多对多。关系既可以是单向的也可以是双向的，既可以是简单的（如一对一模型），也可以是复杂的（如组合模型）。模型管理器为这些关系管理外键约束，关系的定义能提高引用完整性，也能让我们简单快速地通过模型访问相关记录。通过实现关系，就能以一种统一且简单的方式访问某条记录的相关数据了。

There are four types of relationships: one-on-one, one-to-many, many-to-one and many-to-many. The relationship may be unidirectional or bidirectional, and each can be simple (a one to one model) or more complex (a combination of models). The model manager manages foreign key constraints for these relationships, the definition of these helps referential integrity as well as easy and fast access of related records to a model. Through the implementation of relations, it is easy to access data in related models from each record in a uniform way.

单向关系
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
单向关系是那些A到B的关系成立，但B到A的关系却不成立的关系。

Unidirectional relations are those that are generated in relation to one another but not vice versa.

双向关系
^^^^^^^^^^^^^^^^^^^^^^^
双向关系在两个模型中都建立关联，A模型内部定义与B模型相反的关联。

The bidirectional relations build relationships in both models and each model defines the inverse relationship of the other.

定义关系
^^^^^^^^^^^^^^^^^^^^^^
在Phalcon中，关联必须在模型的initialize()方法中定义。belongsTo()，hasOne()和hasMany()三个方法在当前模型字段和另一个模型的字段之间定义关联。每个方法接收3个参数：当前模型字段，关联模型和关联模型字段。

In Phalcon, relationships must be defined in the initialize() method of a model. The methods belongsTo(), hasOne() or hasMany() define the relationship between one or more fields from the current model to fields in another model. Each of these methods requires 3 parameters: local fields, referenced model, referenced fields.

+-----------+----------------------------+
| 方法      | 描述                       |
+===========+============================+
| hasMany   | 定义1-n关系                |
+-----------+----------------------------+
| hasOne    | 定义1-1关系                |
+-----------+----------------------------+
| belongsTo | 定义n-1关系                |
+-----------+----------------------------+

下面的SQL模式创建了3张数据库表，它们之前的关系是一个很好的关联示例：

The following schema shows 3 tables whose relations will serve us as an example regarding relationships:

.. code-block:: sql

    CREATE TABLE `robots` (
        `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
        `name` varchar(70) NOT NULL,
        `type` varchar(32) NOT NULL,
        `year` int(11) NOT NULL,
        PRIMARY KEY (`id`)
    );

    CREATE TABLE `robots_parts` (
        `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
        `robots_id` int(10) NOT NULL,
        `parts_id` int(10) NOT NULL,
        `created_at` DATE NOT NULL,
        PRIMARY KEY (`id`),
        KEY `robots_id` (`robots_id`),
        KEY `parts_id` (`parts_id`)
    );

    CREATE TABLE `parts` (
        `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
        `name` varchar(70) NOT NULL,
        PRIMARY KEY (`id`)
    );

* "Robots"有很多"RobotsParts"。
* "Parts"有很多"RobotsParts"。
* "RobotsParts"属于"Robots"，并且和"Parts"有一对多关系。
    
* The model "Robots" has many "RobotsParts".
* The model "Parts" has many "RobotsParts".
* The model "RobotsParts" belongs to "Robots" and "Parts" models as a one-to-many relation.

模型之间的关系可以如下实现：

The models with their relations could be implemented as follows:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {
        public function initialize()
        {
            $this->hasMany("id", "RobotsParts", "robots_id");
        }

    }

.. code-block:: php

    <?php

    class Parts extends \Phalcon\Mvc\Model
    {

        public function initialize()
        {
            $this->hasMany("id", "RobotsParts", "parts_id");
        }

    }

.. code-block:: php

    <?php

    class RobotsParts extends \Phalcon\Mvc\Model
    {

        public function initialize()
        {
            $this->belongsTo("robots_id", "Robots", "id");
            $this->belongsTo("parts_id", "Parts", "id");
        }

    }

第一个参数是当前模型的字段，第二个参数是关联模型的名称，第三个参数是关联模型的字段。你也可以使用数组定义多字段。
    
The first parameter indicates the field of the local model used in the relationship; the second indicates the name of the referenced model and the third the field name in the referenced model. You could also use arrays to define multiple fields in the relationship.

关系模型的优势
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
明确定义模型之间的关系，让我们很容易找到某特定记录的相关记录。

When explicitly defining the relationships between models, it is easy to find related records for a particular record.

.. code-block:: php

    <?php

    $robot = Robots::findFirst(2);
    foreach ($robot->getRobotsParts() as $robotPart) {
        echo $robotPart->getParts()->name, "\n";
    }

Phalcon使用魔术方法__call从关联模型中获取数据。如果被调用的方法名以"get"开始，:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 会调用findFirst()/find()方法，并返回其结果。下面的示例将会分别使用和不使用魔术方法来获取关联结果：
    
Phalcon uses the magic method __call to retrieve data from a relationship. If the called method has a "get" prefix :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` will return a findFirst()/find() result. The following example compares retrieving related results with using the magic method and without:

.. code-block:: php

    <?php

    $robot = Robots::findFirst(2);

    // Robots model has a 1-n (hasMany)
    // relationship to RobotsParts then
    $robotsParts = $robot->getRobotsParts();

    // Only parts that match conditions
    $robotsParts = $robot->getRobotsParts("created_at='2012-03-15'");

    // Or using bound parameters
    $robotsParts = $robot->getRobotsParts(array("created_at=:date:", "bind" => array("date" => "2012-03-15")));

    $robotPart = RobotsParts::findFirst(1);

    // RobotsParts model has a n-1 (belongsTo)
    // relationship to RobotsParts then
    $robot = $robotPart->getRobots();

.. code-block:: php

    <?php

    $robot = Robots::findFirst(2);

    // Robots model has a 1-n (hasMany)
    // relationship to RobotsParts then
    $robotsParts = RobotsParts::find("robots_id = '" . $robot->id . "'");

    // Only parts that match conditions
    $robotsParts = RobotsParts::find(
        "robots_id = '" . $robot->id . "' AND created_at='2012-03-15'"
    );

    $robotPart = RobotsParts::findFirst(1);

    // RobotsParts model has a n-1 (belongsTo)
    // relationship to RobotsParts then
    $robot = Robots::findFirst("id = '" . $robotPart->robots_id . "'");

"get"前缀告诉Phalcon使用find()/findFirst()方法获取相关记录。你也可以使用"count"作为前缀，获取相关记录的总数：
    
The prefix "get" is used to find()/findFirst() related records. You can also use "count" prefix to return an integer denoting the count of the related records:

.. code-block:: php

    <?php

    $robot = Robots::findFirst(2);
    echo "The robot have ", $robot->countRobotsParts(), " parts\n";

虚拟外键
^^^^^^^^^^^^^^^^^^^^
默认情况下，关系模型不能真正处理外键约束，也即，如果你尝试插入/更新的值在关联模型中不存在，Phalcon不会产生验证失败消息。你可以在定义关系的时候添加第四个参数来改变这种默认的处理方式。

By default, relationships do not act like database foreign keys, that is, if you try to insert/update a value without having a valid value in the referenced model, Phalcon will not produce a validation message. You can modify this behavior by adding a fourth parameter when defining a relationship.

我们现在修改一下RobotsPart模型，用于演示这个特性：

The RobotsPart model can be changed to demonstrate this feature:

.. code-block:: php

    <?php

    class RobotsParts extends \Phalcon\Mvc\Model
    {

        public function initialize()
        {
            $this->belongsTo("robots_id", "Robots", "id", array(
                "foreignKey" => true
            ));

            $this->belongsTo("parts_id", "Parts", "id", array(
                "foreignKey" => array(
                    "message" => "The part_id does not exist on the parts model"
                )
            ));
        }

    }

如果你修改belongsTo()关系让其处理外键约束，Phalcon将会验证插入/更新的值，以确保关联模型中该值也存在。类似地，如果修改hasMany()/hasOne()关系让其处理外键约束，当某记录存在于关联模型中时，Phalcon将会验证该记录不能被删除。
   
If you alter a belongsTo() relationship to act as foreign key, it will validate that the values inserted/updated on those fields have a valid value on the referenced model. Similarly, if a hasMany()/hasOne() is altered it will validate that the records cannot be deleted if that record is used on a referenced model.

.. code-block:: php

    <?php

    class Parts extends \Phalcon\Mvc\Model
    {

        public function initialize()
        {
            $this->hasMany("id", "RobotsParts", "parts_id", array(
                "foreignKey" => array(
                    "message" => "The part cannot be deleted because other robots are using it"
                )
            ));
        }

    }

生成统计
-----------------------
统计基于数据库系统的常用函数，如：COUNT, SUM, MAX, MIN和AVG。:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 允许我们使用类方法直接访问这些函数。

Calculations are helpers for commonly used functions of database systems such as COUNT, SUM, MAX, MIN or AVG. :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` allows to use these functions directly from the exposed methods.

统计总数示例：

Count examples:

.. code-block:: php

    <?php

    // How many employees are?
    $rowcount = Employees::count();

    // How many different areas are assigned to employees?
    $rowcount = Employees::count(array("distinct" => "area"));

    // How many employees are in the Testing area?
    $rowcount = Employees::count("area = 'Testing'");

    //Count employees grouping results by their area
    $group = Employees::count(array("group" => "area"));
    foreach ($group as $row) {
       echo "There are ", $group->rowcount, " in ", $group->area;
    }

    // Count employees grouping by their area and ordering the result by count
    $group = Employees::count(
        array(
            "group" => "area",
            "order" => "rowcount"
        )
    );

统计求和示例：    
    
Sum examples:

.. code-block:: php

    <?php

    // How much are the salaries of all employees?
    $total = Employees::sum(array("column" => "salary"));

    // How much are the salaries of all employees in the Sales area?
    $total = Employees::sum(
        array(
            "column"     => "salary",
            "conditions" => "area = 'Sales'"
        )
    );

    // Generate a grouping of the salaries of each area
    $group = Employees::sum(
        array(
            "column" => "salary",
            "group"  => "area"
        )
    );
    foreach ($group as $row) {
       echo "The sum of salaries of the ", $group->area, " is ", $group->sumatory;
    }

    // Generate a grouping of the salaries of each area ordering
    // salaries from higher to lower
    $group = Employees::sum(
        array(
            "column" => "salary",
            "group"  => "area",
            "order"  => "sumatory DESC"
        )
    );

统计平均数示例：
    
Average examples:

.. code-block:: php

    <?php

    // What is the average salary for all employees?
    $average = Employees::average(array("column" => "salary"));

    // What is the average salary for the Sales's area employees?
    $average = Employees::average(
        array(
            "column" => "salary",
            "conditions" => "area = 'Sales'"
        )
    );

统计最大/最小示例：
    
Max/Min examples:

.. code-block:: php

    <?php

    // What is the oldest age of all employees?
    $age = Employees::maximum(array("column" => "age"));

    // What is the oldest of employees from the Sales area?
    $age = Employees::maximum(
        array(
            "column" => "age",
            "conditions" => "area = 'Sales'"
        )
    );

    // What is the lowest salary of all employees?
    $salary = Employees::minimum(array("column" => "salary"));

缓存结果集
^^^^^^^^^^^^^^^^^^
数据库访问是最常见的性能瓶颈之一。这是因为在每次请求时PHP都需要执行复杂的数据库连接以获取数据。不过幸好已经有较完善的技术方案来避免持续访问数据库，那就是：缓存系统中不会频繁变更的数据以提供快速的查询（通常是内存）。

Access to database systems is often one of the most common bottlenecks in terms of performance. This is due to the complex connection process that PHP must do in each request to obtain data from the database. A well established technique to avoid the continuous access to the database is to cache resultsets that don't change frequently in a system with faster access (usually memory).

当 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 需要缓存结果集时，它将会向依赖注入容器请求名为"modelsCache"的服务。

When :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` requires a service to cache resultsets, it will request it to the Dependency Injector Container with the convention name "modelsCache".

由于Phalcon提供了能缓存任何数据类型的组件，我们稍后会介绍如何在模型中使用它。不过首先你要在服务容器中把它注册为服务：

As Phalcon provides a component to cache any kind of data, we'll explain how to integrate it with Models. First you need to register it as a service in the services container:

.. code-block:: php

    <?php

    //Set the models cache service
    $di->set('modelsCache', function(){

        //Cache data for one day by default
        $frontCache = new Phalcon\Cache\Frontend\Output(array(
            "lifetime" => 86400
        ));

        //Memcached connection settings
        $cache = new Phalcon\Cache\Backend\File($frontCache, array(
            "host" => "localhost",
            "port" => "11211"
        ));

        return $cache;
    });

在服务生效前，你可以按自己意愿创建和定制缓存。当缓存被正确建立后，你就可以像这样缓存结果集：
    
You have complete control in creating and customizing the cache before being used to record the service as an anonymous function. Once the cache setup is properly defined you could cache resultsets as follows:

.. code-block:: php

    <?php

    // Get products without caching
    $products = Products::find();

    // Just cache the resultset. The cache will expire in 1 hour (3600 seconds)
    $products = Products::find(array("cache" => true));

    // Cache the resultset only for 5 minutes
    $products = Products::find(array("cache" => 300));

    // Cache the resultset with a key pre-defined
    $products = Products::find(array("cache" => array("key" => "my-products-key")));

    // Cache the resultset with a key pre-defined and for 2 minutes
    $products = Products::find(
        array(
            "cache" => array(
                "key"      => "my-products-key",
                "lifetime" => 120
            )
        )
    );

    // Using a custom cache
    $products = Products::find(array("cache" => $myCache));

默认情况下，:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 在存储结果集时会为其分配一个唯一键，通常是SQL语句的Md5值。这是非常实用的，因为模型的参数发生任何细微的变化都会导致生成一个新的唯一键。如果你想自定义缓存结果集的键，你可以像上面示例一样始终使用key参数。getLastKey()方法可以获取最近一次缓存的结果集的键，使用该方法你可以事后从缓存中定位和获取结果集：
    
By default, :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` will create a unique key to store the resultset, using a md5 hash of the SQL select statement generated internally. This is very practical because it generates a new unique key for every change in the parameters passed in the object. If you wish to control the cache keys, you could always use the key() parameter as seen in the example above. The getLastKey() method retrieves the key of the last cached entry so that you can target and retrieve the resultset later on from the cache.:

.. code-block:: php

    <?php

    // Cache the resultset using an automatic key
    $products = Products::find(array("cache" => 3600));

    // Get last generated key
    $automaticKey = $products->getCache()->getLastKey();

    // Use resultset as normal
    foreach($products as $product){
        //...
    }

缓存键是由 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 自动生成的，并以"phc"作为前缀。这能帮助我们很容易地识别出和 :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 相关的缓存记录：

Cache keys automatically generated by :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` are always prefixed with "phc". This helps to easily identify the cached entries related to :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`:

.. code-block:: php

    <?php

    // Set the cache to the models manager
    $cache = $di->getModelsCache();

    // Get keys created by Phalcon_Mvc_Model
    foreach ($cache->queryKeys("phc") as $key) {
         echo $key, "\n";
    }

有一点要注意，不是所有的结果集都必须被缓存的，那些频繁变更的结果就不应该被缓存因为它们很快就会变成无效数据，缓存它们会影响性能。此外，不会频繁变更的大数据也可以被缓存，但是开发者必须在以下两个方法做出决定：可用的缓存机制和是否接受简单获取数据的性能损失。
    
Note that not all resultsets must be cached. Results that change very frequently should not be cached since they are invalidated very quickly and caching in that case impacts performance. Additionally, large datasets that do not change frequently could be cached but that is a decision that the developer has to make based on the available caching mechanism and whether the performance impact to simply retrieve that data in the first place is acceptable.

缓存也同样可以作用于关系模型产生的结果集：

Caching could be also applied to resultsets generated using relationships:

.. code-block:: php

    <?php

    // Query some post
    $post = Post::findFirst();

    // Get comments related to a post, also cache it
    $comments = $post->getComments(array("cache" => true));

    // Get comments related to a post, setting lifetime
    $comments = $post->getComments(array("cache" => true, "lifetime" => 3600));

当缓存的结果集过期时，你可以轻易地使用产生的键把它从缓存中删除即可。    
    
When a cached resultset needs to be invalidated, you can simply delete it from the cache using the generated key.

创建或更新记录
-------------------------
Phalcon\\Mvc\\Model::save()方法允许你创建/更新记录，究竟是创建还是更新取决于这些记录在模型映射的表中是否存在。:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` 的create()和update()方法内部调用的就是save()方法。要让这如预期一样运作，有必要在表中恰当地定义主键，以便确定某条记录是需要更新还是创建。

The method Phalcon\\Mvc\\Model::save() allows you to create/update records according to whether they already exist in the table associated with a model. The save method is called internally by the create and update methods of :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`. For this to work as expected it is necessary to have properly defined a primary key in the entity to determine whether a record should be updated or created.

同样，save()方法还会执行相关的验证器，虚拟外键约束以及在模型中定义的事件：

Also the method executes associated validators, virtual foreign keys and events that are defined in the model:

.. code-block:: php

    <?php

    $robot       = new Robots();
    $robot->type = "mechanical";
    $robot->name = "Astro Boy";
    $robot->year = 1952;
    if ($robot->save() == false) {
        echo "Umh, We can't store robots right now: \n";
        foreach ($robot->getMessages() as $message) {
            echo $message, "\n";
        }
    } else {
        echo "Great, a new robot was saved successfully!";
    }

创建和更新记录
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
当一个应用有很多并发访问时，我们期望创建记录，但实际上做的是更新操作。如果我们使用Phalcon\\Mvc\\Model::save()方法保存记录，这完全是有可能发生的。如果我们确定某条记录是要创建还是更新，我们可以使用"create"或"update"方法代替"save"方法：

When an application has a lot of competition, maybe we expect to create a record but that is actually updated. This could happen if we use Phalcon\\Mvc\\Model::save() to persist the records in the database. If we want to be certain that a record will be created or updated created we can change save by "create" or "update":

.. code-block:: php

    <?php

    $robot       = new Robots();
    $robot->type = "mechanical";
    $robot->name = "Astro Boy";
    $robot->year = 1952;

    //This record only must be created
    if ($robot->create() == false) {
        echo "Umh, We can't store robots right now: \n";
        foreach ($robot->getMessages() as $message) {
            echo $message, "\n";
        }
    } else {
        echo "Great, a new robot was created successfully!";
    }


Auto-generated identity columns
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Some models may have identity columns. These columns usually are the primary key of the mapped table. :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` can recognize the identity column and will omit it from the internal SQL INSERT, so the database system could generate an auto-generated value for it. Always after creating a record, the identity field will be registered with the  value generated in the database system for it:

.. code-block:: php

    <?php

    $robot->save();
    echo "The generated id is: ", $robot->id;

Validation Messages
^^^^^^^^^^^^^^^^^^^
:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` has a messaging subsystem that provides a flexible way to output or store the validation messages generated during the insert/update processes.

Each message consists of an instance of the class :doc:`Phalcon\\Mvc\\Model\\Message <../api/Phalcon_Mvc_Model_Message>`. The set of messages generated can be retrieved with the method getMessages(). Each message provides extended information like the field name that generated the message or the message type:

.. code-block:: php

    <?php

    if ($robot->save() == false) {
        foreach ($robot->getMessages() as $message) {
            echo "Message: ", $message->getMessage();
            echo "Field: ", $message->getField();
            echo "Type: ", $message->getType();
        }
    }

The following types of validation messages can be generated by :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`:

+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
| Type                | Description                                                                                                                        |
+=====================+====================================================================================================================================+
| PresenceOf          | Generated when a field with a non-null attribute on the database is trying to insert/update a null value                           |
+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
| ConstraintViolation | Generated when a field part of a virtual foreign key is trying to insert/update a value that doesn't exist in the referenced model |
+---------------------+------------------------------------------------------------------------------------------------------------------------------------+
| InvalidValue        | Generated when a validator failed due to an invalid value                                                                          |
+---------------------+------------------------------------------------------------------------------------------------------------------------------------+

Validation Events and Events Manager
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Models allow you to implement events that will be thrown when performing an insert or update. They help to define business rules for a certain model. The following are the events supported by :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` and their order of execution:

+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Operation          | Name                     | Can stop operation?   | Explanation                                                                                                         |
+====================+==========================+=======================+=====================================================================================================================+
| Inserting/Updating | beforeValidation         | YES                   | Is executed before the fields are validated for not nulls or foreign keys                                           |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting          | beforeValidationOnCreate | YES                   | Is executed before the fields are validated for not nulls or foreign keys when an insertion operation is being made |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Updating           | beforeValidationOnUpdate | YES                   | Is executed before the fields are validated for not nulls or foreign keys when an updating operation is being made  |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting/Updating | onValidationFails        | YES (already stopped) | Is executed after an integrity validator fails                                                                      |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting          | afterValidationOnCreate  | YES                   | Is executed after the fields are validated for not nulls or foreign keys when an insertion operation is being made  |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Updating           | afterValidationOnUpdate  | YES                   | Is executed after the fields are validated for not nulls or foreign keys when an updating operation is being made   |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting/Updating | afterValidation          | YES                   | Is executed after the fields are validated for not nulls or foreign keys                                            |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting/Updating | beforeSave               | YES                   | Runs before the required operation over the database system                                                         |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Updating           | beforeUpdate             | YES                   | Runs before the required operation over the database system only when an updating operation is being made           |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting          | beforeCreate             | YES                   | Runs before the required operation over the database system only when an inserting operation is being made          |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Updating           | afterUpdate              | NO                    | Runs after the required operation over the database system only when an updating operation is being made            |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting          | afterCreate              | NO                    | Runs after the required operation over the database system only when an inserting operation is being made           |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+
| Inserting/Updating | afterSave                | NO                    | Runs after the required operation over the database system                                                          |
+--------------------+--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------+

To make a model to react to an event, we must to implement a method with the same name of the event:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function beforeValidationOnCreate()
        {

            echo "This is executed before create a Robot!";
        }

    }

Events can be useful to assign values before perform a operation, for example:

.. code-block:: php

    <?php

    class Products extends \Phalcon\Mvc\Model
    {

        public function beforeCreate()
        {
            //Set the creation date
            $this->created_at = date('Y-m-d H:i:s');
        }

        public function beforeUpdate()
        {
            //Set the modification date
            $this->modified_in = date('Y-m-d H:i:s');
        }

    }


Additionally, this component is integrated with :doc:`Phalcon\\Events\\Manager <../api/Phalcon_Events_Manager>`, this means we can create listeners that run when an event is triggered.

.. code-block:: php

    <?php

    $eventsManager = new Phalcon\Events\Manager();

    //Attach an anonymous function as a listener for "model" events
    $eventsManager->attach('model', function($event, $robot) {
        if ($event->getType() == 'beforeSave') {
            if ($robot->name == 'Scooby Doo') {
                echo "Scooby Doo isn't a robot!";
                return false;
            }
        }
        return true;
    });

    $robot = new Robots();
    $robot->setEventsManager($eventsManager);
    $robot->name = 'Scooby Doo';
    $robot->year = 1969;
    $robot->save();

In the above example the EventsManager only acted as a bridge between an object and a listener (the anonymous function). If we want all objects created in our application use the same EventsManager then we need to assign this to the Models Manager:

.. code-block:: php

    <?php

    //Registering the modelsManager service
    $di->set('modelsManager', function() {

        $eventsManager = new Phalcon\Events\Manager();

        //Attach an anonymous function as a listener for "model" events
        $eventsManager->attach('model', function($event, $model){
            if (get_class($model) == 'Robots') {
                if ($event->getType() == 'beforeSave') {
                    if ($modle->name == 'Scooby Doo') {
                        echo "Scooby Doo isn't a robot!";
                        return false;
                    }
                }
            }
            return true;
        });

        //Setting a default EventsManager
        $modelsManager = new Phalcon\Mvc\Models\Manager();
        $modelsManager->setEventsManager($eventsManager);
        return $modelsManager;
    });


Implementing a Business Rule
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
When an insert, update or delete is executed, the model verifies if there are any methods with the names of the events listed in the table above.

We recommend that validation methods are declared protected to prevent that business logic implementation from being exposed publicly.

The following example implements an event that validates the year cannot be smaller than 0 on update or insert:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function beforeSave()
        {
            if ($this->year < 0) {
                echo "Year cannot be smaller than zero!";
                return false;
            }
        }

    }

Some events return false as an indication to stop the current operation. If an event doesn't return anything, :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` will assume a true value.

Validating Data Integrity
^^^^^^^^^^^^^^^^^^^^^^^^^
:doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` provides several events to validate data and implement business rules. The special "validation" event allows us to call built-in validators over the record. Phalcon exposes a few built-in validators that can be used at this stage of validation.

The following example shows how to use it:

.. code-block:: php

    <?php

    use Phalcon\Mvc\Model\InclusionIn;
    use Phalcon\Mvc\Model\Uniqueness;

    class Robots extends \Phalcon\Mvc\Model
    {

        public function validation()
        {
            $this->validate(new InclusionIn(
                array(
                    "field"  => "type",
                    "domain" => array("Mechanical", "Virtual")
                )
            ));
            $this->validate(new Uniqueness(
                array(
                    "field"   => "name",
                    "message" => "The robot name must be unique"
                )
            ));
            if ($this->validationHasFailed() == true) {
                return false;
            }
        }

    }

The above example performs a validation using the built-in validator "InclusionIn". It checks the value of the field "type" in a domain list. If the value is not included in the method then the validator will fail and return false. The following built-in validators are available:

+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| Name         | Explanation                                                                                                                            | Example                                                         |
+==============+========================================================================================================================================+=================================================================+
| Email        | Validates that field contains a valid email format                                                                                     | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Email>`       |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| ExclusionIn  | Validates that a value is not within a list of possible values                                                                         | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Exclusionin>` |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| InclusionIn  | Validates that a value is within a list of possible values                                                                             | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Inclusionin>` |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| Numericality | Validates that a field has a numeric format                                                                                            | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Numericality>`|
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| Regex        | Validates that the value of a field matches a regular expression                                                                       | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Regex>`       |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+
| Uniqueness   | Validates that a field or a combination of a set of fields are not present more than once in the existing records of the related table | :doc:`Example <../api/Phalcon_Mvc_Model_Validator_Uniqueness>`  |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+

In addition to the built-in validatiors, you can create your own validators:

.. code-block:: php

    <?php

    class UrlValidator extends \Phalcon\Mvc\Model\Validator
    {

        public function validate($model)
        {
            $field = $this->getOption('field');

            $value = $model->$field;
            $filtered = filter_var($value, FILTER_VALIDATE_URL);
            if (!$filtered) {
                $this->appendMessage("The URL is invalid", $field, "UrlValidator");
                return false;
            }
            return true;
        }

    }

Adding the validator to a model:

.. code-block:: php

    <?php

    class Customers extends \Phalcon\Mvc\Model
    {

        public function validation()
        {
            $this->validate(new UrlValidator(
                array(
                    "field"  => "url",
                )
            ));
            if ($this->validationHasFailed() == true) {
                return false;
            }
        }

    }

The idea of ​​creating validators is make them reusable between several models. A validator can also be as simple as:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function validation()
        {
            if ($this->type == "Old") {
                $message = new Phalcon\Mvc\Model\Message(
                    "Sorry, old robots are not allowed anymore",
                    "type",
                    "MyType"
                );
                $this->appendMessage($message);
                return false;
            }
            return true;
        }

    }

Deleting Records
----------------
The method \Phalcon\Mvc\Model::delete() allows to delete a record. You can use it as follows:

.. code-block:: php

    <?php

    $robot = Robots::findFirst(11);
    if ($robot != false) {
        if ($robot->delete() == false) {
            echo "Sorry, we can't delete the robot right now: \n";
            foreach ($robot->getMessages() as $message) {
                echo $message, "\n";
            }
        } else {
            echo "The robot was deleted successfully!";
        }
    }

You can also delete many records by traversing a resultset with a foreach:

.. code-block:: php

    <?php

    foreach (Robots::find("type='mechanical'") as $robot) {
        if ($robot->delete() == false) {
            echo "Sorry, we can't delete the robot right now: \n";
            foreach ($robot->getMessages() as $message) {
                echo $message, "\n";
            }
        } else {
            echo "The robot was deleted successfully!";
        }
    }

The following events are available to define custom business rules that can be executed when a delete operation is performed:

+-----------+--------------+---------------------+------------------------------------------+
| Operation | Name         | Can stop operation? | Explanation                              |
+===========+==============+=====================+==========================================+
| Deleting  | beforeDelete | YES                 | Runs before the delete operation is made |
+-----------+--------------+---------------------+------------------------------------------+
| Deleting  | afterDelete  | NO                  | Runs after the delete operation was made |
+-----------+--------------+---------------------+------------------------------------------+

Validation Failed Events
------------------------

Another type of events is available when the data validation process finds any inconsistency:

+--------------------------+--------------------+--------------------------------------------------------------------+
| Operation                | Name               | Explanation                                                        |
+==========================+====================+====================================================================+
| Insert or Update         | notSave            | Triggered when the INSERT or UPDATE operation fails for any reason |
+--------------------------+--------------------+--------------------------------------------------------------------+
| Insert, Delete or Update | onValidationFails  | Triggered when any data manipulation operation fails               |
+--------------------------+--------------------+--------------------------------------------------------------------+

Transactions
------------
When a process performs multiple database operations, it is often that each step is completed successfully so that data integrity can be maintained. Transactions offer the ability to ensure that all database operations have been executed successfully before the data is committed in the database.

Transactions in Phalcon allow you to commit all operations if they have been executed successfully or rollback all operations if something went wrong.

.. code-block:: php

    <?php

    try {

        //Create a transaction manager
        $manager = new Phalcon\Mvc\Model\Transaction\Manager();

        // Request a transaction
        $transaction = $manager->get();

        $robot = new Robots();
        $robot->setTransaction($transaction);
        $robot->name = "WALLÂ·E";
        $robot->created_at = date("Y-m-d");
        if ($robot->save() == false) {
            $transaction->rollback("Cannot save robot");
        }

        $robotPart = new RobotParts();
        $robotPart->setTransaction($transaction);
        $robotPart->type = "head";
        if ($robotPart->save() == false) {
            $transaction->rollback("Cannot save robot part");
        }

        //Everything goes fine, let's commit the transaction
        $transaction->commit();

    } catch(Phalcon\Mvc\Model\Transaction\Failed $e) {
        echo "Failed, reason: ", $e->getMessage();
    }

Transactions can be used to delete many records in a consistent way:

.. code-block:: php

    <?php

    try {

        //Create a transaction manager
        $manager = new Phalcon\Mvc\Model\Transaction\Manager();

        //Request a transaction
        $transaction = $manager->get();

        //Get the robots will be deleted
        foreach (Robots::find("type='mechanical'") as $robot) {
            $robot->setTransaction($transaction);
            if ($robot->delete() == false) {
                //Something goes wrong, we should to rollback the transaction
                foreach ($robot->getMessages() as $message) {
                    $transaction->rollback($message->getMessage());
                }
            }
        }

        //Everything goes fine, let's commit the transaction
        $transaction->commit();

        echo "Robots were deleted successfully!";

    } catch(Phalcon\Mvc\Model\Transaction\Failed $e) {
        echo "Failed, reason: ", $e->getMessage();
    }

Transactions are reused no matter where the transaction object is retrieved. A new transaction is generated only when a commit() or rollback() is performed. You can use the service container to create an overall transaction manager for the entire application:

.. code-block:: php

    <?php

    $di->set('transactions', function(){
        return new Phalcon\Mvc\Model\Transaction\Manager();
    });

Then access it from a controller or view:

.. code-block:: php

    <?php

    class ProductsController extends \Phalcon\Mvc\Controller {

        public function saveAction()
        {

            //Obtain the TransactionsManager from the DI container
            $manager = $this->di->getShared('transactions');

            //Request a transaction
            $transaction = $manager->get();

        }

    }

Models Meta-Data
----------------
To speed up development :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` helps you to query fields and constraints from tables related to models. To achieve this, :doc:`Phalcon\\Mvc\\Model\\MetaData <../api/Phalcon_Mvc_Model_MetaData>` is available to manage and cache table meta-data.

Sometimes it is necessary to get those attributes when working with models. You can get a meta-data instance as follows:

.. code-block:: php

    <?php

    $robot = new Robots();

    // Get Phalcon\Mvc\Model\Metadata instance
    $metaData = $robot->getDI()->getModelsMetaData();

    // Get robots fields names
    $attributes = $metaData->getAttributes($robot);
    print_r($attributes);

    // Get robots fields data types
    $dataTypes = $metaData->getDataTypes($robot);
    print_r($dataTypes);


Caching Meta-Data
^^^^^^^^^^^^^^^^^
Once the application is in a production stage, it is not necessary to query the meta-data of the table from the database system each time you use the table. This could be done caching the meta-data using any of the following adapters:

+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
| Adapter | Description                                                                                                                                                                                                                                                                                                                                   | API                                                                                       |
+=========+===============================================================================================================================================================================================================================================================================================================================================+===========================================================================================+
| Memory  | This adapter is the default. The meta-data is cached only during the request. When the request is completed, the meta-data are released as part of the normal memory of the request. This adapter is perfect when the application is in development so as to refresh the meta-data in each request containing the new and/or modified fields. | :doc:`Phalcon\\Mvc\\Model\\MetaData\\Memory <../api/Phalcon_Mvc_Model_MetaData_Memory>`   |
+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
| Session | This adapter stores meta-data in the $_SESSION superglobal. This adapter is recommended only when the application is actually using a small number of models. The meta-data are refreshed everytime a new session starts. This also requires the use of session_start() to start the session before using any models.                         | :doc:`Phalcon\\Mvc\\Model\\MetaData\\Session <../api/Phalcon_Mvc_Model_MetaData_Session>` |
+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+
| Apc     | The Apc adapter uses the `Alternative PHP Cache (APC)`_ to store the table meta-data. You can specify the lifetime of the meta-data with options. This is the most recommended way to store meta-data when the application is in production stage.                                                                                            | :doc:`Phalcon\\Mvc\\Model\\MetaData\\Apc <../api/Phalcon_Mvc_Model_MetaData_Apc>`         |
+---------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------+

As other ORM's dependencies, the metadata manager is requested from the services container:

.. code-block:: php

    <?php

    $di->set('modelsMetadata', function() {

        // Create a meta-data manager with APC
        $metaData = new Phalcon\Model\MetaData\Apc(array(
            "lifetime" => 86400,
            "suffix" => "my-suffix"
        ));

        return $metaData;
    });

Setting a different schema
--------------------------
Models may are mapped to tables that are in different schemas/databases than the default. You can use the getSchema method to define that:

Then, in the Initialize method, we define the connection service for the model:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function getSchema()
        {
            return "toys";
        }

    }

Setting multiple databases
--------------------------
In Phalcon, all models can belong to the same database connection or have an individual one. Actually, when :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` needs to connect to the database it requests the "db" service in the application's services container. You can overwrite this service setting it in the initialize method:

.. code-block:: php

    <?php

    //This service returns a MySQL database
    $di->set('dbMysql', function() {
         return new \Phalcon\Db\Adapter\Pdo\Mysql(array(
            "host" => "localhost",
            "username" => "root",
            "password" => "secret",
            "dbname" => "invo"
        ));
    });

    //This service returns a PostgreSQL database
    $di->set('dbPostgres', function() {
         return new \Phalcon\Db\Adapter\Pdo\PostgreSQL(array(
            "host" => "localhost",
            "username" => "postgres",
            "password" => "",
            "dbname" => "invo"
        ));
    });

Then, in the Initialize method, we define the connection service for the model:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function initialize()
        {
            $this->setConnectionService('dbPostgres');
        }

    }

Logging Low-Level SQL Statements
--------------------------------
Using high-level abstraction components such as :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` to access a database, it is difficult to understand which statements are finally sent to the database system. :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>` is supported internally by :doc:`Phalcon\\Db <../api/Phalcon_Db>`. :doc:`Phalcon\\Logger <../api/Phalcon_Logger>` interacts with :doc:`Phalcon\\Db <../api/Phalcon_Db>`, providing logging capabilities on the database abstraction layer, thus allowing us to log SQL statements as they happen.

.. code-block:: php

    <?php

    $di->set('db', function() {

        $eventsManager = new Phalcon\Events\Manager();

        $logger = new Phalcon\Logger\Adapter\File("app/logs/debug.log");

        //Listen all the database events
        $eventsManager->attach('db', function($event, $connection) use ($logger) {
            if ($event->getType() == 'beforeQuery') {
                $logger->log($connection->getSQLStatement(), \Phalcon\Logger::INFO);
            }
        });

        $connection = new \Phalcon\Db\Adapter\Pdo\Mysql(array(
            "host" => "localhost",
            "username" => "root",
            "password" => "secret",
            "dbname" => "invo"
        ));

        //Assign the eventsManager to the db adapter instance
        $connection->setEventsManager($eventsManager);

        return $connection;
    });

As models access the default database connection, all SQL statements that are sent to the database system will be logged in the file:

.. code-block:: php

    <?php

    $robot = new Robots();
    $robot->name = "Robby the Robot";
    $robot->created_at = "1956-07-21"
    if ($robot->save() == false) {
        echo "Cannot save robot";
    }

As above, the file *app/logs/db.log* will contain something like this:

.. code-block:: irc

    [Mon, 30 Apr 12 13:47:18 -0500][DEBUG][Resource Id #77] INSERT INTO robots (name, created_at) VALUES ('Robby the Robot', '1956-07-21')

Profiling SQL Statements
------------------------
Thanks to :doc:`Phalcon_Db <../api/Phalcon_Db>`, the underlying component of :doc:`Phalcon\\Mvc\\Model <../api/Phalcon_Mvc_Model>`, it's possible to profile the SQL statements generated by the ORM in order to analyze the performance of database operations. With this you can diagnose performance problems and to discover bottlenecks.

.. code-block:: php

    <?php

    $di->set('profiler', function(){
        return new Phalcon\Db\Profiler();
    })

    $di->set('db', function() use($di) {

        $eventsManager = new Phalcon\Events\Manager();

        //Get a shared instance of the DbProfiler
        $profiler = $di->getShared('profiler');

        //Listen all the database events
        $eventsManager->attach('db', function($event, $connection) use ($profiler) {
            if ($event->getType() == 'beforeQuery') {
                $profiler->startProfile($connection->getSQLStatement());
            }
            if ($event->getType() == 'afterQuery') {
                $profiler->stopProfile();
            }
        });

        $connection = new \Phalcon\Db\Adapter\Pdo\Mysql(array(
            "host" => "localhost",
            "username" => "root",
            "password" => "secret",
            "dbname" => "invo"
        ));

        //Assign the eventsManager to the db adapter instance
        $connection->setEventsManager($eventsManager);

        return $connection;
    });

Profiling some queries:

.. code-block:: php

    <?php

    // Send some SQL statements to the database
    Robots::find();
    Robots::find(array("order" => "name");
    Robots::find(array("limit" => 30);

    //Get the generated profiles from the profiler
    $profiles = $di->getShared('profiler')->getProfiles();

    foreach ($profiles as $profile) {
       echo "SQL Statement: ", $profile->getSQLStatement(), "\n";
       echo "Start Time: ", $profile->getInitialTime(), "\n";
       echo "Final Time: ", $profile->getFinalTime(), "\n";
       echo "Total Elapsed Time: ", $profile->getTotalElapsedSeconds(), "\n";
    }

Each generated profile contains the duration in miliseconds that each instruction takes to complete as well as the generated SQL statement.

Injecting services into Models
------------------------------
You may be required to access the application services within a model, the following example explains how to do that:

.. code-block:: php

    <?php

    class Robots extends \Phalcon\Mvc\Model
    {

        public function notSave()
        {
            //Obtain the flash service from the DI container
            $flash = $this->getDI()->getShared('flash');

            //Show validation messages
            foreach($this->getMesages() as $message) {
                $flash->error((string) $message);
            }
        }

    }

The "notSave" event is triggered every time that a create or update action fails. So we're flashing the validation messages obtaining the "flash" service from the DI container. By doing this, we don't have to print messages after each save.


.. _Alternative PHP Cache (APC): http://www.php.net/manual/en/book.apc.php
.. _PDO: http://www.php.net/manual/en/pdo.prepared-statements.php
