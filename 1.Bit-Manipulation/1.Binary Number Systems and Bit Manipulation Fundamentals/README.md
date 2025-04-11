# Chapter: Binary Number Systems and Bit Manipulation Fundamentals

## Overview
This chapter covers fundamental concepts of binary number systems and bit manipulation, essential for understanding how computers store and manipulate data at the lowest level. We explore binary conversion methods, ones and twos complement representation for handling negative numbers, and the five core bitwise operators (AND, OR, XOR, shifts, and NOT). These concepts form the foundation for efficient algorithmic problem-solving in data structures and algorithms, particularly for optimization problems where bit-level operations provide elegant solutions.

## Concept Map: Bit Manipulation Fundamentals
```
                              +-------------------------+
                              |  Binary Number Systems  |
                              +-------------------------+
                                         |
         +------------------------+------+------+-------------------------+
         |                        |             |                         |
+--------v--------+    +---------v---------+   v                  +------v-------+
| Binary-Decimal  |    | Number             |  Integer            | Bitwise       |
| Conversion      |    | Representation     |  Storage            | Operators     |
+--------+--------+    +---------+---------+  * 32 bits           +------+-------+
         |                      |             * Sign bit (MSB)           |
         |                      |             * Positive/Negative        |
   +-----v-----+        +------v------+                           +-----v------+
   | Decimal   |        | Ones         |                          |            |
   | to Binary |        | Complement   |                       +--+ Logical    |
   +-----------+        +------+-------+                       |  | Operators  |
                               |                               |  +------------+
   +-----------+        +------v-------+                       |   * AND (&)
   | Binary    |        | Twos         |                       |   * OR (|)
   | to Decimal|        | Complement   |                       |   * XOR (^)
   +-----------+        +--------------+                       |
                                                               |  +------------+
                                                               +--+ Shift      |
                                                               |  | Operators  |
                                                               |  +------------+
                                                               |   * Left (<<)
                                                               |   * Right (>>)
                                                               |
                                                               |  +------------+
                                                               +--+ NOT (~)    |
                                                                  +------------+
```

## Detailed Sections

### 1. Binary-Decimal Conversion
Binary is the foundational numbering system in computing, where data is represented using only two digits: 0 and 1.

#### Decimal to Binary Conversion
To convert a decimal number to binary:

1. **Process**: Divide the number by 2 repeatedly and record the remainders
2. **Reading**: Read the remainders from bottom to top

**Example: Converting 7 (base 10) to binary**
```
7 ÷ 2 = 3 remainder 1 ↓
3 ÷ 2 = 1 remainder 1 ↓
1 ÷ 2 = 0 remainder 1 ↓
           Reading up: 111
```

**Example: Converting 13 (base 10) to binary**
```
13 ÷ 2 = 6 remainder 1 ↓
 6 ÷ 2 = 3 remainder 0 ↓
 3 ÷ 2 = 1 remainder 1 ↓
 1 ÷ 2 = 0 remainder 1 ↓
           Reading up: 1101
```

#### Binary to Decimal Conversion
To convert a binary number to decimal:

1. **Process**: Multiply each digit by 2 raised to its position power (starting from the right with power 0)
2. **Sum**: Add all the resulting values

**Example: Converting 1101 (base 2) to decimal**
```
1101 = 1×2³ + 1×2² + 0×2¹ + 1×2⁰
     = 1×8 + 1×4 + 0×2 + 1×1
     = 8 + 4 + 0 + 1
     = 13
```

#### Implementation: Decimal to Binary Conversion
```cpp
#include <string>

std::string convertToBinary(int n) {
    std::string result = "";
    
    // Handle case where n=0
    if (n == 0) {
        return "0";
    }
    
    while (n != 0) {
        if (n % 2 == 1) {
            result = "1" + result;  // Add "1" if remainder is 1
        } else {
            result = "0" + result;  // Add "0" if remainder is 0
        }
        n /= 2;  // Integer division by 2
    }
    
    return result;
}
```

