## Transactions

1. [multi, exec, discard](#multi-exec-discard) - Enter and exit transactional mode
2. [watch, unwatch](#watch-unwatch) - Watches a key for modifications by another client.

### multi, exec, discard.
-----
_**Description**_: Enter and exit transactional mode.

##### *Parameters*
(optional) `Redis::MULTI` or `Redis::PIPELINE`. Defaults to `Redis::MULTI`. A `Redis::MULTI` block of commands runs as a single transaction; a `Redis::PIPELINE` block is simply transmitted faster to the server, but without any guarantee of atomicity. `discard` cancels a transaction.

##### *Return value*
`multi()` returns the Redis instance and enters multi-mode. Once in multi-mode, all subsequent method calls return the same object until `exec()` is called.

##### *Example*

```php
$ret = $valkey->multi()
    ->set('key1', 'val1')
    ->get('key1')
    ->set('key2', 'val2')
    ->get('key2')
    ->exec();

/*
$ret == [0 => TRUE, 1 => 'val1', 2 => TRUE, 3 => 'val2'];
*/
```

### watch, unwatch
-----
_**Description**_: Watches a key for modifications by another client.

If the key is modified between `WATCH` and `EXEC`, the MULTI/EXEC transaction will fail (return `FALSE`). `unwatch` cancels all the watching of all keys by this client.

##### *Parameters*
*keys*: string for one key or array for a list of keys

##### *Example*

```php
$valkey->watch('x'); // or for a list of keys: $valkey->watch(['x','another key']);
/* long code here during the execution of which other clients could well modify `x` */
$ret = $valkey->multi()
    ->incr('x')
    ->exec();
/*
$ret = FALSE if x has been modified between the call to WATCH and the call to EXEC.
*/
```
