<<<<<<< HEAD:zh_CN/api/Phalcon_Mvc_Url.rst
Class **Phalcon\\Mvc\\Url**
===========================

This components aids in the generation of: URIs, URLs and Paths 

.. code-block:: php

    <?php

     //Generate a url appending a uri to the base Uri
     echo $url->get('products/edit/1');
    
     //Generate a url for a predefined route
     echo $url->get(array('for' => 'blog-post', 'title' => 'some-cool-stuff', 'year' => '2012'));



Methods
---------

public  **setDI** (:doc:`Phalcon\\DI <Phalcon_DI>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DI <Phalcon_DI>`  **getDI** ()

Returns the DependencyInjector container



public  **setBaseUri** (*string* $baseUri)

Sets a prefix to all the urls generated 

.. code-block:: php

    <?php

    $url->setBasePath('/invo/');




public *string*  **getBaseUri** ()

Returns the prefix for all the generated urls. By default /



public  **setBasePath** (*string* $basePath)

Sets a base paths for all the generated paths 

.. code-block:: php

    <?php

    $url->setBasePath('/var/www/');




public *string*  **getBasePath** ()

Returns a base path



public *string*  **get** (*string|array* $uri)

Generates a URL



public *string*  **path** (*string* $path)

Generates a local path



=======
Class **Phalcon\\Mvc\\Url**
===========================

*implements* :doc:`Phalcon\\Mvc\\UrlInterface <Phalcon_Mvc_UrlInterface>`, :doc:`Phalcon\\DI\\InjectionAwareInterface <Phalcon_DI_InjectionAwareInterface>`

This components aids in the generation of: URIs, URLs and Paths  

.. code-block:: php

    <?php

     //Generate a URL appending the URI to the base URI
     echo $url->get('products/edit/1');
    
     //Generate a URL for a predefined route
     echo $url->get(array('for' => 'blog-post', 'title' => 'some-cool-stuff', 'year' => '2012'));



Methods
-------

public  **setDI** (:doc:`Phalcon\\DiInterface <Phalcon_DiInterface>` $dependencyInjector)

Sets the DependencyInjector container



public :doc:`Phalcon\\DiInterface <Phalcon_DiInterface>`  **getDI** ()

Returns the DependencyInjector container



public :doc:`Phalcon\\Mvc\\Url <Phalcon_Mvc_Url>`  **setBaseUri** (*string* $baseUri)

Sets a prefix for all the URIs to be generated 

.. code-block:: php

    <?php

    $url->setBaseUri('/invo/');
    $url->setBaseUri('/invo/index.php/');




public :doc:`Phalcon\\Mvc\\Url <Phalcon_Mvc_Url>`  **setStaticBaseUri** (*string* $staticBaseUri)

Sets a prefix for all static URLs generated 

.. code-block:: php

    <?php

    $url->setStaticBaseUri('/invo/');




public *string*  **getBaseUri** ()

Returns the prefix for all the generated urls. By default /



public *string*  **getStaticBaseUri** ()

Returns the prefix for all the generated static urls. By default /



public :doc:`Phalcon\\Mvc\\Url <Phalcon_Mvc_Url>`  **setBasePath** (*string* $basePath)

Sets a base path for all the generated paths 

.. code-block:: php

    <?php

    $url->setBasePath('/var/www/htdocs/');




public *string*  **getBasePath** ()

Returns the base path



public *string*  **get** ([*string|array* $uri], [*unknown* $args])

Generates a URL 

.. code-block:: php

    <?php

     //Generate a URL appending the URI to the base URI
     echo $url->get('products/edit/1');
    
     //Generate a URL for a predefined route
     echo $url->get(array('for' => 'blog-post', 'title' => 'some-cool-stuff', 'year' => '2012'));




public *string*  **getStatic** ([*string|array* $uri])

Generates a URL for a static resource



public *string*  **path** ([*string* $path])

Generates a local path



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Mvc_Url.rst
