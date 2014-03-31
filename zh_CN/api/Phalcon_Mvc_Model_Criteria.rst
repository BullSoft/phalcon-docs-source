<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Model_Criteria.rst
Class **Phalcon\\Mvc\\Model\\Criteria**
=======================================

This class allows to build the array parameter required by Phalcon\\Mvc\\Model::find and Phalcon\\Mvc\\Model::findFirst, using an object-oriented interfase


Methods
---------

public  **__construct** ()

...


public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** ()

Returns the DependencyInjector container



public  **setModelName** (*string* $modelName)

Set a model on which the query will be executed



public *string*  **getModelName** ()

Returns an internal model name on which the criteria will be applied



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **bind** (*string* $bindParams)

Adds the bind parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **where** (*string* $conditions)

Adds the conditions parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **conditions** (*string* $conditions)

Adds the conditions parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **order** (*string* $orderColumns)

Adds the order-by parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **limit** (*unknown* $limit)

Adds the limit parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **forUpdate** (*unknown* $forUpdate)

Adds the "for_update" parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **sharedLock** (*unknown* $sharedLock)

Adds the "shared_lock" parameter to the criteria



public *string*  **getWhere** ()

Returns the conditions parameter in the criteria



public *string*  **getConditions** ()

Returns the conditions parameter in the criteria



public *string*  **getLimit** ()

Returns the limit parameter in the criteria



public *string*  **getOrder** ()

Returns the order parameter in the criteria



public *string*  **getParams** ()

Returns all the parameters defined in the criteria



public static  **fromInput** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector, *string* $modelName, *array* $data)

Builds a Phalcon\\Mvc\\Model\\Criteria based on an input array like $_POST



public :doc:`Phalcon\\Mvc\\Model\\Resultset <Phalcon_Mvc_Model_Resultset>`  **execute** ()

Executes a find using the parameters built with the criteria



=======
Class **Phalcon\\Mvc\\Model\\Criteria**
=======================================

*implements* :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`, :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`

This class allows to build the array parameter required by Phalcon\\Mvc\\Model::find and Phalcon\\Mvc\\Model::findFirst using an object-oriented interface  

.. code-block:: php

    <?php

    $robots = Robots::query()
        ->where("type = :type:")
        ->andWhere("year < 2000")
        ->bind(array("type" => "mechanical"))
        ->order("name")
        ->execute();



Methods
-------

public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ()

Returns the DependencyInjector container



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **setModelName** (*string* $modelName)

Set a model on which the query will be executed



public *string*  **getModelName** ()

Returns an internal model name on which the criteria will be applied



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **bind** (*string* $bindParams)

Sets the bound parameters in the criteria This method replaces all previously set bound parameters



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **bindTypes** (*string* $bindTypes)

Sets the bind types in the criteria This method replaces all previously set bound parameters



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **columns** (*string|array* $columns)

Sets the columns to be queried 

.. code-block:: php

    <?php

    $criteria->columns(array('id', 'name'));




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **join** (*string* $model, [*string* $conditions], [*string* $alias], [*string* $type])

Adds a join to the query 

.. code-block:: php

    <?php

    $criteria->join('Robots');
    $criteria->join('Robots', 'r.id = RobotsParts.robots_id');
    $criteria->join('Robots', 'r.id = RobotsParts.robots_id', 'r');
    $criteria->join('Robots', 'r.id = RobotsParts.robots_id', 'r', 'LEFT');




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **innerJoin** (*string* $model, [*string* $conditions], [*string* $alias])

Adds a INNER join to the query 

.. code-block:: php

    <?php

    $criteria->innerJoin('Robots');
    $criteria->innerJoin('Robots', 'r.id = RobotsParts.robots_id');
    $criteria->innerJoin('Robots', 'r.id = RobotsParts.robots_id', 'r');
    $criteria->innerJoin('Robots', 'r.id = RobotsParts.robots_id', 'r', 'LEFT');




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **leftJoin** (*string* $model, [*string* $conditions], [*string* $alias])

Adds a LEFT join to the query 

.. code-block:: php

    <?php

    $criteria->leftJoin('Robots', 'r.id = RobotsParts.robots_id', 'r');




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **rightJoin** (*string* $model, [*string* $conditions], [*string* $alias])

Adds a RIGHT join to the query 

.. code-block:: php

    <?php

    $criteria->rightJoin('Robots', 'r.id = RobotsParts.robots_id', 'r');




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **where** (*string* $conditions)

Sets the conditions parameter in the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **addWhere** (*string* $conditions, [*array* $bindParams], [*array* $bindTypes])

Appends a condition to the current conditions using an AND operator (deprecated)



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **andWhere** (*string* $conditions, [*array* $bindParams], [*array* $bindTypes])

Appends a condition to the current conditions using an AND operator



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **orWhere** (*string* $conditions, [*array* $bindParams], [*array* $bindTypes])

Appends a condition to the current conditions using an OR operator



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **betweenWhere** (*string* $expr, *mixed* $minimum, *mixed* $maximum)

Appends a BETWEEN condition to the current conditions 

.. code-block:: php

    <?php

    $criteria->betweenWhere('price', 100.25, 200.50);




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **notBetweenWhere** (*string* $expr, *mixed* $minimum, *mixed* $maximum)

Appends a NOT BETWEEN condition to the current conditions 

.. code-block:: php

    <?php

    $criteria->notBetweenWhere('price', 100.25, 200.50);




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **inWhere** (*string* $expr, *array* $values)

Appends an IN condition to the current conditions 

.. code-block:: php

    <?php

    $criteria->inWhere('id', [1, 2, 3]);




public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **notInWhere** (*string* $expr, *array* $values)

Appends a NOT IN condition to the current conditions 

.. code-block:: php

    <?php

    $criteria->notInWhere('id', [1, 2, 3]);




public *Phalcon\\Mvc\\Model\\CriteriaIntreface*  **conditions** (*string* $conditions)

Adds the conditions parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **order** (*string* $orderColumns)

Adds the order-by parameter to the criteria (deprecated)



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **orderBy** (*string* $orderColumns)

Adds the order-by parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **limit** (*int* $limit, [*int* $offset])

Adds the limit parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **forUpdate** ([*boolean* $forUpdate])

Adds the "for_update" parameter to the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **sharedLock** ([*boolean* $sharedLock])

Adds the "shared_lock" parameter to the criteria



public *string*  **getWhere** ()

Returns the conditions parameter in the criteria



public *string|array*  **getColumns** ()

Return the columns to be queried



public *string*  **getConditions** ()

Returns the conditions parameter in the criteria



public *string*  **getLimit** ()

Returns the limit parameter in the criteria



public *string*  **getOrder** ()

Returns the order parameter in the criteria



public *array*  **getParams** ()

Returns all the parameters defined in the criteria



public static :doc:`Phalcon\\Mvc\\Model\\Criteria <Phalcon_Mvc_Model_Criteria>`  **fromInput** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector, *string* $modelName, *array* $data)

Builds a Phalcon\\Mvc\\Model\\Criteria based on an input array like $_POST



public :doc:`Phalcon\\Mvc\\Model\\ResultsetInterface <Phalcon_Mvc_Model_ResultsetInterface>`  **execute** ()

Executes a find using the parameters built with the criteria



public :doc:`Phalcon\\Mvc\\Model\\CriteriaInterface <Phalcon_Mvc_Model_CriteriaInterface>`  **cache** (*unknown* $option)

Sets the cache options in the criteria This method replaces all previously set cache options



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Model_Criteria.rst
