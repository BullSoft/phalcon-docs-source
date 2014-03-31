<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Model_Transaction_Failed.rst
Class **Phalcon\\Mvc\\Model\\Transaction\\Failed**
==================================================

*extends* Exception

Phalcon\\Mvc\\Model\\Transaction\\Failed will be thrown to exit a try/catch block for transactions


Methods
---------

public  **__construct** (*string* $message, :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>` $record)

Phalcon\\Mvc\\Model\\Transaction\\Failed constructor



public *string*  **getRecordMessages** ()

Returns validation record messages which stop the transaction



public :doc:`Phalcon\\Mvc\\Model <Phalcon_Mvc_Model>`  **getRecord** ()

Returns validation record messages which stop the transaction



final private *Exception*  **__clone** () inherited from Exception

Clone the exception



final public *string*  **getMessage** () inherited from Exception

Gets the Exception message



final public *int*  **getCode** () inherited from Exception

Gets the Exception code



final public *string*  **getFile** () inherited from Exception

Gets the file in which the exception occurred



final public *int*  **getLine** () inherited from Exception

Gets the line in which the exception occurred



final public *array*  **getTrace** () inherited from Exception

Gets the stack trace



final public *Exception*  **getPrevious** () inherited from Exception

Returns previous Exception



final public *Exception*  **getTraceAsString** () inherited from Exception

Gets the stack trace as a string



public *string*  **__toString** () inherited from Exception

String representation of the exception



=======
Class **Phalcon\\Mvc\\Model\\Transaction\\Failed**
==================================================

*extends* class :doc:`Phalcon\\Mvc\\Model\\Transaction\\Exception <Phalcon_Mvc_Model_Transaction_Exception>`

This class will be thrown to exit a try/catch block for isolated transactions


Methods
-------

public  **__construct** (*string* $message, :doc:`Phalcon\\Mvc\\ModelInterface <Phalcon_Mvc_ModelInterface>` $record)

Phalcon\\Mvc\\Model\\Transaction\\Failed constructor



public :doc:`Phalcon\\Mvc\\Model\\MessageInterface <Phalcon_Mvc_Model_MessageInterface>` [] **getRecordMessages** ()

Returns validation record messages which stop the transaction



public :doc:`Phalcon\\Mvc\\ModelInterface <Phalcon_Mvc_ModelInterface>`  **getRecord** ()

Returns validation record messages which stop the transaction



final private *Exception*  **__clone** () inherited from Exception

Clone the exception



final public *string*  **getMessage** () inherited from Exception

Gets the Exception message



final public *int*  **getCode** () inherited from Exception

Gets the Exception code



final public *string*  **getFile** () inherited from Exception

Gets the file in which the exception occurred



final public *int*  **getLine** () inherited from Exception

Gets the line in which the exception occurred



final public *array*  **getTrace** () inherited from Exception

Gets the stack trace



final public *Exception*  **getPrevious** () inherited from Exception

Returns previous Exception



final public *Exception*  **getTraceAsString** () inherited from Exception

Gets the stack trace as a string



public *string*  **__toString** () inherited from Exception

String representation of the exception



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Model_Transaction_Failed.rst
