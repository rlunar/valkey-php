## HyperLogLogs

### pfAdd
-----

_**Description**_:  Adds the specified elements to the specified HyperLogLog.

##### *Prototype*  
```php
$redis->pfAdd($key, Array $elements);
```

##### *Parameters*
_Key_  
_Array of values_  

##### *Return value*
*Integer*:  1 if at least 1 HyperLogLog internal register was altered. 0 otherwise.

##### *Example*
```php
$redis->pfAdd('hll', ['a', 'b', 'c']); // (int) 1
$redis->pfAdd('hll', ['a', 'b']); // (int) 0
```

### pfCount
-----

_**Description**_:  Return the approximated cardinality of the set(s) observed by the HyperLogLog at key(s).

##### *Prototype*  
```php
$redis->pfCount($key);
$redis->pfCount(Array $keys);
```

##### *Parameters*
_Key_ or _Array of keys_  

##### *Return value*
*Integer*:  The approximated number of unique elements observed via [pfAdd](#pfAdd).

##### *Example*
```php
$redis->pfAdd('hll1', ['a', 'b', 'c']); // (int) 1
$redis->pfCount('hll1'); // (int) 3

$redis->pfAdd('hll2', ['d', 'e', 'a']); // (int) 1
$redis->pfCount('hll2'); // (int) 3

$redis->pfCount(['hll1', 'hll2']); // (int) 5
```

### pfMerge
-----

_**Description**_:  Merge N different HyperLogLogs into a single one.

##### *Prototype*  
```php
$redis->pfMerge($destkey, Array $sourceKeys);
```

##### *Parameters*
_Destination Key_  
_Array of Source Keys_  

##### *Return value*
*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*
```php
$redis->pfAdd('hll1', ['a', 'b', 'c']); // (int) 1
$redis->pfAdd('hll2', ['d', 'e', 'a']); // (int) 1

$redis->pfMerge('hll3', ['hll1', 'hll2']); // true

$redis->pfCount('hll3'); // (int) 5
```