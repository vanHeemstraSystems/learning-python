# Layer 1: Syntax & Basics

## Overview

The foundation of Python programming - essential syntax, variables, basic operations, and control flow.

## Core Concepts

### 1. Variables and Data Types

```python
# Variable assignment
name = "Willem"
age = 55
height = 1.85
is_engineer = True

# Type checking
print(type(name))    # <class 'str'>
print(type(age))     # <class 'int'>
print(type(height))  # <class 'float'>
```

### 2. Basic Operations

```python
# Arithmetic
x = 10 + 5    # Addition
y = 10 - 5    # Subtraction
z = 10 * 5    # Multiplication
w = 10 / 5    # Division (float)
v = 10 // 3   # Floor division
u = 10 % 3    # Modulo
t = 2 ** 3    # Exponentiation

# String operations
greeting = "Hello" + " " + "World"
repeated = "Ha" * 3  # "HaHaHa"
```

### 3. Control Flow

#### If-Else Statements

```python
temperature = 25

if temperature > 30:
    print("It's hot!")
elif temperature > 20:
    print("It's warm")
else:
    print("It's cold")
```

#### For Loops

```python
# Iterate over range
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# Iterate over list
cities = ["Veldhoven", "Eersel", "Eindhoven"]
for city in cities:
    print(city)

# Enumerate for index and value
for index, city in enumerate(cities):
    print(f"{index}: {city}")
```

#### While Loops

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### 4. Basic Input/Output

```python
# Output
print("Hello, World!")
print(f"My name is {name} and I'm {age} years old")

# Input
user_input = input("Enter your name: ")
print(f"Hello, {user_input}!")
```

### 5. Comments

```python
# This is a single-line comment

"""
This is a multi-line comment
or docstring
"""

'''
Alternative multi-line
comment style
'''
```

## Practice Exercises

### Exercise 1: FizzBuzz

```python
# Print numbers 1-100
# For multiples of 3, print "Fizz"
# For multiples of 5, print "Buzz"
# For multiples of both, print "FizzBuzz"

for num in range(1, 101):
    if num % 3 == 0 and num % 5 == 0:
        print("FizzBuzz")
    elif num % 3 == 0:
        print("Fizz")
    elif num % 5 == 0:
        print("Buzz")
    else:
        print(num)
```

### Exercise 2: Temperature Converter

```python
def celsius_to_fahrenheit(celsius):
    return (celsius * 9/5) + 32

def fahrenheit_to_celsius(fahrenheit):
    return (fahrenheit - 32) * 5/9

# Test
temp_c = 25
temp_f = celsius_to_fahrenheit(temp_c)
print(f"{temp_c}°C = {temp_f}°F")
```

### Exercise 3: Simple Calculator

```python
def calculator():
    num1 = float(input("Enter first number: "))
    operator = input("Enter operator (+, -, *, /): ")
    num2 = float(input("Enter second number: "))
    
    if operator == "+":
        result = num1 + num2
    elif operator == "-":
        result = num1 - num2
    elif operator == "*":
        result = num1 * num2
    elif operator == "/":
        result = num1 / num2 if num2 != 0 else "Error: Division by zero"
    else:
        result = "Invalid operator"
    
    print(f"Result: {result}")

# calculator()  # Uncomment to run
```

## Key Takeaways

1. **Indentation matters** - Python uses indentation to define code blocks
1. **Dynamic typing** - Variables don’t need type declarations
1. **Zero-indexed** - Lists and ranges start at 0
1. **Print function** - Use `print()` for output
1. **F-strings** - Modern way to format strings: `f"text {variable}"`

## Common Pitfalls

- Forgetting colons (`:`) after if/for/while statements
- Inconsistent indentation (mix of tabs and spaces)
- Using `=` (assignment) instead of `==` (comparison)
- Integer division vs float division (`//` vs `/`)

## Next Steps

Once comfortable with these basics, move to **Layer 2: Data Structures** to learn about lists, dictionaries, sets, and tuples.

## Resources

- [Python Official Tutorial - Control Flow](https://docs.python.org/3/tutorial/controlflow.html)
- [Real Python - Basic Data Types](https://realpython.com/python-data-types/)
- [W3Schools Python Syntax](https://www.w3schools.com/python/python_syntax.asp)
