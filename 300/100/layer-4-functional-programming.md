# Layer 4: Functional Programming

## Overview

Functional Programming treats functions as first-class citizens, emphasizing pure functions, immutability, and declarative code. Python supports functional programming alongside OOP.

## Core Concepts

### 1. Functions as First-Class Citizens

Functions can be assigned to variables, passed as arguments, and returned from other functions.

```python
# Assign function to variable
def greet(name):
    return f"Hello, {name}!"

say_hello = greet
print(say_hello("Willem"))  # "Hello, Willem!"

# Pass function as argument
def apply_operation(func, value):
    return func(value)

def double(x):
    return x * 2

result = apply_operation(double, 5)  # 10

# Return function from function
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

times_three = create_multiplier(3)
print(times_three(4))  # 12
```

### 2. Lambda Functions

Anonymous, inline functions for simple operations.

```python
# Basic lambda
square = lambda x: x**2
print(square(5))  # 25

# Lambda with multiple arguments
add = lambda x, y: x + y
print(add(3, 7))  # 10

# Lambda in sorted()
employees = [
    {"name": "Willem", "rate": 105},
    {"name": "Alice", "rate": 95},
    {"name": "Bob", "rate": 110}
]
sorted_by_rate = sorted(employees, key=lambda emp: emp["rate"])

# Lambda in filter()
numbers = range(1, 11)
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4, 6, 8, 10]

# Lambda in map()
doubled = list(map(lambda x: x * 2, numbers))  # [2, 4, 6, ..., 20]
```

**When to use lambdas:**

- Simple, one-line operations
- As arguments to higher-order functions
- Temporary, throwaway functions

**When NOT to use lambdas:**

- Complex logic (use regular functions)
- Need for documentation/comments
- Multiple statements

### 3. Higher-Order Functions

#### Map

Apply a function to every item in an iterable.

```python
# Convert temperatures
celsius_temps = [0, 10, 20, 30, 40]
fahrenheit_temps = list(map(lambda c: c * 9/5 + 32, celsius_temps))
# [32.0, 50.0, 68.0, 86.0, 104.0]

# Better with comprehension for readability
fahrenheit_temps = [c * 9/5 + 32 for c in celsius_temps]

# Map with multiple iterables
def add(x, y):
    return x + y

list1 = [1, 2, 3]
list2 = [10, 20, 30]
sums = list(map(add, list1, list2))  # [11, 22, 33]
```

#### Filter

Select items from an iterable based on a condition.

```python
# Filter out low values
numbers = [1, 5, 12, 8, 19, 3, 25]
high_numbers = list(filter(lambda x: x > 10, numbers))  # [12, 19, 25]

# Filter with comprehension (more Pythonic)
high_numbers = [x for x in numbers if x > 10]

# Filter employees by criteria
employees = [
    {"name": "Willem", "years": 30, "role": "Cloud Engineer"},
    {"name": "Alice", "years": 5, "role": "Developer"},
    {"name": "Bob", "years": 15, "role": "Security Engineer"}
]

experienced = list(filter(lambda e: e["years"] > 10, employees))
```

#### Reduce

Reduce an iterable to a single value by repeatedly applying a function.

```python
from functools import reduce

# Sum of numbers
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda acc, x: acc + x, numbers)  # 15
# Equivalent to: ((((1+2)+3)+4)+5)

# Find maximum
maximum = reduce(lambda a, b: a if a > b else b, numbers)  # 5

# Concatenate strings
words = ["Cloud", "Security", "Engineer"]
sentence = reduce(lambda a, b: f"{a} {b}", words)  # "Cloud Security Engineer"

# Better alternative for sum:
total = sum(numbers)  # More readable!
```

### 4. Decorators

Functions that modify or enhance other functions.

```python
# Basic decorator
def uppercase_decorator(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper()
    return wrapper

@uppercase_decorator
def greet(name):
    return f"hello, {name}"

print(greet("Willem"))  # "HELLO, WILLEM"

# Decorator with arguments
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello!")
    
say_hello()  # Prints "Hello!" three times

# Practical decorator: timing
import time
from functools import wraps

def timer(func):
    @wraps(func)  # Preserves original function metadata
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done"

slow_function()

# Practical decorator: logging
def log_calls(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_calls
def add(a, b):
    return a + b

add(3, 5)

# Practical decorator: caching (memoization)
def memoize(func):
    cache = {}
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(100))  # Fast due to caching!
```

