# Valkey PHP - Transactions

|Command            |Description                        |Supported  |Tested     |Class/Trait    |Method         |
|---                |---                                |:-:        |:-:        |---            |---            |
|[multi](#multi)    |Enter and exit transactional mode. |:white\_check\_mark:        |:white\_check\_mark:        |Transactions   |multi          |
|[exec](#exec)      |Enter and exit transactional mode. |:white\_check\_mark:        |:white\_check\_mark:        |Transactions   |exec           |
|[discard](#discard)|Enter and exit transactional mode. |:white\_check\_mark:        |:white\_check\_mark:        |Transactions   |discard        |
|[watch](#watch)    |Enter and exit transactional mode. |:white\_check\_mark:        |:white\_check\_mark:        |Transactions   |watch          |
|[unwatch](#unwatch)|Enter and exit transactional mode. |:white\_check\_mark:        |:white\_check\_mark:        |Transactions   |unwatch        |

- DISCARD Discards a transaction.
- EXEC Executes all commands in a transaction.
- MULTI Starts a transaction.
- UNWATCH Forgets about watched keys of a transaction.
- WATCH Monitors changes to keys to determine the execution of a transaction.

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$ret = $valkey->multi()
    ->set('key1', 'val1')
    ->get('key1')
    ->set('key2', 'val2')
    ->get('key2')
    ->exec();
```

1. [multi, exec, discard](#multi-exec-discard) - Enter and exit transactional mode
2. [watch, unwatch](#watch-unwatch) - Watches a key for modifications by another client.

### multi, exec, discard.
-----
_**Description**_: Enter and exit transactional mode.

##### *Parameters*
(optional) `Valkey::MULTI` or `Valkey::PIPELINE`. Defaults to `Valkey::MULTI`. A `Valkey::MULTI` block of commands runs as a single transaction; a `Valkey::PIPELINE` block is simply transmitted faster to the server, but without any guarantee of atomicity. `discard` cancels a transaction.

##### *Return value*
`multi()` returns the Valkey instance and enters multi-mode. Once in multi-mode, all subsequent method calls return the same object until `exec()` is called.

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
