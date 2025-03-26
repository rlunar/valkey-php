# Valkey PHP - Introspection

|Command                            |Description                                                                    |Supported  |Tested     |Class/Trait    |Method         |
|---                                |---                                                                            |:-:        |:-:        |---            |---            |
|[isConnected](#isConnected)        |A method to determine if a phpredis object thinks it's connected to a server.  |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |isConnected    |
|[getHost](#getHost)                |Retrieve our host or unix socket that we're connected to.                      |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getHost        |
|[getPort](#getPort)                |Get the port we're connected to.                                               |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getPort        |
|[getDbNum](#getDbNum)              |Get the database number phpredis is pointed to.                                |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getDbNum       |
|[getTimeout](#getTimeout)          |Get the (write) timeout in use for phpredis.                                   |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getTimeout     |
|[getReadTimeout](#getReadTimeout)  |Get the read timeout specified to phpredis or FALSE if we're not connected.    |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getReadTimeout |
|[getPersistentID](#getPersistentID)|Gets the persistent ID that phpredis is using.                                 |:white\_check\_mark:        |:white\_check\_mark:        |Introspection  |getPersistentID|
|[getAuth](#getAuth)                |Get the password used to authenticate the phpredis connection.                 |:white\_check\_mark:        |:white\_check\_mark:        |Introspection| getAuth         |

### isConnected
-----
_**Description**_:  A method to determine if a valkey-php object thinks it's connected to a server

##### *Parameters*
None

##### *Return value*
*Boolean* Returns TRUE if valkey-php thinks it's connected and FALSE if not

### getHost
-----
_**Description**_:  Retrieve our host or unix socket that we're connected to

##### *Parameters*
None

##### *Return value*
*Mixed* The host or unix socket we're connected to or FALSE if we're not connected


### getPort
-----
_**Description**_:  Get the port we're connected to

##### *Parameters*
None

##### *Return value*
*Mixed* Returns the port we're connected to or FALSE if we're not connected

### getDbNum
-----
_**Description**_:  Get the database number valkey-php is pointed to

##### *Parameters*
None

##### *Return value*
*Mixed* Returns the database number (LONG) valkey-php thinks it's pointing to or FALSE if we're not connected

### getTimeout
-----
_**Description**_:  Get the (write) timeout in use for valkey-php

##### *Parameters*
None

##### *Return value*
*Mixed* The timeout (DOUBLE) specified in our connect call or FALSE if we're not connected

### getReadTimeout
_**Description**_:  Get the read timeout specified to valkey-php or FALSE if we're not connected

##### *Parameters*
None

##### *Return value*
*Mixed*  Returns the read timeout (which can be set using setOption and Valkey::OPT_READ_TIMEOUT) or FALSE if we're not connected

### getPersistentID
-----
_**Description**_:  Gets the persistent ID that valkey-php is using

##### *Parameters*
None

##### *Return value*
*Mixed* Returns the persistent id valkey-php is using (which will only be set if connected with pconnect), NULL if we're not
using a persistent ID, and FALSE if we're not connected

### getAuth
-----
_**Description**_:  Get the password (or username and password if using Valkey 6 ACLs) used to authenticate the connection.

### *Parameters*
None

### *Return value*
*Mixed*  Returns NULL if no username/password are set, the password string if a password is set, and a `[username, password]` array if authenticated with a username and password.
