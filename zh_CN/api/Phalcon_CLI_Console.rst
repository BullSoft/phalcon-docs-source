<<<<<<< HEAD:zh_CN/api/Phalcon_CLI_Console.rst
Class **Phalcon\\CLI\\Console**
===============================

This component allows to create CLI applications using Phalcon


Methods
---------

public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** ()

Returns the internal dependency injector



public  **setEventsManager** (:doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>` $eventsManager)

Sets the events manager



public :doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>`  **getEventsManager** ()

Returns the internal event manager



public  **registerModules** (*array* $modules)

Register an array of modules present in the console 

.. code-block:: php

    <?php

    $this->registerModules(array(
    	'frontend' => array(
    		'className' => 'Multiple\Frontend\Module',
    		'path' => '../apps/frontend/Module.php'
    	),
    	'backend' => array(
    		'className' => 'Multiple\Backend\Module',
    		'path' => '../apps/backend/Module.php'
    	)
    ));




public  **addModules** (*array* $modules)

Merge modules with the existing ones



public *array*  **getModules** ()

Return the modules registered in the console



public *mixed*  **handle** (*array* $arguments)

Handle the whole command-line tasks



=======
Class **Phalcon\\CLI\\Console**
===============================

*implements* :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`, :doc:`Phalcon\\Events\\EventsAwareInterface <Phalcon_Events_EventsAwareInterface>`

This component allows to create CLI applications using Phalcon


Methods
-------

public  **__construct** ([*unknown* $dependencyInjector])

Phalcon\\CLI\\Console constructor



public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ()

Returns the internal dependency injector



public  **setEventsManager** (:doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>` $eventsManager)

Sets the events manager



public :doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>`  **getEventsManager** ()

Returns the internal event manager



public  **registerModules** (*array* $modules)

Register an array of modules present in the console 

.. code-block:: php

    <?php

    $application->registerModules(array(
    	'frontend' => array(
    		'className' => 'Multiple\Frontend\Module',
    		'path' => '../apps/frontend/Module.php'
    	),
    	'backend' => array(
    		'className' => 'Multiple\Backend\Module',
    		'path' => '../apps/backend/Module.php'
    	)
    ));




public  **addModules** (*array* $modules)

Merge modules with the existing ones 

.. code-block:: php

    <?php

    $application->addModules(array(
    	'admin' => array(
    		'className' => 'Multiple\Admin\Module',
    		'path' => '../apps/admin/Module.php'
    	)
    ));




public *array*  **getModules** ()

Return the modules registered in the console



public *mixed*  **handle** ([*array* $arguments])

Handle the command-line arguments. 

.. code-block:: php

    <?php

     	$arguments = array(
     		'task' => 'taskname',
     		'action' => 'action',
     		'params' => array('parameter1', 'parameter2')
     	);
     	$console->handle($arguments);




>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_CLI_Console.rst
