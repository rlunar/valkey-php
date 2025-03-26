# Valkey PHP - Docs

## Usage

1. [Class Redis](#class-redis)
1. [Class RedisException](#class-redisexception)
1. [Predefined constants](#predefined-constants)

### Class Redis
-----
_**Description**_: Creates a Redis client

##### *Example*

```php
$redis = new Redis();
```

Starting from version 6.0.0 it's possible to specify configuration options.
This allows to connect lazily to the server without explicitly invoking `connect` command.

##### *Example*

```php
$redis = new Redis([
    'host' => '127.0.0.1',
    'port' => 6379,
    'connectTimeout' => 2.5,
    'auth' => ['phpredis', 'phpredis'],
    'ssl' => ['verify_peer' => false],
    'backoff' => [
        'algorithm' => Redis::BACKOFF_ALGORITHM_DECORRELATED_JITTER,
        'base' => 500,
        'cap' => 750,
    ],
]);
```

##### *Parameters*

*host*: string. can be a host, or the path to a unix domain socket.  
*port*: int (default is 6379, should be -1 for unix domain socket)  
*connectTimeout*: float, value in seconds (default is 0 meaning unlimited)  
*retryInterval*: int, value in milliseconds (optional)  
*readTimeout*: float, value in seconds (default is 0 meaning unlimited)  
*persistent*: mixed, if value is string then it used as persistend id, else value casts to boolean  
*auth*: mixed, authentication information  
*ssl*: array, SSL context options  

### Class RedisException
-----
phpredis throws a [RedisException](#class-redisexception) object if it can't reach the Redis server. That can happen in case of connectivity issues,
if the Redis service is down, or if the redis host is overloaded. In any other problematic case that does not involve an
unreachable server (such as a key not existing, an invalid command, etc), phpredis will return `FALSE`.

### Predefined constants
-----
_**Description**_: Available Redis Constants

Redis data types, as returned by [type](#type)
```
Redis::REDIS_STRING - String
Redis::REDIS_SET - Set
Redis::REDIS_LIST - List
Redis::REDIS_ZSET - Sorted set
Redis::REDIS_HASH - Hash
Redis::REDIS_NOT_FOUND - Not found / other
```

@TODO: OPT_SERIALIZER, AFTER, BEFORE,...