<<<<<<< HEAD:zh_CN/api/Phalcon_Db_Adapter_Pdo.rst
Class **Phalcon\\Db\\Adapter\\Pdo**
===================================

*extends* :doc:`Phalcon\\Db <Phalcon_Db>`

Phalcon\\Db\\Adapter\\Pdo is the Phalcon\\Db that internally uses PDO to connect to a database 

.. code-block:: php

    <?php

     $connection = new Phalcon\Db\Adapter\Pdo\Mysql(array(
      'host' => '192.168.0.11',
      'username' => 'sigma',
      'password' => 'secret',
      'dbname' => 'blog',
      'port' => '3306',
     ));



Constants
---------

*integer* **FETCH_ASSOC**

*integer* **FETCH_BOTH**

*integer* **FETCH_NUM**

Methods
---------

public  **__construct** (*array* $descriptor)

Constructor for Phalcon\\Db\\Adapter\\Pdo



public *boolean*  **connect** (*array* $descriptor)

This method is automatically called in Phalcon\\Db\\Adapter\\Pdo constructor. Call it when you need to restore a database connection



public :doc:`Phalcon\\Db\\Result\\Pdo <Phalcon_Db_Result_Pdo>`  **query** (*string* $sqlStatement, *array* $placeholders)

Sends SQL statements to the database server returning the success state. Use this method only when the SQL statement sent to the server return rows 

.. code-block:: php

    <?php

    //Querying data

$resultset = $connection->query("SELECT * FROM robots WHERE type='mechanical'"); $resultset = $connection->query("SELECT * FROM robots WHERE type=?", array("mechanical"));



public *boolean*  **execute** (*string* $sqlStatement, *array* $placeholders)

Sends SQL statements to the database server returning the success state. Use this method only when the SQL statement sent to the server don't return any row 

.. code-block:: php

    <?php

    //Inserting data
    $success = $connection->execute("INSERT INTO robots VALUES (1, 'Astro Boy')");
    $success = $connection->execute("INSERT INTO robots VALUES (?, ?)", array(1, 'Astro Boy'));




public *int*  **affectedRows** ()

Returns the number of affected rows by the last INSERT/UPDATE/DELETE repoted by the database system 

.. code-block:: php

    <?php

    $connection->query("DELETE FROM robots");
    echo $connection->affectedRows(), ' were deleted';




public *boolean*  **close** ()

Closes active connection returning success. Phalcon automatically closes and destroys active connections within Phalcon\\Db\\Pool



public *string*  **escapeString** (*string* $str)

Escapes a value to avoid SQL injections



public  **bindParams** (*string* $sqlSelect, *array* $params)

Bind params to SQL select



public *int*  **lastInsertId** (*string* $table, *string* $primaryKey, *string* $sequenceName)

Returns insert id for the auto_increment column inserted in the last SQL statement



public *boolean*  **begin** ()

Starts a transaction in the connection



public *boolean*  **rollback** ()

Rollbacks the active transaction in the connection



public *boolean*  **commit** ()

Commits the active transaction in the connection



public *boolean*  **isUnderTransaction** ()

Checks whether connection is under database transaction



public *PDO*  **getInternalHandler** ()

Return internal PDO handler



public :doc:`Phalcon\\Db\\Index <Phalcon_Db_Index>` [] **describeIndexes** (*string* $table, *string* $schema)

Lists table indexes



public :doc:`Phalcon\\Db\\Reference <Phalcon_Db_Reference>` [] **describeReferences** (*string* $table, *string* $schema)

Lists table references



public *array*  **tableOptions** (*string* $tableName, *string* $schemaName)

Gets creation options from a table