**Built-in Decorators:**

```python
class MyClass:
    # Property decorator
    @property
    def value(self):
        return self._value
    
    # Class method decorator
    @classmethod
    def create(cls, param):
        return cls(param)
    
    # Static method decorator
    @staticmethod
    def helper_function(x):
        return x * 2
```

### 5. Closures

Functions that remember values from their enclosing scope.

```python
def create_counter():
    count = 0  # Enclosed variable
    
    def increment():
        nonlocal count  # Modify enclosed variable
        count += 1
        return count
    
    return increment

counter = create_counter()
print(counter())  # 1
print(counter())  # 2
print(counter())  # 3

# Practical example: rate limiter
def create_rate_limiter(max_calls, time_window):
    calls = []
    
    def rate_limited_function(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            nonlocal calls
            current_time = time.time()
            # Remove old calls outside time window
            calls = [t for t in calls if current_time - t < time_window]
            
            if len(calls) < max_calls:
                calls.append(current_time)
                return func(*args, **kwargs)
            else:
                return "Rate limit exceeded"
        return wrapper
    return rate_limited_function

@create_rate_limiter(max_calls=3, time_window=10)
def api_call():
    return "API response"
```

### 6. Partial Functions

Pre-fill some arguments of a function.

```python
from functools import partial

# Basic partial
def power(base, exponent):
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(5))  # 25
print(cube(5))    # 125

# Practical example: API clients
def make_api_request(endpoint, method, base_url, **params):
    return f"{method} {base_url}{endpoint} {params}"

# Create specialized functions
api_get = partial(make_api_request, method="GET", base_url="https://api.example.com")
api_post = partial(make_api_request, method="POST", base_url="https://api.example.com")

response = api_get("/users", id=123)
```

### 7. Comprehensions (Functional Style)

```python
# List comprehension
squares = [x**2 for x in range(10)]

# With condition
even_squares = [x**2 for x in range(10) if x % 2 == 0]

# Nested comprehension
matrix = [[i*j for j in range(3)] for i in range(3)]

# Dictionary comprehension
word_lengths = {word: len(word) for word in ["Python", "Docker", "Azure"]}

# Set comprehension
unique_lengths = {len(word) for word in ["Python", "Java", "Go", "Rust"]}

# Generator expression (lazy evaluation)
sum_of_squares = sum(x**2 for x in range(1000000))  # Memory efficient!
```

### 8. Pure Functions

Functions with no side effects that always produce the same output for the same input.

```python
# Pure function
def add(a, b):
    return a + b

# Not pure (has side effect)
total = 0
def add_to_total(value):
    global total
    total += value  # Modifies external state

# Not pure (depends on external state)
counter = 0
def increment():
    global counter
    counter += 1
    return counter

# Pure function example: data transformation
def transform_employee_data(employees):
    """Returns new list, doesn't modify original"""
    return [
        {
            **emp,
            "annual_salary": emp["rate"] * 160 * 12
        }
        for emp in employees
    ]

employees = [{"name": "Willem", "rate": 105}]
transformed = transform_employee_data(employees)
# Original employees unchanged
```

## Practice Exercises

### Exercise 1: Custom Decorators

```python
def validate_positive(func):
    """Decorator to ensure all numeric arguments are positive"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        for arg in args:
            if isinstance(arg, (int, float)) and arg < 0:
                raise ValueError("All arguments must be positive")
        return func(*args, **kwargs)
    return wrapper

@validate_positive
def calculate_area(length, width):
    return length * width

try:
    print(calculate_area(5, 10))   # 50
    print(calculate_area(-5, 10))  # Raises ValueError
except ValueError as e:
    print(f"Error: {e}")
```

### Exercise 2: Function Composition

