## Pub/sub

* [pSubscribe](#psubscribe) - Subscribe to channels by pattern
* [publish](#publish) - Post a message to a channel
* [subscribe](#subscribe) - Subscribe to channels
* [pubSub](#pubsub) - Introspection into the pub/sub subsystem

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
_**Description**_: A command allowing you to get information on the Redis pub/sub system.

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