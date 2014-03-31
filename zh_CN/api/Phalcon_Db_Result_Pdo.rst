<<<<<<< HEAD:zh_CN/api/Phalcon_Db_Result_Pdo.rst
Class **Phalcon\\Db\\Result\\Pdo**
==================================

Encapsulates the resultset internals 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    while($robot = $result->fetchArray()){
    	print_r($robot);
    }



Methods
---------

public  **__construct** (:doc:`Phalcon\\Db\\Adapter\\Pdo <Phalcon_Db_Adapter_Pdo>` $connection, *PDOStatement* $result, *string* $sqlStatement, *array* $placeholders)

Phalcon\\Db\\Result\\Pdo constructor



public *boolean*  **execute** ()

Allows to executes the statement again. Some database systems don't support scrollable cursors, So, as cursors are forward only, we need to execute the cursor again to fetch rows from the begining



public *boolean*  **fetchArray** ()

Returns an array of strings that corresponds to the fetched row, or FALSE if there are no more rows. This method is affected by the active fetch flag set using Phalcon\\Db\\Result\\Pdo::setFetchMode 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    while($robot = $result->fetchArray()){
    	print_r($robot);
    }




public *array*  **fetchAll** ()

Returns an array of arrays containing all the records in the result This method is affected by the active fetch flag set using Phalcon\\Db\\Result\\Pdo::setFetchMode 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $robots = $result->fetchAll();




public *int*  **numRows** ()

Gets number of rows returned by a resulset 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    echo 'There are ', $result->numRows(), ' rows in the resulset';




public  **dataSeek** (*int* $number)

Moves internal resulset cursor to another position letting us to fetch a certain row 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->dataSeek(2); // Move to third row on result
    $row = $result->fetchArray(); // Fetch third row




public  **setFetchMode** (*int* $fetchMode)

Changes the fetching mode affecting Phalcon\\Db\\Result\\Pdo::fetchArray 

.. code-block:: php

    <?php

    //Return array with integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    
    //Return associative array without integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_ASSOC);
    
    //Return associative array together with integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_BOTH);




public *PDOStatement*  **getInternalResult** ()

Gets the internal PDO result object



=======
Class **Phalcon\\Db\\Result\\Pdo**
==================================

Encapsulates the resultset internals  

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    while ($robot = $result->fetchArray()) {
    	print_r($robot);
    }



Methods
-------

public  **__construct** (:doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>` $connection, *\PDOStatement* $result, [*string* $sqlStatement], [*array* $bindParams], [*array* $bindTypes])

Phalcon\\Db\\Result\\Pdo constructor



public *boolean*  **execute** ()

Allows to executes the statement again. Some database systems don't support scrollable cursors, So, as cursors are forward only, we need to execute the cursor again to fetch rows from the begining



public *mixed*  **fetch** ()

Fetches an array/object of strings that corresponds to the fetched row, or FALSE if there are no more rows. This method is affected by the active fetch flag set using Phalcon\\Db\\Result\\Pdo::setFetchMode 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->setFetchMode(Phalcon\Db::FETCH_OBJ);
    while ($robot = $result->fetch()) {
    	echo $robot->name;
    }




public *mixed*  **fetchArray** ()

Returns an array of strings that corresponds to the fetched row, or FALSE if there are no more rows. This method is affected by the active fetch flag set using Phalcon\\Db\\Result\\Pdo::setFetchMode 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    while ($robot = $result->fetchArray()) {
    	print_r($robot);
    }




public *array*  **fetchAll** ()

Returns an array of arrays containing all the records in the result This method is affected by the active fetch flag set using Phalcon\\Db\\Result\\Pdo::setFetchMode 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $robots = $result->fetchAll();




public *int*  **numRows** ()

Gets number of rows returned by a resulset 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    echo 'There are ', $result->numRows(), ' rows in the resulset';




public  **dataSeek** (*int* $number)

Moves internal resulset cursor to another position letting us to fetch a certain row 

.. code-block:: php

    <?php

    $result = $connection->query("SELECT * FROM robots ORDER BY name");
    $result->dataSeek(2); // Move to third row on result
    $row = $result->fetch(); // Fetch third row




public  **setFetchMode** (*int* $fetchMode)

Changes the fetching mode affecting Phalcon\\Db\\Result\\Pdo::fetch() 

.. code-block:: php

    <?php

    //Return array with integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_NUM);
    
    //Return associative array without integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_ASSOC);
    
    //Return associative array together with integer indexes
    $result->setFetchMode(Phalcon\Db::FETCH_BOTH);
    
    //Return an object
    $result->setFetchMode(Phalcon\Db::FETCH_OBJ);




public *\PDOStatement*  **getInternalResult** ()

Gets the internal PDO result object



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Db_Result_Pdo.rst
