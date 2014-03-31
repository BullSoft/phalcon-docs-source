<<<<<<< HEAD:zh_CN/api/Phalcon_Paginator_Adapter_NativeArray.rst
Class **Phalcon\\Paginator\\Adapter\\NativeArray**
==================================================

Component of pagination by array data


Methods
---------

public  **__construct** (*array* $config)

Phalcon\\Paginator\\Adapter\\NativeArray constructor



public  **setCurrentPage** (*int* $page)

Set the current page number



public *stdClass*  **getPaginate** ()

Returns a slice of the resultset to show in the pagination



=======
Class **Phalcon\\Paginator\\Adapter\\NativeArray**
==================================================

*implements* :doc:`Phalcon\\Paginator\\AdapterInterface <Phalcon_Paginator_AdapterInterface>`

Pagination using a PHP array as source of data  

.. code-block:: php

    <?php

    $paginator = new \Phalcon\Paginator\Adapter\Model(
    	array(
    		"data"  => array(
    			array('id' => 1, 'name' => 'Artichoke'),
    			array('id' => 2, 'name' => 'Carrots'),
    			array('id' => 3, 'name' => 'Beet'),
    			array('id' => 4, 'name' => 'Lettuce'),
    			array('id' => 5, 'name' => '')
    		),
    		"limit" => 2,
    		"page"  => $currentPage
    	)
    );



Methods
-------

public  **__construct** (*array* $config)

Phalcon\\Paginator\\Adapter\\NativeArray constructor



public  **setCurrentPage** (*int* $page)

Set the current page number



public *stdClass*  **getPaginate** ()

Returns a slice of the resultset to show in the pagination



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:es/api/Phalcon_Paginator_Adapter_NativeArray.rst