```python
from functools import reduce

def compose(*functions):
    """Compose multiple functions: compose(f, g, h)(x) = f(g(h(x)))"""
    return reduce(lambda f, g: lambda x: f(g(x)), functions, lambda x: x)

# Example functions
def add_ten(x):
    return x + 10

def multiply_by_two(x):
    return x * 2

def square(x):
    return x ** 2

# Compose them
pipeline = compose(square, multiply_by_two, add_ten)
result = pipeline(5)  # ((5 + 10) * 2) ** 2 = 900
print(result)

# Practical example: data processing pipeline
def remove_whitespace(text):
    return text.strip()

def to_lowercase(text):
    return text.lower()

def remove_punctuation(text):
    return ''.join(c for c in text if c.isalnum() or c.isspace())

clean_text = compose(remove_punctuation, to_lowercase, remove_whitespace)
result = clean_text("  Hello, World!  ")
print(result)  # "hello world"
```

### Exercise 3: Functional Data Processing

```python
# Dataset
employees = [
    {"name": "Willem", "dept": "Cloud", "salary": 105000, "years": 30},
    {"name": "Alice", "dept": "Security", "salary": 95000, "years": 5},
    {"name": "Bob", "dept": "Cloud", "salary": 110000, "years": 15},
    {"name": "Charlie", "dept": "DevOps", "salary": 90000, "years": 8},
]

# Functional operations
from functools import reduce

# Filter: experienced employees (>10 years)
experienced = list(filter(lambda e: e["years"] > 10, employees))

# Map: get just names and salaries
names_salaries = list(map(lambda e: (e["name"], e["salary"]), experienced))

# Reduce: total salary of experienced employees
total_salary = reduce(lambda acc, e: acc + e["salary"], experienced, 0)

# Functional pipeline (more Pythonic)
def process_employees(employees):
    return [
        {"name": e["name"], "annual_salary": e["salary"]}
        for e in employees
        if e["years"] > 10
    ]

result = process_employees(employees)
print(f"Experienced employees: {result}")
print(f"Total salary: €{total_salary:,}")
```

### Exercise 4: Memoization Decorator

```python
def memoize_with_limit(max_size=100):
    """Memoization decorator with cache size limit"""
    def decorator(func):
        cache = {}
        cache_order = []
        
        @wraps(func)
        def wrapper(*args):
            if args in cache:
                return cache[args]
            
            result = func(*args)
            cache[args] = result
            cache_order.append(args)
            
            # Remove oldest if over limit
            if len(cache) > max_size:
                oldest = cache_order.pop(0)
                del cache[oldest]
            
            return result
        
        wrapper.cache_info = lambda: {
            "size": len(cache),
            "max_size": max_size
        }
        return wrapper
    return decorator

@memoize_with_limit(max_size=50)
def expensive_calculation(n):
    """Simulates expensive computation"""
    time.sleep(0.1)
    return n ** 2

# First call is slow
start = time.time()
result1 = expensive_calculation(10)
print(f"First call: {time.time() - start:.2f}s")

# Second call is instant
start = time.time()
result2 = expensive_calculation(10)
print(f"Second call: {time.time() - start:.4f}s")
print(f"Cache info: {expensive_calculation.cache_info()}")
```

## Key Takeaways

1. **First-class functions** - Functions are objects
1. **Lambda** - Use for simple, inline functions
1. **Map/Filter/Reduce** - Transform collections functionally
1. **Decorators** - Modify function behavior without changing code
1. **Closures** - Functions that remember their environment
1. **Pure functions** - No side effects, predictable output
1. **Comprehensions** - Functional, readable data transformations

## Functional vs Imperative

```python
# Imperative (how to do it)
numbers = [1, 2, 3, 4, 5]
squares = []
for n in numbers:
    if n % 2 == 0:
        squares.append(n ** 2)

# Functional (what to do)
squares = [n**2 for n in numbers if n % 2 == 0]

# Even more functional
from functools import reduce
squares = list(map(lambda x: x**2, filter(lambda x: x % 2 == 0, numbers)))
```

## Common Pitfalls

- Overusing lambdas for complex logic
- Not using `@wraps` in decorators
- Mutating data in “pure” functions
- Excessive function composition (readability)
- Forgetting that comprehensions are often more Pythonic than map/filter

## Next Steps

Progress to **Layer 5: Advanced Topics** to explore generators, context managers, metaclasses, and more advanced Python features.

## Resources

- [Python Functional Programming HOWTO](https://docs.python.org/3/howto/functional.html)
- [Real Python - Decorators](https://realpython.com/primer-on-python-decorators/)
- [Functional Programming in Python (Book)](https://www.oreilly.com/library/view/functional-programming-in/9781492048633/)
