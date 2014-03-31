<<<<<<< HEAD:zh_CN/api/Phalcon_Translate.rst
Class **Phalcon\\Translate**
============================

*implements* ArrayAccess

Translate component allows the creation of multi-language applications using different adapters for translation lists.


Methods
---------

public *string*  **_** (*string* $translateKey, *array* $placeholders)

Returns the translation string of the given key



public  **offsetSet** (*string* $offset, *string* $value)

Sets a translation value



public *boolean*  **offsetExists** (*string* $translateKey)

Check whether a translation key exists



public  **offsetUnset** (*string* $offset)

Elimina un indice del diccionario



public *string*  **offsetGet** (*string* $traslateKey)

Returns the translation related to the given key



=======
Abstract class **Phalcon\\Translate\\Adapter**
==============================================

*implements* ArrayAccess, :doc:`Phalcon\\Translate\\AdapterInterface <Phalcon_Translate_AdapterInterface>`

Base class for Phalcon\\Translate adapters


Methods
-------

public  **__construct** ()

Class constructore



public *string*  **_** (*string* $translateKey, [*array* $placeholders])

Returns the translation string of the given key



public  **offsetSet** (*unknown* $property, *string* $value)

Sets a translation value



public *boolean*  **offsetExists** (*unknown* $property)

Check whether a translation key exists



public  **offsetUnset** (*unknown* $property)

Unsets a translation from the dictionary



public *string*  **offsetGet** (*unknown* $property)

Returns the translation related to the given key



abstract public *string*  **query** (*string* $index, [*array* $placeholders]) inherited from Phalcon\\Translate\\AdapterInterface

Returns the translation related to the given key



abstract public *bool*  **exists** (*string* $index) inherited from Phalcon\\Translate\\AdapterInterface

Check whether is defined a translation key in the internal array



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Translate_Adapter.rst