**Time Complexity**: O(log₂ n) - We divide by 2 repeatedly
**Space Complexity**: O(log₂ n) - The resulting string length is proportional to log₂ n

#### Implementation: Binary to Decimal Conversion
```cpp
#include <string>

int convertToDecimal(const std::string& binaryStr) {
    int number = 0;
    int powerOfTwo = 1;  // Starting with 2⁰
    
    // Iterate from right to left
    for (int i = binaryStr.length() - 1; i >= 0; i--) {
        if (binaryStr[i] == '1') {
            number += powerOfTwo;
        }
        powerOfTwo *= 2;  // Multiply by 2 for each position
    }
    
    return number;
}
```

**Time Complexity**: O(n) - where n is the length of the binary string
**Space Complexity**: O(1) - We only use constant extra space

### 2. Number Representation in Memory

#### How Integers Are Stored
- An integer typically occupies **32 bits** in memory (4 bytes)
- When we write `int x = 13`, the computer stores `13` as `00000000 00000000 00000000 00001101`
- For numbers with fewer than 32 bits, leading zeros are added to fill the 32-bit space
- The computer always converts decimal numbers to binary for storage

#### Signed Integers
- The **most significant bit (MSB)** or leftmost bit is used as the **sign bit**
  - 0 indicates a positive number
  - 1 indicates a negative number
- This leaves 31 bits for storing the magnitude of the number

#### Integer Range
- **Maximum value**: 2³¹ - 1 (all 31 bits set to 1, sign bit is 0)
- **Minimum value**: -2³¹ (all 31 bits set to 0, sign bit is 1)

### 3. Ones and Twos Complement

Computers use complement representations to handle negative numbers efficiently.

#### Ones Complement
- **Definition**: Flip all bits (change 0s to 1s and 1s to 0s)
- **Example**: Ones complement of `1101` (13)
  ```
  Original: 0000...1101
  Flip all: 1111...0010
  ```

#### Twos Complement
A two-step process:
1. Find the ones complement (flip all bits)
2. Add 1 to the result

**Example**: Twos complement of `1101` (13)
```
Step 1 (Ones complement): 1111...0010
Step 2 (Add 1):           1111...0011
```

#### Storing Negative Numbers
The computer stores negative numbers using twos complement.

**Example**: For `-3`
```
1. Start with 3:       0000...0011
2. Ones complement:    1111...1100
3. Add 1 (Twos comp.): 1111...1101
```

This is how `-3` is actually stored in memory.

### 4. Bitwise Operators

#### Logical Operators

##### AND Operator (&)
- **Rule**: Result is 1 only if both bits are 1; otherwise 0
- **Example**: 13 & 7
  ```
  13: 1101
   7: 0111
  -------------
  &:  0101 (5)
  ```

##### OR Operator (|)
- **Rule**: Result is 1 if at least one of the bits is 1; otherwise 0
- **Example**: 13 | 7
  ```
  13: 1101
   7: 0111
  -------------
  |:  1111 (15)
  ```

##### XOR Operator (^)
- **Rule**: Result is 1 if exactly one of the bits is 1 (odd number of 1s); otherwise 0
- **Example**: 13 ^ 7
  ```
  13: 1101
   7: 0111
  -------------
  ^:  1010 (10)
  ```

#### Shift Operators

##### Right Shift (>>)
- **Operation**: Shifts bits to the right by specified positions
- **Effect**: Equivalent to division by 2ᵏ (where k is the shift amount)
- **Example**: 13 >> 1
  ```
  13:      1101
  13 >> 1: 0110 (6)
  ```

**More examples**:
- 13 >> 2 = 3 (13 ÷ 2²)
- 13 >> 4 = 0 (13 ÷ 2⁴)

