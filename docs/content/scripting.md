## Scripting

* [eval](#eval) - Evaluate a LUA script serverside
* [evalSha](#evalsha) - Evaluate a LUA script serverside, from the SHA1 hash of the script instead of the script itself
* [script](#script) - Execute the Valkey SCRIPT command to perform various operations on the scripting subsystem
* [getLastError](#getlasterror) - The last error message (if any)
* [clearLastError](#clearlasterror) - Clear the last error message
* [_prefix](#_prefix) - A utility method to prefix the value with the prefix setting for valkey-php
* [_unserialize](#_unserialize) - A utility method to unserialize data with whatever serializer is set up
* [_serialize](#_serialize) - A utility method to serialize data with whatever serializer is set up

### eval
-----
_**Description**_: Evaluate a LUA script serverside

##### *Parameters*
*script* string.  
*args* array, optional.  
*num_keys* int, optional.

##### *Return value*
Mixed.  What is returned depends on what the LUA script itself returns, which could be a scalar value (int/string), or an array.
Arrays that are returned can also contain other arrays, if that's how it was set up in your LUA script.  If there is an error
executing the LUA script, the getLastError() function can tell you the message that came back from Valkey (e.g. compile error).

##### *Examples*
```php
$valkey->eval("return 1"); // Returns an integer: 1
$valkey->eval("return {1,2,3}"); // Returns [1,2,3]
$valkey->del('mylist');
$valkey->rpush('mylist','a');
$valkey->rpush('mylist','b');
$valkey->rpush('mylist','c');
// Nested response:  [1,2,3,['a','b','c']];
$valkey->eval("return {1,2,3,redis.call('lrange','mylist',0,-1)}");
```

### evalSha
-----
_**Description**_: Evaluate a LUA script serverside, from the SHA1 hash of the script instead of the script itself.

In order to run this command Valkey will have to have already loaded the script,
either by running it or via the SCRIPT LOAD command.

##### *Parameters*
*script_sha* string.  The sha1 encoded hash of the script you want to run.  
*args* array, optional.  Arguments to pass to the LUA script.  
*num_keys* int, optional.  The number of arguments that should go into the KEYS array, vs. the ARGV array when Valkey spins the script

##### *Return value*
Mixed.  See EVAL

##### *Examples*
```php
$script = 'return 1';
$sha = $valkey->script('load', $script);
$valkey->evalSha($sha); // Returns 1
```

### script
-----
_**Description**_: Execute the Valkey SCRIPT command to perform various operations on the scripting subsystem.

##### *Usage*
```php
$valkey->script('load', $script);
$valkey->script('flush');
$valkey->script('kill');
$valkey->script('exists', $script1, [$script2, $script3, ...]);
```

##### *Return value*
* SCRIPT LOAD will return the SHA1 hash of the passed script on success, and FALSE on failure.
* SCRIPT FLUSH should always return TRUE
* SCRIPT KILL will return true if a script was able to be killed and false if not
* SCRIPT EXISTS will return an array with TRUE or FALSE for each passed script

### client
-----
_**Description**_: Issue the CLIENT command with various arguments.

The Valkey CLIENT command can be used in four ways.
* CLIENT LIST
* CLIENT GETNAME
* CLIENT SETNAME [name]
* CLIENT KILL [ip:port]

##### *Usage*
```php
$valkey->client('list'); // Get a list of clients
$valkey->client('getname'); // Get the name of the current connection
$valkey->client('setname', 'somename'); // Set the name of the current connection
$valkey->client('kill', <ip:port>); // Kill the process at ip:port
```

##### *Return value*
This will vary depending on which client command was executed.

* CLIENT LIST will return an array of arrays with client information.
* CLIENT GETNAME will return the client name or false if none has been set
* CLIENT SETNAME will return true if it can be set and false if not
* CLIENT KILL will return true if the client can be killed, and false if not

Note:  valkey-php will attempt to reconnect so you can actually kill your own connection
but may not notice losing it!
### getLastError
-----
_**Description**_: The last error message (if any)

##### *Parameters*
*none*

##### *Return value*
A string with the last returned script based error message, or NULL if there is no error

##### *Examples*
```php
$valkey->eval('this-is-not-lua');
$err = $valkey->getLastError();
// "ERR Error compiling script (new function): user_script:1: '=' expected near '-'"
```

### clearLastError
-----
_**Description**_: Clear the last error message

##### *Parameters*
*none*

##### *Return value*
*BOOL* TRUE

##### *Examples*
```php
$valkey->set('x', 'a');
$valkey->incr('x');
$err = $valkey->getLastError();
// "ERR value is not an integer or out of range"
$valkey->clearLastError();
$err = $valkey->getLastError();
// NULL
```

### _prefix
-----
_**Description**_: A utility method to prefix the value with the prefix setting for valkey-php.

##### *Parameters*
*value* string.  The value you wish to prefix

##### *Return value*
If a prefix is set up, the value now prefixed.  If there is no prefix, the value will be returned unchanged.

##### *Examples*
```php
$valkey->setOption(Valkey::OPT_PREFIX, 'my-prefix:');
$valkey->_prefix('my-value'); // Will return 'my-prefix:my-value'
```

### _serialize
-----
_**Description**_: A utility method to serialize values manually.

This method allows you to serialize a value with whatever serializer is configured, manually.
This can be useful for serialization/unserialization of data going in and out of EVAL commands
as valkey-php can't automatically do this itself.  Note that if no serializer is set, valkey-php
will change Array values to 'Array', and Objects to 'Object'.

##### *Parameters*
*value*:  Mixed.  The value to be serialized

##### *Examples*
```php
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_NONE);
$valkey->_serialize("foo"); // returns "foo"
$valkey->_serialize([]); // Returns "Array"
$valkey->_serialize(new stdClass()); // Returns "Object"

$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_PHP);
$valkey->_serialize("foo"); // Returns 's:3:"foo";'
```

### _unserialize
-----
_**Description**_: A utility method to unserialize data with whatever serializer is set up.

If there is no serializer set, the value will be returned unchanged.  If there is a serializer set up,
and the data passed in is malformed, an exception will be thrown. This can be useful if valkey-php is
serializing values, and you return something from redis in a LUA script that is serialized.

##### *Parameters*
*value* string.  The value to be unserialized

##### *Examples*
```php
$valkey->setOption(Valkey::OPT_SERIALIZER, Valkey::SERIALIZER_PHP);
$valkey->_unserialize('a:3:{i:0;i:1;i:1;i:2;i:2;i:3;}'); // Will return [1,2,3]
```
