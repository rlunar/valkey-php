# Valkey PHP - HyperLogLogs

|Command                    |Description                                                                                |Supported              |Tested                 |Class/Trait    |Method     |
|---                        |---                                                                                        |:-:                    |:-:                    |---            |---        |
|[pfAdd](#pfAdd)            |Adds the specified elements to the specified HyperLogLog.                                  |:white\_check\_mark:   |:white\_check\_mark:   |HyperLogLogs   |pfAdd      |
|[pfCount](#pfCount)        |Return the approximated cardinality of the set(s) observed by the HyperLogLog at key(s).   |:white\_check\_mark:   |:white\_check\_mark:   |HyperLogLogs   |pfCount    |
|[pfDebug](#pfDebug)        |Adds the specified elements to the specified HyperLogLog.                                  |:x:    |:white\_check\_mark:   |HyperLogLogs   |pfDebug    |
|[pfMerge](#pfMerge)        |Adds the specified elements to the specified HyperLogLog.                                  |:white\_check\_mark:   |:white\_check\_mark:   |HyperLogLogs   |pfMerge    |
|[pfSelfTest](#pfSelfTest)  |Adds the specified elements to the specified HyperLogLog.                                  |:x:    |:x:    |HyperLogLogs   |pfSelfTest |

### pfAdd
-----

_**Description**_:  Adds the specified elements to the specified HyperLogLog.

##### *Prototype*  
```php
$valkey->pfAdd($key, Array $elements);
```

##### *Parameters*
_Key_  
_Array of values_  

##### *Return value*
*Integer*:  1 if at least 1 HyperLogLog internal register was altered. 0 otherwise.

##### *Example*

```php
$valkey->pfAdd('hll', ['a', 'b', 'c']); // (int) 1
$valkey->pfAdd('hll', ['a', 'b']); // (int) 0
```

### pfCount
-----

_**Description**_:  Return the approximated cardinality of the set(s) observed by the HyperLogLog at key(s).

##### *Prototype*  
```php
$valkey->pfCount($key);
$valkey->pfCount(Array $keys);
```

##### *Parameters*
_Key_ or _Array of keys_  

##### *Return value*
*Integer*:  The approximated number of unique elements observed via [pfAdd](#pfAdd).

##### *Example*

```php
$valkey->pfAdd('hll1', ['a', 'b', 'c']); // (int) 1
$valkey->pfCount('hll1'); // (int) 3

$valkey->pfAdd('hll2', ['d', 'e', 'a']); // (int) 1
$valkey->pfCount('hll2'); // (int) 3

$valkey->pfCount(['hll1', 'hll2']); // (int) 5
```

### pfMerge
-----

_**Description**_:  Merge N different HyperLogLogs into a single one.

##### *Prototype*  
```php
$valkey->pfMerge($destkey, Array $sourceKeys);
```

##### *Parameters*
_Destination Key_  
_Array of Source Keys_  

##### *Return value*
*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*

```php
$valkey->pfAdd('hll1', ['a', 'b', 'c']); // (int) 1
$valkey->pfAdd('hll2', ['d', 'e', 'a']); // (int) 1

$valkey->pfMerge('hll3', ['hll1', 'hll2']); // true

$valkey->pfCount('hll3'); // (int) 5
```