<<<<<<< HEAD:zh_CN/api/Phalcon_Session.rst
Class **Phalcon\\Session**
==========================

Session client-server persistent state data management. This component allows you to separate your session data between application or modules. With this, it's possible to use the same index to refer a variable but they can be in different applications. 

.. code-block:: php

    <?php

     $session = new Phalcon\Session\Adapter\Files(array(
        'uniqueId' => 'my-private-app'
     ));
    
     $session->start();
    
     $session->set('var', 'some-value');
    
     echo $session->get('var');



Methods
---------

public  **__construct** (*array* $options)

Phalcon\\Session construtor



public  **start** ()

Starts session, optionally using an adapter



public  **setOptions** (*array* $options)

Sets session options



public *array*  **getOptions** ()

Get internal options



public  **get** (*string* $index)

Gets a session variable from an application context



public  **set** (*string* $index, *string* $value)

Sets a session variable in an application context



public  **has** (*string* $index)

Check whether a session variable is set in an application context



public  **remove** (*string* $index)

Removes a session variable from an application context



public *string*  **getId** ()

Returns active session id



public *boolean*  **isStarted** ()

Check whether the session has been started



public *boolean*  **destroy** ()

Destroys the active session



=======
Abstract class **Phalcon\\Session\\Adapter**
============================================

*implements* :doc:`Phalcon\\Session\\AdapterInterface <Phalcon_Session_AdapterInterface>`, Countable, IteratorAggregate, Traversable, ArrayAccess

Base class for Phalcon\\Session adapters


Methods
-------

public  **__construct** ([*array* $options])

Phalcon\\Session\\Adapter constructor



public  **__destruct** ()

...


public *boolean*  **start** ()

Starts the session (if headers are already sent the session will not be started)



public  **setOptions** (*array* $options)

Sets session's options 

.. code-block:: php

    <?php

    $session->setOptions(array(
    	'uniqueId' => 'my-private-app'
    ));




public *array*  **getOptions** ()

Get internal options



public *mixed*  **get** (*string* $index, [*mixed* $defaultValue])

Gets a session variable from an application context



public  **set** (*string* $index, *string* $value)

Sets a session variable in an application context 

.. code-block:: php

    <?php

    $session->set('auth', 'yes');




public *boolean*  **has** (*string* $index)

Check whether a session variable is set in an application context 

.. code-block:: php

    <?php

    var_dump($session->has('auth'));




public  **remove** (*string* $index)

Removes a session variable from an application context 

.. code-block:: php

    <?php

    $session->remove('auth');




public *string*  **getId** ()

Returns active session id 

.. code-block:: php

    <?php

    echo $session->getId();




public *boolean*  **isStarted** ()

Check whether the session has been started 

.. code-block:: php

    <?php

    var_dump($session->isStarted());




public *boolean*  **destroy** ([*unknown* $session_id])

Destroys the active session 

.. code-block:: php

    <?php

    var_dump($session->destroy());




public  **__get** (*unknown* $property)

...


public  **__set** (*unknown* $property, *unknown* $value)

...


public  **__isset** (*unknown* $property)

...


public  **__unset** (*unknown* $property)

...


public  **offsetGet** (*unknown* $property)

...


public  **offsetSet** (*unknown* $property, *unknown* $value)

...


public  **offsetExists** (*unknown* $property)

...


public  **offsetUnset** (*unknown* $property)

...


public  **count** ()

...


public  **getIterator** ()

...


>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Session_Adapter.rst
