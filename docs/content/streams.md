## Streams

* [xAck](#xack) - Acknowledge one or more pending messages
* [xAdd](#xadd) - Add a message to a stream
* [xClaim](#xclaim) - Acquire ownership of a pending message
* [xDel](#xdel) - Remove a message from a stream
* [xGroup](#xgroup) - Manage consumer groups
* [xInfo](#xinfo) - Get information about a stream
* [xLen](#xlen) - Get the length of a stream
* [xPending](#xpending) - Inspect pending messages in a stream
* [xRange](#xrange) - Query a range of messages from a stream
* [xRead](#xread) - Read message(s) from a stream
* [xReadGroup](#xreadgroup) - Read stream messages with a group and consumer
* [xRevRange](#xrevrange) - Query one or more messages from end to start
* [xTrim](#xtrim) - Trim a stream's size

### xAck
-----

##### *Prototype*
```php
$obj_redis->xAck($stream, $group, $arr_messages);
```

_**Description**_:  Acknowledge one or more messages on behalf of a consumer group.

##### *Return value*
*long*:  The number of messages Redis reports as acknowledged.

##### *Example*
```php
$obj_redis->xAck('stream', 'group1', ['1530063064286-0', '1530063064286-1']);
```

### xAdd
-----

##### *Prototype*
```php
$obj_redis->xAdd($str_key, $str_id, $arr_message[, $i_maxlen, $boo_approximate]);
```

_**Description**_:  Add a message to a stream

##### *Return value*
*String*:  The added message ID

##### *Example*
```php
$obj_redis->xAdd('mystream', "*", ['field' => 'value']);
$obj_redis->xAdd('mystream', "*", ['field' => 'value'], 1000); // set max length of stream to 1000
$obj_redis->xAdd('mystream', "*", ['field' => 'value'], 1000, true); // set max length of stream to ~1000
```

### xClaim
-----

##### *Prototype*
```php
$obj_redis->xClaim($str_key, $str_group, $str_consumer, $min_idle_time, $arr_ids, [$arr_options]);
```

_**Description**_:  Claim ownership of one or more pending messages.

#### *Options Array*
```php
$options = [
    /* Note:  'TIME', and 'IDLE' are mutually exclusive */
    'IDLE' => $value, /* Set the idle time to $value ms  */,
    'TIME' => $value, /* Set the idle time to now - $value */
    'RETRYCOUNT' => $value, /* Update message retrycount to $value */
    'FORCE', /* Claim the message(s) even if they're not pending anywhere */
    'JUSTID', /* Instruct Redis to only return IDs */
];
```

##### *Return value*
*Array*:  Either an array of message IDs along with corresponding data, or just an array of IDs (if the 'JUSTID' option was passed).

##### *Example*
```php
$ids = ['1530113681011-0', '1530113681011-1', '1530113681011-2'];

/* Without any options */
$obj_redis->xClaim(
    'mystream', 'group1', 'myconsumer1', 0, $ids
);

/* With options */
$obj_redis->xClaim(
    'mystream', 'group1', 'myconsumer2', 0, $ids,
    [
        'IDLE' => time() * 1000,
        'RETRYCOUNT' => 5,
        'FORCE',
        'JUSTID'
    ]
);
```

### xDel
-----

##### *Prototype*
```php
$obj_redis->xDel($str_key, $arr_ids);
```

_**Description**_:  Delete one or more messages from a stream.

##### *Return value*
*long*:  The number of messages removed

##### *Example*
```php
$obj_redis->xDel('mystream', ['1530115304877-0', '1530115305731-0']);
```

### xGroup
-----

##### *Prototype*
```php
$obj_redis->xGroup('HELP');
$obj_redis->xGroup('CREATE', $str_key, $str_group, $str_msg_id, [$boo_mkstream]);
$obj_redis->xGroup('SETID', $str_key, $str_group, $str_msg_id);
$obj_redis->xGroup('DESTROY', $str_key, $str_group);
$obj_redis->xGroup('DELCONSUMER', $str_key, $str_group, $str_consumer_name);
```

_**Description**_:  This command is used in order to create, destroy, or manage consumer groups.

##### *Return value*
*Mixed*:  This command returns different types depending on the specific XGROUP command executed.

##### *Example*
```php
$obj_redis->xGroup('CREATE', 'mystream', 'mygroup', '0');
$obj_redis->xGroup('CREATE', 'mystream', 'mygroup2', '0', true); /* Create stream if non-existent. */
$obj_redis->xGroup('DESTROY', 'mystream', 'mygroup');
```

### xInfo
-----

##### *Prototype*
```php
$obj_redis->xInfo('CONSUMERS', $str_stream, $str_group);
$obj_redis->xInfo('GROUPS', $str_stream);
$obj_redis->xInfo('STREAM', $str_stream [, 'FULL' [, $i_count]]);
$obj_redis->xInfo('HELP');
```

_**Description**_:  Get information about a stream or consumer groups.

##### *Return value*
*Mixed*:  This command returns different types depending on which subcommand is used.

##### *Example*
```php
$obj_redis->xInfo('STREAM', 'mystream');
$obj_redis->xInfo('STREAM', 'mystream', 'FULL', 10);
```

### xLen
-----

##### *Prototype*
```php
$obj_redis->xLen($str_stream);
```

_**Description**_:  Get the length of a given stream

##### *Return value*
*Long*:  The number of messages in the stream.

##### *Example*
```php
$obj_redis->xLen('mystream');
```

### xPending
-----

##### *Prototype*
```php
$obj_redis->xPending($str_stream, $str_group [, $str_start, $str_end, $i_count, $str_consumer]);
```

_**Description**_:  Get information about pending messages in a given stream.

##### *Return value*
*Array*:  Information about the pending messages, in various forms depending on the specific invocation of XPENDING.

##### *Examples*
```php
$obj_redis->xPending('mystream', 'mygroup');
$obj_redis->xPending('mystream', 'mygroup', '-', '+', 1, 'consumer-1');
```

### xRange
-----

##### *Prototype*
```php
$obj_redis->xRange($str_stream, $str_start, $str_end [, $i_count]);
```

_**Description**_:  Get a range of messages from a given stream.

##### *Return value*
*Array*:  The messages in the stream within the requested range.

##### *Example*
```php
/* Get everything in this stream */
$obj_redis->xRange('mystream', '-', '+');

/* Only the first two messages */
$obj_redis->xRange('mystream', '-', '+', 2);
```

### xRead
-----

##### *Prototype*
```php
$obj_redis->xRead($arr_streams [, $i_count, $i_block]);
```

_**Description**_:  Read data from one or more streams and only return IDs greater than sent in the command.

##### *Return value*
*Array*:  The messages in the stream newer than the IDs passed to Redis (if any).

##### *Example*
```php
$obj_redis->xRead(['stream1' => '1535222584555-0', 'stream2' => '1535222584555-0']);

/* --- Possible output  ---
Array
(
    [stream1] => Array
        (
            [1535222584555-1] => Array
                (
                    [key:1] => val:1
                )

        )

    [stream2] => Array
        (
            [1535222584555-1] => Array
                (
                    [key:1] => val:1
                )

        )

)
*/

// Receive only new message ($ = last id) and wait for one new message unlimited time
$obj_redis->xRead(['stream1' => '$'], 1, 0);
```

### xReadGroup
-----

##### *Prototype*
```php
$obj_redis->xReadGroup($str_group, $str_consumer, $arr_streams [, $i_count, $i_block]);
```

_**Description**_:  This method is similar to xRead except that it supports reading messages for a specific consumer group.

##### *Return value*
*Array*:  The messages delivered to this consumer group (if any).

##### *Examples*
```php
/* Consume messages for 'mygroup', 'consumer1' */
$obj_redis->xReadGroup('mygroup', 'consumer1', ['s1' => 0, 's2' => 0]);

/* Consume messages for 'mygroup', 'consumer1' which were not consumed yet by the group */
$obj_redis->xReadGroup('mygroup', 'consumer1', ['s1' => '>', 's2' => '>']);

/* Read a single message as 'consumer2' wait for up to a second until a message arrives. */
$obj_redis->xReadGroup('mygroup', 'consumer2', ['s1' => 0, 's2' => 0], 1, 1000);
```

### xRevRange
-----

##### *Prototype*
```php
$obj_redis->xRevRange($str_stream, $str_end, $str_start [, $i_count]);
```

_**Description**_:  This is identical to xRange except the results come back in reverse order.  Also note that Redis reverses the order of "start" and "end".

##### *Return value*
*Array*:  The messages in the range specified.

##### *Example*
```php
$obj_redis->xRevRange('mystream', '+', '-');
```

### xTrim
-----

##### *Prototype*
```php
$obj_redis->xTrim($str_stream, $i_max_len [, $boo_approximate]);
```

_**Description**_:  Trim the stream length to a given maximum.  If the "approximate" flag is pasesed, Redis will use your size as a hint but only trim trees in whole nodes (this is more efficient).

##### *Return value*
*long*:  The number of messages trimmed from the stream.

##### *Example*
```php
/* Trim to exactly 100 messages */
$obj_redis->xTrim('mystream', 100);

/* Let Redis approximate the trimming */
$obj_redis->xTrim('mystream', 100, true);
```