public  **setEventsManager** (:doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>` $eventsManager) inherited from Phalcon\\Db

Sets the event manager



public :doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>`  **getEventsManager** () inherited from Phalcon\\Db

Returns the internal event manager



public *array*  **fetchOne** (*string* $sqlQuery, *int* $fetchMode) inherited from Phalcon\\Db

Returns the first row in a SQL query result 

.. code-block:: php

    <?php

    //Getting first robot
    $robot = $connection->fecthOne("SELECT * FROM robots");
    print_r($robot);
    
    //Getting first robot with associative indexes only
    $robot = $connection->fecthOne("SELECT * FROM robots", Phalcon\Db::FETCH_ASSOC);
    print_r($robot);




public *array*  **fetchAll** (*string* $sqlQuery, *int* $fetchMode) inherited from Phalcon\\Db

Dumps the complete result of a query into an array 

.. code-block:: php

    <?php

    //Getting all robots
    $robots = $connection->fetchAll("SELECT * FROM robots");
    foreach($robots as $robot){
    	print_r($robot);
    }
    
    //Getting all robots with associative indexes only
    $robots = $connection->fetchAll("SELECT * FROM robots", Phalcon\Db::FETCH_ASSOC);
    foreach($robots as $robot){
    	print_r($robot);
    }




public *boolean*  **insert** (*string* $table, *array* $values, *array* $fields) inherited from Phalcon\\Db

Inserts data into a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Inserting a new robot
     $success = $connection->insert(
         "robots",
         array("Astro Boy", 1952),
         array("name", "year")
     );
    
     //Next SQL sentence is sent to the database system
     INSERT INTO `robots` (`name`, `year`) VALUES ("Astro boy", 1952);




public *boolean*  **update** (*string* $table, *array* $fields, *array* $values, *string* $whereCondition) inherited from Phalcon\\Db

Updates data on a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Updating existing robot
     $success = $connection->update(
         "robots",
         array("name")
         array("New Astro Boy"),
         "id = 101"
     );
    
     //Next SQL sentence is sent to the database system
     UPDATE `robots` SET `name` = "Astro boy" WHERE id = 101




public *boolean*  **delete** (*string* $table, *string* $whereCondition, *array* $placeholders) inherited from Phalcon\\Db

Deletes data from a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Deleting existing robot
     $success = $connection->delete(
         "robots",
         "id = 101"
     );
    
     //Next SQL sentence is generated
     DELETE FROM `robots` WHERE `id` = 101




public *string*  **getColumnList** (*array* $columnList) inherited from Phalcon\\Db

Gets a list of columns



public *string*  **limit** (*string* $sqlQuery, *int* $number) inherited from Phalcon\\Db

Appends a LIMIT clause to $sqlQuery argument <code>$connection->limit("SELECT * FROM robots", 5);



public *string*  **tableExists** (*string* $tableName, *string* $schemaName) inherited from Phalcon\\Db

Generates SQL checking for the existence of a schema.table <code>$connection->tableExists("blog", "posts")



public *string*  **viewExists** (*string* $viewName, *string* $schemaName) inherited from Phalcon\\Db

Generates SQL checking for the existence of a schema.view <code>$connection->viewExists("active_users", "posts")



public *string*  **forUpdate** (*string* $sqlQuery) inherited from Phalcon\\Db

Returns a SQL modified with a FOR UPDATE clause



public *string*  **sharedLock** (*string* $sqlQuery) inherited from Phalcon\\Db

Returns a SQL modified with a LOCK IN SHARE MODE clause



public *boolean*  **createTable** (*string* $tableName, *string* $schemaName, *array* $definition) inherited from Phalcon\\Db

Creates a table using MySQL SQL



public *boolean*  **dropTable** (*string* $tableName, *string* $schemaName, *boolean* $ifExists) inherited from Phalcon\\Db

Drops a table from a schema/database



public *boolean*  **addColumn** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\Column <Phalcon_Db_Column>` $column) inherited from Phalcon\\Db

Adds a column to a table



public *boolean*  **modifyColumn** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\Column <Phalcon_Db_Column>` $column) inherited from Phalcon\\Db

Modifies a table column based on a definition



public *boolean*  **dropColumn** (*string* $tableName, *string* $schemaName, *string* $columnName) inherited from Phalcon\\Db

Drops a column from a table



public *boolean*  **addIndex** (*string* $tableName, *string* $schemaName, *DbIndex* $index) inherited from Phalcon\\Db

Adds an index to a table



public *boolean*  **dropIndex** (*string* $tableName, *string* $schemaName, *string* $indexName) inherited from Phalcon\\Db

Drop an index from a table



public *boolean*  **addPrimaryKey** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\Index <Phalcon_Db_Index>` $index) inherited from Phalcon\\Db

Adds a primary key to a table



public *boolean*  **dropPrimaryKey** (*string* $tableName, *string* $schemaName) inherited from Phalcon\\Db

Drops primary key from a table



public *boolean true*  **addForeignKey** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\Reference <Phalcon_Db_Reference>` $reference) inherited from Phalcon\\Db

Adds a foreign key to a table



public *boolean true*  **dropForeignKey** (*string* $tableName, *string* $schemaName, *string* $referenceName) inherited from Phalcon\\Db

