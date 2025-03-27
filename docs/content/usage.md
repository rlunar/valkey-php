# Valkey PHP Usage

1. [Class Valkey](#class-valkey)
2. [Class ValkeyException](#class-valkeyexception)
3. [Predefined constants](#predefined-constants)

## Class Valkey

-----

_**Description**_: Creates a Valkey client

### *Example*

```php
$valkey = new Valkey();
```

Starting from version 6.0.0 it's possible to specify configuration options.
This allows to connect lazily to the server without explicitly invoking `connect` command.

### *Example*

```php
$valkey = new Valkey([
    'host' => '127.0.0.1',
    'port' => 6379,
    'connectTimeout' => 2.5,
    'auth' => ['phpvalkey', 'phpvalkey'],
    'ssl' => ['verify_peer' => false],
    'backoff' => [
        'algorithm' => Valkey::BACKOFF_ALGORITHM_DECORRELATED_JITTER,
        'base' => 500,
        'cap' => 750,
    ],
]);
```

### *Parameters*

_host_: string. can be a host, or the path to a unix domain socket.  
_port_: int (default is 6379, should be -1 for unix domain socket)  
_connectTimeout_: float, value in seconds (default is 0 meaning unlimited)  
_retryInterval_: int, value in milliseconds (optional)  
_readTimeout_: float, value in seconds (default is 0 meaning unlimited)  
_persistent_: mixed, if value is string then it used as persistend id, else value casts to boolean  
_auth_: mixed, authentication information  
_ssl_: array, SSL context options  

## Class ValkeyException

-----

phpvalkey throws a [ValkeyException](#class-valkeyexception) object if it can't reach the Valkey server. That can happen in case of connectivity issues, if the Valkey service is down, or if the valkey host is overloaded. In any other problematic case that does not involve an
unreachable server (such as a key not existing, an invalid command, etc), phpvalkey will return `FALSE`.

## Predefined constants

-----

_**Description**_: Available Valkey Constants

Valkey data types, as returned by [type](#type)

```php
Valkey::VALKEY_STRING; // String
Valkey::VALKEY_SET; // Set
Valkey::VALKEY_LIST; // List
Valkey::VALKEY_ZSET; // Sorted set
Valkey::VALKEY_HASH; // Hash
Valkey::VALKEY_NOT_FOUND; // Not found / other
```

@TODO: OPT_SERIALIZER, AFTER, BEFORE,...
