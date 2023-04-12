# Learn Bits & Bytes Operations for Solidity and Yul

This is a collection of notes for learning bits and bytes operations in Solidity and Yul.

# Bits

## What is a bit?

A `bit` is a single binary digit. This is represenative of the `base 2` number system.

## How many numbers can be represented by a bit?

A `bit` can represent `2` numbers. `0` and `1`.

## Visual example of translating bits to a decimal number:

```text
// 32 16  8  4  2  1
// 0  0   0  0  0  0 = 0
// 0  0   0  0  0  1 = 1
// 0  0   0  0  1  0 = 2
// 0  0   0  0  1  1 = 3
// 0  0   0  1  0  0 = 4
// 0  0   0  1  0  1 = 5
// 0  0   0  1  1  0 = 6
// 0  0   0  1  1  1 = 7
// 1  1   0  1  0  1 = 53
```

# Bytes

## What is a byte?

A `byte` is a collection of `8 bits`.

## How many numbers can be represented by a byte?

A `byte` can represent `256` numbers. (N bits can represent 2^N numbers)

# Hexadecimals

## What is a hexadecimal?

A `hexadecimal` is a number system that uses `16` as its base. This is represenative of the `base 16` number system.

```text
0 .. 9 A  B  C  D  E  F
0 .. 9 10 11 12 13 14 15
```

A single hexadecimal digit can represent `4 bits`, so a `byte` can be represented by `2 hexadecimals`.

## Example of translating hexadecimals to a decimal number:

```text
0x00 = 0
0x01 = 1
0x02 = 2
...
0x09 = 9
0x0a = 10
0x0b = 11
...
0x0f = 15
0x10 = 16
0x11 = 17
...
0x1f = 31
0x20 = 32
0x21 = 33
...
0xfe = 254
0xff = 255
```

## bytes32 and uint256, the most common types in Solidity

`bytes32` = 32 bytes = 32 _ (2 hex) = 64 _ hex = 64 \*(4 bits) = 256 bits = `uint256`

# Bitwise Operators in Solidity and Yul

## AND

The `AND` operator compares bits at the same index of two bits arrays and returns `1` if both bits are `1`, otherwise it returns `0`.

-   Solidity: `&`

```solidity
// x     = 1110 = 8 + 4 + 2 + 0 = 14
// y     = 1011 = 8 + 0 + 2 + 1 = 11
// x & y = 1010 = 8 + 0 + 2 + 0 = 10
function and(uint x, uint y) external pure returns (uint) {
    return x & y;
}
```

-   Yul: `and`

```yul
// x     = 0xfff0
// y     = 0xf0ff
// x & y = 0xf0f0
function and(x, y) -> z {
    z := and(x, y)
}
```

## OR

The `OR` operator compares bits at the same index of two bits arrays and returns `1` if either bit is `1`, otherwise it returns `0`.

-   Solidity: `|`

```solidity
// x       = 1100 = 8 + 4 + 0 + 0 = 12
// y       = 1001 = 8 + 0 + 0 + 1 = 9
// x and y = 1101 = 8 + 4 + 0 + 1 = 13
function or(uint x, uint y) external pure returns (uint) {
    return x | y;
}
```

-   Yul: `or`

```yul
// x      = 0xff00
// y      = 0xf00f
// x or y = 0xff0f
function or(x, y) -> z {
    z := or(x, y)
}
```

## XOR

The `XOR` operator compares bits at the same index of two bits arrays and returns `1` if only one of the bits is 1, otherwise it returns `0`.

-   Solidity: `^`

```solidity
// x       = 1100 = 8 + 4 + 0 + 0 = 12
// y       = 1001 = 8 + 0 + 0 + 1 = 9
// x and y = 0101 = 0 + 4 + 0 + 1 = 5
function xor(uint x, uint y) external pure returns (uint) {
    return x ^ y;
}
```

-   Yul: `xor`

```yul
// x       = 0xff00
// y       = 0xf00f
// x xor y = 0x0f0f
function xor(x, y) -> z {
    z := xor(x, y)
}
```

## NOT

The `NOT` operator inverts the bits of a bit array.

-   Solidity: `~`

```solidity
// x      = 1100 = 8 + 4 + 0 + 0 = 12
// ~x     = 0011 = 0 + 2 + 0 + 0 = 3
function not(uint x) external pure returns (uint) {
    return ~x;
}
```

-   Yul: `not`

```yul
// x      = 0xff00
// ~x     = 0x00ff
function not(x, n) -> z {
    z := not(x)
}
```

When thinking of not() in term of bytes, every byte that is 0x00 will be 0xff and every byte that is 0xff will be 0x00.

## Left Shift

The `Left Shift` operator shifts the bits of a bit array to the left by a specified number of bits.

-   Solidity: `<<`

```solidity
// x        = 0001 = 1
// x << 1   = 0010 = 2
// x << 2   = 0100 = 4
// x << 3   = 1000 = 8
function leftShift(uint x, uint n) external pure returns (uint) {
    return x << n; // shift x to the left by n bits
}
```

-   Yul: `shl`

```yul
// x        = 0x01
// x shl 1  = 0x02
// x shl 2  = 0x04
// x shl 3  = 0x08
function leftShift(x, n) -> z {
    z := shl(n, x) // shift x to the left by n bits
}
```

-   To shift `bytes` in Yul, multiply the number of bytes by `8` and use `shl`:

```yul
// x          = 0x000001
// x shl 1*8  = 0x000100
// x shl 2*8  = 0x010000
function leftShift(n, x) -> z {
    z := shl(n*8, x)
}
```

## Right Shift

The `Right Shift` operator shifts the bits of a bit array to the right by a specified number of bits.

-   Solidity: `>>`

```solidity
// x        = 1000 = 8
// x >> 1   = 0100 = 4
// x >> 2   = 0010 = 2
// x >> 3   = 0001 = 1
function rightShift(uint x) external pure returns (uint) {
    return x >> 1;
}
```

-   Yul: `shr`

```yul
// x        = 0x08
// x shr 1  = 0x04
// x shr 2  = 0x02
// x shr 3  = 0x01
function rightShift(x) -> z {
    z := shr(1, x)
}
```

-   To shift `bytes` in Yul, multiply the number of bytes by `8` and use `shr`:

```yul
// x          = 0x010000
// x shr 1*8  = 0x000100
// x shr 2*8  = 0x000001
function rightShift(n, x) -> z {
    z := shr(n*8, x)
}
```

### When shifting a bit array to the right, the bits that are shifted out are lost.

```solidity
// x        = 10100 = 16 + 4 + 0 + 0 = 20
// x >> 1   = 01010 = 16 + 0 + 2 + 0 = 18
// x >> 2   = 00101 = 16 + 0 + 0 + 1 = 17
// x >> 3   = 00010 = 0 + 0 + 2 + 0 = 2
```
