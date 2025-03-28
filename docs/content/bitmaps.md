# Valkey PHP  - Bitmaps

|Command                                |Description                                                        |Supported              |Tested                 |Class/Trait    |Method             |
|---                                    |---                                                                |:-:                    |:-:                    |---            |---                |
|[bitCount](#bitCount)                  |Count set bits in a string                                         |:white\_check\_mark:   |:white\_check\_mark:   |Bitmaps           |bitCount           |
|[bitField](#bitField)                  |Perform arbitrary bitfield integer operations on strings           |:x:                    |:x:                    |Bitmaps           |bitField           |
|[bitFieldReadOnly](#bitFieldReadOnly)  |Perform arbitrary read-only bitfield integer operations on strings |:x:                    |:x:                    |Bitmaps           |bitFieldReadOnly   |
|[bitOp](#bitOp)                        |Perform bitwise operations between strings                         |:white\_check\_mark:   |:white\_check\_mark:   |Bitmaps           |bitOp              |
|[bitPos](#bitPos)                      |Return the position of the first bit set to 1 or 0 in a string.    |:x:                    |:x:                    |Bitmaps           |bitPos             |
|[getBit](#getBit)                      |Returns the bit value at offset in the string value stored at key  |:white\_check\_mark:   |:white\_check\_mark:   |Bitmaps           |getBit             |
|[setBit](#setBit)                      |Sets or clears the bit at offset in the string value stored at key |:white\_check\_mark:   |:white\_check\_mark:   |Bitmaps           |setBit             |

## Usage

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

## bitCount

_**Description**_: Count set bits in a string.

##### *Prototype*  

```php
public function bitCount(string $key) : int {
    return $this->valkey->bitCount($key);
}
```

##### *Parameters*

- *key*: String. The key append to.

##### *Return value*

*int*: Total of bits set in the string.

##### *Example*

```php
$valkey->set('key', 'a'); // 0110 0001 in Binary or 97 in decimal
$valkey->bitCount('key'); // 3
```

## bitField

_**Description**_: Perform arbitrary bitfield integer operations on strings

##### *Prototype*  

```php
public function bitField(string $key, ...$params): array {
    return [];
}
```

##### *Parameters*

- *key*: String. The key.

##### *Return value*

*array*: each entry being the corresponding result of the sub-command given at the same position.

##### *Example*

```php
$valkey->bitField('key', 'INCRBY, 'i5', 100, 1, 'GET', 'u4', 0);
```

## bitOp

_**Description**_: Perform bitwise operations between strings.

##### *Prototype*  

```php
public function bitOp(string $operation, string $returnKey, ...$keys): int {
    return $this->valkey->bitOp($key);
}
```

##### *Parameters*

- *operation*: String. Bitwise operation to perform: "AND", "OR", "NOT", "XOR"
- *returnKey*: String. The key where the result will be saved.
- *keys*: String(s). The key(s) part of he operation.

##### *Return value*

*int*: The size of the string stored in the destination key.

##### *Example*

```php
$valkey->bitOp('not', 'testBitOpNot', 'testBit');

$valkey->set('testBit1', 0);
$valkey->set('testBit2', 1);
$valkey->bitOp('and', 'testBitOpAnd', 'testBit1', 'testBit2');
$valkey->get('testBitOpAnd'); // 0 since only the two bits that are common 
                            // between 0 and 1 will match
```

## bitPos

_**Description**_: Return the position of the first bit set to 1 or 0 in a string.

##### *Prototype*  

```php
public function bitPos(string $key, int $bit, int $start = 0, int $end, string $type): int {
    if ($type != 'BYTE' || $type != 'BIT') {
        return -1;
    }
    return $this->valkey->bitPos($key, $bit, $start, $end, $type);
}
```

##### *Parameters*

- *key*: String. The Bitmap key.
- *value*: int. 0 or 1, the value of the bit to look for.
- *start*: int. 0 or 1, the value of the bit to look for.

##### *Return value*

*int*: The size of the string stored in the destination key.

##### *Example*

```php
$valkey->set('key', 'A');   // 0100 0001
                            // ^
                            // |
$valkey->bitPos('key', 0);  // 0 found at the first position

$valkey->get('key');        // 0100 0001
                            //  ^
                            //  |
$valkey->bitPos('key', 1);  //  1 found on the second position (zero based)
```

## getBit

_**Description**_: Returns the bit value at offset in the string value stored at key.

##### *Prototype*  

```php
public function getBit(string $key, int $offset) : int {
    return $this->valkey->getBit($key, $offset);
}
```

##### *Parameters*

- *key*: String. The Bitmap key.
- *offset*: String. The bit position within the string.

##### *Return value*

*int*: 0 or 1

##### *Example*

```php
$valkey->set('key', 'A');   // 0100 0001
                            //    ^
                            //    |
$valkey->getBit('key', 3);  //    0  is the 4th element (zero based)

$valkey->get('key');        // 0100 0001
                            //         ^
                            //         |
$valkey->getBit('key', 7);  //         1 is the 8th element (zero based)
```

## setBit

_**Description**_: Sets or clears the bit at offset in the string value stored at key.

##### *Prototype*  

```php
public function setBit(string $key, int $offset, int $value): int {
    return $this->valkey->setBit($key, $offset, $value);
}
```

##### *Parameters*

- *key*: String. The Bitmap key.
- *offset*: int. The bit position within the string.
- *value*: int. 0 or 1, the value of the bit to set.

##### *Return value*

*int*: 0 or 1, the value of the bit before it was set.

##### *Example*

```php
$valkey->setBit('key', 1, 1);   // 01
$valkey->setBit('key', 7, 1);   // 0100 0001
$valkey->get('key');            // A => 0100 0001

$valkey->set('key', "*");       // ord("*") = 42 = 0x2f = "0010 1010"
$valkey->setBit('key', 5, 1);   // returns 0
$valkey->setBit('key', 7, 1);   // returns 0
$valkey->get('key');            // chr(0x2f) = "/" = b("0010 1111")
```
