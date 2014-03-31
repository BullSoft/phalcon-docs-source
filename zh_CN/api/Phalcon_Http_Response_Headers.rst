<<<<<<< HEAD:zh_CN/api/Phalcon_Http_Response_Headers.rst
Class **Phalcon\\Http\\Response\\Headers**
==========================================

This class is a bag to manage the response headers


Methods
---------

public  **__construct** ()

...


public  **set** (*string* $name, *string* $value)

Sets a header to be sent at the end of the request



public *string*  **get** (*string* $name)

Gets a header value from the internal bag



public  **setRaw** (*string* $header)

Sets a raw header to be sent at the end of the request



public *boolean*  **send** ()

Sends the headers to the client



public  **reset** ()

Reset set headers



public static :doc:`Phalcon\\Http\\Response\\Headers <Phalcon_Http_Response_Headers>`  **__set_state** (*unknown* $data)

Restore a Phalcon\\Http\\Response\\Headers object



=======
Class **Phalcon\\Http\\Response\\Headers**
==========================================

*implements* :doc:`Phalcon\\Http\\Response\\HeadersInterface <Phalcon_Http_Response_HeadersInterface>`

This class is a bag to manage the response headers


Methods
-------

public  **set** (*string* $name, *string* $value)

Sets a header to be sent at the end of the request



public *string*  **get** (*string* $name)

Gets a header value from the internal bag



public  **setRaw** (*string* $header)

Sets a raw header to be sent at the end of the request



public  **remove** (*unknown* $header_index)

Removes a header to be sent at the end of the request



public *boolean*  **send** ()

Sends the headers to the client



public  **reset** ()

Reset set headers



public *array*  **toArray** ()

Returns the current headers as an array



public static :doc:`Phalcon\\Http\\Response\\Headers <Phalcon_Http_Response_Headers>`  **__set_state** (*array* $data)

Restore a Phalcon\\Http\\Response\\Headers object



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Http_Response_Headers.rst
