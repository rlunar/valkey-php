## Connection

1. [connect, open](#connect-open) - Connect to a server
1. [pconnect, popen](#pconnect-popen) - Connect to a server (persistent)
1. [auth](#auth) - Authenticate to the server
1. [select](#select) - Change the selected database for the current connection
1. [swapdb](#swapdb) - Swaps two Redis databases
1. [close](#close) - Close the connection
1. [setOption](#setoption) - Set client option
1. [getOption](#getoption) - Get client option
1. [ping](#ping) - Ping the server
1. [echo](#echo) - Echo the given string

### connect, open
-----
_**Description**_: Connects to a Redis instance.

##### *Parameters*

*host*: string. can be a host, or the path to a unix domain socket. Starting from version 5.0.0 it is possible to specify schema  
*port*: int, optional  
*timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  
*reserved*: should be '' if retry_interval is specified  
*retry_interval*: int, value in milliseconds (optional)  
*read_timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  
*others*: array, with PhpRedis >= 5.3.0, it allows setting _auth_ and [_stream_](https://www.php.net/manual/en/context.ssl.php) configuration.  

##### *Return value*

*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*

```php
$redis->connect('127.0.0.1', 6379);
$redis->connect('127.0.0.1'); // port 6379 by default
$redis->connect('tls://127.0.0.1', 6379); // enable transport level security.
$redis->connect('tls://127.0.0.1'); // enable transport level security, port 6379 by default.
$redis->connect('127.0.0.1', 6379, 2.5); // 2.5 sec timeout.
$redis->connect('/tmp/redis.sock'); // unix domain socket.
$redis->connect('127.0.0.1', 6379, 1, '', 100); // 1 sec timeout, 100ms delay between reconnection attempts.
$redis->connect('/tmp/redis.sock', 0, 1.5, NULL, 0, 1.5); // Unix socket with 1.5s timeouts (connect and read)

/* With PhpRedis >= 5.3.0 you can specify authentication and stream information on connect */
$redis->connect('127.0.0.1', 6379, 1, '', 0, 0, ['auth' => ['phpredis', 'phpredis']]);
```

**Note:** `open` is an alias for `connect` and will be removed in future versions of phpredis.

### pconnect, popen
-----
_**Description**_: Connects to a Redis instance or reuse a connection already established with `pconnect`/`popen`.

The connection will not be closed on end of request until the php process ends.
So be prepared for too many open FD's errors (specially on redis server side) when using persistent
connections on many servers connecting to one redis server.

Also more than one persistent connection can be made identified by either host + port + timeout
or host + persistent_id or unix socket + timeout.

Starting from version 4.2.1, it became possible to use connection pooling by setting INI variable `redis.pconnect.pooling_enabled` to 1.

This feature is not available in threaded versions. `pconnect` and `popen` then working like their non
persistent equivalents.

##### *Parameters*

*host*: string. can be a host, or the path to a unix domain socket. Starting from version 5.0.0 it is possible to specify schema  
*port*: int, optional  
*timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  
*persistent_id*: string. identity for the requested persistent connection  
*retry_interval*: int, value in milliseconds (optional)  
*read_timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  

##### *Return value*

*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*

```php
$redis->pconnect('127.0.0.1', 6379);
$redis->pconnect('127.0.0.1'); // port 6379 by default - same connection like before.
$redis->pconnect('tls://127.0.0.1', 6379); // enable transport level security.
$redis->pconnect('tls://127.0.0.1'); // enable transport level security, port 6379 by default.
$redis->pconnect('127.0.0.1', 6379, 2.5); // 2.5 sec timeout and would be another connection than the two before.
$redis->pconnect('127.0.0.1', 6379, 2.5, 'x'); // x is sent as persistent_id and would be another connection than the three before.
$redis->pconnect('/tmp/redis.sock'); // unix domain socket - would be another connection than the four before.
```

**Note:** `popen` is an alias for `pconnect` and will be removed in future versions of phpredis.

### auth
-----
_**Description**_: Authenticate the connection using a password or a username and password.
*Warning*: The password is sent in plain-text over the network.

##### *Parameters*
*MIXED*: password

##### *Return value*
*BOOL*: `TRUE` if the connection is authenticated, `FALSE` otherwise.

*Note*: In order to authenticate with a username and password you need Redis >= 6.0.

##### *Example*
```php
/* Authenticate with the password 'foobared' */
$redis->auth('foobared');

/* Authenticate with the username 'phpredis', and password 'haxx00r' */
$redis->auth(['phpredis', 'haxx00r']);

/* Authenticate with the password 'foobared' */
$redis->auth(['foobared']);

/* You can also use an associative array specifying user and pass */
$redis->auth(['user' => 'phpredis', 'pass' => 'phpredis']);
$redis->auth(['pass' => 'phpredis']);
```

### select
-----
_**Description**_: Change the selected database for the current connection.

##### *Parameters*
*INTEGER*: dbindex, the database number to switch to.

##### *Return value*
`TRUE` in case of success, `FALSE` in case of failure.
##### *Example*
See method for example: [move](#move)

### swapdb
-----
_**Description**_:  Swap one Redis database with another atomically  

##### *Parameters*  
*INTEGER*: db1  
*INTEGER*: db2  

##### *Return value*  
`TRUE` on success and `FALSE` on failure.

*Note*: Requires Redis >= 4.0.0

##### *Example*  
```php
$redis->swapdb(0, 1); /* Swaps DB 0 with DB 1 atomically */
```

### close
-----
_**Description**_: Disconnects from the Redis instance.

*Note*: Closing a persistent connection requires PhpRedis >= 4.2.0.

##### *Parameters*
None.

##### *Return value*
*BOOL*: `TRUE` on success, `FALSE` on failure.

### setOption
-----
_**Description**_: Set client option.

##### *Parameters*
*parameter name*  
*parameter value*

##### *Return value*
*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*
```php
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_NONE);	  // Don't serialize data
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_PHP);	  // Use built-in serialize/unserialize
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_IGBINARY); // Use igBinary serialize/unserialize
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_MSGPACK);  // Use msgpack serialize/unserialize
$redis->setOption(Redis::OPT_SERIALIZER, Redis::SERIALIZER_JSON);  // Use JSON to serialize/unserialize

$redis->setOption(Redis::OPT_PREFIX, 'myAppName:');	// use custom prefix on all keys

/* Options for the SCAN family of commands, indicating whether to abstract
   empty results from the user.  If set to SCAN_NORETRY (the default), phpredis
   will just issue one SCAN command at a time, sometimes returning an empty
   array of results.  If set to SCAN_RETRY, phpredis will retry the scan command
   until keys come back OR Redis returns an iterator of zero
*/
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_NORETRY);
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_RETRY);

/* Scan can also be configured to automatically prepend the currently set PhpRedis
   prefix to any MATCH pattern. */
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_PREFIX);
$redis->setOption(Redis::OPT_SCAN, Redis::SCAN_NOPREFIX);
```


### getOption
-----
_**Description**_: Get client option.

##### *Parameters*
*parameter name*

##### *Return value*
Parameter value.

##### *Example*
```php
// return Redis::SERIALIZER_NONE, Redis::SERIALIZER_PHP, 
//        Redis::SERIALIZER_IGBINARY, Redis::SERIALIZER_MSGPACK or Redis::SERIALIZER_JSON
$redis->getOption(Redis::OPT_SERIALIZER);
```

### ping
-----
_**Description**_: Check the current connection status.

##### *Prototype*
```php
$redis->ping([string $message]);
```

##### *Return value*
*Mixed*:  This method returns `TRUE` on success, or the passed string if called with an argument.

##### *Example*
```php
/* When called without an argument, PING returns `TRUE` */
$redis->ping();

/* If passed an argument, that argument is returned.  Here 'hello' will be returned */
$redis->ping('hello');
```

*Note*:  Prior to PhpRedis 5.0.0 this command simply returned the string `+PONG`.

### echo
-----
_**Description**_: Sends a string to Redis, which replies with the same string

##### *Parameters*

*STRING*: The message to send.

##### *Return value*

*STRING*: the same message.
