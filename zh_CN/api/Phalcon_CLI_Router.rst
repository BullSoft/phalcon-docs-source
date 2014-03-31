<<<<<<< HEAD:zh_CN/api/Phalcon_CLI_Router.rst
Class **Phalcon\\CLI\\Router**
==============================

Phalcon\\CLI\\Router is the standard framework router. Routing is the process of taking a command-line arguments and decomposing it into parameters to determine which module, task, and action of that task should receive the request   

.. code-block:: php

    <?php

    $router = new Phalcon\CLI\Router();
    $router->handle(array());
    echo $router->getTaskName();



Methods
---------

public  **__construct** ()

...


public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector)

Sets the dependency injector



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** ()

Returns the internal dependency injector



public  **setDefaultModule** (*string* $moduleName)

Sets the name of the default module



public  **setDefaultTask** (*unknown* $taskName)

Sets the default controller name



public  **setDefaultAction** (*string* $actionName)

Sets the default action name



public  **handle** ()

Handles routing information received from command-line arguments



public *string*  **getModuleName** ()

Returns proccesed module name



public *string*  **getTaskName** ()

Returns proccesed task name



public *string*  **getActionName** ()

Returns proccesed action name



public *array*  **getParams** ()

Returns proccesed extra params



=======
Class **Phalcon\\CLI\\Router**
==============================

*implements* :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`

Phalcon\\CLI\\Router is the standard framework router. Routing is the process of taking a command-line arguments and decomposing it into parameters to determine which module, task, and action of that task should receive the request    

.. code-block:: php

    <?php

    $router = new Phalcon\CLI\Router();
    $router->handle(array(
    	'module' => 'main',
    	'task' => 'videos',
    	'action' => 'process'
    ));
    echo $router->getTaskName();



Methods
-------

public  **__construct** ()

Phalcon\\CLI\\Router constructor



public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector)

Sets the dependency injector



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ()

Returns the internal dependency injector



public  **setDefaultModule** (*string* $moduleName)

Sets the name of the default module



public  **setDefaultTask** (*string* $taskName)

Sets the default controller name



public  **setDefaultAction** (*string* $actionName)

Sets the default action name



public  **handle** ([*array* $arguments])

Handles routing information received from command-line arguments



public *string*  **getModuleName** ()

Returns proccesed module name



public *string*  **getTaskName** ()

Returns proccesed task name



public *string*  **getActionName** ()

Returns proccesed action name



public *array*  **getParams** ()

Returns proccesed extra params



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_CLI_Router.rst