Drops a foreign key from a table



public *string*  **getColumnDefinition** (:doc:`Phalcon\\Db\\Column <Phalcon_Db_Column>` $column) inherited from Phalcon\\Db

Returns the SQL column definition from a column



public *array*  **listTables** (*string* $schemaName) inherited from Phalcon\\Db

List all tables on a database <code> print_r($connection->listTables("blog") ?>



public *string*  **getDescriptor** () inherited from Phalcon\\Db

Return descriptor used to connect to the active database



public *string*  **getConnectionId** () inherited from Phalcon\\Db

Gets the active connection unique identifier



public  **getSQLStatement** () inherited from Phalcon\\Db

Active SQL statement in the object



public *string*  **getType** () inherited from Phalcon\\Db

Returns type of database system the adapter is used for



public *string*  **getDialectType** () inherited from Phalcon\\Db

Returns the name of the dialect used



public :doc:`Phalcon\\Db\\Dialect <Phalcon_Db_Dialect>`  **getDialect** () inherited from Phalcon\\Db

Returns internal dialect instance



=======
Abstract class **Phalcon\\Db\\Adapter\\Pdo**
============================================

*extends* abstract class :doc:`Phalcon\\Db\\Adapter <Phalcon_Db_Adapter>`

*implements* :doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>`, :doc:`Phalcon\\Events\\EventsAwareInterface <Phalcon_Events_EventsAwareInterface>`

Phalcon\\Db\\Adapter\\Pdo is the Phalcon\\Db that internally uses PDO to connect to a database  

.. code-block:: php

    <?php

    $connection = new Phalcon\Db\Adapter\Pdo\Mysql(array(
    	'host' => '192.168.0.11',
    	'username' => 'sigma',
    	'password' => 'secret',
    	'dbname' => 'blog',
    	'port' => '3306'
    ));



Methods
-------

public  **__construct** (*array* $descriptor)

Constructor for Phalcon\\Db\\Adapter\\Pdo



public *boolean*  **connect** ([*array* $descriptor])

This method is automatically called in Phalcon\\Db\\Adapter\\Pdo constructor. Call it when you need to restore a database connection 

.. code-block:: php

    <?php

     //Make a connection
     $connection = new Phalcon\Db\Adapter\Pdo\Mysql(array(
      'host' => '192.168.0.11',
      'username' => 'sigma',
      'password' => 'secret',
      'dbname' => 'blog',
     ));
    
     //Reconnect
     $connection->connect();




public *\PDOStatement*  **prepare** (*string* $sqlStatement)

Returns a PDO prepared statement to be executed with 'executePrepared' 

.. code-block:: php

    <?php

     $statement = $connection->prepare('SELECT * FROM robots WHERE name = :name');
     $pdoResult = $connection->executePrepared($statement, array('name' => 'Voltron'));




public *\PDOStatement*  **executePrepared** (*\PDOStatement* $statement, *array* $placeholders, *array* $dataTypes)

Executes a prepared statement binding. This function uses integer indexes starting from zero 

.. code-block:: php

    <?php

     $statement = $connection->prepare('SELECT * FROM robots WHERE name = :name');
     $pdoResult = $connection->executePrepared($statement, array('name' => 'Voltron'));




public :doc:`Phalcon\\Db\\ResultInterface <Phalcon_Db_ResultInterface>`  **query** (*string* $sqlStatement, [*unknown* $placeholders], [*unknown* $dataTypes])

Sends SQL statements to the database server returning the success state. Use this method only when the SQL statement sent to the server is returning rows 

.. code-block:: php

    <?php

    //Querying data
    $resultset = $connection->query("SELECT * FROM robots WHERE type='mechanical'");
    $resultset = $connection->query("SELECT * FROM robots WHERE type=?", array("mechanical"));




public *boolean*  **execute** (*string* $sqlStatement, [*unknown* $placeholders], [*unknown* $dataTypes])

Sends SQL statements to the database server returning the success state. Use this method only when the SQL statement sent to the server doesn't return any row 

.. code-block:: php

    <?php

    //Inserting data
    $success = $connection->execute("INSERT INTO robots VALUES (1, 'Astro Boy')");
    $success = $connection->execute("INSERT INTO robots VALUES (?, ?)", array(1, 'Astro Boy'));




public *int*  **affectedRows** ()

Returns the number of affected rows by the lastest INSERT/UPDATE/DELETE executed in the database system 

.. code-block:: php

    <?php

    $connection->execute("DELETE FROM robots");
    echo $connection->affectedRows(), ' were deleted';




public *boolean*  **close** ()

Closes the active connection returning success. Phalcon automatically closes and destroys active connections when the request ends



public *string*  **escapeIdentifier** (*string* $identifier)

Escapes a column/table/schema name 

.. code-block:: php

    <?php

    $escapedTable = $connection->escapeIdentifier('robots');
    $escapedTable = $connection->escapeIdentifier(array('store', 'robots'));




public *string*  **escapeString** (*string* $str)

Escapes a value to avoid SQL injections according to the active charset in the connection 

.. code-block:: php

    <?php

    $escapedStr = $connection->escapeString('some dangerous value');




public *array*  **convertBoundParams** (*unknown* $sqlStatement, *array* $params)

Converts bound parameters such as :name: or ?1 into PDO bind params ? 

.. code-block:: php

    <?php

     print_r($connection->convertBoundParams('SELECT * FROM robots WHERE name = :name:', array('Bender')));




public *int*  **lastInsertId** ([*string* $sequenceName])

Returns the insert id for the auto_increment/serial column inserted in the lastest executed SQL statement 

.. code-block:: php

    <?php

     //Inserting a new robot
     $success = $connection->insert(
         "robots",
         array("Astro Boy", 1952),
         array("name", "year")
     );
    
     //Getting the generated id
     $id = $connection->lastInsertId();




public *boolean*  **begin** ([*boolean* $nesting])

Starts a transaction in the connection



public *boolean*  **rollback** ([*boolean* $nesting])

Rollbacks the active transaction in the connection



public *boolean*  **commit** ([*boolean* $nesting])

Commits the active transaction in the connection



public *int*  **getTransactionLevel** ()

Returns the current transaction nesting level



public *boolean*  **isUnderTransaction** ()

Checks whether the connection is under a transaction 

.. code-block:: php

    <?php

    $connection->begin();
    var_dump($connection->isUnderTransaction()); //true




public *\PDO*  **getInternalHandler** ()

Return internal PDO handler



public  **setEventsManager** (:doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>` $eventsManager) inherited from Phalcon\\Db\\Adapter

