## Geocoding

### geoAdd
-----

##### *Prototype*  
```php
$redis->geoAdd($key, $longitude, $latitude, $member [, $longitude, $latitude, $member, ...]);
```

_**Description**_:  Add one or more geospatial items to the specified key.  This function must be called with at least one _longitude, latitude, member_ triplet.

##### *Return value*
*Integer*:  The number of elements added to the geospatial key.

##### *Example*
```php
$redis->del("myplaces");

/* Since the key will be new, $result will be 2 */
$result = $redis->geoAdd(
    "myplaces",
    -122.431, 37.773, "San Francisco",
    -157.858, 21.315, "Honolulu"
);
```  

### geoHash
-----

##### *Prototype*
```php
$redis->geoHash($key, $member [, $member, $member, ...]);
```

_**Description**_:  Retrieve Geohash strings for one or more elements of a geospatial index.  

##### *Return value*  
*Array*:  One or more Redis Geohash encoded strings.  

##### *Example*  
```php
$redis->geoAdd("hawaii", -157.858, 21.306, "Honolulu", -156.331, 20.798, "Maui");
$hashes = $redis->geoHash("hawaii", "Honolulu", "Maui");
var_dump($hashes);
```

##### *Output*  
```
array(2) {
  [0]=>
  string(11) "87z9pyek3y0"
  [1]=>
  string(11) "8e8y6d5jps0"
}
```

### geoPos
-----

##### *Prototype*  
```php
$redis->geoPos($key, $member [, $member, $member, ...]);
```

_**Description**_:  Return longitude, latitude positions for each requested member.

##### *Return value*  
*Array*:  One or more longitude/latitude positions

##### *Example*  
```php
$redis->geoAdd("hawaii", -157.858, 21.306, "Honolulu", -156.331, 20.798, "Maui");
$positions = $redis->geoPos("hawaii", "Honolulu", "Maui");
var_dump($positions);
```

##### *Output*  
```
array(2) {
  [0]=>
  array(2) {
    [0]=>
    string(22) "-157.85800248384475708"
    [1]=>
    string(19) "21.3060004581273077"
  }
  [1]=>
  array(2) {
    [0]=>
    string(22) "-156.33099943399429321"
    [1]=>
    string(20) "20.79799924753607598"
  }
}
```

### GeoDist  
-----

##### *Prototype*  
```php
$redis->geoDist($key, $member1, $member2 [, $unit]);
```


_**Description**_:  Return the distance between two members in a geospatial set.  If units are passed it must be one of the following values:

* 'm' => Meters
* 'km' => Kilometers
* 'mi' => Miles
* 'ft' => Feet

##### *Return value*
*Double*:  The distance between the two passed members in the units requested (meters by default).  

##### *Example*
```php
$redis->geoAdd("hawaii", -157.858, 21.306, "Honolulu", -156.331, 20.798, "Maui");

$meters = $redis->geoDist("hawaii", "Honolulu", "Maui");
$kilometers = $redis->geoDist("hawaii", "Honolulu", "Maui", 'km');
$miles = $redis->geoDist("hawaii", "Honolulu", "Maui", 'mi');
$feet = $redis->geoDist("hawaii", "Honolulu", "Maui", 'ft');

echo "Distance between Honolulu and Maui:\n";
echo "  meters    : $meters\n";
echo "  kilometers: $kilometers\n";
echo "  miles     : $miles\n";
echo "  feet      : $feet\n";

/* Bad unit */
$inches = $redis->geoDist("hawaii", "Honolulu", "Maui", 'in');
echo "Invalid unit returned:\n";
var_dump($inches);
```  

##### *Output*  
```
Distance between Honolulu and Maui:
  meters    : 168275.204
  kilometers: 168.2752
  miles     : 104.5616
  feet      : 552084.0028
Invalid unit returned:
bool(false)
```

### geoRadius
-----

##### *Prototype*
```php
$redis->geoRadius($key, $longitude, $latitude, $radius, $unit [, Array $options]);
```

_**Description**_:  Return members of a set with geospatial information that are within the radius specified by the caller. 

##### *Options Array*
The georadius command can be called with various options that control how Redis returns results.  The following table describes the options phpredis supports.  All options are case insensitive.  

