# Valkey PHP - Strings

|Command                        |Description                                                            |Supported              |Tested                 |Class/Trait    |Method     |
|---                            |---                                                                    |:-:                    |:-:                    |---            |---        |
|[append](#append)              |Append a value to a key                                                |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |append   |
|[decr](#decr)                  |Decrement the value of a key                                           |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |decr   |
|[decrBy](#decrBy)              |Decrement the value of a key                                           |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |decrBy   |
|[get](#get)                    |Get the value of a key                                                 |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |get   |
|[getRange](#getRange)          |Get a substring of the string stored at a key                          |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |getRange   |
|[getSet](#getSet)              |Set the string value of a key and return its old value                 |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |getSet   |
|[incr](#incr)                  |Increment the value of a key                                           |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |incr   |
|[incrBy](#incrBy)              |Increment the value of a key                                           |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |incrBy   |
|[incrByFloat](#incrByFloat)    |Increment the float value of a key by the given amount                 |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |incrByFloat   |
|[mGet](#mGet)                  |Get the values of all the given keys                                   |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |mGet   |
|[getMultiple](#getMultiple)    |Get the values of all the given keys                                   |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |getMultiple   |
|[mSet](#mSet)                  |Set multiple keys to multiple values                                   |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |mSet   |
|[mSetNX](#mSetNX)              |Set multiple keys to multiple values                                   |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |mSetNX   |
|[set](#set)                    |Set the string value of a key                                          |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |set   |
|[setEx](#setEx)                |Set the value and expiration of a key                                  |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |setEx   |
|[pSetEx](#pSetEx)              |Set the value and expiration of a key                                  |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |pSetEx   |
|[setNx](#setNx)                |Set the value of a key, only if the key does not exist                 |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |setNx   |
|[setRange](#setRange)          |Overwrite part of a string at key starting at the specified offset     |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |setRange   |
|[strLen](#strLen)              |Get the length of the value stored in a key                            |:white\_check\_mark:   |:white\_check\_mark:   |Strings        |strLen   |

- APPEND Appends a string to the value of a key. Creates the key if it doesn't exist.
- DECR Decrements the integer value of a key by one. Uses 0 as initial value if the key doesn't exist.
- DECRBY Decrements a number from the integer value of a key. Uses 0 as initial value if the key doesn't exist.
- GET Returns the string value of a key.
- GETDEL Returns the string value of a key after deleting the key.
- GETEX Returns the string value of a key after setting its expiration time.
- GETRANGE Returns a substring of the string stored at a key.
- GETSET Returns the previous string value of a key after setting it to a new value.
- INCR Increments the integer value of a key by one. Uses 0 as initial value if the key doesn't exist.
- INCRBY Increments the integer value of a key by a number. Uses 0 as initial value if the key doesn't exist.
- INCRBYFLOAT Increment the floating point value of a key by a number. Uses 0 as initial value if the key doesn't exist.
- LCS Finds the longest common substring.
- MGET Atomically returns the string values of one or more keys.
- MSET Atomically creates or modifies the string values of one or more keys.
- MSETNX Atomically modifies the string values of one or more keys only when all keys don't exist.
- PSETEX Sets both string value and expiration time in milliseconds of a key. The key is created if it doesn't exist.
- SET Sets the string value of a key, ignoring its type. The key is created if it doesn't exist.
- SETEX Sets the string value and expiration time of a key. Creates the key if it doesn't exist.
- SETNX Set the string value of a key only when the key doesn't exist.
- SETRANGE Overwrites a part of a string value with another by an offset. Creates the key if it doesn't exist.
- STRLEN Returns the length of a string value.
- SUBSTR Returns a substring from a string value.

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
// Simple key -> value set
$valkey->set('key', 'value');
// Will redirect, and actually make an SETEX call
$valkey->set('key', 'value', 10);
// Will set the key, if it doesn't exist, with a ttl of 10 seconds
$valkey->set('key:'.time(), 'value', ['nx', 'ex' => 10]);
// Will set a key, if it does exist, with a ttl of 1000 miliseconds
$valkey->set('key', 'value', ['xx', 'px' => 1000]);
$valkey->setEx('key', 10, 'value1');
$valkey->append('key', 'value2'); /* 12 */
$valkey->get('key'); /* 'value1value2' */
$valkey->exists('key'); /* 1 */
$valkey->exists('NonExistingKey'); /* 0 */
$valkey->mset(['foo' => 'foo', 'bar' => 'bar', 'baz' => 'baz']);
$valkey->exists(['foo', 'bar', 'baz']); /* 3 */
```

* [append](#append) - Append a value to a key
* [decr, decrBy](#decr-decrby) - Decrement the value of a key
* [get](#get) - Get the value of a key
* [getEx](#getex) - Get the value of a key and set its expiration
* [getRange](#getrange) - Get a substring of the string stored at a key
* [getSet](#getset) - Set the string value of a key and return its old value
* [incr, incrBy](#incr-incrby) - Increment the value of a key
* [incrByFloat](#incrbyfloat) - Increment the float value of a key by the given amount
* [mGet](#mget) - Get the values of all the given keys
* [mSet, mSetNX](#mset-msetnx) - Set multiple keys to multiple values
* [set](#set) - Set the string value of a key
* [setEx, pSetEx](#setex-psetex) - Set the value and expiration of a key
* [setNx](#setnx) - Set the value of a key, only if the key does not exist
* [setRange](#setrange) - Overwrite part of a string at key starting at the specified offset
* [strLen](#strlen) - Get the length of the value stored in a key

-----

## append

-----

_**Description**_: Append specified string to the string stored in specified key.

#### *Parameters*

*Key*
*Value*

#### *Return value*

*INTEGER*: Size of the value after the append

#### *Example*

```php
$valkey->set('key', 'value1');
$valkey->append('key', 'value2'); /* 12 */
$valkey->get('key'); /* 'value1value2' */
```

## get

-----

_**Description**_: Get the value related to the specified key

#### *Parameters*

*key*

#### *Return value*

*String* or *Bool*: If key didn't exist, `FALSE` is returned. Otherwise, the value related to this key is returned.

#### *Examples*

```php
$valkey->get('key');
```

## getEx

-----

_**Description**_: Get the value related to the specified key and set its expiration

#### *Parameters*

*key* 
*options array* (optional) with the following keys:
  * `EX` - expire time in seconds
  * `PX` - expire time in milliseconds
  * `EXAT` - expire time in seconds since UNIX epoch
  * `PXAT` - expire time in milliseconds since UNIX epoch
  * `PERSIST` - remove the expiration from the key

#### *Return value*

*String* or *Bool*: If key didn't exist, `FALSE` is returned. Otherwise, the value related to this key is returned.

#### *Examples*

```php
$valkey->getEx('key', ['EX' => 10]); // get key and set its expiration to 10 seconds
```

## set
-----
_**Description**_: Set the string value in argument as value of the key.  If you're using Valkey >= 2.6.12, you can pass extended options as explained below

#### *Parameters*

*Key*  
*Value*  
*Timeout or Options Array* (optional). If you pass an integer, valkey-php will redirect to SETEX, and will try to use Valkey >= 2.6.12 extended options if you pass an array with valid values

#### *Return value*

*Bool* `TRUE` if the command is successful.

#### *Examples*

```php
// Simple key -> value set
$valkey->set('key', 'value');

// Will redirect, and actually make an SETEX call
$valkey->set('key','value', 10);

// Will set the key, if it doesn't exist, with a ttl of 10 seconds
$valkey->set('key', 'value', ['nx', 'ex'=>10]);

// Will set a key, if it does exist, with a ttl of 1000 milliseconds
$valkey->set('key', 'value', ['xx', 'px'=>1000]);
```

## setEx, pSetEx

-----

_**Description**_: Set the string value in argument as value of the key, with a time to live. PSETEX uses a TTL in milliseconds.

#### *Parameters*

*Key*
*TTL*
*Value*

#### *Return value*

*Bool* `TRUE` if the command is successful.

#### *Examples*

```php
$valkey->setEx('key', 3600, 'value'); // sets key → value, with 1h TTL.
$valkey->pSetEx('key', 100, 'value'); // sets key → value, with 0.1 sec TTL.
```

## setNx
-----
_**Description**_: Set the string value in argument as value of the key if the key doesn't already exist in the database.

#### *Parameters*
*key*
*value*

#### *Return value*

*Bool* `TRUE` in case of success, `FALSE` in case of failure.

#### *Examples*

```php
$valkey->setNx('key', 'value'); /* return TRUE */
$valkey->setNx('key', 'value'); /* return FALSE */
```

## del, delete, unlink

-----

_**Description**_: Remove specified keys.

#### *Parameters*

An array of keys, or an undefined number of parameters, each a key: *key1* *key2* *key3* ... *keyN*

*Note*: If you are connecting to Valkey server >= 4.0.0 you can remove a key with the `unlink` method in the exact same way you would use `del`.  The Valkey [unlink](https://redis.io/commands/unlink) command is non-blocking and will perform the actual deletion asynchronously.

#### *Return value*

*Long* Number of keys deleted.

#### *Examples*

```php
$valkey->set('key1', 'val1');
$valkey->set('key2', 'val2');
$valkey->set('key3', 'val3');
$valkey->set('key4', 'val4');

$valkey->del('key1', 'key2'); /* return 2 */
$valkey->del(['key3', 'key4']); /* return 2 */

/* If using Valkey >= 4.0.0 you can call unlink */
$valkey->unlink('key1', 'key2');
$valkey->unlink(['key1', 'key2']);
```

**Note:** `delete` is an alias for `del` and will be removed in future versions of valkey-php.

## exists

-----

_**Description**_: Verify if the specified key exists.

#### *Parameters*
*key*

#### *Return value*
*long*: The number of keys tested that do exist.

#### *Examples*
```php
$valkey->set('key', 'value');
$valkey->exists('key'); /* 1 */
$valkey->exists('NonExistingKey'); /* 0 */

$valkey->mset(['foo' => 'foo', 'bar' => 'bar', 'baz' => 'baz']);
$valkey->exists(['foo', 'bar', 'baz']); /* 3 */
$valkey->exists('foo', 'bar', 'baz'); /* 3 */
```

**Note**: This function took a single argument and returned TRUE or FALSE in valkey-php versions < 4.0.0.

## incr, incrBy
-----
_**Description**_: Increment the number stored at key by one. If the second argument is filled, it will be used as the integer value of the increment.

#### *Parameters*
*key*  
*value*: value that will be added to key (only for incrBy)

#### *Return value*
*INT* the new value

#### *Examples*
```php
$valkey->incr('key1'); /* key1 didn't exists, set to 0 before the increment */
                      /* and now has the value 1  */

$valkey->incr('key1'); /* 2 */
$valkey->incr('key1'); /* 3 */
$valkey->incr('key1'); /* 4 */

// Will redirect, and actually make an INCRBY call
$valkey->incr('key1', 10);   /* 14 */

$valkey->incrBy('key1', 10); /* 24 */
```

## incrByFloat
-----
_**Description**_: Increment the key with floating point precision.

#### *Parameters*
*key*  
*value*: (float) value that will be added to the key

#### *Return value*
*FLOAT* the new value

#### *Examples*
```php
$valkey->incrByFloat('key1', 1.5); /* key1 didn't exist, so it will now be 1.5 */


$valkey->incrByFloat('key1', 1.5); /* 3 */
$valkey->incrByFloat('key1', -1.5); /* 1.5 */
$valkey->incrByFloat('key1', 2.5); /* 4 */
```

## decr, decrBy
-----
_**Description**_: Decrement the number stored at key by one. If the second argument is filled, it will be used as the integer value of the decrement.

#### *Parameters*
*key*  
*value*: value that will be subtracted to key (only for decrBy)

#### *Return value*
*INT* the new value

#### *Examples*
```php
$valkey->decr('key1'); /* key1 didn't exists, set to 0 before the increment */
                      /* and now has the value -1  */

$valkey->decr('key1'); /* -2 */
$valkey->decr('key1'); /* -3 */

// Will redirect, and actually make an DECRBY call
$valkey->decr('key1', 10);   /* -13 */

$valkey->decrBy('key1', 10); /* -23 */
```

## mGet
-----
_**Description**_: Get the values of all the specified keys. If one or more keys don't exist, the array will contain `FALSE` at the position of the key.

#### *Parameters*
*Array*: Array containing the list of the keys

#### *Return value*
*Array*: Array containing the values related to keys in argument

#### *Examples*
```php
$valkey->set('key1', 'value1');
$valkey->set('key2', 'value2');
$valkey->set('key3', 'value3');
$valkey->mGet(['key1', 'key2', 'key3']); /* ['value1', 'value2', 'value3'];
$valkey->mGet(['key0', 'key1', 'key5']); /* [`FALSE`, 'value1', `FALSE`];
```

## getSet
-----
_**Description**_: Sets a value and returns the previous entry at that key.
#### *Parameters*
*Key*: key

*STRING*: value

#### *Return value*
A string, the previous value located at this key.
#### *Example*

```php
$valkey->set('x', '42');
$exValue = $valkey->getSet('x', 'lol');	// return '42', replaces x by 'lol'
$newValue = $valkey->get('x')'		// return 'lol'
```



## getRange
-----
_**Description**_: Return a substring of a larger string

#### *Parameters*
*key*  
*start*  
*end*

#### *Return value*
*STRING*: the substring

#### *Example*

```php
$valkey->set('key', 'string value');
$valkey->getRange('key', 0, 5); /* 'string' */
$valkey->getRange('key', -5, -1); /* 'value' */
```

## setRange
-----
_**Description**_: Changes a substring of a larger string.

#### *Parameters*
*key*
*offset*
*value*

#### *Return value*
*STRING*: the length of the string after it was modified.

#### *Example*

```php
$valkey->set('key', 'Hello world');
$valkey->setRange('key', 6, "redis"); /* returns 11 */
$valkey->get('key'); /* "Hello redis" */
```

## strLen
-----
_**Description**_: Get the length of a string value.

#### *Parameters*
*key*

#### *Return value*
*INTEGER*

#### *Example*

```php
$valkey->set('key', 'value');
$valkey->strlen('key'); /* 5 */
```

## mSet, mSetNx
-----
_**Description**_: Sets multiple key-value pairs in one atomic command. MSETNX only returns TRUE if all the keys were set (see SETNX).

#### *Parameters*
*Pairs*: [key => value, ...]

#### *Return value*
*Bool* `TRUE` in case of success, `FALSE` in case of failure.

#### *Example*

```php

$valkey->mSet(['key0' => 'value0', 'key1' => 'value1']);
var_dump($valkey->get('key0'));
var_dump($valkey->get('key1'));

```
Output:
```
string(6) "value0"
string(6) "value1"
```