Sets the event manager



public :doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>`  **getEventsManager** () inherited from Phalcon\\Db\\Adapter

Returns the internal event manager



public  **setDialect** (*unknown* $dialect) inherited from Phalcon\\Db\\Adapter

Sets the dialect used to produce the SQL



public :doc:`Phalcon\\Db\\DialectInterface <Phalcon_Db_DialectInterface>`  **getDialect** () inherited from Phalcon\\Db\\Adapter

Returns internal dialect instance



public *array*  **fetchOne** (*string* $sqlQuery, [*int* $fetchMode], [*unknown* $placeholders]) inherited from Phalcon\\Db\\Adapter

Returns the first row in a SQL query result 

.. code-block:: php

    <?php

    //Getting first robot
    $robot = $connection->fetchOne("SELECT * FROM robots");
    print_r($robot);
    
    //Getting first robot with associative indexes only
    $robot = $connection->fetchOne("SELECT * FROM robots", Phalcon\Db::FETCH_ASSOC);
    print_r($robot);




public *array*  **fetchAll** (*string* $sqlQuery, [*int* $fetchMode], [*unknown* $placeholders]) inherited from Phalcon\\Db\\Adapter

Dumps the complete result of a query into an array 

.. code-block:: php

    <?php

    //Getting all robots with associative indexes only
    $robots = $connection->fetchAll("SELECT * FROM robots", Phalcon\Db::FETCH_ASSOC);
    foreach ($robots as $robot) {
    	print_r($robot);
    }
    
      //Getting all robots that contains word "robot" withing the name
      $robots = $connection->fetchAll("SELECT * FROM robots WHERE name LIKE :name",
    	Phalcon\Db::FETCH_ASSOC,
    	array('name' => '%robot%')
      );
    foreach($robots as $robot){
    	print_r($robot);
    }




public *boolean*  **insert** (*string* $table, *array* $values, [*array* $fields], [*array* $dataTypes]) inherited from Phalcon\\Db\\Adapter

Inserts data into a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Inserting a new robot
     $success = $connection->insert(
         "robots",
         array("Astro Boy", 1952),
         array("name", "year")
     );
    
     //Next SQL sentence is sent to the database system
     INSERT INTO `robots` (`name`, `year`) VALUES ("Astro boy", 1952);




public *boolean*  **update** (*string* $table, *array* $fields, *array* $values, [*string* $whereCondition], [*array* $dataTypes]) inherited from Phalcon\\Db\\Adapter

Updates data on a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Updating existing robot
     $success = $connection->update(
         "robots",
         array("name"),
         array("New Astro Boy"),
         "id = 101"
     );
    
     //Next SQL sentence is sent to the database system
     UPDATE `robots` SET `name` = "Astro boy" WHERE id = 101




public *boolean*  **delete** (*string* $table, [*string* $whereCondition], [*array* $placeholders], [*array* $dataTypes]) inherited from Phalcon\\Db\\Adapter

Deletes data from a table using custom RBDM SQL syntax 

.. code-block:: php

    <?php

     //Deleting existing robot
     $success = $connection->delete(
         "robots",
         "id = 101"
     );
    
     //Next SQL sentence is generated
     DELETE FROM `robots` WHERE `id` = 101




public *string*  **getColumnList** (*array* $columnList) inherited from Phalcon\\Db\\Adapter

Gets a list of columns



public *string*  **limit** (*string* $sqlQuery, *int* $number) inherited from Phalcon\\Db\\Adapter

Appends a LIMIT clause to $sqlQuery argument 

.. code-block:: php

    <?php

     	echo $connection->limit("SELECT * FROM robots", 5);




public *string*  **tableExists** (*string* $tableName, [*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

Generates SQL checking for the existence of a schema.table 

.. code-block:: php

    <?php

     	var_dump($connection->tableExists("blog", "posts"));




public *string*  **viewExists** (*string* $viewName, [*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

Generates SQL checking for the existence of a schema.view 

.. code-block:: php

    <?php

     var_dump($connection->viewExists("active_users", "posts"));




public *string*  **forUpdate** (*string* $sqlQuery) inherited from Phalcon\\Db\\Adapter

Returns a SQL modified with a FOR UPDATE clause



public *string*  **sharedLock** (*string* $sqlQuery) inherited from Phalcon\\Db\\Adapter

Returns a SQL modified with a LOCK IN SHARE MODE clause



public *boolean*  **createTable** (*string* $tableName, *string* $schemaName, *array* $definition) inherited from Phalcon\\Db\\Adapter

Creates a table



public *boolean*  **dropTable** (*string* $tableName, [*string* $schemaName], [*boolean* $ifExists]) inherited from Phalcon\\Db\\Adapter

Drops a table from a schema/database



public *boolean*  **createView** (*unknown* $viewName, *array* $definition, [*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

Creates a view



public *boolean*  **dropView** (*string* $viewName, [*string* $schemaName], [*boolean* $ifExists]) inherited from Phalcon\\Db\\Adapter

Drops a view



public *boolean*  **addColumn** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\ColumnInterface <Phalcon_Db_ColumnInterface>` $column) inherited from Phalcon\\Db\\Adapter

