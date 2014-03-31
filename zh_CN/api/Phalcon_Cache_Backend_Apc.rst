<<<<<<< HEAD:zh_CN/api/Phalcon_Cache_Backend_Apc.rst
Class **Phalcon\\Cache\\Backend\\Apc**
======================================

*extends* :doc:`Phalcon\\Cache\\Backend <Phalcon_Cache_Backend>`

Allows to cache output fragments, PHP data and raw data using a memcache backend 

.. code-block:: php

    <?php

    //Cache data for 2 days
    $frontendOptions = array(
    	'lifetime' => 172800
    );
    
    //Cache data for 2 days
    $frontCache = new Phalcon\Cache\Frontend\Data(array(
    	'lifetime' => 172800
    ));
    
      $cache = new Phalcon\Cache\Backend\Apc($frontCache);
    
    //Cache arbitrary data
    $cache->store('my-data', array(1, 2, 3, 4, 5));
    
    //Get data
    $data = $cache->get('my-data');



Methods
---------

public *mixed*  **get** (*string* $keyName, *long* $lifetime)

Returns a cached content



public  **save** (*string* $keyName, *string* $content, *long* $lifetime, *boolean* $stopBuffer)

Stores cached content into the file backend



public *boolean*  **delete** (*string* $keyName)

Deletes a value from the cache by its key



public *array*  **queryKeys** (*string* $prefix)

Query the existing cached keys



public  **__construct** (*mixed* $frontendObject, *array* $backendOptions) inherited from Phalcon\\Cache\\Backend

Phalcon\\Cache\\Backend constructor



public *mixed*  **start** (*int|string* $keyName) inherited from Phalcon\\Cache\\Backend

Starts a cache. The $keyname allow to identify the created fragment



public *mixed*  **getFrontend** () inherited from Phalcon\\Cache\\Backend

Returns front-end instance adapter related to the back-end



public *boolean*  **isFresh** () inherited from Phalcon\\Cache\\Backend

Checks whether the last cache is fresh or cached



public *boolean*  **isStarted** () inherited from Phalcon\\Cache\\Backend

Checks whether the cache has started buffering or not



public *string*  **getLastKey** () inherited from Phalcon\\Cache\\Backend

Gets the last key stored by the cache



=======
Class **Phalcon\\Cache\\Backend\\Apc**
======================================

*extends* abstract class :doc:`Phalcon\\Cache\\Backend <Phalcon_Cache_Backend>`

*implements* :doc:`Phalcon\\Cache\\BackendInterface <Phalcon_Cache_BackendInterface>`

Allows to cache output fragments, PHP data and raw data using an APC backend  

.. code-block:: php

    <?php

    //Cache data for 2 days
    $frontCache = new Phalcon\Cache\Frontend\Data(array(
    	'lifetime' => 172800
    ));
    
      $cache = new Phalcon\Cache\Backend\Apc($frontCache, array(
          'prefix' => 'app-data'
      ));
    
    //Cache arbitrary data
    $cache->save('my-data', array(1, 2, 3, 4, 5));
    
    //Get data
    $data = $cache->get('my-data');



Methods
-------

public *mixed*  **get** (*string* $keyName, [*long* $lifetime])

Returns a cached content



public  **save** ([*string* $keyName], [*string* $content], [*long* $lifetime], [*boolean* $stopBuffer])

Stores cached content into the APC backend and stops the frontend



public *boolean*  **delete** (*string* $keyName)

Deletes a value from the cache by its key



public *array*  **queryKeys** ([*string* $prefix])

Query the existing cached keys



public *boolean*  **exists** ([*string* $keyName], [*long* $lifetime])

Checks if cache exists and it hasn't expired



public *mixed*  **increment** ([*unknown* $key_name], [*long* $value])

Increment of a given key, by number $value



public *mixed*  **decrement** ([*unknown* $key_name], [*long* $value])

Decrement of a given key, by number $value



public *boolean*  **flush** ()

Immediately invalidates all existing items.



public  **__construct** (:doc:`Phalcon\\Cache\\FrontendInterface <Phalcon_Cache_FrontendInterface>` $frontend, [*array* $options]) inherited from Phalcon\\Cache\\Backend

Phalcon\\Cache\\Backend constructor



public *mixed*  **start** (*int|string* $keyName, [*long* $lifetime]) inherited from Phalcon\\Cache\\Backend

Starts a cache. The $keyname allows to identify the created fragment



public  **stop** ([*boolean* $stopBuffer]) inherited from Phalcon\\Cache\\Backend

Stops the frontend without store any cached content



public *mixed*  **getFrontend** () inherited from Phalcon\\Cache\\Backend

Returns front-end instance adapter related to the back-end



public *array*  **getOptions** () inherited from Phalcon\\Cache\\Backend

Returns the backend options



public *boolean*  **isFresh** () inherited from Phalcon\\Cache\\Backend

Checks whether the last cache is fresh or cached



public *boolean*  **isStarted** () inherited from Phalcon\\Cache\\Backend

Checks whether the cache has starting buffering or not



public  **setLastKey** (*string* $lastKey) inherited from Phalcon\\Cache\\Backend

Sets the last key used in the cache



public *string*  **getLastKey** () inherited from Phalcon\\Cache\\Backend

Gets the last key stored by the cache



public *int*  **getLifetime** () inherited from Phalcon\\Cache\\Backend

Gets the last lifetime set



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Cache_Backend_Apc.rst
