# Valkey PHP - Sorted sets

|Command                                    |Description                                                                                |Supported  |Tested     |Class/Trait    |Method             |
|---                                        |---                                                                                        |:-:        |:-:        |---            |---                |
|[bzPop](#bzPop)                            |Block until Redis can pop the highest or lowest scoring member from one or more ZSETs.     |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |bzPop              |
|[bzPopMin](#bzPopMin)                      |Block until Redis can pop the lowest scoring member from one or more ZSETs.                |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |bzPopMin           |
|[bzPopMax](#bzPopMax)                      |Block until Redis can pop the highest scoring member from one or more ZSETs.               |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |bzPopMax           |
|[zAdd](#zAdd)                              |Add one or more members to a sorted set or update its score if it already exists.          |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zAdd               |
|[zCard](#zCard)                            |Get the number of members in a sorted set.                                                 |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zCard              |
|[zSize](#zSize)                            |Get the number of members in a sorted set.                                                 |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zSize              |
|[zCount](#zCount)                          |Count the members in a sorted set with scores within the given values.                     |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zCount             |
|[zIncrBy](#zIncrBy)                        |Increment the score of a member in a sorted set.                                           |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zIncrBy            |
|[zinterstore](#zinterstore)                |Intersect multiple sorted sets and store the resulting sorted set in a new key.            |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zinterstore        |
|[zInter](#zInter)                          |Intersect multiple sorted sets and store the resulting sorted set in a new key.            |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zInter             |
|[zPop](#zPop)                              |Redis can pop the highest or lowest scoring member from one a ZSET.                        |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zPop               |
|[zRange](#zRange)                          |Return a range of members in a sorted set, by index.                                       |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRange             |
|[zRangeByScore](#zRangeByScore)            |Return a range of members in a sorted set, by score.                                       |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRangeByScore      |
|[zRevRangeByScore](#zRevRangeByScore)      |Return a range of members in a sorted set, by score.                                       |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRevRangeByScore   |
|[zRangeByLex](#zRangeByLex)                |Return a lexicographical range from members that share the same score.                     |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRangeByLex        |
|[zRank](#zRank)                            |Determine the index of a member in a sorted set.                                           |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRank              |
|[zRevRank](#zRevRank)                      |Determine the index of a member in a sorted set.                                           |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRevRank           |
|[zRem](#zRem)                              |Remove one or more members from a sorted set.                                              |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRem               |
|[zDelete](#zDelete)                        |Remove one or more members from a sorted set.                                              |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zDelete            |
|[zRemove](#zRemove)                        |Remove one or more members from a sorted set.                                              |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRemove            |
|[zRemRangeByRank](#zRemRangeByRank)        |Remove all members in a sorted set within the given indexes.                               |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRemRangeByRank    |
|[zDeleteRangeByRank](#zDeleteRangeByRank)  |Remove all members in a sorted set within the given indexes.                               |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zDeleteRangeByRank |
|[zRemRangeByScore](#zRemRangeByScore)      |Remove all members in a sorted set within the given scores.                                |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRemRangeByScore   |
|[zDeleteRangeByScore](#zDeleteRangeByScore)|Remove all members in a sorted set within the given scores.                                |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zDeleteRangeByScore|
|[zRemoveRangeByScore](#zRemoveRangeByScore)|Remove all members in a sorted set within the given scores.                                |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRemoveRangeByScore|
|[zRevRange](#zRevRange)                    |Return a range of members in a sorted set, by index, with scores ordered from high to low. |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zRevRange          |
|[zScore](#zScore)                          |Get the score associated with the given member in a sorted set.                            |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zScore             |
|[zunionstore](#zunionstore)                |Add multiple sorted sets and store the resulting sorted set in a new key.                  |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zunionstore        |
|[zUnion](#zUnion)                          |Add multiple sorted sets and store the resulting sorted set in a new key.                  |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zUnion             |
|[zScan](#zScan)                            |Scan a sorted set for members.                                                             |:white\_check\_mark:        |:white\_check\_mark:        |SortedSets     |zScan              |

BZMPOP Removes and returns a member by score from one or more sorted sets. Blocks until a member is available otherwise. Deletes the sorted set if the last element was popped.
BZPOPMAX Removes and returns the member with the highest score from one or more sorted sets. Blocks until a member available otherwise. Deletes the sorted set if the last element was popped.
BZPOPMIN Removes and returns the member with the lowest score from one or more sorted sets. Blocks until a member is available otherwise. Deletes the sorted set if the last element was popped.
ZADD Adds one or more members to a sorted set, or updates their scores. Creates the key if it doesn't exist.
ZCARD Returns the number of members in a sorted set.
ZCOUNT Returns the count of members in a sorted set that have scores within a range.
ZDIFF Returns the difference between multiple sorted sets.
ZDIFFSTORE Stores the difference of multiple sorted sets in a key.
ZINCRBY Increments the score of a member in a sorted set.
ZINTER Returns the intersect of multiple sorted sets.
ZINTERCARD Returns the number of members of the intersect of multiple sorted sets.
ZINTERSTORE Stores the intersect of multiple sorted sets in a key.
ZLEXCOUNT Returns the number of members in a sorted set within a lexicographical range.
ZMPOP Returns the highest- or lowest-scoring members from one or more sorted sets after removing them. Deletes the sorted set if the last member was popped.
ZMSCORE Returns the score of one or more members in a sorted set.
ZPOPMAX Returns the highest-scoring members from a sorted set after removing them. Deletes the sorted set if the last member was popped.
ZPOPMIN Returns the lowest-scoring members from a sorted set after removing them. Deletes the sorted set if the last member was popped.
ZRANDMEMBER Returns one or more random members from a sorted set.
ZRANGE Returns members in a sorted set within a range of indexes.
ZRANGEBYLEX Returns members in a sorted set within a lexicographical range.
ZRANGEBYSCORE Returns members in a sorted set within a range of scores.
ZRANGESTORE Stores a range of members from sorted set in a key.
ZRANK Returns the index of a member in a sorted set ordered by ascending scores.
ZREM Removes one or more members from a sorted set. Deletes the sorted set if all members were removed.
ZREMRANGEBYLEX Removes members in a sorted set within a lexicographical range. Deletes the sorted set if all members were removed.
ZREMRANGEBYRANK Removes members in a sorted set within a range of indexes. Deletes the sorted set if all members were removed.
ZREMRANGEBYSCORE Removes members in a sorted set within a range of scores. Deletes the sorted set if all members were removed.
ZREVRANGE Returns members in a sorted set within a range of indexes in reverse order.
ZREVRANGEBYLEX Returns members in a sorted set within a lexicographical range in reverse order.
ZREVRANGEBYSCORE Returns members in a sorted set within a range of scores in reverse order.
ZREVRANK Returns the index of a member in a sorted set ordered by descending scores.
ZSCAN Iterates over members and scores of a sorted set.
ZSCORE Returns the score of a member in a sorted set.
ZUNION Returns the union of multiple sorted sets.
ZUNIONSTORE Stores the union of multiple sorted sets in a key.

* [bzPop](#bzpop) - Block until Valkey can pop the highest or lowest scoring member from one or more ZSETs.
* [zAdd](#zadd) - Add one or more members to a sorted set or update its score if it already exists
* [zCard](#zcard) - Get the number of members in a sorted set
* [zCount](#zcount) - Count the members in a sorted set with scores within the given values
* [zDiff](#zdiff) - Computes the difference between the first and all successive input sorted sets and return the resulting sorted set
* [zdiffstore](#zdiffstore) - Computes the difference between the first and all successive input sorted sets and stores the result in a new key
* [zIncrBy](#zincrby) - Increment the score of a member in a sorted set
* [zInter](#zinter) - Intersect multiple sorted sets and return the resulting sorted set
* [zinterstore](#zinterstore) - Intersect multiple sorted sets and store the resulting sorted set in a new key
* [zMscore](#zmscore) - Get the scores associated with the given members in a sorted set
* [zPop](#zpop) - Valkey can pop the highest or lowest scoring member from one a ZSET.
* [zRange](#zrange) - Return a range of members in a sorted set, by index
* [zRangeByScore, zRevRangeByScore](#zrangebyscore-zrevrangebyscore) - Return a range of members in a sorted set, by score
* [zRangeByLex](#zrangebylex) - Return a lexicographical range from members that share the same score
* [zRank, zRevRank](#zrank-zrevrank) - Determine the index of a member in a sorted set
* [zRem](#zrem) - Remove one or more members from a sorted set
* [zRemRangeByRank](#zremrangebyrank) - Remove all members in a sorted set within the given indexes
* [zRemRangeByScore](#zremrangebyscore) - Remove all members in a sorted set within the given scores
* [zRevRange](#zrevrange) - Return a range of members in a sorted set, by index, with scores ordered from high to low
* [zScore](#zscore) - Get the score associated with the given member in a sorted set
* [zUnion](#zunion) - Add multiple sorted sets and return the resulting sorted set
* [zunionstore](#zunionstore) - Add multiple sorted sets and store the resulting sorted set in a new key
* [zScan](#zscan) - Scan a sorted set for members

### bzPop
-----
_**Description**_: Block until Valkey can pop the highest or lowest scoring members from one or more ZSETs.  There are two commands (`BZPOPMIN` and `BZPOPMAX` for popping the lowest and highest scoring elements respectively.)

##### *Prototype*
```php
$valkey->bzPopMin(array $keys, int $timeout): array
$valkey->bzPopMax(array $keys, int $timeout): array

$valkey->bzPopMin(string $key1, string $key2, ... int $timeout): array
$valkey->bzPopMax(string $key1, string $key2, ... int $timeout): array
```

##### *Return value*
*ARRAY:* Either an array with the key member and score of the highest or lowest element or an empty array if the timeout was reached without an element to pop.

##### *Example*

```php
/* Wait up to 5 seconds to pop the *lowest* scoring member from sets `zs1` and `zs2`. */
$valkey->bzPopMin(['zs1', 'zs2'], 5);
$valkey->bzPopMin('zs1', 'zs2', 5);

/* Wait up to 5 seconds to pop the *highest* scoring member from sets `zs1` and `zs2` */
$valkey->bzPopMax(['zs1', 'zs2'], 5);
$valkey->bzPopMax('zs1', 'zs2', 5);
```

**Note:** Calling these functions with an array of keys or with a variable number of arguments is functionally identical.

### zAdd
-----
_**Description**_: Add one or more members to a sorted set or update its score if it already exists


##### *Prototype*
```php
$valkey->zAdd($key, [ $options ,] $score, $value [, $score1, $value1, ...]);
```

##### *Parameters*
*key*: string
*options*: array (optional)
*score*: double  
*value*: string
*score1*: double
*value1*: string

##### *Return value*
*Long* 1 if the element is added. 0 otherwise.

##### *Example*

```php
$valkey->zAdd('key', 1, 'val1');
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 5, 'val5');
$valkey->zRange('key', 0, -1); // [val0, val1, val5]

// From Valkey 3.0.2 it's possible to add options like XX, NX, CH, INCR
$valkey->zAdd('key', ['CH'], 5, 'val5', 10, 'val10', 15, 'val15');
```

### zCard
-----
_**Description**_: Returns the cardinality of an ordered set.

##### *Parameters*
*key*

##### *Return value*
*Long*, the set's cardinality

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 2, 'val2');
$valkey->zAdd('key', 10, 'val10');
$valkey->zCard('key'); /* 3 */
```

### zCount
-----
_**Description**_: Returns the *number* of elements of the sorted set stored at the specified key which have scores in the range [start,end]. Adding a parenthesis before `start` or `end` excludes it from the range. +inf and -inf are also valid limits.

##### *Parameters*
*key*  
*start*: string  
*end*: string

##### *Return value*
*LONG* the size of a corresponding zRangeByScore.

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 2, 'val2');
$valkey->zAdd('key', 10, 'val10');
$valkey->zCount('key', 0, 3); /* 2, corresponding to ['val0', 'val2'] */
```

### zDiff
-----
_**Description**_: Computes the difference between the first and all successive input sorted sets in the first argument.  The result of the difference will be returned.

The second argument is a set of options.  It can define `WITHSCORES` so that the scores are returned as well.

##### *Parameters*
*arrayZSetKeys*  
*arrayOptions* One option is available: `withscores => TRUE`.

##### *Return value*
*ARRAY* The result of the difference of sets.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');
$valkey->zAdd('k1', 3, 'val3');

$valkey->zAdd('k2', 5, 'val1');

$valkey->zAdd('k3', 5, 'val0');
$valkey->zAdd('k3', 3, 'val4');

$valkey->zDiff(['k1', 'k2']); 				                 /* ['val0', 'val3'] */
$valkey->zDiff(['k2', 'k1']); 				                 /* [] */
$valkey->zDiff(['k1', 'k2'], ['withscores' => true]); /* ['val0' => 0.0, 'val3' => 3.0] */

$valkey->zDiff(['k1', 'k2', 'k3']);                   /* ['val3'] */
$valkey->zDiff(['k3', 'k2', 'k1']);                   /* ['val4'] */
```

### zdiffstore
-----
_**Description**_: Computes the difference between the first and all successive input sorted sets in the second argument. The result of the difference will be stored in the sorted set defined by the first argument.

##### *Parameters*
*keyOutput*  
*arrayZSetKeys*  

##### *Return value*
*LONG* The number of values in the new sorted set.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');
$valkey->zAdd('k1', 3, 'val3');

$valkey->zAdd('k2', 5, 'val1');

$valkey->zAdd('k3', 5, 'val0');
$valkey->zAdd('k3', 3, 'val4');

$valkey->zdiffstore('ko1', ['k1', 'k2']); 		   /* 2, 'ko1' => ['val0', 'val3'] */
$valkey->zdiffstore('ko2', ['k2', 'k1']); 			 /* 0, 'ko2' => [] */

$valkey->zdiffstore('ko3', ['k1', 'k2', 'k3']); /* 1, 'ko3' => ['val3'] */
$valkey->zdiffstore('ko4', ['k3', 'k2', 'k1']); /* 1, 'k04' => ['val4'] */
```

### zIncrBy
-----
_**Description**_: Increments the score of a member from a sorted set by a given amount.

##### *Parameters*
*key*  
*value*: (double) value that will be added to the member's score  
*member*

##### *Return value*
*DOUBLE* the new value

##### *Examples*
```php
$valkey->del('key');
$valkey->zIncrBy('key', 2.5, 'member1'); /* key or member1 didn't exist, so member1's score is to 0 before the increment */
                      /* and now has the value 2.5  */
$valkey->zIncrBy('key', 1, 'member1'); /* 3.5 */
```

### zInter
-----
_**Description**_: Creates an intersection of sorted sets given in first argument. The result of the intersection will be returned.

The second optional argument defines `weights` to apply to the sorted sets in input. In this case, the `weights` will be multiplied by the score of each element in the sorted set before applying the aggregation.
The third argument is a set of options.  It can define the `AGGREGATE` option which specify how the results of the intersection are aggregated.  It can also define `WITHSCORES` so that the scores are returned as well.

##### *Parameters*
*arrayZSetKeys*  
*arrayWeights*  
*arrayOptions* Two options are available: `withscores => TRUE`, and `aggregate => $behaviour`.  Either "SUM", "MIN", or "MAX" defines the behaviour to use on duplicate entries during the zinter.

##### *Return value*
*ARRAY* The result of the intersection of sets.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');
$valkey->zAdd('k1', 3, 'val3');

$valkey->zAdd('k2', 5, 'val1');
$valkey->zAdd('k2', 3, 'val3');

$valkey->zinter(['k1', 'k2']); 				/* ['val1', 'val3'] */
$valkey->zinter(['k1', 'k2'], [1, 1]); /* ['val1', 'val3'] */

/* Weighted zinter */
$valkey->zinter(['k1', 'k2'], [1, 5], 'min'); /* ['val1', 'val3'] */
$valkey->zinter(['k1', 'k2'], [1, 5], 'max'); /* ['val3', 'val1'] */
```

### zinterstore
-----
_**Description**_: Creates an intersection of sorted sets given in second argument. The result of the intersection will be stored in the sorted set defined by the first argument.

The third optional argument defines `weights` to apply to the sorted sets in input. In this case, the `weights` will be multiplied by the score of each element in the sorted set before applying the aggregation.
The forth argument defines the `AGGREGATE` option which specify how the results of the intersection are aggregated.

##### *Parameters*
*keyOutput*  
*arrayZSetKeys*  
*arrayWeights*  
*aggregateFunction* Either "SUM", "MIN", or "MAX": defines the behaviour to use on duplicate entries during the zinterstore.

##### *Return value*
*LONG* The number of values in the new sorted set.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');

$valkey->del('ko1');
$valkey->del('ko2');
$valkey->del('ko3');
$valkey->del('ko4');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');
$valkey->zAdd('k1', 3, 'val3');

$valkey->zAdd('k2', 5, 'val1');
$valkey->zAdd('k2', 3, 'val3');

$valkey->zinterstore('ko1', ['k1', 'k2']); 				/* 2, 'ko1' => ['val1', 'val3'] */
$valkey->zinterstore('ko2', ['k1', 'k2'], [1, 1]); /* 2, 'ko2' => ['val1', 'val3'] */

/* Weighted zinterstore */
$valkey->zinterstore('ko3', ['k1', 'k2'], [1, 5], 'min'); /* 2, 'ko3' => ['val1', 'val3'] */
$valkey->zinterstore('ko4', ['k1', 'k2'], [1, 5], 'max'); /* 2, 'ko4' => ['val3', 'val1'] */
```

### zMscore
-----
_**Description**_: Returns the scores of the given members in the specified sorted set.

##### *Parameters*
*key*  
*members*: member1, member2, ... , memberN: Any number of members in the specified sorted set.

##### *Return value*
*ARRAY* or *FALSE* when the key is not found.  Array entries corresponding to members that do not exist will be `false`.

##### *Example*

```php
$valkey->zAdd('key', 2.5, 'val2');
$valkey->zAdd('key', 4.5, 'val4');

$valkey->zMscore('key', 'val2', 'val3', 'val4'); /* [2.5, false, 4.5] */
```

### zPop
-----
_**Description**_: Can pop the highest or lowest scoring members from one ZSETs. There are two commands (`ZPOPMIN` and `ZPOPMAX` for popping the lowest and highest scoring elements respectively.)

##### *Prototype*
```php
$valkey->zPopMin(string $key, int $count): array
$valkey->zPopMax(string $key, int $count): array

$valkey->zPopMin(string $key, int $count): array
$valkey->zPopMax(string $key, int $count): array
```

##### *Return value*
*ARRAY:* Either an array with the key member and score of the highest or lowest element or an empty array if there is no element available.

##### *Example*

```php
/* Pop the *lowest* scoring member from set `zs1`. */
$valkey->zPopMin('zs1', 5);

/* Pop the *highest* scoring member from set `zs1`. */
$valkey->zPopMax('zs1', 5);
```

### zRange
-----
_**Description**_: Returns a range of elements from the ordered set stored at the specified key, with values in the range [start, end].

Start and stop are interpreted as zero-based indices:  
`0` the first element, `1` the second ...  
`-1` the last element, `-2` the penultimate ...  

##### *Parameters*
*key*
*start*: long  
*end*: long  
*withscores*: bool = false

##### *Return value*
*Array* containing the values in specified range.

##### *Example*

```php
$valkey->zAdd('key1', 0, 'val0');
$valkey->zAdd('key1', 2, 'val2');
$valkey->zAdd('key1', 10, 'val10');
$valkey->zRange('key1', 0, -1); /* ['val0', 'val2', 'val10'] */

// with scores
$valkey->zRange('key1', 0, -1, true); /* ['val0' => 0, 'val2' => 2, 'val10' => 10] */
```

### zRangeByScore, zRevRangeByScore
-----
_**Description**_: Returns the elements of the sorted set stored at the specified key which have scores in the range [start,end]. Adding a parenthesis before `start` or `end` excludes it from the range. +inf and -inf are also valid limits. zRevRangeByScore returns the same items in reverse order, when the `start` and `end` parameters are swapped.

##### *Parameters*
*key*  
*start*: string  
*end*: string  
*options*: array

Two options are available: `withscores => TRUE`, and `limit => [$offset, $count]`

##### *Return value*
*Array* containing the values in specified range.

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 2, 'val2');
$valkey->zAdd('key', 10, 'val10');
$valkey->zRangeByScore('key', 0, 3); /* ['val0', 'val2'] */
$valkey->zRangeByScore('key', 0, 3, ['withscores' => TRUE]); /* ['val0' => 0, 'val2' => 2] */
$valkey->zRangeByScore('key', 0, 3, ['limit' => [1, 1]]); /* ['val2'] */
$valkey->zRangeByScore('key', 0, 3, ['withscores' => TRUE, 'limit' => [1, 1]]); /* ['val2' => 2] */
$valkey->zRangeByScore('key', '-inf', '+inf', ['withscores' => TRUE]); /* ['val0' => 0, 'val2' => 2, 'val10' => 10] */
```

### zRangeByLex
-----
_**Description**_:  Returns a lexicographical range of members in a sorted set, assuming the members have the same score.  The min and max values are required to start with '(' (exclusive), '[' (inclusive), or be exactly the values '-' (negative inf) or '+' (positive inf).  The command must be called with either three *or* five arguments or will return FALSE.

##### *Parameters*
*key*: The ZSET you wish to run against  
*min*: The minimum alphanumeric value you wish to get  
*max*: The maximum alphanumeric value you wish to get  
*offset*:  Optional argument if you wish to start somewhere other than the first element.  
*limit*: Optional argument if you wish to limit the number of elements returned.

##### *Return value*
*Array* containing the values in the specified range.

##### *Example*

```php
foreach(['a','b','c','d','e','f','g'] as $c)
    $valkey->zAdd('key',0,$c);

$valkey->zRangeByLex('key','-','[c') /* ['a','b','c']; */
$valkey->zRangeByLex('key','-','(c') /* ['a','b'] */
$valkey->zRangeByLex('key','-','[c',1,2) /* ['b','c'] */
```

### zRank, zRevRank
-----
_**Description**_: Returns the rank of a given member in the specified sorted set, starting at 0 for the item with the smallest score. zRevRank starts at 0 for the item with the *largest* score.

##### *Parameters*
*key*  
*member*

##### *Return value*
*Long*, the item's rank.

##### *Example*

```php
$valkey->del('z');
$valkey->zAdd('key', 1, 'one');
$valkey->zAdd('key', 2, 'two');
$valkey->zRank('key', 'one'); /* 0 */
$valkey->zRank('key', 'two'); /* 1 */
$valkey->zRevRank('key', 'one'); /* 1 */
$valkey->zRevRank('key', 'two'); /* 0 */
```

### zRem
-----
_**Description**_: Delete one or more members from a sorted set.

##### *Prototype*
```php
$valkey->zRem($key, $member [, $member ...]);
```

##### *Return value*
*LONG:* The number of members deleted.

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0', 1, 'val1', 2, 'val2');
$valkey->zRem('key', 'val0', 'val1', 'val2'); // Returns: 3
```

### zRemRangeByRank
-----
_**Description**_: Deletes the elements of the sorted set stored at the specified key which have rank in the range [start,end].

##### *Parameters*
*key*  
*start*: LONG  
*end*: LONG

##### *Return value*
*LONG* The number of values deleted from the sorted set

##### *Example*

```php
$valkey->zAdd('key', 1, 'one');
$valkey->zAdd('key', 2, 'two');
$valkey->zAdd('key', 3, 'three');
$valkey->zRemRangeByRank('key', 0, 1); /* 2 */
$valkey->zRange('key', 0, -1, ['withscores' => TRUE]); /* ['three' => 3] */
```

### zRemRangeByScore
-----
_**Description**_: Deletes the elements of the sorted set stored at the specified key which have scores in the range [start,end].

##### *Parameters*
*key*  
*start*: double or "+inf" or "-inf" string  
*end*: double or "+inf" or "-inf" string

##### *Return value*
*LONG* The number of values deleted from the sorted set

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 2, 'val2');
$valkey->zAdd('key', 10, 'val10');
$valkey->zRemRangeByScore('key', 0, 3); /* 2 */
```

### zRevRange
-----
_**Description**_: Returns the elements of the sorted set stored at the specified key in the range [start, end] in reverse order. start and stop are interpreted as zero-based indices:  
`0` the first element, `1` the second ...  
`-1` the last element, `-2` the penultimate ...

##### *Parameters*
*key*  
*start*: long  
*end*: long  
*withscores*: bool = false

##### *Return value*
*Array* containing the values in specified range.

##### *Example*

```php
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 2, 'val2');
$valkey->zAdd('key', 10, 'val10');
$valkey->zRevRange('key', 0, -1); /* ['val10', 'val2', 'val0'] */

// with scores
$valkey->zRevRange('key', 0, -1, true); /* ['val10' => 10, 'val2' => 2, 'val0' => 0] */
```

### zScore
-----
_**Description**_: Returns the score of a given member in the specified sorted set.

##### *Parameters*
*key*  
*member*

##### *Return value*
*Double* or *FALSE* when the value is not found

##### *Example*

```php
$valkey->zAdd('key', 2.5, 'val2');
$valkey->zScore('key', 'val2'); /* 2.5 */
```

### zUnion
-----
_**Description**_: Creates an union of sorted sets given in first argument. The result of the union will be returned.

The second optional argument defines `weights` to apply to the sorted sets in input. In this case, the `weights` will be multiplied by the score of each element in the sorted set before applying the aggregation.
The third argument is a set of options.  It can define the `AGGREGATE` option which specify how the results of the intersection are aggregated.  It can also define `WITHSCORES` so that the scores are returned as well.

##### *Parameters*
*arrayZSetKeys*  
*arrayWeights*  
*arrayOptions* Two options are available: `withscores => TRUE`, and `aggregate => $behaviour`.  Either "SUM", "MIN", or "MAX" defines the behaviour to use on duplicate entries during the zunion.

##### *Return value*
*ARRAY* The result of the union of sets.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');

$valkey->zAdd('k2', 2, 'val2');
$valkey->zAdd('k2', 3, 'val3');

$valkey->zunion(['k1', 'k2']); /* ['val0', 'val1', 'val2', 'val3'] */

/* Weighted zunion */
$valkey->zunion(['k1', 'k2'], [1, 1]); /* ['val0', 'val1', 'val2', 'val3'] */
$valkey->zunion(['k1', 'k2'], [5, 1]); /* ['val0', 'val2', 'val3', 'val1'] */
```

### zunionstore
-----
_**Description**_: Creates an union of sorted sets given in second argument. The result of the union will be stored in the sorted set defined by the first argument.

The third optional argument defines `weights` to apply to the sorted sets in input. In this case, the `weights` will be multiplied by the score of each element in the sorted set before applying the aggregation.
The forth argument defines the `AGGREGATE` option which specify how the results of the union are aggregated.

##### *Parameters*
*keyOutput*  
*arrayZSetKeys*  
*arrayWeights*  
*aggregateFunction* Either "SUM", "MIN", or "MAX": defines the behaviour to use on duplicate entries during the zunionstore.

##### *Return value*
*LONG* The number of values in the new sorted set.

##### *Example*

```php
$valkey->del('k1');
$valkey->del('k2');
$valkey->del('k3');
$valkey->del('ko1');
$valkey->del('ko2');
$valkey->del('ko3');

$valkey->zAdd('k1', 0, 'val0');
$valkey->zAdd('k1', 1, 'val1');

$valkey->zAdd('k2', 2, 'val2');
$valkey->zAdd('k2', 3, 'val3');

$valkey->zunionstore('ko1', ['k1', 'k2']); /* 4, 'ko1' => ['val0', 'val1', 'val2', 'val3'] */

/* Weighted zunionstore */
$valkey->zunionstore('ko2', ['k1', 'k2'], [1, 1]); /* 4, 'ko2' => ['val0', 'val1', 'val2', 'val3'] */
$valkey->zunionstore('ko3', ['k1', 'k2'], [5, 1]); /* 4, 'ko3' => ['val0', 'val2', 'val3', 'val1'] */
```

### zScan
-----
_**Description**_: Scan a sorted set for members, with optional pattern and count

##### *Parameters*
*key*: String, the set to scan  
*iterator*: Long (reference), initialized to NULL  
*pattern*: String (optional), the pattern to match  
*count*: How many keys to return per iteration (Valkey might return a different number)

##### *Return value*
*Array, boolean* PHPValkey will return matching keys from Valkey, or FALSE when iteration is complete

##### *Example*

```php
$it = NULL;
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_RETRY);
while($arr_matches = $valkey->zScan('zset', $it, '*pattern*')) {
    foreach($arr_matches as $str_mem => $f_score) {
        echo "Key: $str_mem, Score: $f_score\n";
    }
}
```
