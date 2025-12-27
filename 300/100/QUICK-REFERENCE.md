# Python 5 Layers - Quick Reference

## Layer 1: Syntax & Basics

```python
# Variables
name = "Willem"
age = 55

# Control Flow
if age > 50:
    print("Experienced")
elif age > 30:
    print("Mid-career")
else:
    print("Early career")

# Loops
for i in range(5):
    print(i)

while count < 10:
    count += 1

# Functions
def greet(name):
    return f"Hello, {name}!"
```

## Layer 2: Data Structures

```python
# Lists (ordered, mutable)
techs = ["Python", "Docker", "Azure"]
techs.append("Kubernetes")
techs[0] = "Python 3.12"

# Tuples (ordered, immutable)
coords = (51.4695, 5.4714)
lat, lon = coords

# Dictionaries (key-value)
employee = {
    "name": "Willem",
    "role": "Cloud Engineer",
    "rate": 105
}

# Sets (unique elements)
skills = {"Python", "Docker", "Azure"}
skills.add("Terraform")

# Comprehensions
squares = [x**2 for x in range(10)]
even_squares = [x**2 for x in range(10) if x % 2 == 0]
```

## Layer 3: Object-Oriented Programming

```python
# Basic Class
class Employee:
    def __init__(self, name, role):
        self.name = name
        self.role = role
    
    def introduce(self):
        return f"I'm {self.name}, {self.role}"

# Inheritance
class Engineer(Employee):
    def __init__(self, name, role, specialization):
        super().__init__(name, role)
        self.specialization = specialization

# Properties
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

# Magic Methods
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
```

## Layer 4: Functional Programming

```python
# Lambda
square = lambda x: x**2
add = lambda x, y: x + y

# Map, Filter, Reduce
from functools import reduce

doubled = list(map(lambda x: x * 2, [1, 2, 3]))
evens = list(filter(lambda x: x % 2 == 0, range(10)))
total = reduce(lambda acc, x: acc + x, [1, 2, 3, 4, 5])

# Decorators
def timer(func):
    import time
    from functools import wraps
    
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Time: {time.time() - start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

# Closures
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

times_three = create_multiplier(3)
print(times_three(5))  # 15

# Partial Functions
from functools import partial

def power(base, exponent):
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)
```

## Layer 5: Advanced Topics

```python
# Generators
def count_up_to(n):
    count = 1
    while count <= n:
        yield count
        count += 1

# Generator Expression
squares_gen = (x**2 for x in range(1000000))

# Context Managers
from contextlib import contextmanager

@contextmanager
def timer_context(name):
    import time
    start = time.time()
    try:
        yield
    finally:
        print(f"{name}: {time.time() - start:.4f}s")

with timer_context("Processing"):
    # Your code here
    pass

# Descriptors
class Validator:
    def __init__(self, min_value=None):
        self.min_value = min_value
    
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.name)
    
    def __set__(self, obj, value):
        if self.min_value and value < self.min_value:
            raise ValueError(f"Must be >= {self.min_value}")
        setattr(obj, self.name, value)

class Employee:
    rate = Validator(min_value=0)
    
    def __init__(self, rate):
        self.rate = rate

# Async/Await
import asyncio

async def fetch_data(url):
    await asyncio.sleep(1)  # Simulate network
    return f"Data from {url}"

async def main():
    tasks = [
        fetch_data("api.com/users"),
        fetch_data("api.com/posts")
    ]
    results = await asyncio.gather(*tasks)
    print(results)

# asyncio.run(main())
```

## Common Patterns

### Singleton (Layer 3)

```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Registry (Layer 3 + 5)

```python
class PluginRegistry(type):
    plugins = {}
    
    def __new__(mcs, name, bases, namespace):
        cls = super().__new__(mcs, name, bases, namespace)
        if name != 'Plugin':
            mcs.plugins[name] = cls
        return cls

class Plugin(metaclass=PluginRegistry):
    pass
```

### Pipeline (Layer 4)

```python
from functools import reduce

def compose(*functions):
    return reduce(lambda f, g: lambda x: f(g(x)), functions, lambda x: x)

# Use it
pipeline = compose(str.upper, str.strip, lambda x: x.replace(" ", "_"))
result = pipeline("  hello world  ")  # "HELLO_WORLD"
```

### Factory (Layer 3)

```python
class CloudProviderFactory:
    _providers = {}
    
    @classmethod
    def register(cls, name):
        def decorator(provider_class):
            cls._providers[name] = provider_class
            return provider_class
        return decorator
    
    @classmethod
    def create(cls, name, **kwargs):
        provider_class = cls._providers.get(name)
        if not provider_class:
            raise ValueError(f"Unknown provider: {name}")
        return provider_class(**kwargs)

@CloudProviderFactory.register('azure')
class AzureProvider:
    pass
```

## Anti-Patterns to Avoid

### ❌ Mutable Default Arguments

```python
# Bad
def append_to(element, lst=[]):
    lst.append(element)
    return lst

# Good
def append_to(element, lst=None):
    if lst is None:
        lst = []
    lst.append(element)
    return lst
```

### ❌ Bare Except

```python
# Bad
try:
    risky_operation()
except:
    pass

# Good
try:
    risky_operation()
except SpecificError as e:
    logging.error(f"Error: {e}")
```

### ❌ Using isinstance for Everything

```python
# Bad (rigid)
def process(data):
    if isinstance(data, list):
        # process list
    elif isinstance(data, dict):
        # process dict

# Good (duck typing)
def process(data):
    for item in data:  # Works with any iterable
        # process item
```

## Performance Tips

```python
# Use list comprehension over map/filter (usually faster)
squares = [x**2 for x in range(1000)]  # Faster
squares = list(map(lambda x: x**2, range(1000)))  # Slower

# Use set for membership testing
items = [1, 2, 3, 4, 5]
if 3 in items:  # O(n) for list
    pass

items_set = {1, 2, 3, 4, 5}
if 3 in items_set:  # O(1) for set
    pass

# Use generators for large data
sum_of_squares = sum(x**2 for x in range(1000000))  # Memory efficient
sum_of_squares = sum([x**2 for x in range(1000000)])  # Memory intensive

# Use defaultdict to avoid initialization
from collections import defaultdict
counts = defaultdict(int)
for item in data:
    counts[item] += 1  # No need to check if key exists
```

## Best Practices

1. **PEP 8**: Follow Python style guide
1. **Type Hints**: Use for better documentation
1. **Docstrings**: Document functions and classes
1. **EAFP**: “Easier to Ask for Forgiveness than Permission”
1. **Duck Typing**: “If it walks like a duck…”
1. **Context Managers**: Use for resource management
1. **List Comprehensions**: Prefer over map/filter
1. **Generators**: Use for large or infinite sequences

-----

*Quick reference for the 5 Layers of Learning Python*
