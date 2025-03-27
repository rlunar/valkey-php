# Valkey PHP - Connections management

|Command                    |Description                                                |Supported              |Tested                 |Class/Trait    |Method     |
|---                        |---                                                        |:-:                    |:-:                    |---            |---        |
|[auth](#auth)              |Authenticate the connectionto the server                   |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |auth       |
|[connect](#connect)        |Connect to a server                                        |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |connect    |
|[close](#close)            |Close the connection                                       |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |close      |
|[echo](#echo)              |Echo the given string                                      |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |echo       |
|[getOption](#getOption)    |Get client option                                          |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |getOption  |
|[open](#open)              |Connect to a server                                        |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |open       |
|[pconnect](#pconnect)      |Connect to a server (persistent)                           |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |pconnect   |
|[ping](#ping)              |Ping the server                                            |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |ping       |
|[popen](#popen)            |Connect to a server (persistent)                           |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |popen      |
|[select](#select)          |Change the selected database for the current connection    |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |select     |
|[setOption](#setOption)    |Set client option                                          |:white\_check\_mark:   |:white\_check\_mark:   |Connections    |setOption  |
|[swapdb](#swapdb)          |Swaps two Valkey databases                                  |:white\_check\_mark:   |:white\_check\_mark:  |Connections    |swapdb     |

## Usage

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->open('127.0.0.1', 6379);
$valkey->pconnect('127.0.0.1', 6379);
$valkey->popen('127.0.0.1', 6379);
$valkey->auth('secret');
$valkey->select(1);
$valkey->swapdb(0, 1);
$valkey->close();
$valkey->setOption(\Valkey::OPT_PREFIX, 'redis:');
$valkey->getOption(\Valkey::OPT_PREFIX)
$valkey->ping('pong');
$valkey->echo('redis');
```

## auth

_**Description**_: Authenticate the connection using a password or a username and password.
*Warning*: The password is sent in plain-text over the network.

##### *Prototype*  

```php
public function auth(string $password | array $credentials = []) : bool {
    return $this->valkey->auth($password);
}
```

##### *Parameters*
*MIXED*: password

##### *Return value*
*BOOL*: `TRUE` if the connection is authenticated, `FALSE` otherwise.

*Note*: In order to authenticate with a username and password you need Valkey >= 6.0.

##### *Example*

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);

/* Authenticate with the password 'foobared' */
$valkey->auth('foobared');

/* Authenticate with the username 'valkey-php', and password 'haxx00r' */
$valkey->auth(['valkey-php', 'haxx00r']);

/* Authenticate with the password 'foobared' */
$valkey->auth(['foobared']);

/* You can also use an associative array specifying user and pass */
$valkey->auth(['user' => 'valkey-php', 'pass' => 'valkey-php']);
$valkey->auth(['pass' => 'valkey-php']);
```

### connect, open
-----
_**Description**_: Connects to a Valkey instance.

##### *Prototype*  

```php
public function connect(
    string $host = '127.0.0.1', 
    int $port = 6379, 
    float $timeout = 0.0, 
    $reserved = null, 
    int $retry_interval = 0, 
    float $read_timeout = 0,
    array $others = []
) : bool
```

##### *Parameters*

*host*: string. can be a host, or the path to a unix domain socket. Starting from version 5.0.0 it is possible to specify schema  
*port*: int, optional  
*timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  
*reserved*: should be '' if retry_interval is specified  
*retry_interval*: int, value in milliseconds (optional)  
*read_timeout*: float, value in seconds (optional, default is 0 meaning it will use default_socket_timeout)  
*others*: array, with PhpValkey >= 5.3.0, it allows setting _auth_ and [_stream_](https://www.php.net/manual/en/context.ssl.php) configuration.  

##### *Return value*

*BOOL*: `TRUE` on success, `FALSE` on error.

##### *Example*

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->connect('127.0.0.1'); // port 6379 by default
$valkey->connect('tls://127.0.0.1', 6379); // enable transport level security.
$valkey->connect('tls://127.0.0.1'); // enable transport level security, port 6379 by default.
$valkey->connect('127.0.0.1', 6379, 2.5); // 2.5 sec timeout.
$valkey->connect('/tmp/redis.sock'); // unix domain socket.
$valkey->connect('127.0.0.1', 6379, 1, '', 100); // 1 sec timeout, 100ms delay between reconnection attempts.
$valkey->connect('/tmp/redis.sock', 0, 1.5, NULL, 0, 1.5); // Unix socket with 1.5s timeouts (connect and read)

/* With PhpValkey >= 5.3.0 you can specify authentication and stream information on connect */
$valkey->connect('127.0.0.1', 6379, 1, '', 0, 0, ['auth' => ['valkey-php', 'valkey-php']]);
```

**Note:** `open` is an alias for `connect` and will be removed in future versions of valkey-php.

## close

-----

_**Description**_: Disconnects from the Valkey instance.

*Note*: Closing a persistent connection requires PhpValkey >= 4.2.0.

##### *Prototype*  

```php
public function close() : bool {
    return $this->valkey->close();
}
```

##### *Parameters*

None.

##### *Return value*
*BOOL*: `TRUE` on success, `FALSE` on failure.

##### *Example*

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->close();
```

### pconnect, popen

-----

_**Description**_: Connects to a Valkey instance or reuse a connection already established with `pconnect`/`popen`.

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
$valkey->pconnect('127.0.0.1', 6379);
$valkey->pconnect('127.0.0.1'); // port 6379 by default - same connection like before.
$valkey->pconnect('tls://127.0.0.1', 6379); // enable transport level security.
$valkey->pconnect('tls://127.0.0.1'); // enable transport level security, port 6379 by default.
$valkey->pconnect('127.0.0.1', 6379, 2.5); // 2.5 sec timeout and would be another connection than the two before.
$valkey->pconnect('127.0.0.1', 6379, 2.5, 'x'); // x is sent as persistent_id and would be another connection than the three before.
$valkey->pconnect('/tmp/redis.sock'); // unix domain socket - would be another connection than the four before.
```

**Note:** `popen` is an alias for `pconnect` and will be removed in future versions of valkey-php.

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

_**Description**_:  Swap one Valkey database with another atomically  

##### *Parameters*  
*INTEGER*: db1  
*INTEGER*: db2  

##### *Return value*  
`TRUE` on success and `FALSE` on failure.

*Note*: Requires Valkey >= 4.0.0

##### *Example*  
```php
$valkey->swapdb(0, 1); /* Swaps DB 0 with DB 1 atomically */
```

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
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_NONE);	  // Don't serialize data
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_PHP);	  // Use built-in serialize/unserialize
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_IGBINARY); // Use igBinary serialize/unserialize
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_MSGPACK);  // Use msgpack serialize/unserialize
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_JSON);  // Use JSON to serialize/unserialize

$valkey->setOption(Valkey::OPT_PREFIX, 'myAppName:');	// use custom prefix on all keys

/* Options for the SCAN family of commands, indicating whether to abstract
   empty results from the user.  If set to SCAN_NORETRY (the default), valkey-php
   will just issue one SCAN command at a time, sometimes returning an empty
   array of results.  If set to SCAN_RETRY, valkey-php will retry the scan command
   until keys come back OR Valkey returns an iterator of zero
*/
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_NORETRY);
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_RETRY);

/* Scan can also be configured to automatically prepend the currently set PhpValkey
   prefix to any MATCH pattern. */
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_PREFIX);
$valkey->setOption(Valkey::OPT_SCAN, Valkey::SCAN_NOPREFIX);
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
// return Valkey::SERIALIZER_NONE, Valkey::SERIALIZER_PHP, 
//        Valkey::SERIALIZER_IGBINARY, Valkey::SERIALIZER_MSGPACK or Valkey::SERIALIZER_JSON
$valkey->getOption(Valkey::OPT_SERIALIZER);
```

### ping

-----

_**Description**_: Check the current connection status.

##### *Prototype*
```php
$valkey->ping([string $message]);
```

##### *Return value*
*Mixed*:  This method returns `TRUE` on success, or the passed string if called with an argument.

##### *Example*

```php
/* When called without an argument, PING returns `TRUE` */
$valkey->ping();

/* If passed an argument, that argument is returned.  Here 'hello' will be returned */
$valkey->ping('hello');
```

*Note*:  Prior to PhpValkey 5.0.0 this command simply returned the string `+PONG`.

### echo

-----

_**Description**_: Sends a string to Valkey, which replies with the same string

##### *Parameters*

*STRING*: The message to send.

##### *Return value*

*STRING*: the same message.
