### Strings
-----

* [append](#append) - Append a value to a key
* [bitCount](#bitcount) - Count set bits in a string
* [bitOp](#bitop) - Perform bitwise operations between strings
* [decr, decrBy](#decr-decrby) - Decrement the value of a key
* [get](#get) - Get the value of a key
* [getEx](#getex) - Get the value of a key and set its expiration
* [getBit](#getbit) - Returns the bit value at offset in the string value stored at key
* [getRange](#getrange) - Get a substring of the string stored at a key
* [getSet](#getset) - Set the string value of a key and return its old value
* [incr, incrBy](#incr-incrby) - Increment the value of a key
* [incrByFloat](#incrbyfloat) - Increment the float value of a key by the given amount
* [mGet](#mget) - Get the values of all the given keys
* [mSet, mSetNX](#mset-msetnx) - Set multiple keys to multiple values
* [set](#set) - Set the string value of a key
* [setBit](#setbit) - Sets or clears the bit at offset in the string value stored at key
* [setEx, pSetEx](#setex-psetex) - Set the value and expiration of a key
* [setNx](#setnx) - Set the value of a key, only if the key does not exist
* [setRange](#setrange) - Overwrite part of a string at key starting at the specified offset
* [strLen](#strlen) - Get the length of the value stored in a key



-----

### get
-----
_**Description**_: Get the value related to the specified key

##### *Parameters*
*key*

##### *Return value*
*String* or *Bool*: If key didn't exist, `FALSE` is returned. Otherwise, the value related to this key is returned.

##### *Examples*

```php
$redis->get('key');
```

### getEx
-----
_**Description**_: Get the value related to the specified key and set its expiration

##### *Parameters*
*key* 
*options array* (optional) with the following keys:
  * `EX` - expire time in seconds
  * `PX` - expire time in milliseconds
  * `EXAT` - expire time in seconds since UNIX epoch
  * `PXAT` - expire time in milliseconds since UNIX epoch
  * `PERSIST` - remove the expiration from the key

##### *Return value*
*String* or *Bool*: If key didn't exist, `FALSE` is returned. Otherwise, the value related to this key is returned.

##### *Examples*

```php
$redis->getEx('key', ['EX' => 10]); // get key and set its expiration to 10 seconds
```

### set
-----
_**Description**_: Set the string value in argument as value of the key.  If you're using Redis >= 2.6.12, you can pass extended options as explained below

##### *Parameters*
*Key*  
*Value*  
*Timeout or Options Array* (optional). If you pass an integer, phpredis will redirect to SETEX, and will try to use Redis >= 2.6.12 extended options if you pass an array with valid values

##### *Return value*
*Bool* `TRUE` if the command is successful.

##### *Examples*
```php
// Simple key -> value set
$redis->set('key', 'value');

// Will redirect, and actually make an SETEX call
$redis->set('key','value', 10);

// Will set the key, if it doesn't exist, with a ttl of 10 seconds
$redis->set('key', 'value', ['nx', 'ex'=>10]);

// Will set a key, if it does exist, with a ttl of 1000 milliseconds
$redis->set('key', 'value', ['xx', 'px'=>1000]);

```

### setEx, pSetEx
-----
_**Description**_: Set the string value in argument as value of the key, with a time to live. PSETEX uses a TTL in milliseconds.

##### *Parameters*
*Key*
*TTL*
*Value*

##### *Return value*
*Bool* `TRUE` if the command is successful.

##### *Examples*

```php
$redis->setEx('key', 3600, 'value'); // sets key → value, with 1h TTL.
$redis->pSetEx('key', 100, 'value'); // sets key → value, with 0.1 sec TTL.
```

### setNx
-----
_**Description**_: Set the string value in argument as value of the key if the key doesn't already exist in the database.

##### *Parameters*
*key*
*value*

##### *Return value*
*Bool* `TRUE` in case of success, `FALSE` in case of failure.

##### *Examples*
```php
$redis->setNx('key', 'value'); /* return TRUE */
$redis->setNx('key', 'value'); /* return FALSE */
```

### del, delete, unlink
-----
_**Description**_: Remove specified keys.

##### *Parameters*
An array of keys, or an undefined number of parameters, each a key: *key1* *key2* *key3* ... *keyN*

*Note*: If you are connecting to Redis server >= 4.0.0 you can remove a key with the `unlink` method in the exact same way you would use `del`.  The Redis [unlink](https://redis.io/commands/unlink) command is non-blocking and will perform the actual deletion asynchronously.

##### *Return value*
*Long* Number of keys deleted.

##### *Examples*
```php
$redis->set('key1', 'val1');
$redis->set('key2', 'val2');
$redis->set('key3', 'val3');
$redis->set('key4', 'val4');

$redis->del('key1', 'key2'); /* return 2 */
$redis->del(['key3', 'key4']); /* return 2 */

/* If using Redis >= 4.0.0 you can call unlink */
$redis->unlink('key1', 'key2');
$redis->unlink(['key1', 'key2']);
```

**Note:** `delete` is an alias for `del` and will be removed in future versions of phpredis.

### exists
-----
_**Description**_: Verify if the specified key exists.

##### *Parameters*
*key*

##### *Return value*
*long*: The number of keys tested that do exist.

##### *Examples*
```php
$redis->set('key', 'value');
$redis->exists('key'); /* 1 */
$redis->exists('NonExistingKey'); /* 0 */

$redis->mset(['foo' => 'foo', 'bar' => 'bar', 'baz' => 'baz']);
$redis->exists(['foo', 'bar', 'baz']); /* 3 */
$redis->exists('foo', 'bar', 'baz'); /* 3 */
```

**Note**: This function took a single argument and returned TRUE or FALSE in phpredis versions < 4.0.0.

### incr, incrBy
-----
_**Description**_: Increment the number stored at key by one. If the second argument is filled, it will be used as the integer value of the increment.

##### *Parameters*
*key*  
*value*: value that will be added to key (only for incrBy)

##### *Return value*
*INT* the new value

##### *Examples*
```php
$redis->incr('key1'); /* key1 didn't exists, set to 0 before the increment */
                      /* and now has the value 1  */

$redis->incr('key1'); /* 2 */
$redis->incr('key1'); /* 3 */
$redis->incr('key1'); /* 4 */

// Will redirect, and actually make an INCRBY call
$redis->incr('key1', 10);   /* 14 */

$redis->incrBy('key1', 10); /* 24 */
```

### incrByFloat
-----
_**Description**_: Increment the key with floating point precision.

##### *Parameters*
*key*  
*value*: (float) value that will be added to the key

##### *Return value*
*FLOAT* the new value

##### *Examples*
```php
$redis->incrByFloat('key1', 1.5); /* key1 didn't exist, so it will now be 1.5 */


$redis->incrByFloat('key1', 1.5); /* 3 */
$redis->incrByFloat('key1', -1.5); /* 1.5 */
$redis->incrByFloat('key1', 2.5); /* 4 */
```

### decr, decrBy
-----
_**Description**_: Decrement the number stored at key by one. If the second argument is filled, it will be used as the integer value of the decrement.

##### *Parameters*
*key*  
*value*: value that will be subtracted to key (only for decrBy)

##### *Return value*
*INT* the new value

##### *Examples*
```php
$redis->decr('key1'); /* key1 didn't exists, set to 0 before the increment */
                      /* and now has the value -1  */

$redis->decr('key1'); /* -2 */
$redis->decr('key1'); /* -3 */

// Will redirect, and actually make an DECRBY call
$redis->decr('key1', 10);   /* -13 */

$redis->decrBy('key1', 10); /* -23 */
```

### mGet
-----
_**Description**_: Get the values of all the specified keys. If one or more keys don't exist, the array will contain `FALSE` at the position of the key.

##### *Parameters*
*Array*: Array containing the list of the keys

##### *Return value*
*Array*: Array containing the values related to keys in argument

##### *Examples*
```php
$redis->set('key1', 'value1');
$redis->set('key2', 'value2');
$redis->set('key3', 'value3');
$redis->mGet(['key1', 'key2', 'key3']); /* ['value1', 'value2', 'value3'];
$redis->mGet(['key0', 'key1', 'key5']); /* [`FALSE`, 'value1', `FALSE`];
```

### getSet
-----
_**Description**_: Sets a value and returns the previous entry at that key.
##### *Parameters*
*Key*: key

*STRING*: value

##### *Return value*
A string, the previous value located at this key.
##### *Example*
```php
$redis->set('x', '42');
$exValue = $redis->getSet('x', 'lol');	// return '42', replaces x by 'lol'
$newValue = $redis->get('x')'		// return 'lol'
```

### randomKey
-----
_**Description**_: Returns a random key.

##### *Parameters*
None.
##### *Return value*
*STRING*: an existing key in redis.

##### *Example*
```php
$key = $redis->randomKey();
$surprise = $redis->get($key);	// who knows what's in there.
```

### move
-----
_**Description**_: Moves a key to a different database.

##### *Parameters*
*Key*: key, the key to move.

*INTEGER*: dbindex, the database number to move the key to.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure.
##### *Example*

```php
$redis->select(0);	// switch to DB 0
$redis->set('x', '42');	// write 42 to x
$redis->move('x', 1);	// move to DB 1
$redis->select(1);	// switch to DB 1
$redis->get('x');	// will return 42
```

### rename
-----
_**Description**_: Renames a key.
##### *Parameters*
*STRING*: srckey, the key to rename.

*STRING*: dstkey, the new name for the key.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure.
##### *Example*
```php
$redis->set('x', '42');
$redis->rename('x', 'y');
$redis->get('y'); 	// → 42
$redis->get('x'); 	// → `FALSE`
```

### renameNx
-----
_**Description**_: Same as rename, but will not replace a key if the destination already exists. This is the same behaviour as setNx.

### expire, pexpire
-----
_**Description**_: Sets an expiration on a key in either seconds or milliseconds.

##### *Prototype*
```php
public function expire(string $key, int $seconds, ?string $mode = NULL): Redis|bool;
public function pexpire(string $key, int $milliseconds, ?string $mode = NULL): Redis|bool;
```

##### *Return value*
*BOOL*: `TRUE` if an expiration was set, and `FALSE` on failure or if one was not set.  You can distinguish between an error and an expiration not being set by checking `getLastError()`.
##### *Example*
```php
$redis->set('x', '42');
$redis->expire('x', 3);	// x will disappear in 3 seconds.
sleep(5);				// wait 5 seconds
$redis->get('x'); 		// will return `FALSE`, as 'x' has expired.
```

### expireAt, pexpireAt
-----
_**Description**_: Seta specific timestamp for a key to expire in seconds or milliseconds.

##### *Prototype*
```php
public function expireat(string $key, int $unix_timestamp, ?string $mode = NULL): Redis|bool;
public function pexpireat(string $key, int $unix_timestamp_millis, ?string $mode = NULL): Redis|bool;
```

##### *Return value*
*BOOL*: `TRUE` if an expiration was set and `FALSE` if one was not set or in the event on an error.  You can detect an actual error by checking `getLastError()`.

##### *Example*
```php
$redis->set('x', '42');
$redis->expireAt('x', time(NULL) + 3); // x will disappear in 3 seconds.
sleep(5);				// wait 5 seconds
$redis->get('x'); 	// will return `FALSE`, as 'x' has expired.
```

### keys
-----
_**Description**_: Returns the keys that match a certain pattern.

##### *Parameters*
*STRING*: pattern, using '*' as a wildcard.

##### *Return value*
*Array of STRING*: The keys that match a certain pattern.

##### *Example*
```php
$allKeys = $redis->keys('*');	// all keys will match this.
$keyWithUserPrefix = $redis->keys('user*');
```

### scan
-----
_**Description**_:  Scan the keyspace for keys

##### *Parameters*
*LONG (reference)*:  Iterator, initialized to NULL
*STRING, Optional*:  Pattern to match
*LONG, Optional*: Count of keys per iteration (only a suggestion to Redis)

##### *Return value*
*Array, boolean*:  This function will return an array of keys or FALSE if Redis returned zero keys

*Note*: SCAN is a "directed node" command in [RedisCluster](cluster.md#directed-node-commands)

##### *Example*
```php

/* Without enabling Redis::SCAN_RETRY (default condition) */
$it = NULL;
do {
    // Scan for some keys
    $arr_keys = $redis->scan($it);

    // Redis may return empty results, so protect against that
    if ($arr_keys !== FALSE) {
        foreach($arr_keys as $str_key) {
            echo "Here is a key: $str_key\n";
        }
    }
} while ($it > 0);
echo "No more keys to scan!\n";

/* With Redis::SCAN_RETRY enabled */
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_RETRY);
$it = NULL;

/* phpredis will retry the SCAN command if empty results are returned from the
   server, so no empty results check is required. */
while ($arr_keys = $redis->scan($it)) {
    foreach ($arr_keys as $str_key) {
        echo "Here is a key: $str_key\n";
    }
}
echo "No more keys to scan!\n";
```

### object
-----
_**Description**_: Describes the object pointed to by a key.

##### *Parameters*
The information to retrieve (string) and the key (string). Info can be one of the following:

* "encoding"
* "refcount"
* "idletime"

##### *Return value*
*STRING* for "encoding", *LONG* for "refcount" and "idletime", `FALSE` if the key doesn't exist.

##### *Example*
```php
$redis->object("encoding", "l"); // → ziplist
$redis->object("refcount", "l"); // → 1
$redis->object("idletime", "l"); // → 400 (in seconds, with a precision of 10 seconds).
```

### type
-----
_**Description**_: Returns the type of data pointed by a given key.

##### *Parameters*
*Key*: key

##### *Return value*

Depending on the type of the data pointed by the key, this method will return the following value:  
string: Redis::REDIS_STRING  
set: Redis::REDIS_SET  
list: Redis::REDIS_LIST  
zset: Redis::REDIS_ZSET  
hash: Redis::REDIS_HASH  
other: Redis::REDIS_NOT_FOUND

##### *Example*
```php
$redis->type('key');
```

### append
-----
_**Description**_: Append specified string to the string stored in specified key.

##### *Parameters*
*Key*
*Value*

##### *Return value*
*INTEGER*: Size of the value after the append

##### *Example*
```php
$redis->set('key', 'value1');
$redis->append('key', 'value2'); /* 12 */
$redis->get('key'); /* 'value1value2' */
```

### getRange
-----
_**Description**_: Return a substring of a larger string

##### *Parameters*
*key*  
*start*  
*end*

##### *Return value*
*STRING*: the substring

##### *Example*
```php
$redis->set('key', 'string value');
$redis->getRange('key', 0, 5); /* 'string' */
$redis->getRange('key', -5, -1); /* 'value' */
```

### setRange
-----
_**Description**_: Changes a substring of a larger string.

##### *Parameters*
*key*
*offset*
*value*

##### *Return value*
*STRING*: the length of the string after it was modified.

##### *Example*
```php
$redis->set('key', 'Hello world');
$redis->setRange('key', 6, "redis"); /* returns 11 */
$redis->get('key'); /* "Hello redis" */
```

### strLen
-----
_**Description**_: Get the length of a string value.

##### *Parameters*
*key*

##### *Return value*
*INTEGER*

##### *Example*
```php
$redis->set('key', 'value');
$redis->strlen('key'); /* 5 */
```

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
$redis->set('key', "\x7f"); // this is 0111 1111
$redis->getBit('key', 0); /* 0 */
$redis->getBit('key', 1); /* 1 */
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
$redis->set('key', "*");	// ord("*") = 42 = 0x2f = "0010 1010"
$redis->setBit('key', 5, 1); /* returns 0 */
$redis->setBit('key', 7, 1); /* returns 0 */
$redis->get('key'); /* chr(0x2f) = "/" = b("0010 1111") */
```

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

### bitCount
-----
_**Description**_: Count bits in a string.

##### *Parameters*
*key*

##### *Return value*
*LONG*: The number of bits set to 1 in the value behind the input key.

### sort
-----
_**Description**_: Sort the elements in a list, set or sorted set.

##### *Parameters*
*Key*: key
*Options*: [key => value, ...] - optional, with the following keys and values:
```
    'by' => 'some_pattern_*',
    'limit' => [0, 1],
    'get' => 'some_other_pattern_*' or an array of patterns,
    'sort' => 'asc' or 'desc',
    'alpha' => TRUE,
    'store' => 'external-key'
```
##### *Return value*
An array of values, or a number corresponding to the number of elements stored if that was used.

##### *Example*
```php
$redis->del('s');
$redis->sAdd('s', 5);
$redis->sAdd('s', 4);
$redis->sAdd('s', 2);
$redis->sAdd('s', 1);
$redis->sAdd('s', 3);

var_dump($redis->sort('s')); // 1,2,3,4,5
var_dump($redis->sort('s', ['sort' => 'desc'])); // 5,4,3,2,1
var_dump($redis->sort('s', ['sort' => 'desc', 'store' => 'out'])); // (int)5
```




### ttl, pttl
-----
_**Description**_: Returns the time to live left for a given key in seconds (ttl), or milliseconds (pttl).

##### *Parameters*
*Key*: key

##### *Return value*
*LONG*:  The time to live in seconds.  If the key has no ttl, `-1` will be returned, and `-2` if the key doesn't exist.

##### *Example*
```php
$redis->ttl('key');
```

### persist
-----
_**Description**_: Remove the expiration timer from a key.

##### *Parameters*
*Key*: key

##### *Return value*
*BOOL*: `TRUE` if a timeout was removed, `FALSE` if the key didn’t exist or didn’t have an expiration timer.

##### *Example*
```php
$redis->persist('key');
```

### mSet, mSetNx
-----
_**Description**_: Sets multiple key-value pairs in one atomic command. MSETNX only returns TRUE if all the keys were set (see SETNX).

##### *Parameters*
*Pairs*: [key => value, ...]

##### *Return value*
*Bool* `TRUE` in case of success, `FALSE` in case of failure.

##### *Example*
```php

$redis->mSet(['key0' => 'value0', 'key1' => 'value1']);
var_dump($redis->get('key0'));
var_dump($redis->get('key1'));

```
Output:
```
string(6) "value0"
string(6) "value1"
```



### dump
-----
_**Description**_: Dump a key out of a redis database, the value of which can later be passed into redis using the RESTORE command.  The data
that comes out of DUMP is a binary representation of the key as Redis stores it.
##### *Parameters*
*key* string
##### *Return value*
The Redis encoded value of the key, or FALSE if the key doesn't exist
##### *Examples*
```php
$redis->set('foo', 'bar');
$val = $redis->dump('foo'); // $val will be the Redis encoded key value
```

### restore
-----
_**Description**_: Restore a key from the result of a DUMP operation.
##### *Parameters*
*key* string.  The key name  
*ttl* integer.  How long the key should live (if zero, no expire will be set on the key)  
*value* string (binary).  The Redis encoded key value (from DUMP)
##### *Examples*
```php
$redis->set('foo', 'bar');
$val = $redis->dump('foo');
$redis->restore('bar', 0, $val); // The key 'bar', will now be equal to the key 'foo'
```

### migrate
-----
_**Description**_: Migrates a key to a different Redis instance.

**Note:**: Redis introduced migrating multiple keys in 3.0.6, so you must have at least
that version in order to call `migrate` with an array of keys.

##### *Parameters*
*host* string.  The destination host  
*port* integer.  The TCP port to connect to.  
*key(s)* string or array.  
*destination-db* integer.  The target DB.  
*timeout* integer.  The maximum amount of time given to this transfer.  
*copy* boolean, optional.  Should we send the COPY flag to redis.  
*replace* boolean, optional.  Should we send the REPLACE flag to redis  
##### *Examples*
```php
$redis->migrate('backup', 6379, 'foo', 0, 3600);
$redis->migrate('backup', 6379, 'foo', 0, 3600, true, true); /* copy and replace */
$redis->migrate('backup', 6379, 'foo', 0, 3600, false, true); /* just REPLACE flag */

/* Migrate multiple keys (requires Redis >= 3.0.6)
$redis->migrate('backup', 6379, ['key1', 'key2', 'key3'], 0, 3600);
```