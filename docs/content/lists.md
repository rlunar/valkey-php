# Valkey PHP - Lists

|Command                    |Description                                                                                |Supported  |Tested     |Class/Trait    |Method         |
|---                        |---                                                                                        |:-:        |:-:        |---            |---            |
|[blPop](#blPop)            |Remove and get the first element in a list                                            |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |blPop          |
|[brPop](#brPop)            |Remove and get the last element in a list                                            |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |brPop          |
|[bRPopLPush](#bRPopLPush)  |Pop a value from a list, push it to another list and return it                             |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |bRPopLPush     |
|[lIndex](#lIndex)          |Get an element from a list by its index                                                    |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lIndex         |
|[lGet](#lGet)              |Get an element from a list by its index                                                    |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lGet           |
|[lInsert](#lInsert)        |Insert an element before or after another element in a list                                |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lInsert        |
|[lLen](#lLen)              |Get the length/size of a list                                                              |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lLen           |
|[lSize](#lSize)            |Get the length/size of a list                                                              |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lSize          |
|[lPop](#lPop)              |Remove and get the first element in a list                                                 |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lPop           |
|[lPush](#lPush)            |Prepend one or multiple values to a list                                                   |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lPush          |
|[lPushx](#lPushx)          |Prepend a value to a list, only if the list exists                                         |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lPushx         |
|[lRange](#lRange)          |Get a range of elements from a list                                                        |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lRange         |
|[lGetRange](#lGetRange)    |Get a range of elements from a list                                                        |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lGetRange      |
|[lRem](#lRem)              |Remove elements from a list                                                                |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lRem           |
|[lRemove](#lRemove)        |Remove elements from a list                                                                |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lRemove        |
|[lSet](#lSet)              |Set the value of an element in a list by its index                                         |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lSet           |
|[lTrim](#lTrim)            |Trim a list to the specified range                                                         |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |lTrim          |
|[listTrim](#listTrim)      |Trim a list to the specified range                                                         |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |listTrim       |
|[rPop](#rPop)              |Remove and get the last element in a list                                                  |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |rPop           |
|[rPopLPush](#rPopLPush)    |Remove the last element in a list, append it to another list and return it (redis >= 1.1)  |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |rPopLPush      |
|[rPush](#rPush)            |Append one or multiple values to a list                                                    |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |rPush          |
|[rPushX](#rPushX)          |Append a value to a list, only if the list exists                                          |:white\_check\_mark:        |:white\_check\_mark:        |Lists          |rPushX         |

* [blPop, brPop](#blpop-brpop) - Remove and get the first/last element in a list
* [bRPopLPush](#brpoplpush) - Pop a value from a list, push it to another list and return it
* [lIndex](#lindex) - Get an element from a list by its index
* [lInsert](#linsert) - Insert an element before or after another element in a list
* [lLen](#llen) - Get the length/size of a list
* [lPop](#lpop) - Remove and get the first element in a list
* [lPush](#lpush) - Prepend one or multiple values to a list
* [lPushx](#lpushx) - Prepend a value to a list, only if the list exists
* [lRange](#lrange) - Get a range of elements from a list
* [lRem](#lrem) - Remove elements from a list
* [lSet](#lset) - Set the value of an element in a list by its index
* [lTrim](#ltrim) - Trim a list to the specified range
* [rPop](#rpop) - Remove and get the last element in a list
* [rPopLPush](#rpoplpush) - Remove the last element in a list, append it to another list and return it (redis >= 1.1)
* [rPush](#rpush) - Append one or multiple values to a list
* [rPushX](#rpushx) - Append a value to a list, only if the list exists

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
/* Non blocking feature */
$valkey->lPush('key1', 'A');
$valkey->del('key2');
$valkey->blPop('key1', 'key2', 10); /* ['key1', 'A'] */
$valkey->rPush('key1', 'A');
$valkey->lInsert('key1', Valkey::AFTER, 'A', 'X'); /* 0 */
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'X', 'C'] */
```

### blPop, brPop
-----
_**Description**_: Is a blocking lPop(rPop) primitive. If at least one of the lists contains at least one element, the element will be popped from the head of the list and returned to the caller.
If all the list identified by the keys passed in arguments are empty, blPop will block during the specified timeout until an element is pushed to one of those lists. This element will be popped.

##### *Parameters*
*ARRAY* Array containing the keys of the lists  
*INTEGER* Timeout  
Or  
*STRING* Key1  
*STRING* Key2  
*STRING* Key3  
...  
*STRING* Keyn  
*INTEGER* Timeout

##### *Return value*
*ARRAY* ['listName', 'element']

##### *Example*

```php
/* Non blocking feature */
$valkey->lPush('key1', 'A');
$valkey->del('key2');

$valkey->blPop('key1', 'key2', 10); /* ['key1', 'A'] */
/* OR */
$valkey->blPop(['key1', 'key2'], 10); /* ['key1', 'A'] */

$valkey->brPop('key1', 'key2', 10); /* ['key1', 'A'] */
/* OR */
$valkey->brPop(['key1', 'key2'], 10); /* ['key1', 'A'] */

/* Blocking feature */

/* process 1 */
$valkey->del('key1');
$valkey->blPop('key1', 10);
/* blocking for 10 seconds */

/* process 2 */
$valkey->lPush('key1', 'A');

/* process 1 */
/* ['key1', 'A'] is returned*/
```

### bRPopLPush
-----
_**Description**_: A blocking version of `rPopLPush`, with an integral timeout in the third parameter.

##### *Parameters*
*Key*: srckey  
*Key*: dstkey  
*Long*: timeout

##### *Return value*
*STRING* The element that was moved in case of success, `FALSE` in case of timeout.

### lIndex
-----
_**Description**_: Return the specified element of the list stored at the specified key.

0 the first element, 1 the second ...  
-1 the last element, -2 the penultimate ...

Return `FALSE` in case of a bad index or a key that doesn't point to a list.

##### *Parameters*
*key*  
*index*

##### *Return value*
*String* the element at this index  
*Bool* `FALSE` if the key identifies a non-string data type, or no value corresponds to this index in the list `Key`.

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C'); /* key1 => [ 'A', 'B', 'C' ] */
$valkey->lindex('key1', 0); /* 'A' */
$valkey->lindex('key1', -1); /* 'C' */
$valkey->lindex('key1', 10); /* `FALSE` */
```

### lInsert
-----
_**Description**_: Insert value in the list before or after the pivot value.

The parameter options specify the position of the insert (before or after).
If the list didn't exists, or the pivot didn't exists, the value is not inserted.

##### *Parameters*
*key*  
*position*  Valkey::BEFORE | Valkey::AFTER  
*pivot*  
*value*

##### *Return value*
The number of the elements in the list, -1 if the pivot didn't exists.

##### *Example*

```php
$valkey->del('key1');
$valkey->lInsert('key1', Valkey::AFTER, 'A', 'X'); /* 0 */

$valkey->lPush('key1', 'A');
$valkey->lPush('key1', 'B');
$valkey->lPush('key1', 'C');

$valkey->lInsert('key1', Valkey::BEFORE, 'C', 'X'); /* 4 */
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'X', 'C'] */

$valkey->lInsert('key1', Valkey::AFTER, 'C', 'Y'); /* 5 */
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'X', 'C', 'Y'] */

$valkey->lInsert('key1', Valkey::AFTER, 'W', 'value'); /* -1 */
```

### lPop
-----
_**Description**_: Return and remove the first element of the list.

##### *Parameters*
*key*

##### *Return value*
*STRING* if command executed successfully  
*BOOL* `FALSE` in case of failure (empty list)

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C'); /* key1 => [ 'A', 'B', 'C' ] */
$valkey->lPop('key1'); /* key1 => [ 'B', 'C' ] */
```

### lPush
-----
_**Description**_: Adds one or more values to the head of a LIST.  Creates the list if the key didn't exist. If the key exists and is not a list, `FALSE` is returned.

##### *Prototype*
```php
$valkey->lPush($key, $entry [, $entry, $entry]);
```

##### *Return value*
*LONG* The new length of the list in case of success, `FALSE` in case of Failure.

##### *Examples*
```php
$valkey->del('key1');
$valkey->lPush('key1', 'F'); // returns 1
$valkey->lPush('key1', 'E'); // returns 2
$valkey->lPush('key1', 'D'); // returns 3
/* key1 now contains: [ 'D', 'E', 'F' ] */

$valkey->lPush('key1', 'C', 'B', 'A'); // Returns 6
/* key1 now contains: [ 'A', 'B', 'C', 'D', 'E', 'F' ]
```

### lPushx
-----
_**Description**_: Adds the string value to the head (left) of the list if the list exists.

##### *Parameters*
*key*  
*value* String, value to push in key

##### *Return value*
*LONG* The new length of the list in case of success, `FALSE` in case of Failure.

##### *Examples*
```php
$valkey->del('key1');
$valkey->lPushx('key1', 'A'); // returns 0
$valkey->lPush('key1', 'A'); // returns 1
$valkey->lPushx('key1', 'B'); // returns 2
$valkey->lPushx('key1', 'C'); // returns 3
/* key1 now points to the following list: [ 'A', 'B', 'C' ] */
```

### lRange
-----
_**Description**_: Returns the specified elements of the list stored at the specified key in the range [start, end]. start and stop are interpreted as indices:  
0 the first element, 1 the second ...  
-1 the last element, -2 the penultimate ...

##### *Parameters*
*key*  
*start*  
*end*

##### *Return value*
*Array* containing the values in specified range.

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C');
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'C'] */
```

### lRem
-----
_**Description**_: Removes the first `count` occurrences of the value element from the list. If count is zero, all the matching elements are removed. If count is negative, elements are removed from tail to head.

**Note**: The argument order is not the same as in the Valkey documentation. This difference is kept for compatibility reasons.

##### *Parameters*
*key*  
*value*  
*count*

##### *Return value*
*LONG* the number of elements to remove  
*BOOL* `FALSE` if the value identified by key is not a list.

##### *Example*

```php
$valkey->lPush('key1', 'A');
$valkey->lPush('key1', 'B');
$valkey->lPush('key1', 'C');
$valkey->lPush('key1', 'A');
$valkey->lPush('key1', 'A');

$valkey->lRange('key1', 0, -1); /* ['A', 'A', 'C', 'B', 'A'] */
$valkey->lRem('key1', 'A', 2); /* 2 */
$valkey->lRange('key1', 0, -1); /* ['C', 'B', 'A'] */
```

### lSet
-----
_**Description**_: Set the list at index with the new value.

##### *Parameters*
*key*  
*index*  
*value*

##### *Return value*
*BOOL* `TRUE` if the new value was set. `FALSE` if the index is out of range, or data type identified by key is not a list.

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C'); /* key1 => [ 'A', 'B', 'C' ] */
$valkey->lindex('key1', 0); /* 'A' */
$valkey->lSet('key1', 0, 'X');
$valkey->lindex('key1', 0); /* 'X' */
```

### lTrim
-----
_**Description**_: Trims an existing list so that it will contain only a specified range of elements.

##### *Parameters*
*key*  
*start*  
*stop*

##### *Return value*
*Array*  
*Bool* return `FALSE` if the key identify a non-list value.

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C');
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'C'] */
$valkey->lTrim('key1', 0, 1);
$valkey->lRange('key1', 0, -1); /* ['A', 'B'] */
```

### rPop
-----
_**Description**_: Returns and removes the last element of the list.

##### *Parameters*
*key*

##### *Return value*
*STRING* if command executed successfully  
*BOOL* `FALSE` in case of failure (empty list)

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C'); /* key1 => [ 'A', 'B', 'C' ] */
$valkey->rPop('key1'); /* key1 => [ 'A', 'B' ] */
```

### rPopLPush
-----
_**Description**_: Pops a value from the tail of a list, and pushes it to the front of another list. Also return this value. (redis >= 1.1)

##### *Parameters*
*Key*: srckey  
*Key*: dstkey

##### *Return value*
*STRING* The element that was moved in case of success, `FALSE` in case of failure.

##### *Example*

```php
$valkey->del('x', 'y');

$valkey->lPush('x', 'abc');
$valkey->lPush('x', 'def');
$valkey->lPush('y', '123');
$valkey->lPush('y', '456');

// move the last of x to the front of y.
var_dump($valkey->rPopLPush('x', 'y'));
var_dump($valkey->lRange('x', 0, -1));
var_dump($valkey->lRange('y', 0, -1));

```
Output:
```
string(3) "abc"
array(1) {
  [0]=>
  string(3) "def"
}
array(3) {
  [0]=>
  string(3) "abc"
  [1]=>
  string(3) "456"
  [2]=>
  string(3) "123"
}
```

### rPush
-----
_**Description**_: Adds one or more entries to the tail of a LIST. Valkey will create the list if it doesn't exist.

##### *Prototype*
```php
$valkey->rPush($key, $entry [, $entry, $entry]);
```

##### *Return value*
*LONG* The new length of the list in case of success, `FALSE` in case of Failure.

##### *Examples*
```php
$valkey->del('key1');
$valkey->rPush('key1', 'A');           // returns 1
$valkey->rPush('key1', 'B');           // returns 2
$valkey->rPush('key1', 'C');           // returns 3
$valkey->rPush('key1', 'D', 'E', 'F'); // returns 6
/* key1 now contains: [ 'A', 'B', 'C', 'D', 'E', 'F' ] */
```

### rPushX
-----
_**Description**_: Adds the string value to the tail (right) of the list if the list exists. `FALSE` in case of Failure.

##### *Parameters*
*key*  
*value* String, value to push in key

##### *Return value*
*LONG* The new length of the list in case of success, `FALSE` in case of Failure.

##### *Examples*
```php
$valkey->del('key1');
$valkey->rPushX('key1', 'A'); // returns 0
$valkey->rPush('key1', 'A'); // returns 1
$valkey->rPushX('key1', 'B'); // returns 2
$valkey->rPushX('key1', 'C'); // returns 3
/* key1 now points to the following list: [ 'A', 'B', 'C' ] */
```

### lLen
-----
_**Description**_: Returns the size of a list identified by Key.

If the list didn't exist or is empty, the command returns 0. If the data type identified by Key is not a list, the command return `FALSE`.

##### *Parameters*
*Key*

##### *Return value*
*LONG* The size of the list identified by Key exists.  
*BOOL* `FALSE` if the data type identified by Key is not list

##### *Example*

```php
$valkey->rPush('key1', 'A');
$valkey->rPush('key1', 'B');
$valkey->rPush('key1', 'C'); /* key1 => [ 'A', 'B', 'C' ] */
$valkey->lLen('key1');/* 3 */
$valkey->rPop('key1');
$valkey->lLen('key1');/* 2 */
```