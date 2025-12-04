# Binary Number Conversion & Computer Storage

## Table of Contents
1. [Decimal to Binary Conversion](#decimal-to-binary)
2. [Binary to Decimal Conversion](#binary-to-decimal)
3. [1's Complement](#ones-complement)
4. [2's Complement](#twos-complement)
5. [How Computers Store Numbers](#computer-storage)

---

## 1. Decimal to Binary Conversion {#decimal-to-binary}

### The Concept
Converting decimal (base-10) to binary (base-2) means expressing a number using only 0s and 1s. Think of it like breaking down money into coins - we keep dividing by 2 and track the remainders.

### Visual Example: Converting 13 to Binary

```
13 ÷ 2 = 6  remainder 1  ← (rightmost bit)
 6 ÷ 2 = 3  remainder 0
 3 ÷ 2 = 1  remainder 1
 1 ÷ 2 = 0  remainder 1  ← (leftmost bit)

Reading remainders from bottom to top: 1101
```

**Visual Representation:**
```
Position:  8  4  2  1  (powers of 2)
Binary:    1  1  0  1
Value:     8+ 4+ 0+ 1 = 13 ✓
```

### Python Implementation

```python
def decimal_to_binary(n):
    """
    Convert decimal number to binary string
    """
    if n == 0:
        return "0"
    
    binary = ""
    original = n
    
    while n > 0:
        remainder = n % 2
        binary += str(remainder)
        n = n // 2
    
    return binary[::-1]

# Example usage
print(f"13 in binary: {decimal_to_binary(13)}")  # Output: 1101
print(f"25 in binary: {decimal_to_binary(25)}")  # Output: 11001
print(f"255 in binary: {decimal_to_binary(255)}")  # Output: 11111111

# Python's built-in method
print(f"13 in binary (built-in): {bin(13)}")  # Output: 0b1101
print(f"13 in binary (no prefix): {bin(13)[2:]}")  # Output: 1101
```

### Method 2: Using Bit Positions

```python
def decimal_to_binary_bits(n, num_bits=8):
    """
    Convert to binary with fixed bit width
    """
    binary = ""
    for i in range(num_bits - 1, -1, -1):
        if n & (1 << i):  # check if bit at position i is set
            binary += "1"
        else:
            binary += "0"
    return binary

print(decimal_to_binary_bits(13, 8))  # Output: 00001101
```

---

## 2. Binary to Decimal Conversion {#binary-to-decimal}

### The Concept
Each binary digit (bit) represents a power of 2. We multiply each bit by its position value and sum them up.

### Visual Example: Converting 1101 to Decimal

```
Position (right to left):  3    2    1    0
Binary digit:              1    1    0    1
Power of 2:               2³   2²   2¹   2⁰
Value:                     8    4    0    1
                          ─────────────────
                          8 + 4 + 0 + 1 = 13
```

**Step-by-step breakdown:**
```
1101₂ = (1 × 8) + (1 × 4) + (0 × 2) + (1 × 1)
      = 8 + 4 + 0 + 1
      = 13₁₀
```

### Python Implementation

```python
def binary_to_decimal(binary_str):
    """
    Convert binary string to decimal number
    """
    decimal = 0
    power = 0
    
    # Process from right to left
    for i in range(len(binary_str) - 1, -1, -1):
        if binary_str[i] == '1':
            decimal += 2 ** power
        power += 1
    
    return decimal

# Example usage
print(f"1101 in decimal: {binary_to_decimal('1101')}")  # Output: 13
print(f"11001 in decimal: {binary_to_decimal('11001')}")  # Output: 25
print(f"11111111 in decimal: {binary_to_decimal('11111111')}")  # Output: 255

# Python's built-in method
print(f"1101 in decimal (built-in): {int('1101', 2)}")  # Output: 13
```

### Method 2: Using Bit Shifting

```python
def binary_to_decimal_shift(binary_str):
    """
    Convert using bit shifting (more efficient)
    """
    decimal = 0
    for bit in binary_str:
        decimal = (decimal << 1) | int(bit)  # shift left and add bit
    return decimal

print(binary_to_decimal_shift('1101'))  # Output: 13
```

---

## 3. 1's Complement {#ones-complement}

### The Concept
The 1's complement of a binary number is obtained by **flipping all bits** (0 becomes 1, 1 becomes 0). It's like creating a "negative" version in the binary world.

### Visual Example

```
Original number:  0 1 0 1 1 0 1 0  (90 in decimal)
                  ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓  (flip all bits)
1's complement:   1 0 1 0 0 1 0 1  (165 in decimal)
```

**Another example:**
```
5 in 8-bit binary:    0 0 0 0 0 1 0 1
1's complement of 5:  1 1 1 1 1 0 1 0  (represents -5 in 1's complement system)
```

### Python Implementation

```python
def ones_complement(n, num_bits=8):
    """
    Calculate 1's complement of a number
    """
    # Create a mask with all bits set to 1
    mask = (1 << num_bits) - 1  # For 8 bits: 11111111
    
    # XOR with mask flips all bits
    complement = n ^ mask
    
    return complement

# Example usage
num = 5  # Binary: 00000101
comp = ones_complement(num, 8)
print(f"Number: {num} → Binary: {bin(num)}")
print(f"1's complement: {comp} → Binary: {bin(comp)}")
# Output: Number: 5 → Binary: 0b101
#         1's complement: 250 → Binary: 0b11111010

# Manual method - flip each bit
def ones_complement_manual(binary_str):
    """
    Flip each bit in binary string
    """
    result = ""
    for bit in binary_str:
        result += '0' if bit == '1' else '1'
    return result

print(f"1's complement of '00000101': {ones_complement_manual('00000101')}")
# Output: 11111010
```

---

## 4. 2's Complement {#twos-complement}

### The Concept
The 2's complement is the **most common way** computers represent negative numbers. It's calculated as: **1's complement + 1**

### Why 2's Complement?
- Only one representation of zero
- Addition and subtraction use the same circuitry
- No special handling needed for negative numbers

### Visual Example: Finding -5 in 8-bit

```
Step 1: Start with 5
        0 0 0 0 0 1 0 1

Step 2: Find 1's complement (flip bits)
        1 1 1 1 1 0 1 0

Step 3: Add 1
        1 1 1 1 1 0 1 0
      +               1
        ───────────────
        1 1 1 1 1 0 1 1  ← This is -5 in 2's complement!
```

### Verification: -5 + 5 = 0

```
    1 1 1 1 1 0 1 1  (-5)
  + 0 0 0 0 0 1 0 1  (+5)
    ───────────────
  1 0 0 0 0 0 0 0 0  
  ↑
  Overflow bit (discarded in 8-bit system)
  
Result: 0 0 0 0 0 0 0 0 = 0 ✓
```

### Python Implementation

```python
def twos_complement(n, num_bits=8):
    """
    Calculate 2's complement of a number
    """
    # Step 1: Get 1's complement
    mask = (1 << num_bits) - 1
    ones_comp = n ^ mask
    
    # Step 2: Add 1
    twos_comp = (ones_comp + 1) & mask  # mask ensures we stay within bit width
    
    return twos_comp

# Example usage
num = 5
comp = twos_complement(num, 8)
print(f"Number: {num} → Binary: {format(num, '08b')}")
print(f"2's complement: {comp} → Binary: {format(comp, '08b')}")
# Output: Number: 5 → Binary: 00000101
#         2's complement: 251 → Binary: 11111011

# Convert 2's complement back to decimal (with sign)
def twos_complement_to_decimal(binary_value, num_bits=8):
    """
    Interpret a binary value as 2's complement signed integer
    """
    # Check if sign bit is set (negative number)
    if binary_value & (1 << (num_bits - 1)):
        # It's negative - convert back
        return binary_value - (1 << num_bits)
    else:
        # It's positive
        return binary_value

print(f"251 as signed 8-bit: {twos_complement_to_decimal(251, 8)}")
# Output: -5
```

### Range of 2's Complement Numbers

For **n bits**, the range is: **-2^(n-1)** to **2^(n-1) - 1**

```
8-bit range:  -128 to +127
16-bit range: -32,768 to +32,767
32-bit range: -2,147,483,648 to +2,147,483,647
```

---

## 5. How Computers Store Positive and Negative Numbers {#computer-storage}

### The Sign Bit System

In signed integers, the **leftmost bit** is the **sign bit**:
- **0** = Positive
- **1** = Negative

```
8-bit signed integer representation:

┌─────────┬─────────────────────┐
│ Sign Bit│   Magnitude Bits    │
└─────────┴─────────────────────┘
    ↓              ↓
    0 = +      value in 2's complement
    1 = -
```

### Visual Examples (8-bit signed)

```
Positive Numbers (sign bit = 0):
┌───────────────────────────────────┐
│ 0 0 0 0 0 0 0 0  →  0            │
│ 0 0 0 0 0 0 0 1  →  +1           │
│ 0 0 0 0 0 1 0 1  →  +5           │
│ 0 1 1 1 1 1 1 1  →  +127 (max)   │
└───────────────────────────────────┘

Negative Numbers (sign bit = 1):
┌───────────────────────────────────┐
│ 1 1 1 1 1 1 1 1  →  -1           │
│ 1 1 1 1 1 0 1 1  →  -5           │
│ 1 0 0 0 0 0 0 1  →  -127         │
│ 1 0 0 0 0 0 0 0  →  -128 (min)   │
└───────────────────────────────────┘
```

### Complete 8-bit Number Line

```
Binary          Unsigned    Signed (2's complement)
────────────────────────────────────────────────────
0000 0000          0              0
0000 0001          1             +1
  ...             ...            ...
0111 1111        127           +127  ← Max positive
1000 0000        128           -128  ← Min negative
1000 0001        129           -127
  ...             ...            ...
1111 1110        254            -2
1111 1111        255            -1
```

### Python: Working with Signed Numbers

```python
import struct

def show_storage(num, num_bits=8):
    """
    Show how a number is stored in different representations
    """
    print(f"\n{'='*50}")
    print(f"Number: {num}")
    print(f"{'='*50}")
    
    # Unsigned representation
    if num >= 0:
        unsigned = num
        print(f"Unsigned {num_bits}-bit: {format(unsigned, f'0{num_bits}b')} = {unsigned}")
    
    # Signed representation (2's complement)
    if num < 0:
        # Convert negative to 2's complement
        mask = (1 << num_bits) - 1
        signed_binary = (abs(num) ^ mask) + 1
        signed_binary &= mask
    else:
        signed_binary = num
    
    print(f"Signed {num_bits}-bit:   {format(signed_binary, f'0{num_bits}b')} = {num}")
    
    # Show the sign bit
    sign_bit = (signed_binary >> (num_bits - 1)) & 1
    print(f"Sign bit: {sign_bit} ({'negative' if sign_bit else 'positive'})")

# Examples
show_storage(5, 8)
show_storage(-5, 8)
show_storage(127, 8)
show_storage(-128, 8)

# Python's ctypes for signed conversions
import ctypes

def to_signed(n, num_bits=8):
    """Convert unsigned to signed interpretation"""
    if n >= (1 << (num_bits - 1)):
        return n - (1 << num_bits)
    return n

def to_unsigned(n, num_bits=8):
    """Convert signed to unsigned interpretation"""
    if n < 0:
        return (1 << num_bits) + n
    return n

print(f"\n255 as signed 8-bit: {to_signed(255, 8)}")  # -1
print(f"-1 as unsigned 8-bit: {to_unsigned(-1, 8)}")  # 255
```

### Real-World Integer Types

```python
"""
Common integer types in programming:

Type              Bits    Range
────────────────────────────────────────────────
int8 / byte       8       -128 to 127
uint8 / ubyte     8       0 to 255
int16 / short     16      -32,768 to 32,767
uint16 / ushort   16      0 to 65,535
int32 / int       32      -2,147,483,648 to 2,147,483,647
uint32 / uint     32      0 to 4,294,967,295
int64 / long      64      -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
uint64 / ulong    64      0 to 18,446,744,073,709,551,615
"""

# Python handles arbitrary precision, but we can simulate fixed-width
def simulate_overflow(value, num_bits=8, signed=True):
    """
    Simulate integer overflow in fixed-width integers
    """
    if signed:
        max_val = (1 << (num_bits - 1)) - 1
        min_val = -(1 << (num_bits - 1))
        # Wrap around using modulo
        range_size = 1 << num_bits
        value = ((value + (1 << (num_bits - 1))) % range_size) - (1 << (num_bits - 1))
    else:
        max_val = (1 << num_bits) - 1
        value = value % (1 << num_bits)
    
    return value

# Examples of overflow
print(f"127 + 1 in signed 8-bit: {simulate_overflow(128, 8, True)}")  # -128
print(f"255 + 1 in unsigned 8-bit: {simulate_overflow(256, 8, False)}")  # 0
```

---

## Key Takeaways 🎯

1. **Decimal to Binary**: Repeatedly divide by 2, track remainders
2. **Binary to Decimal**: Multiply each bit by its power of 2 and sum
3. **1's Complement**: Flip all bits (rarely used for storage)
4. **2's Complement**: Standard method for storing negative numbers (1's complement + 1)
5. **Sign Bit**: Leftmost bit indicates positive (0) or negative (1)
6. **Range**: n-bit signed integers range from -2^(n-1) to 2^(n-1) - 1

### Quick Reference Card

```
Operation                  Formula/Method
─────────────────────────────────────────────
Decimal → Binary          Divide by 2, track remainders
Binary → Decimal          Sum of (bit × 2^position)
1's Complement            Flip all bits (XOR with mask)
2's Complement            1's complement + 1
Negative Number           2's complement of positive
Check Sign                Test leftmost bit
```