| Key       | Value       | Description
| :---      | :---        | :---- |
| COUNT     | integer > 0 | Limit how many results are returned
|           | WITHCOORD   | Return longitude and latitude of matching members
|           | WITHDIST    | Return the distance from the center
|           | WITHHASH    | Return the raw geohash-encoded score
|           | ASC         | Sort results in ascending order
|           | DESC        | Sort results in descending order
| STORE     | _key_       | Store results in _key_
| STOREDIST | _key_       | Store the results as distances in _key_

 *Note*:  It doesn't make sense to pass both `ASC` and `DESC` options but if both are passed the last one passed will be used.  
 *Note*:  When using `STORE[DIST]` in Redis Cluster, the store key must has to the same slot as the query key or you will get a `CROSSLOT` error.

##### *Return value*
*Mixed*:  When no `STORE` option is passed, this function returns an array of results.  If it is passed this function returns the number of stored entries.
 
##### *Example*
```php
/* Add some cities */
$redis->geoAdd("hawaii", -157.858, 21.306, "Honolulu", -156.331, 20.798, "Maui");

echo "Within 300 miles of Honolulu:\n";
var_dump($redis->geoRadius("hawaii", -157.858, 21.306, 300, 'mi'));

echo "\nWithin 300 miles of Honolulu with distances:\n";
$options = ['WITHDIST'];
var_dump($redis->geoRadius("hawaii", -157.858, 21.306, 300, 'mi', $options));

echo "\nFirst result within 300 miles of Honolulu with distances:\n";
$options['count'] = 1;
var_dump($redis->geoRadius("hawaii", -157.858, 21.306, 300, 'mi', $options));

echo "\nFirst result within 300 miles of Honolulu with distances in descending sort order:\n";
$options[] = 'DESC';
var_dump($redis->geoRadius("hawaii", -157.858, 21.306, 300, 'mi', $options));
```

##### *Output*
```
Within 300 miles of Honolulu:
array(2) {
  [0]=>
  string(8) "Honolulu"
  [1]=>
  string(4) "Maui"
}

Within 300 miles of Honolulu with distances:
array(2) {
  [0]=>
  array(2) {
    [0]=>
    string(8) "Honolulu"
    [1]=>
    string(6) "0.0002"
  }
  [1]=>
  array(2) {
    [0]=>
    string(4) "Maui"
    [1]=>
    string(8) "104.5615"
  }
}

First result within 300 miles of Honolulu with distances:
array(1) {
  [0]=>
  array(2) {
    [0]=>
    string(8) "Honolulu"
    [1]=>
    string(6) "0.0002"
  }
}

First result within 300 miles of Honolulu with distances in descending sort order:
array(1) {
  [0]=>
  array(2) {
    [0]=>
    string(4) "Maui"
    [1]=>
    string(8) "104.5615"
  }
}
```

### geoRadiusByMember

##### *Prototype*
```php
$redis->geoRadiusByMember($key, $member, $radius, $units [, Array $options]);
```

_**Description**_: This method is identical to [geoRadius](#georadius) except that instead of passing a longitude and latitude as the "source" you pass an existing member in the geospatial set.

##### *Options Array*
See [geoRadius](#georadius) command for options array.

##### *Return value*
*Array*:  The zero or more entries that are close enough to the member given the distance and radius specified.  

##### *Example*
```php
$redis->geoAdd("hawaii", -157.858, 21.306, "Honolulu", -156.331, 20.798, "Maui");

echo "Within 300 miles of Honolulu:\n";
var_dump($redis->geoRadiusByMember("hawaii", "Honolulu", 300, 'mi'));

echo "\nFirst match within 300 miles of Honolulu:\n";
var_dump($redis->geoRadiusByMember("hawaii", "Honolulu", 300, 'mi', ['count' => 1]));
```

##### *Output*
```
Within 300 miles of Honolulu:
array(2) {
  [0]=>
  string(8) "Honolulu"
  [1]=>
  string(4) "Maui"
}

First match within 300 miles of Honolulu:
array(1) {
  [0]=>
  string(8) "Honolulu"
}
```