Adds a column to a table



public *boolean*  **modifyColumn** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\ColumnInterface <Phalcon_Db_ColumnInterface>` $column) inherited from Phalcon\\Db\\Adapter

Modifies a table column based on a definition



public *boolean*  **dropColumn** (*string* $tableName, *string* $schemaName, *string* $columnName) inherited from Phalcon\\Db\\Adapter

Drops a column from a table



public *boolean*  **addIndex** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\IndexInterface <Phalcon_Db_IndexInterface>` $index) inherited from Phalcon\\Db\\Adapter

Adds an index to a table



public *boolean*  **dropIndex** (*string* $tableName, *string* $schemaName, *string* $indexName) inherited from Phalcon\\Db\\Adapter

Drop an index from a table



public *boolean*  **addPrimaryKey** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\IndexInterface <Phalcon_Db_IndexInterface>` $index) inherited from Phalcon\\Db\\Adapter

Adds a primary key to a table



public *boolean*  **dropPrimaryKey** (*string* $tableName, *string* $schemaName) inherited from Phalcon\\Db\\Adapter

Drops a table's primary key



public *boolean true*  **addForeignKey** (*string* $tableName, *string* $schemaName, :doc:`Phalcon\\Db\\ReferenceInterface <Phalcon_Db_ReferenceInterface>` $reference) inherited from Phalcon\\Db\\Adapter

Adds a foreign key to a table



public *boolean true*  **dropForeignKey** (*string* $tableName, *string* $schemaName, *string* $referenceName) inherited from Phalcon\\Db\\Adapter

Drops a foreign key from a table



public *string*  **getColumnDefinition** (:doc:`Phalcon\\Db\\ColumnInterface <Phalcon_Db_ColumnInterface>` $column) inherited from Phalcon\\Db\\Adapter

Returns the SQL column definition from a column



public *array*  **listTables** ([*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

List all tables on a database 

.. code-block:: php

    <?php

     	print_r($connection->listTables("blog"));




public *array*  **listViews** ([*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

List all views on a database 

.. code-block:: php

    <?php

    print_r($connection->listViews("blog")); ?>




public :doc:`Phalcon\\Db\\Index <Phalcon_Db_Index>` [] **describeIndexes** (*string* $table, [*string* $schema]) inherited from Phalcon\\Db\\Adapter

Lists table indexes 

.. code-block:: php

    <?php

    print_r($connection->describeIndexes('robots_parts'));




public :doc:`Phalcon\\Db\\Reference <Phalcon_Db_Reference>` [] **describeReferences** (*string* $table, [*string* $schema]) inherited from Phalcon\\Db\\Adapter

Lists table references 

.. code-block:: php

    <?php

     print_r($connection->describeReferences('robots_parts'));




public *array*  **tableOptions** (*string* $tableName, [*string* $schemaName]) inherited from Phalcon\\Db\\Adapter

Gets creation options from a table 

.. code-block:: php

    <?php

     print_r($connection->tableOptions('robots'));




public *boolean*  **createSavepoint** (*string* $name) inherited from Phalcon\\Db\\Adapter

Creates a new savepoint



public *boolean*  **releaseSavepoint** (*string* $name) inherited from Phalcon\\Db\\Adapter

Releases given savepoint



public *boolean*  **rollbackSavepoint** (*string* $name) inherited from Phalcon\\Db\\Adapter

Rollbacks given savepoint



public :doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>`  **setNestedTransactionsWithSavepoints** (*boolean* $nestedTransactionsWithSavepoints) inherited from Phalcon\\Db\\Adapter

