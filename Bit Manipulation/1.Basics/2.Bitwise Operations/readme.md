# Bitwise Operators - Complete Guide

## Table of Contents
1. [AND Operator (&)](#and-operator)
2. [OR Operator (|)](#or-operator)
3. [XOR Operator (^)](#xor-operator)
4. [NOT Operator (~)](#not-operator)
5. [LEFT SHIFT (<<)](#left-shift)
6. [RIGHT SHIFT (>>)](#right-shift)
7. [Quick Reference & Comparison](#quick-reference)

---

## 1. AND Operator (&) {#and-operator}

### The Concept
The AND operator compares two bits and returns **1 only if BOTH bits are 1**, otherwise returns **0**.

Think of it like: "Both conditions must be true"

### Truth Table
```
A    B    A & B
─────────────────
0    0      0
0    1      0
1    0      0
1    1      1    ← Only this case gives 1
```

### Visual Example

```
Example 1: 12 & 10

    12 in binary:  0 0 0 0 1 1 0 0
    10 in binary:  0 0 0 0 1 0 1 0
                   ─────────────────  (AND operation)
    Result:        0 0 0 0 1 0 0 0  = 8

Position by position:
    0 & 0 = 0
    0 & 0 = 0
    0 & 0 = 0
    0 & 0 = 0
    1 & 1 = 1  ✓
    1 & 0 = 0
    0 & 1 = 0
    0 & 0 = 0
```

### Real-World Visual

```
Think of AND as a security system with TWO keys:

Key A: [1] Present    Key A: [0] Missing
Key B: [1] Present    Key B: [1] Present
       ───────               ───────
Door:  [1] OPEN ✓     Door:  [0] LOCKED ✗
```

### Common Use Cases

**1. Check if number is even or odd**
```python
is_even = (n & 1) == 0  # Last bit is 0 for even
```

**2. Check if a specific bit is set**
```python
is_bit_set = (num & (1 << position)) != 0
```

**3. Extract lower bits (masking)**
```python
lower_4_bits = num & 0b1111  # Get last 4 bits
lower_8_bits = num & 0xFF    # Get last 8 bits
```

**4. Permission checking (Unix-style)**
```python
READ = 4    # 100
WRITE = 2   # 010
EXECUTE = 1 # 001

permissions = READ | WRITE  # 110
has_read = (permissions & READ) != 0
```

---

## 2. OR Operator (|) {#or-operator}

### The Concept
The OR operator returns **1 if AT LEAST ONE bit is 1**, otherwise returns **0**.

Think of it like: "At least one condition must be true"

### Truth Table
```
A    B    A | B
─────────────────
0    0      0    ← Only this case gives 0
0    1      1
1    0      1
1    1      1
```

### Visual Example

```
Example: 12 | 10

    12 in binary:  0 0 0 0 1 1 0 0
    10 in binary:  0 0 0 0 1 0 1 0
                   ─────────────────  (OR operation)
    Result:        0 0 0 0 1 1 1 0  = 14

Position by position:
    0 | 0 = 0
    0 | 0 = 0
    0 | 0 = 0
    0 | 0 = 0
    1 | 1 = 1
    1 | 0 = 1  ✓
    0 | 1 = 1  ✓
    0 | 0 = 0
```

### Real-World Visual

```
Think of OR as a door with TWO buttons (either works):

Button A: [1] Pressed    Button A: [0] Not pressed
Button B: [0] Not pressed Button B: [0] Not pressed
          ───────               ───────
Door:     [1] OPEN ✓     Door:  [0] CLOSED ✗
```

### Common Use Cases

**1. Set a specific bit to 1**
```python
result = num | (1 << position)
```

**2. Combine flags/permissions**
```python
READ = 4     # 100
WRITE = 2    # 010
EXECUTE = 1  # 001
full_access = READ | WRITE | EXECUTE  # 111 = 7
```

**3. Merge bit patterns**
```python
# Combine two 4-bit values into 8-bit
result = (high << 4) | low
```

---

## 3. XOR Operator (^) {#xor-operator}

### The Concept
XOR (Exclusive OR) returns **1 if bits are DIFFERENT**, **0 if bits are SAME**.

Think of it like: "Exactly one must be true, not both"

### Truth Table
```
A    B    A ^ B
─────────────────
0    0      0    ← Same, returns 0
0    1      1    ← Different, returns 1
1    0      1    ← Different, returns 1
1    1      0    ← Same, returns 0
```

### Visual Example

```
Example: 12 ^ 10

    12 in binary:  0 0 0 0 1 1 0 0
    10 in binary:  0 0 0 0 1 0 1 0
                   ─────────────────  (XOR operation)
    Result:        0 0 0 0 0 1 1 0  = 6

Position by position:
    0 ^ 0 = 0 (same)
    0 ^ 0 = 0 (same)
    0 ^ 0 = 0 (same)
    0 ^ 0 = 0 (same)
    1 ^ 1 = 0 (same)
    1 ^ 0 = 1 (different) ✓
    0 ^ 1 = 1 (different) ✓
    0 ^ 0 = 0 (same)
```

### Real-World Visual

```
Think of XOR as a light switch toggle:

Switch A: [ON]     Switch B: [ON]
          ↓                   ↓
       Different?          Same?
          ↓                   ↓
     Light: [ON]        Light: [OFF]
```

### XOR Properties

**Important XOR Properties:**
```
n ^ 0 = n      (XOR with 0 returns the number)
n ^ n = 0      (XOR with itself returns 0)
n ^ m ^ m = n  (XOR is reversible/self-inverse)
```

### Common Use Cases

**1. Toggle a specific bit**
```python
result = num ^ (1 << position)  # 0→1 or 1→0
```

**2. Swap two numbers without temp variable**
```python
a = a ^ b
b = a ^ b  # b becomes original a
a = a ^ b  # a becomes original b
```

**3. Find unique number (others appear twice)**
```python
# Array: [4, 2, 7, 2, 4]
result = 0
for num in array:
    result ^= num  # XOR all numbers
# result = 7 (the unique one)
```

**4. Simple encryption (XOR cipher)**
```python
encrypted = message ^ key
decrypted = encrypted ^ key  # Gets back original message
```

---

## 4. NOT Operator (~) {#not-operator}

### The Concept
The NOT operator **flips all bits**: 0 becomes 1, and 1 becomes 0. It's also called the **complement operator**.

**Important**: In Python, `~n` returns `-(n+1)` due to two's complement representation!

### Truth Table
```
A    ~A
─────────
0     1
1     0
```

### Visual Example

```
Example: ~12 (in 8-bit)

    12 in binary:  0 0 0 0 1 1 0 0
                   ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓  (flip all bits)
    Result:        1 1 1 1 0 0 1 1

In unsigned: 243
In signed (2's complement): -13
```

### Real-World Visual

```
Think of NOT as an inverter switch:

Input:  [0] [1] [0] [1] [1] [0]
         ↓   ↓   ↓   ↓   ↓   ↓
Output: [1] [0] [1] [0] [0] [1]
```

### Python Note

**Important**: In Python, `~n` returns `-(n+1)` due to two's complement!

```
~5 = -6
~0 = -1
~(-1) = 0
```

For fixed bit-width NOT:
```python
result = (~num) & 0xFF  # For 8-bit
result = (~num) & 0xFFFF  # For 16-bit
```

### Common Use Cases

**1. Create bit masks**
```python
mask_4_bits = (1 << 4) - 1  # 0b1111 = 15
mask_8_bits = (1 << 8) - 1  # 0xFF = 255
```

**2. Clear specific bits**
```python
# Clear bit at position
result = num & ~(1 << position)
```

**3. Invert specific bits with mask**
```python
# Invert bits where mask is 1
result = num ^ mask
```

---

## 5. LEFT SHIFT (<<) {#left-shift}

### The Concept
Left shift moves all bits to the **left** by n positions and fills the right side with **0s**.

**Mathematical effect**: Each left shift by 1 **multiplies the number by 2**!

### Visual Example

```
Example: 5 << 2 (shift left by 2)

Original:  0 0 0 0 0 1 0 1  (5)
           ← ← shift left
Step 1:    0 0 0 0 1 0 1 0  (10)  ← added one 0
Step 2:    0 0 0 1 0 1 0 0  (20)  ← added another 0

Result: 20 = 5 × 2²
```

### Bit Movement Visual

```
   7 6 5 4 3 2 1 0  ← bit positions
   ─────────────────
   0 0 0 0 0 1 0 1  (5)
       ↙ ↙ ↙ ↙ ↙ 
   0 0 0 1 0 1 0 0  (20)
                 ↑↑
           filled with 0s
```

### Quick Calculation Formula

```
n << k = n × 2^k

Examples:
5 << 1 = 5 × 2¹ = 5 × 2 = 10
5 << 2 = 5 × 2² = 5 × 4 = 20
5 << 3 = 5 × 2³ = 5 × 8 = 40
7 << 4 = 7 × 2⁴ = 7 × 16 = 112
```

### Common Use Cases

**1. Fast multiplication by powers of 2**
```python
result = n << k  # Same as n × 2^k
```

**2. Create powers of 2**
```python
power = 1 << n   # Same as 2^n
# 1 << 0 = 1
# 1 << 1 = 2
# 1 << 2 = 4
# 1 << 3 = 8
```

**3. Set bit at position**
```python
result = num | (1 << position)
```

**4. Create bit masks**
```python
mask = (1 << n) - 1  # Creates n bits of 1s
# (1 << 4) - 1 = 16 - 1 = 15 = 0b1111
```

**5. Pack multiple values**
```python
# Pack RGB into single integer
color = (red << 16) | (green << 8) | blue
```

---

## 6. RIGHT SHIFT (>>) {#right-shift}

### The Concept
Right shift moves all bits to the **right** by n positions. Bits on the left are filled based on:
- **Logical shift**: Fill with **0s** (unsigned numbers)
- **Arithmetic shift**: Fill with **sign bit** (signed numbers)

**Mathematical effect**: Each right shift by 1 **divides the number by 2** (integer division)!

### Visual Example

```
Example: 20 >> 2 (shift right by 2)

Original:  0 0 0 1 0 1 0 0  (20)
                 → → shift right
Step 1:    0 0 0 0 1 0 1 0  (10)  ← lost one bit
Step 2:    0 0 0 0 0 1 0 1  (5)   ← lost another bit

Result: 5 = 20 ÷ 2² (integer division)
```

### Bit Movement Visual

```
   7 6 5 4 3 2 1 0  ← bit positions
   ─────────────────
   0 0 0 1 0 1 0 0  (20)
       ↘ ↘ ↘ ↘ ↘ 
   0 0 0 0 0 1 0 1  (5)
   ↑↑
   filled with 0s
```

### Signed vs Unsigned Right Shift

```
Positive number: 8 >> 1
   0 0 0 0 1 0 0 0  (8)
     ↘ ↘ ↘ ↘ ↘ ↘ ↘
   0 0 0 0 0 1 0 0  (4)
   ↑
   fill with 0

Negative number: -8 >> 1 (in 8-bit)
   1 1 1 1 1 0 0 0  (-8)
     ↘ ↘ ↘ ↘ ↘ ↘ ↘
   1 1 1 1 1 1 0 0  (-4)
   ↑
   fill with 1 (sign bit)
```

### Quick Calculation Formula

```
n >> k = n ÷ 2^k  (integer division)

Examples:
20 >> 1 = 20 ÷ 2¹ = 20 ÷ 2 = 10
20 >> 2 = 20 ÷ 2² = 20 ÷ 4 = 5
20 >> 3 = 20 ÷ 2³ = 20 ÷ 8 = 2
100 >> 4 = 100 ÷ 2⁴ = 100 ÷ 16 = 6 (integer division)
```

### Common Use Cases

**1. Fast division by powers of 2**
```python
result = n >> k  # Same as n ÷ 2^k (integer division)
```

**2. Extract specific bits**
```python
# Extract bits from position 'start' with 'length' bits
result = (num >> start) & ((1 << length) - 1)
```

**3. Check if power of 2**
```python
is_power_of_2 = n > 0 and (n & (n - 1)) == 0
```

**4. Get middle element index (safe from overflow)**
```python
mid = left + ((right - left) >> 1)
```

**5. Unpack color values**
```python
# Unpack RGB from 24-bit color
red = (color >> 16) & 0xFF
green = (color >> 8) & 0xFF
blue = color & 0xFF
```

**6. Divide by 8 (shift right by 3)**
```python
result = num >> 3  # Same as num ÷ 8
```

---

## 7. Quick Reference & Comparison {#quick-reference}

### Operator Summary Table

```
┌─────────────┬──────────┬─────────────────────────────────────┐
│  Operator   │  Symbol  │  Description                        │
├─────────────┼──────────┼─────────────────────────────────────┤
│  AND        │    &     │  1 if BOTH bits are 1               │
│  OR         │    |     │  1 if AT LEAST ONE bit is 1         │
│  XOR        │    ^     │  1 if bits are DIFFERENT            │
│  NOT        │    ~     │  FLIP all bits                      │
│  LEFT       │    <<    │  Shift left (multiply by 2^n)       │
│  RIGHT      │    >>    │  Shift right (divide by 2^n)        │
└─────────────┴──────────┴─────────────────────────────────────┘
```

### Side-by-Side Comparison

```python
def compare_all_operators():
    """Compare all operators on same inputs"""
    a = 12  # Binary: 1100
    b = 10  # Binary: 1010
    
    print("=" * 60)
    print("BITWISE OPERATORS COMPARISON")
    print("=" * 60)
    print(f"a = {a:2d} → Binary: {format(a, '08b')}")
    print(f"b = {b:2d} → Binary: {format(b, '08b')}")
    print("-" * 60)
    
    print(f"a & b  = {a & b:2d} → Binary: {format(a & b, '08b')}  (AND)")
    print(f"a | b  = {a | b:2d} → Binary: {format(a | b, '08b')}  (OR)")
    print(f"a ^ b  = {a ^ b:2d} → Binary: {format(a ^ b, '08b')}  (XOR)")
    print(f"~a     = {(~a & 0xFF):2d} → Binary: {format(~a & 0xFF, '08b')}  (NOT, 8-bit)")
    print(f"a << 1 = {a << 1:2d} → Binary: {format(a << 1, '08b')}  (LEFT SHIFT)")
    print(f"a >> 1 = {a >> 1:2d} → Binary: {format(a >> 1, '08b')}  (RIGHT SHIFT)")
    print()

compare_all_operators()
```

### Truth Tables Combined

```
A  B │ AND  OR  XOR │ NOT A
─────┼──────────────┼───────
0  0 │  0   0   0   │   1
0  1 │  0   1   1   │   1
1  0 │  0   1   1   │   0
1  1 │  1   1   0   │   0
```

### Complete Example Program

```python
def bitwise_calculator():
    """Interactive bitwise operations calculator"""
    
    print("\n" + "="*50)
    print("BITWISE OPERATIONS CALCULATOR")
    print("="*50)
    
    a = int(input("Enter first number: "))
    b = int(input("Enter second number: "))
    
    print(f"\nInputs:")
    print(f"a = {a:4d} → {format(a, '08b')}")
    print(f"b = {b:4d} → {format(b, '08b')}")
    
    print(f"\nResults:")
    print(f"a & b  = {a & b:4d} → {format(a & b, '08b')}")
    print(f"a | b  = {a | b:4d} → {format(a | b, '08b')}")
    print(f"a ^ b  = {a ^ b:4d} → {format(a ^ b, '08b')}")
    print(f"~a     = {~a:4d} (Python two's complement)")
    print(f"a << 2 = {a << 2:4d} → {format(a << 2, '08b')}")
    print(f"a >> 2 = {a >> 2:4d} → {format(a >> 2, '08b')}")

# Uncomment to run:
# bitwise_calculator()
```

### Common Patterns Cheat Sheet

```python
# ============================================
# COMMON BITWISE PATTERNS
# ============================================

# Check if nth bit is set
def check_bit(num, n):
    return (num & (1 << n)) != 0

# Set nth bit to 1
def set_bit(num, n):
    return num | (1 << n)

# Clear nth bit to 0  
def clear_bit(num, n):
    return num & ~(1 << n)

# Toggle nth bit
def toggle_bit(num, n):
    return num ^ (1 << n)

# Check if even
def is_even(n):
    return (n & 1) == 0

# Check if power of 2
def is_power_of_2(n):
    return n > 0 and (n & (n - 1)) == 0

# Count set bits (Brian Kernighan's algorithm)
def count_set_bits(n):
    count = 0
    while n:
        n &= (n - 1)  # Clear rightmost set bit
        count += 1
    return count

# Get rightmost set bit
def rightmost_set_bit(n):
    return n & -n

# Turn off rightmost set bit
def turn_off_rightmost(n):
    return n & (n - 1)

print("\nCOMMON PATTERNS:")
print("-" * 50)
num = 12
print(f"Number: {num} → {format(num, '04b')}")
print(f"Bit 2 set? {check_bit(num, 2)}")
print(f"Set bit 0: {set_bit(num, 0)} → {format(set_bit(num, 0), '04b')}")
print(f"Clear bit 2: {clear_bit(num, 2)} → {format(clear_bit(num, 2), '04b')}")
print(f"Toggle bit 1: {toggle_bit(num, 1)} → {format(toggle_bit(num, 1), '04b')}")
print(f"Is even? {is_even(num)}")
print(f"Power of 2? {is_power_of_2(num)}")
print(f"Set bits: {count_set_bits(num)}")
```

---

## 🎯 Key Takeaways

1. **AND (&)**: Use for checking bits, masking, permissions
2. **OR (|)**: Use for setting bits, combining flags
3. **XOR (^)**: Use for toggling, finding unique elements, simple encryption
4. **NOT (~)**: Use for bit inversion, creating masks
5. **LEFT SHIFT (<<)**: Use for fast multiplication by 2^n, setting bit positions
6. **RIGHT SHIFT (>>)**: Use for fast division by 2^n, extracting bits

### Performance Benefits
- Bitwise operations are **extremely fast** (single CPU cycle)
- Much faster than arithmetic operations
- Useful in competitive programming and embedded systems

### Remember
- Always consider **bit width** when working with bitwise operations
- Be aware of **signed vs unsigned** behavior
- **Test with examples** to verify your logic
