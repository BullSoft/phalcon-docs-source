<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Controller.rst
Class **Phalcon\\Mvc\\Controller**
==================================

*extends* :doc:`Phalcon\\DI\\Injectable <Phalcon_DI_Injectable>`

Every application controller should extend this class that encapsulates all the controller functionality The controllers provide the “flow” between models and views. Controllers are responsible for processing the incoming requests from the web browser, interrogating the models for data, and passing that data on to the views for presentation. 

.. code-block:: php

    <?php

    class PeopleController extends \Phalcon\Mvc\Controller
    {
    
      //This action will be executed by default
      public function indexAction()
      {
    
      }
    
      public function findAction()
      {
    
      }
    
      public function saveAction()
      {
       //Forwards flow to the index action
       return $this->dispatcher->forward(array('controller' => 'people', 'action' => 'index'));
      }
    
      //This action will be executed when a non existent action is requested
      public function notFoundAction()
      {
    
      }
    
    }



Methods
---------

final public  **__construct** ()

Phalcon\\Mvc\\Controller constructor



public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector) inherited from Phalcon\\DI\\Injectable

Sets the dependency injector



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** () inherited from Phalcon\\DI\\Injectable

Returns the internal dependency injector



public  **setEventsManager** (:doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>` $eventsManager) inherited from Phalcon\\DI\\Injectable

Sets the event manager



public :doc:`Phalcon\\Events\\Manager <Phalcon_Events_Manager>`  **getEventsManager** () inherited from Phalcon\\DI\\Injectable

Returns the internal event manager



public  **__get** (*string* $propertyName) inherited from Phalcon\\DI\\Injectable

Magic method __get



=======
Abstract class **Phalcon\\Mvc\\Controller**
===========================================

*extends* abstract class :doc:`Phalcon\\DI\\Injectable <Phalcon_DI_Injectable>`

*implements* :doc:`Phalcon\\Events\\EventsAwareInterface <Phalcon_Events_EventsAwareInterface>`, :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`, :doc:`Phalcon\\Mvc\\ControllerInterface <Phalcon_Mvc_ControllerInterface>`

Every application controller should extend this class that encapsulates all the controller functionality  The controllers provide the “flow” between models and views. Controllers are responsible for processing the incoming requests from the web browser, interrogating the models for data, and passing that data on to the views for presentation.  

.. code-block:: php

    <?php

    class PeopleController extends \Phalcon\Mvc\Controller
    {
    
      //This action will be executed by default
      public function indexAction()
      {
    
      }
    
      public function findAction()
      {
    
      }
    
      public function saveAction()
      {
       //Forwards flow to the index action
       return $this->dispatcher->forward(array('controller' => 'people', 'action' => 'index'));
      }
    
    }



Methods
-------

final public  **__construct** ()

Phalcon\\Mvc\\Controller constructor



public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector) inherited from Phalcon\\DI\\Injectable

Sets the dependency injector



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** () inherited from Phalcon\\DI\\Injectable

Returns the internal dependency injector



public  **setEventsManager** (:doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>` $eventsManager) inherited from Phalcon\\DI\\Injectable

Sets the event manager



public :doc:`Phalcon\\Events\\ManagerInterface <Phalcon_Events_ManagerInterface>`  **getEventsManager** () inherited from Phalcon\\DI\\Injectable

Returns the internal event manager



public  **__get** (*unknown* $property) inherited from Phalcon\\DI\\Injectable

Magic method __get



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Controller.rst