Set if nested transactions should use savepoints



public *boolean*  **isNestedTransactionsWithSavepoints** () inherited from Phalcon\\Db\\Adapter

Returns if nested transactions should use savepoints



public *string*  **getNestedTransactionSavepointName** () inherited from Phalcon\\Db\\Adapter

Returns the savepoint name to use for nested transactions



public :doc:`Phalcon\\Db\\RawValue <Phalcon_Db_RawValue>`  **getDefaultIdValue** () inherited from Phalcon\\Db\\Adapter

Returns the default identity value to be inserted in an identity column 

.. code-block:: php

    <?php

     //Inserting a new robot with a valid default value for the column 'id'
     $success = $connection->insert(
         "robots",
         array($connection->getDefaultIdValue(), "Astro Boy", 1952),
         array("id", "name", "year")
     );




public *boolean*  **supportSequences** () inherited from Phalcon\\Db\\Adapter

Check whether the database system requires a sequence to produce auto-numeric values



public *boolean*  **useExplicitIdValue** () inherited from Phalcon\\Db\\Adapter

Check whether the database system requires an explicit value for identity columns



public *array*  **getDescriptor** () inherited from Phalcon\\Db\\Adapter

Return descriptor used to connect to the active database



public *string*  **getConnectionId** () inherited from Phalcon\\Db\\Adapter

Gets the active connection unique identifier



public *string*  **getSQLStatement** () inherited from Phalcon\\Db\\Adapter

Active SQL statement in the object



public *string*  **getRealSQLStatement** () inherited from Phalcon\\Db\\Adapter

Active SQL statement in the object without replace bound paramters



public *array*  **getSQLVariables** () inherited from Phalcon\\Db\\Adapter

Active SQL statement in the object



public *array*  **getSQLBindTypes** () inherited from Phalcon\\Db\\Adapter

Active SQL statement in the object



public *string*  **getType** () inherited from Phalcon\\Db\\Adapter

Returns type of database system the adapter is used for



public *string*  **getDialectType** () inherited from Phalcon\\Db\\Adapter

Returns the name of the dialect used



abstract public :doc:`Phalcon\\Db\\ColumnInterface <Phalcon_Db_ColumnInterface>` [] **describeColumns** (*string* $table, [*string* $schema]) inherited from Phalcon\\Db\\AdapterInterface

Returns an array of Phalcon\\Db\\Column objects describing a table



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Db_Adapter_Pdo.rst