##### Left Shift (<<)
- **Operation**: Shifts bits to the left by specified positions
- **Effect**: Equivalent to multiplication by 2ᵏ (where k is the shift amount)
- **Example**: 13 << 1
  ```
  13:      1101
  13 << 1: 11010 (26)
  ```

**Warning**: Left shifts can cause overflow if the result exceeds the maximum representable integer

#### Bitwise NOT Operator (~)

## Basic Operation
The NOT operator (`~`) performs a bitwise complement operation, which means it flips each bit in the operand (changes 0s to 1s and 1s to 0s).

## Key Concept
When working with signed integers, the result appears in two's complement form, which is how computers represent negative numbers.

## Example: ~5

### Step 1: Binary Representation
```
5 in binary: 00000000 00000000 00000000 00000101
```

### Step 2: Apply NOT Operation (Flip All Bits)
```
After NOT: 11111111 11111111 11111111 11111010
```
This result has a 1 in the sign bit position, which indicates a negative number.

### Step 3: Interpret the Result
In two's complement representation, this equals -6.

## The Simple Formula
For any integer x:
```
~x = -(x+1)
```

## Verification
~5 = -(5+1) = -6 ✓

## Practical Examples

### Example 1: Binary Conversion Functions

```cpp
#include <iostream>
#include <string>

std::string decimalToBinary(int decimalNum) {
    /* Convert decimal to binary string */
    if (decimalNum == 0) {
        return "0";
    }
    
    std::string binary = "";
    while (decimalNum > 0) {
        binary = std::to_string(decimalNum % 2) + binary;
        decimalNum /= 2;
    }
    return binary;
}

int binaryToDecimal(const std::string& binaryStr) {
    /* Convert binary string to decimal integer */
    int decimal = 0;
    for (char digit : binaryStr) {
        decimal = decimal * 2 + (digit - '0');
    }
    return decimal;
}

// Example usage
int main() {
    std::cout << decimalToBinary(13) << std::endl;  // Output: 1101
    std::cout << binaryToDecimal("1101") << std::endl;  // Output: 13
    return 0;
}
```

### Example 2: Checking if a number is odd or even using bitwise operators

```cpp
#include <iostream>

bool isOdd(int n) {
    /**
     * Returns true if n is odd, false otherwise
     * Using bitwise AND with 1 checks the least significant bit
     */
    return (n & 1) == 1;
}

// Example usage
int main() {
    std::cout << std::boolalpha;  // Print true/false instead of 1/0
    std::cout << isOdd(5) << std::endl;  // Output: true
    std::cout << isOdd(10) << std::endl;  // Output: false
    return 0;
}
```

### Example 3: Counting set bits (1s) in an integer

```cpp
#include <iostream>

int countSetBits(int n) {
    /**
     * Count the number of 1's in the binary representation of n
     */
    int count = 0;
    while (n) {
        count += n & 1;  // Check if least significant bit is 1
        n >>= 1;         // Right shift by 1
    }
    return count;
}

// Example usage
int main() {
    std::cout << countSetBits(13) << std::endl;  // Output: 3 (13 is 1101 in binary)
    return 0;
}
```

## Interview Preparation Tips

1. **Understand twos complement thoroughly**: Many bit manipulation questions rely on understanding how negative numbers are represented.

2. **Know the bitwise operations by heart**: Be able to perform AND, OR, XOR, shifts, and NOT operations manually.

3. **Practice common bit manipulation techniques**:
   - Setting a specific bit: `num | (1 << position)`
   - Clearing a specific bit: `num & ~(1 << position)`
   - Toggling a specific bit: `num ^ (1 << position)`
   - Checking if a bit is set: `(num & (1 << position)) != 0`

4. **Remember these common patterns**:
   - `n & (n-1)` removes the rightmost set bit
   - `n & -n` isolates the rightmost set bit
   - `x ^ y ^ x` equals `y` (XOR property for finding unique elements)

