# Valkey PHP - Pub/sub

|Command                    |Description                                |Supported  |Tested     |Class/Trait    |Method     |
|---                        |---                                        |:-:        |:-:        |---            |---        |
|[pSubscribe](#pSubscribe)  |Subscribe to channels by pattern           |:x:        |:x:        |PubSub         |pSubscribe |
|[publish](#publish)        |Post a message to a channel                |:x:        |:x:        |PubSub         |publish    |
|[subscribe](#subscribe)    |Subscribe to channels                      |:x:        |:x:        |PubSub         |subscribe  |
|[pubSub](#pubSub)          |Introspection into the pub/sub subsystem   |:x:        |:x:        |PubSub         |pubSub     |

PSUBSCRIBE Listens for messages published to channels that match one or more patterns.
PUBLISH Posts a message to a channel.
PUBSUB A container for Pub/Sub commands.
PUBSUB CHANNELS Returns the active channels.
PUBSUB HELP Returns helpful text about the different subcommands.
PUBSUB NUMPAT Returns a count of unique pattern subscriptions.
PUBSUB NUMSUB Returns a count of subscribers to channels.
PUBSUB SHARDCHANNELS Returns the active shard channels.
PUBSUB SHARDNUMSUB Returns the count of subscribers of shard channels.
PUNSUBSCRIBE Stops listening to messages published to channels that match one or more patterns.
SPUBLISH Post a message to a shard channel
SSUBSCRIBE Listens for messages published to shard channels.
SUBSCRIBE Listens for messages published to channels.
SUNSUBSCRIBE Stops listening to messages posted to shard channels.
UNSUBSCRIBE Stops listening to messages posted to channels.

* [pSubscribe](#psubscribe) - Subscribe to channels by pattern
* [publish](#publish) - Post a message to a channel
* [subscribe](#subscribe) - Subscribe to channels
* [pubSub](#pubsub) - Introspection into the pub/sub subsystem

## Usage

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

### pSubscribe
-----
_**Description**_: Subscribe to channels by pattern

##### *Parameters*
*patterns*: An array of patterns to match  
*callback*: Either a string or an array with an object and method.  The callback will get four arguments ($redis, $pattern, $channel, $message)  
*return value*: Mixed.  Any non-null return value in the callback will be returned to the caller.  
##### *Example*

```php
function pSubscribe($redis, $pattern, $chan, $msg) {
    echo "Pattern: $pattern\n";
    echo "Channel: $chan\n";
    echo "Payload: $msg\n";
}
```

### publish
-----
_**Description**_: Publish messages to channels. Warning: this function will probably change in the future.

##### *Parameters*
*channel*: a channel to publish to  
*message*: string

##### *Example*

```php
$valkey->publish('chan-1', 'hello, world!'); // send message.
```

### subscribe
-----
_**Description**_: Subscribe to channels. Warning: this function will probably change in the future.

##### *Parameters*
*channels*: an array of channels to subscribe to  
*callback*: either a string or [$instance, 'method_name']. The callback function receives 3 parameters: the redis instance, the channel name, and the message.
*return value*:  Mixed.  Any non-null return value in the callback will be returned to the caller.
##### *Example*

```php
function f($redis, $chan, $msg) {
    switch($chan) {
        case 'chan-1':
            ...
            break;

        case 'chan-2':
            ...
            break;

        case 'chan-2':
            ...
            break;
    }
}

$valkey->subscribe(['chan-1', 'chan-2', 'chan-3'], 'f'); // subscribe to 3 chans
```

### pubSub
-----
_**Description**_: A command allowing you to get information on the Valkey pub/sub system.

##### *Parameters*
*keyword*: String, which can be: "channels", "numsub", or "numpat"  
*argument*:  Optional, variant.  For the "channels" subcommand, you can pass a string pattern.  For "numsub" an array of channel names.

##### *Return value*
*CHANNELS*: Returns an array where the members are the matching channels.  
*NUMSUB*:  Returns a key/value array where the keys are channel names and values are their counts.  
*NUMPAT*:  Integer return containing the number active pattern subscriptions

##### *Example*

```php
$valkey->pubSub("channels"); /*All channels */
$valkey->pubSub("channels", "*pattern*"); /* Just channels matching your pattern */
$valkey->pubSub("numsub", ["chan1", "chan2"]); /*Get subscriber counts for 'chan1' and 'chan2'*/
$valkey->pubSub("numpat"); /* Get the number of pattern subscribers */


```