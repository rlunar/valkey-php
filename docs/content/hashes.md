# Valkey PHP - Hashes

|Command                        |Description                                                            |Supported              |Tested                 |Class/Trait    |Method         |
|---                            |---                                                                    |:-:                    |:-:                    |---            |---            |
|[hDel](#hDel)                  |Delete one or more hash fields                                         |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hDel           |
|[hExists](#hExists)            |Determine if a hash field exists                                       |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hExists        |
|[hGet](#hGet)                  |Get the value of a hash field                                          |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hGet           |
|[hGetAll](#hGetAll)            |Get all the fields and values in a hash                                |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hGetAll        |
|[hIncrBy](#hIncrBy)            |Increment the integer value of a hash field by the given number        |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hIncrBy        |
|[hIncrByFloat](#hIncrByFloat)  |Increment the float value of a hash field by the given amount          |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hIncrByFloat   |
|[hKeys](#hKeys)                |Get all the fields in a hash                                           |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hKeys          |
|[hLen](#hLen)                  |Get the number of fields in a hash                                     |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hLen           |
|[hMGet](#hMGet)                |Get the values of all the given hash fields                            |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hMGet          |
|[hMSet](#hMSet)                |Set multiple hash fields to multiple values                            |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hMSet          |
|[hScan](#hScan)                |Scan a hash key for members                                            |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hScan          |
|[hSet](#hSet)                  |Set the string value of a hash field                                   |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hSet           |
|[hSetNx](#hSetNx)              |Set the value of a hash field, only if the field does not exist        |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hSetNx         |
|[hStrLen](#hStrLen)            |Get the string length of the value associated with field in the hash   |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hStrLen        |
|[hVals](#hVals)                |Get all the values in a hash                                           |:white\_check\_mark:   |:white\_check\_mark:   |Hashes         |hVals          |

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->hSet('hash', 'key1', 'hello'); /* 1, 'key1' => 'hello' in the hash at "h" */
$valkey->hGet('hash', 'key1'); /* returns "hello" */
$valkey->hSetNx('hash', 'key1', 'hello'); /* TRUE, 'key1' => 'hello' in the hash at "hash" */
$valkey->hLen('hash'); /* returns 2 */
$valkey->hGetAll('hash');
$valkey->hKeys('hash');
$valkey->hVals('hash');
```

### hSet
-----
_**Description**_: Adds a value to the hash stored at key.
##### *Parameters*
*key*  
*hashKey*  
*value*

##### *Return value*
*LONG* `1` if value didn't exist and was added successfully, `0` if the value was already present and was replaced, `FALSE` if there was an error.
##### *Example*

```php
$valkey->del('hash')
$valkey->hSet('hash', 'key1', 'hello'); /* 1, 'key1' => 'hello' in the hash at "h" */
$valkey->hGet('hash', 'key1'); /* returns "hello" */

$valkey->hSet('hash', 'key1', 'plop'); /* 0, value was replaced. */
$valkey->hGet('hash', 'key1'); /* returns "plop" */
```

### hSetNx
-----
_**Description**_: Adds a value to the hash stored at key only if this field isn't already in the hash.

##### *Return value*
*BOOL* `TRUE` if the field was set, `FALSE` if it was already present.

##### *Example*

```php
$valkey->del('hash')
$valkey->hSetNx('hash', 'key1', 'hello'); /* TRUE, 'key1' => 'hello' in the hash at "h" */
$valkey->hSetNx('hash', 'key1', 'world'); /* FALSE, 'key1' => 'hello' in the hash at "h". No change since the field wasn't replaced. */
```


### hGet
-----
_**Description**_: Gets a value from the hash stored at key. If the hash table doesn't exist, or the key doesn't exist, `FALSE` is returned.
##### *Parameters*
*key*  
*hashKey*

##### *Return value*
*STRING* The value, if the command executed successfully  
*BOOL* `FALSE` in case of failure


### hLen
-----
_**Description**_: Returns the length of a hash, in number of items
##### *Parameters*
*key*

##### *Return value*
*LONG* the number of items in a hash, `FALSE` if the key doesn't exist or isn't a hash.
##### *Example*

```php
$valkey->del('hash')
$valkey->hSet('hash', 'key1', 'hello');
$valkey->hSet('hash', 'key2', 'plop');
$valkey->hLen('hash'); /* returns 2 */
```

### hDel
-----
_**Description**_: Removes a value from the hash stored at key. If the hash table doesn't exist, or the key doesn't exist, `FALSE` is returned.
##### *Parameters*
*key*  
*hashKey1*  
*hashKey2*  
...

##### *Return value*
*LONG* the number of deleted keys, 0 if the key doesn't exist, `FALSE` if the key isn't a hash.


### hKeys
-----
_**Description**_: Returns the keys in a hash, as an array of strings.

##### *Parameters*
*Key*: key

##### *Return value*
An array of elements, the keys of the hash. This works like PHP's array_keys().

##### *Example*

```php
$valkey->del('hash');
$valkey->hSet('hash', 'a', 'x');
$valkey->hSet('hash', 'b', 'y');
$valkey->hSet('hash', 'c', 'z');
$valkey->hSet('hash', 'd', 't');
var_dump($valkey->hKeys('hash'));
```

Output:
```
array(4) {
  [0]=>
  string(1) "a"
  [1]=>
  string(1) "b"
  [2]=>
  string(1) "c"
  [3]=>
  string(1) "d"
}
```
The order is random and corresponds to redis' own internal representation of the set structure.

### hVals
-----
_**Description**_: Returns the values in a hash, as an array of strings.

##### *Parameters*
*Key*: key

##### *Return value*
An array of elements, the values of the hash. This works like PHP's array_values().

##### *Example*

```php
$valkey->del('hash');
$valkey->hSet('hash', 'a', 'x');
$valkey->hSet('hash', 'b', 'y');
$valkey->hSet('hash', 'c', 'z');
$valkey->hSet('hash', 'd', 't');
var_dump($valkey->hVals('hash'));
```

Output:
```
array(4) {
  [0]=>
  string(1) "x"
  [1]=>
  string(1) "y"
  [2]=>
  string(1) "z"
  [3]=>
  string(1) "t"
}
```
The order is random and corresponds to redis' own internal representation of the set structure.

### hGetAll
-----
_**Description**_: Returns the whole hash, as an array of strings indexed by strings.

##### *Parameters*
*Key*: key

##### *Return value*
An array of elements, the contents of the hash.

##### *Example*

```php
$valkey->del('hash');
$valkey->hSet('hash', 'a', 'x');
$valkey->hSet('hash', 'b', 'y');
$valkey->hSet('hash', 'c', 'z');
$valkey->hSet('hash', 'd', 't');
var_dump($valkey->hGetAll('hash'));
```

Output:
```
array(4) {
  ["a"]=>
  string(1) "x"
  ["b"]=>
  string(1) "y"
  ["c"]=>
  string(1) "z"
  ["d"]=>
  string(1) "t"
}
```
The order is random and corresponds to redis' own internal representation of the set structure.

### hExists
-----
_**Description**_: Verify if the specified member exists in a key.
##### *Parameters*
*key*  
*memberKey*
##### *Return value*
*BOOL*: If the member exists in the hash table, return `TRUE`, otherwise return `FALSE`.
##### *Examples*
```php
$valkey->hSet('hash', 'a', 'x');
$valkey->hExists('hash', 'a'); /*  TRUE */
$valkey->hExists('hash', 'NonExistingKey'); /* FALSE */
```

### hIncrBy
-----
_**Description**_: Increments the value of a member from a hash by a given amount.
##### *Parameters*
*key*  
*member*  
*value*: (integer) value that will be added to the member's value
##### *Return value*
*LONG* the new value
##### *Examples*
```php
$valkey->del('hash');
$valkey->hIncrBy('hash', 'x', 2); /* returns 2: h[x] = 2 now. */
$valkey->hIncrBy('hash', 'x', 1); /* h[x] ← 2 + 1. Returns 3 */
```

### hIncrByFloat
-----
_**Description**_: Increments the value of a hash member by the provided float value
##### *Parameters*
*key*  
*member*  
*value*: (float) value that will be added to the member's value
##### *Return value*
*FLOAT* the new value
##### *Examples*
```php
$valkey->del('hash');
$valkey->hIncrByFloat('hash','x', 1.5); /* returns 1.5: h[x] = 1.5 now */
$valkey->hIncrByFloat('hash', 'x', 1.5); /* returns 3.0: h[x] = 3.0 now */
$valkey->hIncrByFloat('hash', 'x', -3.0); /* returns 0.0: h[x] = 0.0 now */
```

### hMSet
-----
_**Description**_: Fills in a whole hash. Non-string values are converted to string, using the standard `(string)` cast. NULL values are stored as empty strings.
##### *Parameters*
*key*  
*members*: key → value array
##### *Return value*
*BOOL*
##### *Examples*
```php
$valkey->del('user:1');
$valkey->hMSet('user:1', ['name' => 'Joe', 'salary' => 2000]);
$valkey->hIncrBy('user:1', 'salary', 100); // Joe earns 100 more now.
```

### hMGet
-----
_**Description**_: Retrieve the values associated to the specified fields in the hash.
##### *Parameters*
*key*  
*memberKeys* Array
##### *Return value*
*Array* An array of elements, the values of the specified fields in the hash, with the hash keys as array keys.
##### *Examples*
```php
$valkey->del('hash');
$valkey->hSet('hash', 'field1', 'value1');
$valkey->hSet('hash', 'field2', 'value2');
$valkey->hMGet('hash', ['field1', 'field2']); /* returns ['field1' => 'value1', 'field2' => 'value2'] */
```

### hScan
-----
_**Description**_:  Scan a HASH value for members, with an optional pattern and count
##### *Parameters*
*key*: String  
*iterator*: Long (reference)  
*pattern*: Optional pattern to match against  
*count*: How many keys to return in a go (only a suggestion to Valkey)
##### *Return value*
*Array* An array of members that match our pattern

##### *Examples*
```php
$it = NULL;
/* Don't ever return an empty array until we're done iterating */
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_RETRY);
while($arr_keys = $valkey->hScan('hash', $it)) {
    foreach($arr_keys as $str_field => $str_value) {
        echo "$str_field => $str_value\n"; /* Print the hash member and value */
    }
}
```

### hStrLen
-----
_**Description**_: Get the string length of the value associated with field in the hash stored at key.
##### *Parameters*
*key*: String  
*field*: String
##### *Return value*
*LONG* the string length of the value associated with field, or zero when field is not present in the hash or key does not exist at all.
