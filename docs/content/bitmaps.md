# Valkey PHP  - Bitmaps

* [bitCount](#bitcount) - Count set bits in a string
* [bitOp](#bitop) - Perform bitwise operations between strings
* [getBit](#getbit) - Returns the bit value at offset in the string value stored at key
* [setBit](#setbit) - Sets or clears the bit at offset in the string value stored at key

### bitCount
-----
_**Description**_: Count bits in a string.

##### *Parameters*
*key*

##### *Return value*
*LONG*: The number of bits set to 1 in the value behind the input key.

### bitOp
-----
_**Description**_: Bitwise operation on multiple keys.

##### *Parameters*
*operation*: either "AND", "OR", "NOT", "XOR"  
*ret_key*: return key  
*key1*  
*key2...*

##### *Return value*
*LONG*: The size of the string stored in the destination key.


### getBit
-----
_**Description**_: Return a single bit out of a larger string

##### *Parameters*
*key*  
*offset*

##### *Return value*
*LONG*: the bit value (0 or 1)

##### *Example*

```php
$valkey->set('key', "\x7f"); // this is 0111 1111
$valkey->getBit('key', 0); /* 0 */
$valkey->getBit('key', 1); /* 1 */
```

### setBit
-----
_**Description**_: Changes a single bit of a string.

##### *Parameters*
*key*  
*offset*  
*value*: bool or int (1 or 0)

##### *Return value*
*LONG*: 0 or 1, the value of the bit before it was set.

##### *Example*

```php
$valkey->set('key', "*");	// ord("*") = 42 = 0x2f = "0010 1010"
$valkey->setBit('key', 5, 1); /* returns 0 */
$valkey->setBit('key', 7, 1); /* returns 0 */
$valkey->get('key'); /* chr(0x2f) = "/" = b("0010 1111") */
```
