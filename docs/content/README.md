# Valkey PHP - Readme

## Installing/Configuring

-----

### Installation

For everything you should need to install PhpRedis on your system,
see the [INSTALL.md](./INSTALL.md) page.

### PHP Session handler

phpredis can be used to store PHP sessions. To do this, configure `session.save_handler` and `session.save_path` in your php.ini to tell phpredis where to store the sessions.

`session.save_path` can have a simple `host:port` format too, but you need to provide the `tcp://` scheme if you want to use the parameters. The following parameters are available:

- __weight__ (integer): the weight of a host is used in comparison with the others in order to customize the session distribution on several hosts. If host A has twice the weight of host B, it will get twice the amount of sessions. In the example, *host1* stores 20% of all the sessions (1/(1+2+2)) while *host2* and *host3* each store 40% (2/(1+2+2)). The target host is determined once and for all at the start of the session, and doesn't change. The default weight is 1.
- __timeout__ (float): the connection timeout to a redis host, expressed in seconds. If the host is unreachable in that amount of time, the session storage will be unavailable for the client. The default timeout is very high (86400 seconds).
- __persistent__ (integer, should be 1 or 0): defines if a persistent connection should be used.
- __prefix__ (string, defaults to "PHPVALKEY_SESSION:"): used as a prefix to the Redis key in which the session is stored. The key is composed of the prefix followed by the session ID.
- __auth__ (string, or an array with one or two elements): used to authenticate with the server prior to sending commands.
- __database__ (integer): selects a different database.

Sessions have a lifetime expressed in seconds and stored in the INI variable "session.gc_maxlifetime". You can change it with [`ini_set()`](http://php.net/ini_set).
The session handler requires a version of Redis supporting `EX` and `NX` options of `SET` command (at least 2.6.12).
phpredis can also connect to a unix domain socket: `session.save_path = "unix:///var/run/redis/redis.sock?persistent=1&weight=1&database=0"`.

#### Examples

Multiple Redis servers:

```ini
session.save_handler = redis
session.save_path = "tcp://host1:6379?weight=1, tcp://host2:6379?weight=2&timeout=2.5, tcp://host3:6379?weight=2&read_timeout=2.5"
```

Login to Redis using username and password:

```ini
session.save_handler = redis
session.save_path = "tcp://127.0.0.1:6379?auth[]=user&auth[]=password"
```

Login to Redis using username, password, and set prefix:

```ini
session.save_handler = redis
session.save_path = "tcp://127.0.0.1:6379?auth[]=user&auth[]=password&prefix=user_PHPVALKEY_SESSION:"
```

#### Session locking

__Support__: Locking feature is currently only supported for Redis setup with single master instance (e.g. classic master/slave Sentinel environment).
So locking may not work properly in RedisArray or RedisCluster environments.

Following INI variables can be used to configure session locking:

```ini
; Should the locking be enabled? Defaults to: 0.
redis.session.locking_enabled = 1
; How long should the lock live (in seconds)? Defaults to: value of max_execution_time.
redis.session.lock_expire = 60
; How long to wait between attempts to acquire lock, in microseconds (µs)?. Defaults to: 20000
redis.session.lock_wait_time = 50000
; Maximum number of times to retry (-1 means infinite). Defaults to: 100
redis.session.lock_retries = 2000
```

#### Session compression

Following INI variables can be used to configure session compression:

```ini
; Should session compression be enabled? Possible values are zstd, lzf, lz4, none. Defaults to: none
redis.session.compression = zstd
; What compression level should be used? Compression level depends on used library. For most deployments range 1-9 should be fine. Defaults to: 3
redis.session.compression_level = 3
```

### Running the unit tests

phpredis uses a small custom unit test suite for testing functionality of the various classes.  To run tests, simply do the following:

```bash
## Run tests for Redis class (note this is the default)
php tests/TestRedis.php --class Redis

## Run tests for RedisArray class
tests/mkring.sh start
php tests/TestRedis.php --class RedisArray
tests/mkring.sh stop

## Run tests for the RedisCluster class
tests/make-cluster.sh start
php tests/TestRedis.php --class RedisCluster
tests/make-cluster.sh stop

## Run tests for RedisSentinel class
php tests/TestRedis.php --class RedisSentinel
```

Note that it is possible to run only tests which match a substring of the test itself by passing the additional argument '--test <str>' when invoking.

```bash
## Just run the 'echo' test
php tests/TestRedis.php --class Redis --test echo
```
