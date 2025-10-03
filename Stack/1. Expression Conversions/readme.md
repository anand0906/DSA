# Expression Notation Conversions

A comprehensive guide to converting between Infix, Postfix, and Prefix notations.

---

## Table of Contents

1. [Infix to Postfix](#1-infix-to-postfix)
2. [Infix to Prefix](#2-infix-to-prefix)
3. [Postfix to Infix](#3-postfix-to-infix)
4. [Prefix to Infix](#4-prefix-to-infix)
5. [Postfix to Prefix](#5-postfix-to-prefix)
6. [Prefix to Postfix](#6-prefix-to-postfix)
7. [Quick Reference Table](#quick-reference-table)
8. [Key Takeaways](#key-takeaways)

---

## 1. Infix to Postfix

### Problem
Convert an infix expression ("operand1 operator operand2") to postfix notation ("operand1 operand2 operator").

### Intuition
Use a stack to hold operators temporarily. When we encounter an operand, add it directly to the result. For operators, pop from the stack based on precedence rules before pushing the current operator. This ensures operators are added to the result in the correct order.

**Key Concept:** Operators with higher precedence should be evaluated first, so they're popped earlier.

### Precedence Rules
- `^` (highest precedence) - right to left associativity
- `*`, `/`, `%` (medium precedence) - left to right associativity  
- `+`, `-` (lowest precedence) - left to right associativity

### Examples
- Input: `a*(b+c)/d` → Output: `abc+*d/`
- Input: `a+b*c+d` → Output: `abc*+d+`

### Code
```python
def solve(n, s):
    ans = ""  # Result string to build postfix expression
    stack = []  # Stack to hold operators temporarily
    
    # Define operator precedence
    order = {"(": 0, ")": 0, "+": 1, "-": 1, "*": 2, "/": 2, "%": 2, "^": 3}
    
    for char in s:
        if char.isalnum():  # If operand, add directly to result
            ans += char
            
        elif char == "(":  # Left parenthesis goes to stack
            stack.append(char)
            
        elif char == ")":  # Right parenthesis: pop until left parenthesis
            while stack and stack[-1] != "(":
                ans += stack.pop()
            stack.pop()  # Remove the left parenthesis
            
        elif char == "^":  # Right associative: pop only if strictly greater precedence
            while stack and (order[char] < order[stack[-1]]):
                ans += stack.pop()
            stack.append(char)
            
        else:  # Left associative operators: pop if equal or greater precedence
            while stack and (order[char] <= order[stack[-1]]):
                ans += stack.pop()
            stack.append(char)
    
    # Pop all remaining operators from stack
    while stack:
        ans += stack.pop()
    
    return ans
```

---

## 2. Infix to Prefix

### Problem
Convert an infix expression to prefix notation ("op operand1 operand2").

### Intuition
**Clever Trick:** Reverse the infix expression, swap parentheses, convert to postfix (with slight modifications), then reverse the result!

**Steps:**
1. Reverse the infix string
2. Replace `(` with `)` and vice versa
3. Apply modified postfix conversion (handle `^` differently)
4. Reverse the result

### Examples
- Input: `a*(b+c)/d` → Output: `/*a+bcd`
- Input: `(a-b/c)*(a/k-l)` → Output: `*-a/bc-/akl`

### Code
```python
def postfix(n, s):
    ans = ""
    stack = []
    order = {"(": 0, ")": 0, "+": 1, "-": 1, "*": 2, "/": 2, "%": 2, "^": 3}
    
    for char in s:
        if char.isalnum():
            ans += char
            
        elif char == "(":
            stack.append(char)
            
        elif char == ")":
            while stack and stack[-1] != "(":
                ans += stack.pop()
            stack.pop()
            
        elif char == "^":  # For prefix, handle ^ with <= (modified for reversal)
            while stack and (order[char] <= order[stack[-1]]):
                ans += stack.pop() 
            stack.append(char)
            
        else:  # Other operators use < (modified for reversal)
            while stack and (order[char] < order[stack[-1]]):
                ans += stack.pop()
            stack.append(char)
    
    while stack:
        ans += stack.pop()
    
    return ans

def solve(n, s):
    # Step 1: Reverse the string
    s = list(s[::-1])
    
    # Step 2: Swap parentheses
    for i in range(n):
        if s[i] == "(":
            s[i] = ")"
        elif s[i] == ")":
            s[i] = "("
    
    # Step 3: Apply modified postfix conversion
    ans = postfix(n, s)
    
    # Step 4: Reverse the result to get prefix
    ans = ans[::-1]
    
    return ans
```

---

## 3. Postfix to Infix

### Problem
Convert a postfix expression to infix notation with proper parentheses.

### Intuition
Use a stack to build sub-expressions. When we see an operand, push it. When we see an operator, pop two operands, combine them with parentheses, and push back. The final stack element is the complete infix expression.

**Pattern:** `operand2 operator operand1` becomes `(operand2 operator operand1)`

### Example
- Input: `ab*c+` → Output: `((a*b)+c)`

### Code
```python
def solve(n, s):
    stack = []
    
    for char in s:
        if char.isalnum():  # If operand, push to stack
            stack.append(char)
            
        else:  # If operator, pop two operands and combine
            op1 = stack.pop()  # Second operand (right)
            op2 = stack.pop()  # First operand (left)
            
            # Create infix expression with parentheses
            exp = f'({op2}{char}{op1})'
            stack.append(exp)
    
    # Final element is the complete infix expression
    return stack[-1]
```

---

## 4. Prefix to Infix

### Problem
Convert a prefix expression to infix notation with proper parentheses.

### Intuition
Similar to postfix to infix, but scan from **right to left**. When we see an operator, pop two operands and combine them. The key difference is the order remains the same since we're processing backwards.

**Pattern:** Reverse the string, process like postfix to infix.

### Example
- Input: `*-A/BC-/AKL` → Output: `((A-(B/C))*((A/K)-L))`

### Code
```python
def solve(n, s):
    # Reverse to process from right to left
    s = s[::-1]
    stack = []
    
    for char in s:
        if char.isalnum():  # If operand, push to stack
            stack.append(char)
            
        else:  # If operator, pop two operands and combine
            op1 = stack.pop()  # First operand
            op2 = stack.pop()  # Second operand
            
            # Create infix expression (order matters!)
            exp = f'({op1}{char}{op2})'
            stack.append(exp)
    
    # Final element is the complete infix expression
    return stack[-1]
```

---

## 5. Postfix to Prefix

### Problem
Convert a postfix expression to prefix notation.

### Intuition
Similar to postfix to infix, but instead of wrapping in parentheses, we place the operator **before** the operands.

**Pattern:** `operand2 operand1 operator` becomes `operator operand2 operand1`

### Examples
- Input: `ABC/-AK/L-*` → Output: `*-A/BC-/AKL`
- Input: `ab+` → Output: `+ab`

### Code
```python
def solve(n, s):
    stack = []
    
    for char in s:
        if char.isalnum():  # If operand, push to stack
            stack.append(char)
            
        else:  # If operator, pop two operands and combine
            op1 = stack.pop()  # Second operand (right)
            op2 = stack.pop()  # First operand (left)
            
            # Create prefix expression: operator first
            exp = f'{char}{op2}{op1}'
            stack.append(exp)
    
    # Final element is the complete prefix expression
    return stack[-1]
```

---

## 6. Prefix to Postfix

### Problem
Convert a prefix expression to postfix notation.

### Intuition
Reverse the prefix expression and scan. When we see an operator, pop two operands and place the operator **after** them, then push back.

**Pattern:** Reverse, process, and build postfix order.

### Example
- Input: `*-A/BC-/AKL` → Output: `ABC/-AK/L-*`

### Code
```python
def solve(n, s):
    # Reverse to process from right to left
    s = s[::-1]
    stack = []
    
    for char in s:
        if char.isalnum():  # If operand, push to stack
            stack.append(char)
            
        else:  # If operator, pop two operands and combine
            op1 = stack.pop()  # First operand
            op2 = stack.pop()  # Second operand
            
            # Create postfix expression: operands first, then operator
            exp = f'{op1}{op2}{char}'
            stack.append(exp)
    
    # Final element is the complete postfix expression
    return stack[-1]
```

---

## Quick Reference Table

| Conversion | Key Technique | Scan Direction | Combination Pattern |
|------------|---------------|----------------|---------------------|
| Infix → Postfix | Stack with precedence | Left to Right | Push operators by precedence |
| Infix → Prefix | Reverse + Modified Postfix | Right to Left | Reverse result |
| Postfix → Infix | Stack operands | Left to Right | `(op2 oper op1)` |
| Prefix → Infix | Stack operands | Right to Left | `(op1 oper op2)` |
| Postfix → Prefix | Stack operands | Left to Right | `oper op2 op1` |
| Prefix → Postfix | Stack operands | Right to Left | `op1 op2 oper` |

---

## Key Takeaways

1. **Stack is essential** for all conversions
2. **Infix conversions** need precedence handling
3. **To/From Prefix** usually involves reversing
4. **Operand order matters** when building expressions
5. **Parentheses** ensure correct order in infix notation
