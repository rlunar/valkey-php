## Server

1. [acl](#acl) - Manage Redis ACLs
1. [bgRewriteAOF](#bgrewriteaof) - Asynchronously rewrite the append-only file
1. [bgSave](#bgsave) - Asynchronously save the dataset to disk (in background)
1. [config](#config) - Get or Set the Redis server configuration parameters
1. [dbSize](#dbsize) - Return the number of keys in selected database
1. [flushAll](#flushall) - Remove all keys from all databases
1. [flushDb](#flushdb) - Remove all keys from the current database
1. [info](#info) - Get information and statistics about the server
1. [lastSave](#lastsave) - Get the timestamp of the last disk save
1. [save](#save) - Synchronously save the dataset to disk (wait to complete)
1. [slaveOf](#slaveof) - Make the server a slave of another instance, or promote it to master
1. [time](#time) - Return the current server time
1. [slowLog](#slowlog) - Access the Redis slowLog entries

### acl
-----
_**Description**_: Execute the Redis ACL command.

##### *Parameters*
_variable_:  Minimum of one argument for `Redis` and two for `RedisCluster`.

##### *Example*

```php
$valkey->acl('USERS'); /* Get a list of users */
$valkey->acl('LOG');   /* See log of Redis' ACL subsystem */
```

*Note*:  In order to user the `ACL` command you must be communicating with Redis >= 6.0 and be logged into an account that has access to administration commands such as ACL.  Please reference [this tutorial](https://redis.io/topics/acl) for an overview of Redis 6 ACLs and [the redis command reference](https://redis.io/commands) for every ACL subcommand.

*Note*: If you are connecting to Redis server >= 4.0.0 you can remove a key with the `unlink` method in the exact same way you would use `del`.  The Redis [unlink](https://redis.io/commands/unlink) command is non-blocking and will perform the actual deletion asynchronously.

### bgRewriteAOF
-----
_**Description**_: Start the background rewrite of AOF (Append-Only File)

##### *Parameters*
None.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure.

##### *Example*

```php
$valkey->bgRewriteAOF();
```

### bgSave
-----
_**Description**_: Asynchronously save the dataset to disk (in background)

##### *Parameters*
None.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure. If a save is already running, this command will fail and return `FALSE`.

##### *Example*

```php
$valkey->bgSave();
```

### config
-----
_**Description**_: Get or Set the Redis server configuration parameters.

##### *Prototype*
```php
$valkey->config(string $operation, string|array|null $key = NULL, ?string $value = NULL): mixed;
```

##### *Return value*
*Associative array* for `GET`, key(s) -> value(s)  
*bool* for `SET`, `RESETSTAT`, and `REWRITE`

##### *Examples*
```php
$valkey->config("GET", "*max-*-entries*");
$valkey->config("SET", ['timeout', 'loglevel']);
$valkey->config("SET", "dir", "/var/run/redis/dumps/");
$valkey->config("SET", ['timeout' => 128, 'loglevel' => 'warning']);
$valkey->config('RESETSTAT');
```

### dbSize
-----
_**Description**_: Return the number of keys in selected database.

##### *Parameters*
None.

##### *Return value*
*INTEGER*: DB size, in number of keys.

##### *Example*

```php
$count = $valkey->dbSize();
echo "Redis has $count keys\n";
```

### flushAll
-----
_**Description**_: Remove all keys from all databases.

##### *Parameters*
*async* (bool) requires server version 4.0.0 or greater

##### *Return value*
*BOOL*: Always `TRUE`.

##### *Example*

```php
$valkey->flushAll();
```

### flushDb
-----
_**Description**_: Remove all keys from the current database.

##### *Prototype*
```php
$valkey->flushdb(?bool $sync = NULL): Redis|bool;
```

##### *Return value*
*BOOL*:  This command returns true on success and false on failure.

##### *Example*

```php
$valkey->flushDb();
```

### info
-----
_**Description**_: Get information and statistics about the server

Returns an associative array that provides information about the server. Passing no arguments to
INFO will call the standard REDIS INFO command, which returns information such as the following:

* redis_version
* arch_bits
* uptime_in_seconds
* uptime_in_days
* connected_clients
* connected_slaves
* used_memory
* changes_since_last_save
* bgsave_in_progress
* last_save_time
* total_connections_received
* total_commands_processed
* role

You can pass a variety of options to INFO ([per the Redis documentation](http://redis.io/commands/info)),
which will modify what is returned.

##### *Parameters*
*option*: The option to provide redis (e.g. "COMMANDSTATS", "CPU")

##### *Example*

```php
$valkey->info(); /* standard redis INFO command */
$valkey->info("COMMANDSTATS"); /* Information on the commands that have been run (>=2.6 only)
$valkey->info("CPU"); /* just CPU information from Redis INFO */
```

### lastSave
-----
_**Description**_: Returns the timestamp of the last disk save.

##### *Parameters*
None.

##### *Return value*
*INT*: timestamp.

##### *Example*

```php
$valkey->lastSave();
```

### save
-----
_**Description**_: Synchronously save the dataset to disk (wait to complete)

##### *Parameters*
None.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure. If a save is already running, this command will fail and return `FALSE`.

##### *Example*

```php
$valkey->save();
```

### slaveOf
-----
_**Description**_: Changes the slave status

##### *Parameters*
Either host (string) and port (int), or no parameter to stop being a slave.

##### *Return value*
*BOOL*: `TRUE` in case of success, `FALSE` in case of failure.

##### *Example*

```php
$valkey->slaveOf('10.0.1.7', 6379);
/* ... */
$valkey->slaveOf();
```

### time
-----
_**Description**_: Return the current server time.

##### *Parameters*
(none)

##### *Return value*
If successful, the time will come back as an associative array with element zero being
the unix timestamp, and element one being microseconds.

##### *Examples*
```php
$valkey->time();
```

### slowLog
-----
_**Description**_: Access the Redis slowLog

##### *Parameters*
*Operation* (string): This can be either `GET`, `LEN`, or `RESET`  
*Length* (integer), optional: If executing a `SLOWLOG GET` command, you can pass an optional length.
#####

##### *Return value*
The return value of SLOWLOG will depend on which operation was performed.
SLOWLOG GET: Array of slowLog entries, as provided by Redis
SLOGLOG LEN: Integer, the length of the slowLog
SLOWLOG RESET: Boolean, depending on success
#####

##### *Examples*
```php
// Get ten slowLog entries
$valkey->slowLog('get', 10);
// Get the default number of slowLog entries

$valkey->slowLog('get');
// Reset our slowLog
$valkey->slowLog('reset');

// Retrieve slowLog length
$valkey->slowLog('len');
```