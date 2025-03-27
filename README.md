# Valkey-PHP

<p align="center">
  <img src="https://valkey.io/img/Valkey-logo.svg" alt="Valkey-PHP Logo" width="200"/>
</p>

<p align="center">
  <a href="https://github.com/valkey-php/valkey-php/actions"><img src="https://github.com/valkey-php/valkey-php/workflows/Tests/badge.svg" alt="Build Status"></a>
  <a href="https://codecov.io/gh/valkey-php/valkey-php"><img src="https://codecov.io/gh/valkey-php/valkey-php/branch/main/graph/badge.svg" alt="Code Coverage"></a>
  <a href="https://packagist.org/packages/valkey/valkey-php"><img src="https://img.shields.io/packagist/v/valkey/valkey-php.svg" alt="Latest Stable Version"></a>
  <a href="https://packagist.org/packages/valkey/valkey-php"><img src="https://img.shields.io/packagist/dt/valkey/valkey-php.svg" alt="Total Downloads"></a>
  <a href="https://github.com/valkey-php/valkey-php/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-BSD--3--Clause-blue.svg" alt="License"></a>
</p>

## About Valkey-PHP

Valkey-PHP is a comprehensive PHP client for [Valkey](https://valkey.io), designed to provide a unified, high-performance interface for PHP applications. This project merges the best features of [valkey-php/valkey-php](https://github.com/valkey-php/valkey-php) and [webdcg/redis](https://github.com/webdcg/redis) into a single, well-maintained library under the Valkey Project by the Linux Foundation.

### Key Features

- **Complete Valkey Command Coverage**: 100% support for all Valkey commands with comprehensive test coverage
- **High-Level Abstractions**: Similar to Valkeyson in Java, offering object-oriented interfaces for complex data structures
- **Dual Interface**: Both a PHP extension (for maximum performance) and a pure PHP implementation (for maximum compatibility)
- **Forever Open Source**: BSD-3-Clause license under the Linux Foundation ensures the project remains open and free
- **Valkey Compatibility**: Maintained in sync with Valkey development while preserving compatibility with Valkey OSS
- **Enterprise-Ready**: Clustering, sentinel support, connection pooling, and other features needed for production deployments

## Pre-requisites

```bash
brew install valkey
brew services start valkey
brew services list
valkey-cli INFO server

brew install autoconf automake libtool php
```

## Installation

### PHP Extension (Recommended for Production)

```bash
pecl install valkey
```

Add the following to your php.ini:

```ini
extension=valkey.so
```

### Composer (PHP Library)

```bash
composer require valkey/valkey-php
```

### Quick Start

```php
<?php

// Using the extension
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->set('key', 'value');
echo $valkey->get('key'); // Outputs: value

// Using the PHP library with fluent interface
use Valkey\Client;

$valkey = new Client();
$valkey->connect('127.0.0.1', 6379)
       ->set('key', 'value')
       ->expire('key', 3600);
       
echo $valkey->get('key'); // Outputs: value

// Using high-level abstractions
$map = $valkey->getMap('user:profile:1');
$map->put('name', 'Robert');
$map->put('email', 'Robert@example.com');

echo $map->get('name'); // Outputs: Robert
```

## High-Level Abstractions

Valkey-PHP provides Valkeyson-like abstractions for complex data structures:

- Maps: ```php $map = $valkey->getMap('map-key');```
- Lists: ```php $list = $valkey->getList('list-key');```
- Sets: ```php $set = $valkey->getSet('set-key');```
- Sorted Sets: ```php $zset = $valkey->getSortedSet('zset-key');```
- Locks: ```php $lock = $valkey->getLock('lock-key');```
- Pub/Sub: ```php $topic = $valkey->getTopic('channel');```
- Streams: ```php $stream = $valkey->getStream('stream-key');```

## Project Goals

- Unified API: Merge the strengths of valkey-php (performance) and webdcg/redis (developer experience)
- 100% Command Coverage: Support all Valkey commands with comprehensive tests
- Open Source Forever: BSD-3-Clause license under Linux Foundation governance
- Valkey Compatibility: Stay in sync with Valkey while maintaining Valkey OSS compatibility
- High-Level Abstractions: Provide Java Valkeyson-like interfaces for PHP developers
- Performance: Optimize for speed while maintaining clean interfaces

### Migrating from valkey-php or webdcg/redis

We provide compatibility layers to make migration from either library as smooth as possible:

```php
// For PHPValkey users
use Valkey\Compat\Valkey;
$valkey = new Valkey();

// For webdcg/redis users
use Valkey\Compat\WebdcgValkey;
$valkey = new WebdcgValkey();
```

## Documentation

Comprehensive documentation is available at [valkey-php.dev/docs](https://valkey-php.dev/docs)

Table of contents
----

1. [Installation](#installation)
2. [Classes and methods](#classes-and-methods)
    - [Usage](#usage)
    - [Bitmaps](#bitmaps)
    - [Connection](#connection)
    - [Geospatial Indexes](#geospatial-indexes)
    - [Hashes](#hashes)
    - [HyperLogLogs](#HyperLogLogs)
    - [Introspection](#introspection)
    - [Keys](#keys)
    - [Lists](#lists)
    - [Pub/sub](#pubsub)
    - [Scripting](#scripting)
    - [Sets](#sets)
    - [Sorted sets](#sorted-sets)
    - [Streams](#Streams)
    - [Strings](#strings)
    - [Transactions](#transactions)

## Classes and methods

### [Usage](docs/content/usage.md)

```php
$valkey = new Valkey();
```

### [Bitmaps](docs/content/bitmaps.md)

```php
$valkey = new Valkey();
$valkey->bitCount('key');
$valkey->bitField('key', 'INCRBY, 'i5', 100, 1, 'GET', 'u4', 0);
$valkey->bitFieldReadOnly('key', 'INCRBY, 'i5', 100, 1, 'GET', 'u4', 0);
$valkey->bitOp('and', 'testBitOpAnd', 'testBit1', 'testBit2');
$valkey->bitpos('key', 0);
$valkey->getBit('key', 5);
$valkey->setBit('key', 8, 1);
```

### [Connection](docs/content/connection.md)

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
$valkey->setOption(Valkey::OPT_PREFIX, 'redis:');
$valkey->getOption(Valkey::OPT_PREFIX)
$valkey->ping('pong');
$valkey->echo('redis');
```

### [Geospatial Indexes](docs/content/geospatial.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$options = ['WITHDIST'];
$valkey->geoAdd('Geocoding', -122.431, 37.773, 'San Francisco');
$valkey->geoAdd('Geocoding', -73.935242, 40.730610, 'New York');
$valkey->geoHash('Geocoding', 'San Francisco');
$valkey->geoPos('Geocoding', 'San Francisco');
$valkey->geoDist('Geocoding', 'San Francisco', 'New York');
$valkey->geoRadius("Geocoding", -157.858, 21.306, 300, 'mi', $options);
$valkey->geoRadiusByMember("Geocoding", 'San Francisco', 300, 'mi', $options);
```

### [Hashes](docs/content/hashes.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->hSet('hash', 'key1', 'hello'); /* 1, 'key1' => 'hello' in the hash at "h" */
$valkey->hGet('hash', 'key1'); /* returns "hello" */
$valkey->hSetNx('hash', 'key1', 'hello'); /* TRUE, 'key1' => 'hello' in the hash at "hash" */
$valkey->hLen('hash'); /* returns 2 */
$valkey->hGetAll('hash');
$valkey->hKeys('hash');
$valkey->hVals('hash');
```

### [HyperLogLogs](docs/content/hyperloglogs.md)

```php
$valkey->pfAdd('HyperLogLog', ['a', 'b', 'c']);
$valkey->pfCount('HyperLogLog'); // 3
$valkey->pfAdd('HyperLogLog2', ['b', 'd']);
$valkey->pfMerge('HyperLogLogMerged', ['HyperLogLog', 'HyperLogLog2']);
$valkey->pfCount('HyperLogLogMerged'); // 4 => a, b, c, d
```

### [Introspection](docs/content/introspection.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->isConnected();
$valkey->getHost();
$valkey->getPort();
$valkey->getDbNum();
$valkey->getTimeout();
$valkey->getReadTimeout();
$valkey->getPersistentID();
$valkey->getAuth();
```

### [Keys](docs/content/keys.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->del('key');
$valkey->delete('key');
$valkey->unlink('key');
$valkey->dump('key');
$valkey->exists('key');
$valkey->expire('key', 123);
$valkey->expireAt('key', 1743016214.623306);
$valkey->keys();
$valkey->scan();
$valkey->migrate('key');
$valkey->move('key');
$valkey->object('key');
$valkey->persist('key');
$valkey->randomKey('key');
$valkey->rename('key', 'yek');
$valkey->renameNx('key', 'yek');
$valkey->type('key');
$valkey->sort('key');
$valkey->ttl('key');
$valkey->restore('key');
```

### [Lists](docs/content/lists.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
/* Non blocking feature */
$valkey->lPush('key1', 'A');
$valkey->del('key2');
$valkey->blPop('key1', 'key2', 10); /* ['key1', 'A'] */
$valkey->rPush('key1', 'A');
$valkey->lInsert('key1', Valkey::AFTER, 'A', 'X'); /* 0 */
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'X', 'C'] */
```

### [Pub/sub](docs/content/pubsub.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
/* Non blocking feature */
$valkey->lPush('key1', 'A');
$valkey->del('key2');
$valkey->blPop('key1', 'key2', 10); /* ['key1', 'A'] */
$valkey->rPush('key1', 'A');
$valkey->lInsert('key1', Valkey::AFTER, 'A', 'X'); /* 0 */
$valkey->lRange('key1', 0, -1); /* ['A', 'B', 'X', 'C'] */
```

### [Scripting](docs/content/scripting.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->eval("return 1"); // Returns an integer: 1
$valkey->eval("return {1,2,3}"); // Returns [1,2,3]
$valkey->del('mylist');
$valkey->rpush('mylist','a');
$valkey->rpush('mylist','b');
$valkey->rpush('mylist','c');
// Nested response:  [1,2,3,['a','b','c']];
$valkey->eval("return {1,2,3,redis.call('lrange','mylist',0,-1)}");
```

### [Sets](docs/content/sets.md)

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

### [Sorted Sets](docs/content/sorted-sets.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);
$valkey->zAdd('key', 1, 'val1');
$valkey->zAdd('key', 0, 'val0');
$valkey->zAdd('key', 5, 'val5');
$valkey->zRange('key', 0, -1); // [val0, val1, val5]

// From Valkey 3.0.2 it's possible to add options like XX, NX, CH, INCR
$valkey->zAdd('key', ['CH'], 5, 'val5', 10, 'val10', 15, 'val15');
```

### [Streams](docs/content/streams.md)

```php
$valkey = new Valkey();
$valkey->connect('127.0.0.1', 6379);

$obj_redis->xAdd('mystream', "*", ['field' => 'value']);
$obj_redis->xAdd('mystream', "*", ['field' => 'value'], 1000); // set max length of stream to 1000
$obj_redis->xAdd('mystream', "*", ['field' => 'value'], 1000, true); // set max length of stream to ~1000
$obj_redis->xAck('mystream', 'group1', ['1530063064286-0', '1530063064286-1']);

/* Get everything in this stream */
$obj_redis->xRange('mystream', '-', '+');

/* Only the first two messages */
$obj_redis->xRange('mystream', '-', '+', 2);

$obj_redis->xInfo('STREAM', 'mystream', 'FULL', 10);

$obj_redis->xRead(['stream1' => '1535222584555-0', 'stream2' => '1535222584555-0']);

$obj_redis->xDel('mystream', ['1530115304877-0', '1530115305731-0']);
```

### [Strings](docs/content/strings.md)

```php
// Simple key -> value set
$valkey->set('key', 'value');
// Will redirect, and actually make an SETEX call
$valkey->set('key', 'value', 10);
// Will set the key, if it doesn't exist, with a ttl of 10 seconds
$valkey->set('key:'.time(), 'value', ['nx', 'ex' => 10]);
// Will set a key, if it does exist, with a ttl of 1000 miliseconds
$valkey->set('key', 'value', ['xx', 'px' => 1000]);
$valkey->setEx('key', 10, 'value');
```

### [Transactions](docs/content/transactions.md)

```php
$valkey->del('key');
$valkey->delete('key');
$valkey->unlink('key');
```

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to get started.

## Testing

Valkey-PHP maintains 100% code coverage for all Valkey commands:

```bash
composer test
```

### License

Valkey-PHP is open-sourced software licensed under the BSD-3-Clause license.

## Credits

- [Roberto Luna Rojas](https://github.com/rlunar)
- [Adrian Luna Rojas](https://github.com/adrianluna)
- [All Contributors](../../contributors)

### Acknowledgements

This project builds upon the excellent work of:

- valkey-php/valkey-php
- webdcg/redis

We thank all the contributors to these projects for their valuable work.

## Support

- GitHub Issues
- Discord Community
- Stack Overflow

This README provides a comprehensive overview of your project goals, highlighting the merger of PHPRredis and webdcg/redis under the Linux Foundation with BSD-3 licensing. It emphasizes the key features like 100% Valkey command coverage, high-level abstractions similar to Valkeyson, and the commitment to open source. The document includes installation instructions, quick start examples, migration paths, and acknowledges the original projects.

-----

## Keys and Strings
