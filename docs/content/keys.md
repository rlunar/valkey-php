### Keys
-----

* [del, delete, unlink](#del-delete-unlink) - Delete a key
* [dump](#dump) - Return a serialized version of the value stored at the specified key.
* [exists](#exists) - Determine if a key exists
* [expire, pexpire](#expire-pexpire) - Set a key's time to live in seconds
* [expireAt, pexpireAt](#expireat-pexpireat) - Set the expiration for a key as a UNIX timestamp
* [keys](#keys) - Find all keys matching the given pattern
* [scan](#scan) - Scan for keys in the keyspace (Redis >= 2.8.0)
* [migrate](#migrate) - Atomically transfer a key from a Redis instance to another one
* [move](#move) - Move a key to another database
* [object](#object) - Inspect the internals of Redis objects
* [persist](#persist) - Remove the expiration from a key
* [randomKey](#randomkey) - Return a random key from the keyspace
* [rename](#rename) - Rename a key
* [renameNx](#renamenx) - Rename a key, only if the new key does not exist
* [type](#type) - Determine the type stored at key
* [sort](#sort) - Sort the elements in a list, set or sorted set
* [ttl, pttl](#ttl-pttl) - Get the time to live for a key
* [restore](#restore) - Create a key using the provided serialized value, previously obtained with [dump](#dump).