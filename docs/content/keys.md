### Keys
-----

* [del, delete, unlink](#del-delete-unlink) - Delete a key
* [dump](#dump) - Return a serialized version of the value stored at the specified key.
* [exists](#exists) - Determine if a key exists
* [expire, pexpire](#expire-pexpire) - Set a key's time to live in seconds
* [expireAt, pexpireAt](#expireat-pexpireat) - Set the expiration for a key as a UNIX timestamp
* [keys](#keys) - Find all keys matching the given pattern
* [scan](#scan) - Scan for keys in the keyspace (Redis >= 2.8.0)
* [migrate](#migrate) - Atomically transfer a key from a Redis instance to another one
* [move](#move) - Move a key to another database
* [object](#object) - Inspect the internals of Redis objects
* [persist](#persist) - Remove the expiration from a key
* [randomKey](#randomkey) - Return a random key from the keyspace
* [rename](#rename) - Rename a key
* [renameNx](#renamenx) - Rename a key, only if the new key does not exist
* [type](#type) - Determine the type stored at key
* [sort](#sort) - Sort the elements in a list, set or sorted set
* [ttl, pttl](#ttl-pttl) - Get the time to live for a key
* [restore](#restore) - Create a key using the provided serialized value, previously obtained with [dump](#dump).

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
$valkey->set('foo', 'bar');
$val = $valkey->dump('foo'); // $val will be the Redis encoded key value
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
$valkey->migrate('backup', 6379, 'foo', 0, 3600);
$valkey->migrate('backup', 6379, 'foo', 0, 3600, true, true); /* copy and replace */
$valkey->migrate('backup', 6379, 'foo', 0, 3600, false, true); /* just REPLACE flag */

/* Migrate multiple keys (requires Redis >= 3.0.6)
$valkey->migrate('backup', 6379, ['key1', 'key2', 'key3'], 0, 3600);
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
$valkey->persist('key');
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
$valkey->set('foo', 'bar');
$val = $valkey->dump('foo');
$valkey->restore('bar', 0, $val); // The key 'bar', will now be equal to the key 'foo'
```

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
$valkey->del('s');
$valkey->sAdd('s', 5);
$valkey->sAdd('s', 4);
$valkey->sAdd('s', 2);
$valkey->sAdd('s', 1);
$valkey->sAdd('s', 3);

var_dump($valkey->sort('s')); // 1,2,3,4,5
var_dump($valkey->sort('s', ['sort' => 'desc'])); // 5,4,3,2,1
var_dump($valkey->sort('s', ['sort' => 'desc', 'store' => 'out'])); // (int)5
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
$valkey->ttl('key');
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
$key = $valkey->randomKey();
$surprise = $valkey->get($key);	// who knows what's in there.
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
$valkey->select(0);	// switch to DB 0
$valkey->set('x', '42');	// write 42 to x
$valkey->move('x', 1);	// move to DB 1
$valkey->select(1);	// switch to DB 1
$valkey->get('x');	// will return 42
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
$valkey->set('x', '42');
$valkey->rename('x', 'y');
$valkey->get('y'); 	// → 42
$valkey->get('x'); 	// → `FALSE`
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
$valkey->set('x', '42');
$valkey->expire('x', 3);	// x will disappear in 3 seconds.
sleep(5);				// wait 5 seconds
$valkey->get('x'); 		// will return `FALSE`, as 'x' has expired.
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
$valkey->set('x', '42');
$valkey->expireAt('x', time(NULL) + 3); // x will disappear in 3 seconds.
sleep(5);				// wait 5 seconds
$valkey->get('x'); 	// will return `FALSE`, as 'x' has expired.
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
$allKeys = $valkey->keys('*');	// all keys will match this.
$keyWithUserPrefix = $valkey->keys('user*');
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
    $arr_keys = $valkey->scan($it);

    // Redis may return empty results, so protect against that
    if ($arr_keys !== FALSE) {
        foreach($arr_keys as $str_key) {
            echo "Here is a key: $str_key\n";
        }
    }
} while ($it > 0);
echo "No more keys to scan!\n";

/* With Redis::SCAN_RETRY enabled */
$valkey->setOption(Redis::OPT_SCAN, Redis::SCAN_RETRY);
$it = NULL;

/* phpredis will retry the SCAN command if empty results are returned from the
   server, so no empty results check is required. */
while ($arr_keys = $valkey->scan($it)) {
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
$valkey->object("encoding", "l"); // → ziplist
$valkey->object("refcount", "l"); // → 1
$valkey->object("idletime", "l"); // → 400 (in seconds, with a precision of 10 seconds).
```

### type
-----
_**Description**_: Returns the type of data pointed by a given key.

##### *Parameters*
*Key*: key

##### *Return value*

Depending on the type of the data pointed by the key, this method will return the following value:  
string: Redis::VALKEY_STRING  
set: Redis::VALKEY_SET  
list: Redis::VALKEY_LIST  
zset: Redis::VALKEY_ZSET  
hash: Redis::VALKEY_HASH  
other: Redis::VALKEY_NOT_FOUND

##### *Example*

```php
$valkey->type('key');
```
