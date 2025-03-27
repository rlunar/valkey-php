## Sets

|Command                    |Description                                                    |Supported  |Tested     |Class/Trait    |Method         |
|---                        |---                                                            |:-:        |:-:        |---            |---            |
|[sAdd](#sAdd)              |Add one or more members to a set.                              |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sAdd           |
|[sCard](#sCard)            |Get the number of members in a set.                            |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sCard          |
|[sSize](#sSize)            |Get the number of members in a set.                            |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sSize          |
|[sDiff](#sDiff)            |Subtract multiple sets.                                        |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sDiff          |
|[sDiffStore](#sDiffStore)  |Subtract multiple sets and store the resulting set in a key.   |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sDiffStore     |
|[sInter](#sInter)          |Intersect multiple sets.                                       |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sInter         |
|[sInterStore](#sInterStore)|Intersect multiple sets and store the resulting set in a key.  |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sInterStore    |
|[sIsMember](#sIsMember)    |Determine if a given value is a member of a set.               |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sIsMember      |
|[sContains](#sContains)    |Determine if a given value is a member of a set.               |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sContains      |
|[sMembers](#sMembers)      |Get all the members in a set.                                  |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sMembers       |
|[sGetMembers](#sGetMembers)|Get all the members in a set.                                  |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sGetMembers    |
|[sMove](#sMove)            |Move a member from one set to another.                         |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sMove          |
|[sPop](#sPop)              |Remove and return one or more members of a set at random.      |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sPop           |
|[sRandMember](#sRandMember)|Get one or multiple random members from a set.                 |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sRandMember    |
|[sRem](#sRem)              |Remove one or more members from a set.                         |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sRem           |
|[sRemove](#sRemove)        |Remove one or more members from a set.                         |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sRemove        |
|[sUnion](#sUnion)          |Add multiple sets.                                             |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sUnion         |
|[sUnionStore](#sUnionStore)|Add multiple sets and store the resulting set in a key.        |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sUnionStore    |
|[sScan](#sScan)            |Scan a set for members.                                        |:white\_check\_mark:        |:white\_check\_mark:        |Sets           |sScan          |

SADD Adds one or more members to a set. Creates the key if it doesn't exist.
SCARD Returns the number of members in a set.
SDIFF Returns the difference of multiple sets.
SDIFFSTORE Stores the difference of multiple sets in a key.
SINTER Returns the intersect of multiple sets.
SINTERCARD Returns the number of members of the intersect of multiple sets.
SINTERSTORE Stores the intersect of multiple sets in a key.
SISMEMBER Determines whether a member belongs to a set.
SMEMBERS Returns all members of a set.
SMISMEMBER Determines whether multiple members belong to a set.
SMOVE Moves a member from one set to another.
SPOP Returns one or more random members from a set after removing them. Deletes the set if the last member was popped.
SRANDMEMBER Get one or multiple random members from a set
SREM Removes one or more members from a set. Deletes the set if the last member was removed.
SSCAN Iterates over members of a set.
SUNION Returns the union of multiple sets.
SUNIONSTORE Stores the union of multiple sets in a key.

* [sAdd](#sadd) - Add one or more members to a set
* [sCard](#scard) - Get the number of members in a set
* [sDiff](#sdiff) - Subtract multiple sets
* [sDiffStore](#sdiffstore) - Subtract multiple sets and store the resulting set in a key
* [sInter](#sinter) - Intersect multiple sets
* [sInterStore](#sinterstore) - Intersect multiple sets and store the resulting set in a key
* [sIsMember](#sismember) - Determine if a given value is a member of a set
* [sMembers](#smembers) - Get all the members in a set
* [sMove](#smove) - Move a member from one set to another
* [sPop](#spop) - Remove and return one or more members of a set at random
* [sRandMember](#srandmember) - Get one or multiple random members from a set
* [sRem](#srem) - Remove one or more members from a set
* [sUnion](#sunion) - Add multiple sets
* [sUnionStore](#sunionstore) - Add multiple sets and store the resulting set in a key
* [sScan](#sscan) - Scan a set for members

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member1', 'member2', 'member3'}*/
$valkey->sCard('key1'); /* 3 */
$valkey->sIsMember('key1', 'member1'); /* TRUE */
$valkey->sIsMember('key1', 'memberX'); /* FALSE */
```

### sAdd
-----
_**Description**_: Adds a value to the set value stored at key.
##### *Parameters*
*key*  
*value*

##### *Return value*
*LONG* the number of elements added to the set.
##### *Example*

```php
$valkey->sAdd('key1' , 'member1'); /* 1, 'key1' => {'member1'} */
$valkey->sAdd('key1' , 'member2', 'member3'); /* 2, 'key1' => {'member1', 'member2', 'member3'}*/
$valkey->sAdd('key1' , 'member2'); /* 0, 'key1' => {'member1', 'member2', 'member3'}*/
```

### sCard
-----
_**Description**_: Returns the cardinality of the set identified by key.
##### *Parameters*
*key*
##### *Return value*
*LONG* the cardinality of the set identified by key, 0 if the set doesn't exist.
##### *Example*

```php
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member1', 'member2', 'member3'}*/
$valkey->sCard('key1'); /* 3 */
$valkey->sCard('keyX'); /* 0 */
```

### sDiff
-----
_**Description**_: Performs the difference between N sets and returns it.

##### *Parameters*
*Keys*: key1, key2, ... , keyN: Any number of keys corresponding to sets in redis.

##### *Return value*
*Array of strings*: The difference of the first set will all the others.

##### *Example*

```php
$valkey->del('s0', 's1', 's2');

$valkey->sAdd('s0', '1');
$valkey->sAdd('s0', '2');
$valkey->sAdd('s0', '3');
$valkey->sAdd('s0', '4');

$valkey->sAdd('s1', '1');
$valkey->sAdd('s2', '3');

var_dump($valkey->sDiff('s0', 's1', 's2'));
```
Return value: all elements of s0 that are neither in s1 nor in s2.
```
array(2) {
  [0]=>
  string(1) "4"
  [1]=>
  string(1) "2"
}
```

### sDiffStore
-----
_**Description**_: Performs the same action as sDiff, but stores the result in the first key
##### *Parameters*
*Key*: dstkey, the key to store the diff into.

*Keys*: key1, key2, ... , keyN: Any number of keys corresponding to sets in redis
##### *Return value*
*INTEGER*: The cardinality of the resulting set, or `FALSE` in case of a missing key.

##### *Example*

```php
$valkey->del('s0', 's1', 's2');

$valkey->sAdd('s0', '1');
$valkey->sAdd('s0', '2');
$valkey->sAdd('s0', '3');
$valkey->sAdd('s0', '4');

$valkey->sAdd('s1', '1');
$valkey->sAdd('s2', '3');

var_dump($valkey->sDiffStore('dst', 's0', 's1', 's2'));
var_dump($valkey->sMembers('dst'));
```
Return value: the number of elements of s0 that are neither in s1 nor in s2.
```
int(2)
array(2) {
  [0]=>
  string(1) "4"
  [1]=>
  string(1) "2"
}
```

### sInter
-----
_**Description**_: Returns the members of a set resulting from the intersection of all the sets held at the specified keys.

If just a single key is specified, then this command produces the members of this set. If one of the keys
is missing, `FALSE` is returned.

##### *Parameters*

key1, key2, keyN: keys identifying the different sets on which we will apply the intersection.

##### *Return value*

Array, contain the result of the intersection between those keys. If the intersection between the different sets is empty, the return value will be empty array.

##### *Examples*
```php
$valkey->sAdd('key1', 'val1');
$valkey->sAdd('key1', 'val2');
$valkey->sAdd('key1', 'val3');
$valkey->sAdd('key1', 'val4');

$valkey->sAdd('key2', 'val3');
$valkey->sAdd('key2', 'val4');

$valkey->sAdd('key3', 'val3');
$valkey->sAdd('key3', 'val4');

var_dump($valkey->sInter('key1', 'key2', 'key3'));
```

Output:

```
array(2) {
  [0]=>
  string(4) "val4"
  [1]=>
  string(4) "val3"
}
```

### sInterStore
-----
_**Description**_: Performs a sInter command and stores the result in a new set.
##### *Parameters*
*Key*: dstkey, the key to store the diff into.

*Keys*: key1, key2... keyN. key1..keyN are intersected as in sInter.

##### *Return value*
*INTEGER*: The cardinality of the resulting set, or `FALSE` in case of a missing key.

##### *Example*

```php
$valkey->sAdd('key1', 'val1');
$valkey->sAdd('key1', 'val2');
$valkey->sAdd('key1', 'val3');
$valkey->sAdd('key1', 'val4');

$valkey->sAdd('key2', 'val3');
$valkey->sAdd('key2', 'val4');

$valkey->sAdd('key3', 'val3');
$valkey->sAdd('key3', 'val4');

var_dump($valkey->sInterStore('output', 'key1', 'key2', 'key3'));
var_dump($valkey->sMembers('output'));
```

Output:

```
int(2)

array(2) {
  [0]=>
  string(4) "val4"
  [1]=>
  string(4) "val3"
}
```

### sIsMember
-----
_**Description**_: Checks if `value` is a member of the set stored at the key `key`.
##### *Parameters*
*key*  
*value*

##### *Return value*
*BOOL* `TRUE` if `value` is a member of the set at key `key`, `FALSE` otherwise.
##### *Example*

```php
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member1', 'member2', 'member3'}*/

$valkey->sIsMember('key1', 'member1'); /* TRUE */
$valkey->sIsMember('key1', 'memberX'); /* FALSE */
```

### sMembers
-----
_**Description**_: Returns the contents of a set.

##### *Parameters*
*Key*: key

##### *Return value*
An array of elements, the contents of the set.

##### *Example*

```php
$valkey->del('s');
$valkey->sAdd('s', 'a');
$valkey->sAdd('s', 'b');
$valkey->sAdd('s', 'a');
$valkey->sAdd('s', 'c');
var_dump($valkey->sMembers('s'));
```

Output:
```
array(3) {
  [0]=>
  string(1) "c"
  [1]=>
  string(1) "a"
  [2]=>
  string(1) "b"
}
```
The order is random and corresponds to redis' own internal representation of the set structure.

### sMove
-----
_**Description**_: Moves the specified member from the set at srcKey to the set at dstKey.
##### *Parameters*
*srcKey*  
*dstKey*  
*member*
##### *Return value*
*BOOL* If the operation is successful, return `TRUE`. If the srcKey and/or dstKey didn't exist, and/or the member didn't exist in srcKey, `FALSE` is returned.
##### *Example*

```php
$valkey->sAdd('key1' , 'member11');
$valkey->sAdd('key1' , 'member12');
$valkey->sAdd('key1' , 'member13'); /* 'key1' => {'member11', 'member12', 'member13'}*/
$valkey->sAdd('key2' , 'member21');
$valkey->sAdd('key2' , 'member22'); /* 'key2' => {'member21', 'member22'}*/
$valkey->sMove('key1', 'key2', 'member13'); /* 'key1' =>  {'member11', 'member12'} */
                    /* 'key2' =>  {'member21', 'member22', 'member13'} */

```

### sPop
-----
_**Description**_: Removes and returns a random element from the set value at Key.
##### *Parameters*
*key*  
*count*: Integer, optional
##### *Return value (without count argument)*
*String* "popped" value  
*Bool* `FALSE` if set identified by key is empty or doesn't exist.
##### *Return value (with count argument)*
*Array*: Member(s) returned or an empty array if the set doesn't exist  
*Bool*: `FALSE` on error if the key is not a set
##### *Example*

```php
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member3', 'member1', 'member2'}*/
$valkey->sPop('key1'); /* 'member1', 'key1' => {'member3', 'member2'} */
$valkey->sPop('key1'); /* 'member3', 'key1' => {'member2'} */

/* With count */
$valkey->sAdd('key2', 'member1', 'member2', 'member3');
$valkey->sPop('key2', 3); /* Will return all members but in no particular order */
```

### sRandMember
-----
_**Description**_: Returns a random element from the set value at Key, without removing it.
##### *Parameters*
*key*  
*count* (Integer, optional)
##### *Return value*
If no count is provided, a random *String* value from the set will be returned.  If a count
is provided, an array of values from the set will be returned.  Read about the different
ways to use the count here: [SRANDMEMBER](http://redis.io/commands/srandmember)
*Bool* `FALSE` if set identified by key is empty or doesn't exist.
##### *Example*

```php
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member3', 'member1', 'member2'}*/

// No count
$valkey->sRandMember('key1'); /* 'member1', 'key1' => {'member3', 'member1', 'member2'} */
$valkey->sRandMember('key1'); /* 'member3', 'key1' => {'member3', 'member1', 'member2'} */

// With a count
$valkey->sRandMember('key1', 3); // Will return an array with all members from the set
$valkey->sRandMember('key1', 2); // Will an array with 2 members of the set
$valkey->sRandMember('key1', -100); // Will return an array of 100 elements, picked from our set (with dups)
$valkey->sRandMember('empty-set', 100); // Will return an empty array
$valkey->sRandMember('not-a-set', 100); // Will return FALSE
```

### sRem
-----
_**Description**_: Removes the specified member from the set value stored at key.
##### *Parameters*
*key*  
*member*
##### *Return value*
*LONG* The number of elements removed from the set.
##### *Example*

```php
$valkey->sAdd('key1' , 'member1');
$valkey->sAdd('key1' , 'member2');
$valkey->sAdd('key1' , 'member3'); /* 'key1' => {'member1', 'member2', 'member3'}*/
$valkey->sRem('key1', 'member2', 'member3'); /*return 2. 'key1' => {'member1'} */
```

### sUnion
-----
_**Description**_: Performs the union between N sets and returns it.

##### *Parameters*
*Keys*: key1, key2, ... , keyN: Any number of keys corresponding to sets in redis.

##### *Return value*
*Array of strings*: The union of all these sets.

**Note:** `sUnion` can also take a single array with keys (see example below).

##### *Example*

```php
$valkey->del('s0', 's1', 's2');

$valkey->sAdd('s0', '1');
$valkey->sAdd('s0', '2');
$valkey->sAdd('s1', '3');
$valkey->sAdd('s1', '1');
$valkey->sAdd('s2', '3');
$valkey->sAdd('s2', '4');

/* Get the union with variadic arguments */
var_dump($valkey->sUnion('s0', 's1', 's2'));

/* Pass a single array */
var_dump($valkey->sUnion(['s0', 's1', 's2']));

```
Return value: all elements that are either in s0 or in s1 or in s2.
```
array(4) {
  [0]=>
  string(1) "3"
  [1]=>
  string(1) "4"
  [2]=>
  string(1) "1"
  [3]=>
  string(1) "2"
}
```

### sUnionStore
-----
_**Description**_: Performs the same action as sUnion, but stores the result in the first key

##### *Parameters*
*Key*: dstkey, the key to store the diff into.

*Keys*: key1, key2, ... , keyN: Any number of keys corresponding to sets in redis.

##### *Return value*
*INTEGER*: The cardinality of the resulting set, or `FALSE` in case of a missing key.

##### *Example*

```php
$valkey->del('s0', 's1', 's2');

$valkey->sAdd('s0', '1');
$valkey->sAdd('s0', '2');
$valkey->sAdd('s1', '3');
$valkey->sAdd('s1', '1');
$valkey->sAdd('s2', '3');
$valkey->sAdd('s2', '4');

var_dump($valkey->sUnionStore('dst', 's0', 's1', 's2'));
var_dump($valkey->sMembers('dst'));
```
Return value: the number of elements that are either in s0 or in s1 or in s2.
```
int(4)
array(4) {
  [0]=>
  string(1) "3"
  [1]=>
  string(1) "4"
  [2]=>
  string(1) "1"
  [3]=>
  string(1) "2"
}
```

### sScan
-----
_**Description**_: Scan a set for members

##### *Parameters*
*Key*: The set to search  
*iterator*: LONG (reference) to the iterator as we go  
*pattern*: String, optional pattern to match against  
*count*: How many members to return at a time (Valkey might return a different amount)

##### *Return value*
*Array, boolean*: PHPValkey will return an array of keys or FALSE when we're done iterating

##### *Example*

```php
$it = NULL;
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_RETRY); /* don't return empty results until we're done */
while($arr_mems = $valkey->sScan('set', $it, "*pattern*")) {
    foreach($arr_mems as $str_mem) {
        echo "Member: $str_mem\n";
    }
}

$it = NULL;
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_NORETRY); /* return after each iteration, even if empty */
while(($arr_mems = $valkey->sScan('set', $it, "*pattern*"))!==FALSE) {
    if(count($arr_mems) > 0) {
        foreach($arr_mems as $str_mem) {
            echo "Member found: $str_mem\n";
        }
    } else {
        echo "No members in this iteration, iterator value: $it\n";
    }
}
```