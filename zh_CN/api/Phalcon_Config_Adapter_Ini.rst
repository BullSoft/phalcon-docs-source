<<<<<<< HEAD:zh_CN/api/Phalcon_Config_Adapter_Ini.rst
Class **Phalcon\\Config\\Adapter\\Ini**
=======================================

*extends* :doc:`Phalcon\\Config <Phalcon_Config>`

Reads ini files and convert it to Phalcon\\Config objects. Given the next configuration file: 

.. code-block:: ini

    <?php

    [database]
    adapter = Mysql
    host = localhost
    username = scott
    password = cheetah
    name = test_db
    
    [phalcon]
    controllersDir = "../app/controllers/"
    modelsDir = "../app/models/"
    viewsDir = "../app/views/"

You can read it as follows: 

.. code-block:: php

    <?php

    $config = new Phalcon\Config\Adapter\Ini("path/config.ini");
    echo $config->phalcon->controllersDir;
    echo $config->database->username;



Methods
---------

public :doc:`Phalcon\\Config\\Adapter\\Ini <Phalcon_Config_Adapter_Ini>`  **__construct** (*string* $filePath)

Phalcon\\Config\\Adapter\\Ini constructor



=======
Class **Phalcon\\Config\\Adapter\\Ini**
=======================================

*extends* class :doc:`Phalcon\\Config <Phalcon_Config>`

*implements* Countable, ArrayAccess

Reads ini files and converts them to Phalcon\\Config objects.  Given the next configuration file:  

.. code-block:: ini

    <?php

    [database]
    adapter = Mysql
    host = localhost
    username = scott
    password = cheetah
    dbname = test_db
    
    [phalcon]
    controllersDir = "../app/controllers/"
    modelsDir = "../app/models/"
    viewsDir = "../app/views/"

  You can read it as follows:  

.. code-block:: php

    <?php

    $config = new Phalcon\Config\Adapter\Ini("path/config.ini");
    echo $config->phalcon->controllersDir;
    echo $config->database->username;



Methods
-------

public  **__construct** (*string* $filePath)

Phalcon\\Config\\Adapter\\Ini constructor



public *boolean*  **offsetExists** (*unknown* $property) inherited from Phalcon\\Config

Allows to check whether an attribute is defined using the array-syntax 

.. code-block:: php

    <?php

     var_dump(isset($config['database']));




public *mixed*  **get** (*string* $index, [*mixed* $defaultValue]) inherited from Phalcon\\Config

Gets an attribute from the configuration, if the attribute isn't defined returns null If the value is exactly null or is not defined the default value will be used instead 

.. code-block:: php

    <?php

     echo $config->get('controllersDir', '../app/controllers/');




public *string*  **offsetGet** (*unknown* $property) inherited from Phalcon\\Config

Gets an attribute using the array-syntax 

.. code-block:: php

    <?php

     print_r($config['database']);




public  **offsetSet** (*unknown* $property, *mixed* $value) inherited from Phalcon\\Config

Sets an attribute using the array-syntax 

.. code-block:: php

    <?php

     $config['database'] = array('type' => 'Sqlite');




public  **offsetUnset** (*unknown* $property) inherited from Phalcon\\Config

Unsets an attribute using the array-syntax 

.. code-block:: php

    <?php

     unset($config['database']);




public  **merge** (:doc:`Phalcon\\Config <Phalcon_Config>` $config) inherited from Phalcon\\Config

Merges a configuration into the current one 

.. code-block:: php

    <?php

    $appConfig = new Phalcon\Config(array('database' => array('host' => 'localhost')));
    $globalConfig->merge($config2);




public *array*  **toArray** () inherited from Phalcon\\Config

Converts recursively the object to an array 

.. code-block:: php

    <?php

    print_r($config->toArray());




public  **count** () inherited from Phalcon\\Config

...


public  **__wakeup** () inherited from Phalcon\\Config

...


public static :doc:`Phalcon\\Config <Phalcon_Config>`  **__set_state** ([*unknown* $properties]) inherited from Phalcon\\Config

Restores the state of a Phalcon\\Config object



public  **__get** (*unknown* $property) inherited from Phalcon\\Config

...


public  **__set** (*unknown* $property, *unknown* $value) inherited from Phalcon\\Config

...


public  **__isset** (*unknown* $property) inherited from Phalcon\\Config

...


public  **__unset** (*unknown* $property) inherited from Phalcon\\Config

...


>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Config_Adapter_Ini.rst
