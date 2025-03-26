## Generic
1. [rawCommand](#rawcommand) - Execute any generic command against the server.

### rawCommand
-----
_**Description**_: A method to execute any arbitrary command against the a Redis server

##### *Parameters*
This method is variadic and takes a dynamic number of arguments of various types (string, long, double), but must be passed at least one argument (the command keyword itself).

##### *Return value*
The return value can be various types depending on what the server itself returns.   No post processing is done to the returned value and must be handled by the client code.

##### *Example*
```php
/* Returns: true */
$redis->rawCommand("set", "foo", "bar");

/* Returns: "bar" */
$redis->rawCommand("get", "foo");

/* Returns: 3 */
$redis->rawCommand("rpush", "mylist", "one", 2, 3.5);

/* Returns: ["one", "2", "3.5000000000000000"] */
$redis->rawCommand("lrange", "mylist", 0, -1);
```