<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Model_Transaction.rst
Class **Phalcon\\Mvc\\Model\\Transaction**
==========================================

Transactions are protective blocks where SQL statements are only permanent if they can all succeed as one atomic action. Phalcon\\Transaction is intended to be used with Phalcon_Model_Base. Phalcon Transactions should be created using Phalcon\\Transaction\\Manager. 

.. code-block:: php

    <?php

    try {
    
      $transaction = Phalcon\Mvc\Model\Transaction\Manager::get();
    
      $robot = new Robots();
      $robot->setTransaction($transaction);
      $robot->name = 'WALL·E';
      $robot->created_at = date('Y-m-d');
      if($robot->save()==false){
        $transaction->rollback("Can't save robot");
      }
    
      $robotPart = new RobotParts();
      $robotPart->setTransaction($transaction);
      $robotPart->type = 'head';
      if($robotPart->save()==false){
        $transaction->rollback("Can't save robot part");
      }
    
      $transaction->commit();
    
    }
    catch(Phalcon\Mvc\Model\Transaction\Failed $e){
      echo 'Failed, reason: ', $e->getMessage();
    }



Methods
---------

public  **__construct** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector, *boolean* $autoBegin)

Phalcon\\Mvc\\Model\\Transaction constructor



public  **setTransactionManager** (:doc:`Phalcon\\Mvc\\Model\\Transaction\\Manager <Phalcon_Mvc_Model_Transaction_Manager>` $manager)

Sets transaction manager related to the transaction



public *boolean*  **begin** ()

Starts the transaction



public *boolean*  **commit** ()

Commits the transaction



public *boolean*  **rollback** (*string* $rollbackMessage, :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $rollbackRecord)

Rollbacks the transaction



public *string*  **getConnection** ()

Returns connection related to transaction



public  **setIsNewTransaction** (*boolean* $isNew)

Sets if is a reused transaction or new once



public  **setRollbackOnAbort** (*boolean* $rollbackOnAbort)

Sets flag to rollback on abort the HTTP connection



public *boolean*  **isManaged** ()

Checks whether transaction is managed by a transaction manager



public *array*  **getMessages** ()

Returns validations messages from last save try



public *boolean*  **isValid** ()

Checks whether internal connection is under an active transaction



public  **setRollbackedRecord** (:doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $record)

Sets object which generates rollback action



=======
Class **Phalcon\\Mvc\\Model\\Transaction**
==========================================

*implements* :doc:`Phalcon\\Mvc\\Model\\TransactionInterface <Phalcon_Mvc_Model_TransactionInterface>`

Transactions are protective blocks where SQL statements are only permanent if they can all succeed as one atomic action. Phalcon\\Transaction is intended to be used with Phalcon_Model_Base. Phalcon Transactions should be created using Phalcon\\Transaction\\Manager.  

.. code-block:: php

    <?php

    try {
    
      $manager = new Phalcon\Mvc\Model\Transaction\Manager();
    
      $transaction = $manager->get();
    
      $robot = new Robots();
      $robot->setTransaction($transaction);
      $robot->name = 'WALL·E';
      $robot->created_at = date('Y-m-d');
      if ($robot->save() == false) {
        $transaction->rollback("Can't save robot");
      }
    
      $robotPart = new RobotParts();
      $robotPart->setTransaction($transaction);
      $robotPart->type = 'head';
      if ($robotPart->save() == false) {
        $transaction->rollback("Can't save robot part");
      }
    
      $transaction->commit();
    
    } catch(Phalcon\Mvc\Model\Transaction\Failed $e) {
      echo 'Failed, reason: ', $e->getMessage();
    }



Methods
-------

public  **__construct** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector, [*boolean* $autoBegin], [*string* $service])

Phalcon\\Mvc\\Model\\Transaction constructor



public  **setTransactionManager** (:doc:`Phalcon\\Mvc\\Model\\Transaction\\ManagerInterface <Phalcon_Mvc_Model_Transaction_ManagerInterface>` $manager)

Sets transaction manager related to the transaction



public *boolean*  **begin** ()

Starts the transaction



public *boolean*  **commit** ()

Commits the transaction



public *boolean*  **rollback** ([*string* $rollbackMessage], [:doc:`Phalcon\\Mvc\\ModelInterface <Phalcon_Mvc_ModelInterface>` $rollbackRecord])

Rolls back the transaction



public :doc:`Phalcon\\Db\\AdapterInterface <Phalcon_Db_AdapterInterface>`  **getConnection** ()

Returns the connection related to transaction



public  **setIsNewTransaction** (*boolean* $isNew)

Sets if is a reused transaction or new once



public  **setRollbackOnAbort** (*boolean* $rollbackOnAbort)

Sets flag to rollback on abort the HTTP connection



public *boolean*  **isManaged** ()

Checks whether transaction is managed by a transaction manager



public *array*  **getMessages** ()

Returns validations messages from last save try



public *boolean*  **isValid** ()

Checks whether internal connection is under an active transaction



public  **setRollbackedRecord** (:doc:`Phalcon\\Mvc\\ModelInterface <Phalcon_Mvc_ModelInterface>` $record)

Sets object which generates rollback action



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Model_Transaction.rst
