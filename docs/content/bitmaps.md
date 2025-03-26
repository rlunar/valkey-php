# Valkey PHP  - Bitmaps

|Command                                |Description                                                        |Supported              |Tested                 |Class/Trait    |Method             |
|---                                    |---                                                                |:-:                    |:-:                    |---            |---                |
|[bitCount](#bitCount)                  |Count set bits in a string                                         |:white\_check\_mark:   |:white\_check\_mark:   |Bits           |bitCount           |
|[bitField](#bitField)                  |Perform arbitrary bitfield integer operations on strings           |:x:                    |:x:                    |Bits           |bitField           |
|[bitFieldReadOnly](#bitFieldReadOnly)  |Perform arbitrary read-only bitfield integer operations on strings |:x:                    |:x:                    |Bits           |bitFieldReadOnly   |
|[bitOp](#bitOp)                        |Perform bitwise operations between strings                         |:white\_check\_mark:   |:white\_check\_mark:   |Bits           |bitOp              |
|[bitPos](#bitPos)                      |Find first bit set or clear in a string                            |:x:                    |:x:                    |Bits           |bitPos             |
|[getBit](#getBit)                      |Returns the bit value at offset in the string value stored at key  |:white\_check\_mark:   |:white\_check\_mark:   |Bits           |getBit             |
|[setBit](#setBit)                      |Sets or clears the bit at offset in the string value stored at key |:white\_check\_mark:   |:white\_check\_mark:   |Bits           |setBit             |

## Usage

```php
$valkey = new Valkey();
$valkey->bitCount('key');
$valkey->bitField('key');
$valkey->bitFieldReadOnly('key');
$valkey->bitPos('key');
$valkey->bitOp('key');
$valkey->getBit('key');
$valkey->setBit('key');
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
$valkey->set('key', 'a'); // 1100001 in Binary or 97 in decimal
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
$valkey->set('key', 'A'); // 01000001
$valkey->getBit('key', 0); // 0
$valkey->getBit('key', 1); // 1
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
$valkey->setBit('key', 1, 1);
$valkey->setBit('key', 7, 1);
$valkey->get('key');            // A => 0100 0001

$valkey->set('key', "*");       // ord("*") = 42 = 0x2f = "0010 1010"
$valkey->setBit('key', 5, 1);   // returns 0
$valkey->setBit('key', 7, 1);   // returns 0
$valkey->get('key');            // chr(0x2f) = "/" = b("0010 1111")
```
