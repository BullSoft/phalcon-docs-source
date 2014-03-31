<<<<<<< HEAD:zh_CN/api/Phalcon_Translate_Adapter_NativeArray.rst
Class **Phalcon\\Translate\\Adapter\\NativeArray**
==================================================

*extends* :doc:`Phalcon\\Translate <Phalcon_Translate>`

*implements* ArrayAccess

Allows to define translation lists using PHP arrays


Methods
---------

public  **__construct** (*unknown* $options)

Phalcon\\Translate\\Adapter\\NativeArray constructor



public *string*  **query** (*string* $index, *array* $placeholders)

Returns the translation related to the given key



public *bool*  **exists** (*string* $index)

Check whether is defined a translation key in the internal array



public *string*  **_** (*string* $translateKey, *array* $placeholders) inherited from Phalcon\\Translate

Returns the translation string of the given key



public  **offsetSet** (*string* $offset, *string* $value) inherited from Phalcon\\Translate

Sets a translation value



public *boolean*  **offsetExists** (*string* $translateKey) inherited from Phalcon\\Translate

Check whether a translation key exists



public  **offsetUnset** (*string* $offset) inherited from Phalcon\\Translate

Elimina un indice del diccionario



public *string*  **offsetGet** (*string* $traslateKey) inherited from Phalcon\\Translate

Returns the translation related to the given key



=======
Class **Phalcon\\Translate\\Adapter\\NativeArray**
==================================================

*extends* abstract class :doc:`Phalcon\\Translate\\Adapter <Phalcon_Translate_Adapter>`

*implements* :doc:`Phalcon\\Translate\\AdapterInterface <Phalcon_Translate_AdapterInterface>`, ArrayAccess

Allows to define translation lists using PHP arrays


Methods
-------

public  **__construct** (*array* $options)

Phalcon\\Translate\\Adapter\\NativeArray constructor



public *string*  **query** (*string* $index, [*array* $placeholders])

Returns the translation related to the given key



public *bool*  **exists** (*string* $index)

Check whether is defined a translation key in the internal array



public *string*  **_** (*string* $translateKey, [*array* $placeholders]) inherited from Phalcon\\Translate\\Adapter

Returns the translation string of the given key



public  **offsetSet** (*unknown* $property, *string* $value) inherited from Phalcon\\Translate\\Adapter

Sets a translation value



public *boolean*  **offsetExists** (*unknown* $property) inherited from Phalcon\\Translate\\Adapter

Check whether a translation key exists



public  **offsetUnset** (*unknown* $property) inherited from Phalcon\\Translate\\Adapter

Unsets a translation from the dictionary



public *string*  **offsetGet** (*unknown* $property) inherited from Phalcon\\Translate\\Adapter

Returns the translation related to the given key



>>>>>>> e49c0b4730fea45ee3885b7b7b6cbc894b8de0d4:zh/api/Phalcon_Translate_Adapter_NativeArray.rst
