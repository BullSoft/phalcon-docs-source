<<<<<<< HEAD:zh_CN/api/Phalcon_Db_Reference.rst
Class **Phalcon\\Db\\Reference**
================================

Allows to define reference constraints on tables 

.. code-block:: php

    <?php

    $reference = new Phalcon\Db\Reference("field_fk", array(
    	'referencedSchema' => "invoicing",
    	'referencedTable' => "products",
    	'columns' => array("product_type", "product_code"),
    	'referencedColumns' => array("type", "code")
    ));



Methods
---------

public  **__construct** (*string* $referenceName, *array* $definition)

Phalcon\\Db\\Reference constructor



public *string*  **getName** ()

Gets the index name



public *string*  **getSchemaName** ()

Gets the schema where referenced table is



public *string*  **getReferencedSchema** ()

Gets the schema where referenced table is



public *array*  **getColumns** ()

Gets local columns which reference is based



public *string*  **getReferencedTable** ()

Gets the referenced table



public *array*  **getReferencedColumns** ()

Gets referenced columns



public static :doc:`Phalcon\\Db\\Reference <Phalcon_Db_Reference>`  **__set_state** (*array* $data)

Restore a Phalcon\\Db\\Reference object from export



=======
Class **Phalcon\\Db\\Reference**
================================

*implements* :doc:`Phalcon\\Db\\ReferenceInterface <Phalcon_Db_ReferenceInterface>`

Allows to define reference constraints on tables  

.. code-block:: php

    <?php

    $reference = new Phalcon\Db\Reference("field_fk", array(
    	'referencedSchema' => "invoicing",
    	'referencedTable' => "products",
    	'columns' => array("product_type", "product_code"),
    	'referencedColumns' => array("type", "code")
    ));



Methods
-------

public  **__construct** (*string* $referenceName, *array* $definition)

Phalcon\\Db\\Reference constructor



public *string*  **getName** ()

Gets the index name



public *string*  **getSchemaName** ()

Gets the schema where referenced table is



public *string*  **getReferencedSchema** ()

Gets the schema where referenced table is



public *array*  **getColumns** ()

Gets local columns which reference is based



public *string*  **getReferencedTable** ()

Gets the referenced table



public *array*  **getReferencedColumns** ()

Gets referenced columns



public static :doc:`Phalcon\\Db\\Reference <Phalcon_Db_Reference>`  **__set_state** ([*unknown* $properties])

Restore a Phalcon\\Db\\Reference object from export



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Db_Reference.rst