5. **Memorize these key formulas**:
   - Right shift: `n >> k` = `n / 2^k`
   - Left shift: `n << k` = `n * 2^k`
   - NOT: `~n` = `-(n+1)`

6. **Know how to approach these common problems**:
   - Find the single element in an array where all others appear twice
   - Swap two numbers without using a temporary variable
   - Check if a number is a power of 2
   - Count the number of set bits in an integer

7. **Understand practical applications**: Many bit manipulation techniques are used for optimizing space and time in algorithms.

8. **Be aware of integer overflow**: Especially when using left shift operations.

## Common Mistakes or Misunderstandings

1. **Confusing ones complement and twos complement**: Remember that twos complement requires adding 1 after flipping the bits.

2. **Forgetting about the sign bit**: When working with signed integers, the leftmost bit determines the sign.

3. **Overlooking integer overflow**: Operations like left shift can easily cause overflow if not careful.

4. **Misinterpreting right shift results with negative numbers**: In C++, right shift on negative numbers performs arithmetic shift (preserving sign bit).

5. **Incorrectly calculating binary-decimal conversions**: Double-check your conversions by working through the examples step by step.

6. **Forgetting that computers store integers in binary**: Many beginners forget that `int x = 13` is actually stored as `1101` in memory.

7. **Mistaking XOR properties**: XOR is commutative and associative, but many forget that `a ^ a = 0` and `a ^ 0 = a`.

8. **Applying bit operations without understanding**: Randomly applying bit operations without understanding the underlying principles often leads to errors.

## Tooling & IDE Integration

### Visual Studio
- **Binary display**: Debug → Windows → Memory → Memory 1 to view binary representation of variables
- **Bitwise evaluation**: The Watch window can evaluate bitwise expressions during debugging

### CLion
- **Binary viewer**: Memory View shows binary representation during debugging
- **Expression evaluation**: Use the Evaluate Expression feature to test bitwise operations

### Other IDEs
- Most modern IDEs provide features to examine variables at the bit level during debugging
- Some offer plugins specifically for bit manipulation visualization

### Online Tools
- **Binary converters**: Websites like [Bit Calculator](https://www.calculator.net/binary-calculator.html) provide interactive binary conversion
- **Bitwise operation visualizers**: Tools that show step-by-step application of bitwise operators

## Advanced Notes / Behind the Scenes

### Integer Storage Variations
- **Unsigned integers**: In unsigned integers, all 32 bits are used for magnitude (no sign bit)
- **Long integers**: Usually 64 bits, following the same principles as 32-bit integers

### Endianness
- **Little-endian**: Least significant byte stored at the lowest memory address
- **Big-endian**: Most significant byte stored at the lowest memory address
- Affects how multi-byte integers are stored in memory

### Bit Manipulation in Hardware
- **Circuit-level operations**: Bitwise operations are among the most fundamental operations in digital circuits
- **Performance**: Bit manipulation operations are typically single-cycle operations at the hardware level

### Algorithmic Applications
- **Hash functions**: Extensive use of bit manipulation for creating hash values
- **Cryptography**: Bit manipulation is fundamental in encryption algorithms
- **Graphics processing**: Pixel manipulation often uses bitwise operations for efficiency

## Glossary

- **Binary**: Base-2 number system using only 0 and 1
- **Bit**: The smallest unit of data, a single binary digit (0 or 1)
- **MSB**: Most Significant Bit, the leftmost bit in a binary number
- **LSB**: Least Significant Bit, the rightmost bit in a binary number
- **Ones Complement**: Binary representation created by flipping all bits
- **Twos Complement**: Binary representation for negative numbers (ones complement + 1)
- **Bitwise Operation**: Operation that works directly on individual bits
- **Sign Bit**: The bit that indicates whether a number is positive or negative
- **Integer Overflow**: Occurs when an operation produces a value outside the representable range
- **Shift Operation**: Moving bits left or right by a specified number of positions
- **Endianness**: The order in which bytes are stored in memory
