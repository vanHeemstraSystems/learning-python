# Layer 5: Advanced Topics

## Overview

Advanced Python features including generators, iterators, context managers, metaclasses, descriptors, and advanced patterns for professional development.

## Core Concepts

### 1. Generators

Functions that yield values one at a time, enabling lazy evaluation and memory efficiency.

```python
# Basic generator
def count_up_to(n):
    count = 1
    while count <= n:
        yield count
        count += 1

# Usage
for num in count_up_to(5):
    print(num)  # 1, 2, 3, 4, 5

# Generator expressions (like list comprehensions but lazy)
squares_gen = (x**2 for x in range(1000000))  # No memory for million items!
first_ten = [next(squares_gen) for _ in range(10)]

# Practical example: file processing
def read_large_file(file_path):
    """Memory-efficient file reading"""
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# Use it
# for line in read_large_file('large_log.txt'):
#     process(line)  # Process one line at a time

# Generator for infinite sequences
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Get first 10 Fibonacci numbers
fib = fibonacci()
first_ten_fib = [next(fib) for _ in range(10)]
print(first_ten_fib)  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# Generator pipeline
def read_logs(filename):
    with open(filename) as f:
        for line in f:
            yield line

def extract_errors(lines):
    for line in lines:
        if 'ERROR' in line:
            yield line

def parse_timestamp(lines):
    for line in lines:
        # Extract timestamp logic
        yield line

# Chain generators
# error_logs = parse_timestamp(extract_errors(read_logs('app.log')))
```

**When to use generators:**

- Processing large datasets
- Infinite sequences
- Pipeline processing
- Memory-constrained environments

### 2. Iterators

Objects that implement `__iter__()` and `__next__()` methods.

```python
# Custom iterator
class Countdown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        self.current -= 1
        return self.current + 1

# Usage
for num in Countdown(5):
    print(num)  # 5, 4, 3, 2, 1

# Iterator protocol with iter() and next()
my_list = [1, 2, 3]
iterator = iter(my_list)
print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3
# print(next(iterator))  # Raises StopIteration

# Practical example: batch iterator
class BatchIterator:
    def __init__(self, data, batch_size):
        self.data = data
        self.batch_size = batch_size
        self.index = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration
        
        batch = self.data[self.index:self.index + self.batch_size]
        self.index += self.batch_size
        return batch

# Usage
data = list(range(20))
for batch in BatchIterator(data, batch_size=5):
    print(batch)
```

### 3. Context Managers

Manage resources with automatic setup and cleanup.

```python
# Using built-in context manager
with open('file.txt', 'w') as f:
    f.write('Hello, World!')
# File automatically closed

# Custom context manager using class
class DatabaseConnection:
    def __init__(self, connection_string):
        self.connection_string = connection_string
        self.connection = None
    
    def __enter__(self):
        # Setup
        print(f"Connecting to {self.connection_string}")
        self.connection = f"Connected to {self.connection_string}"
        return self.connection
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        # Cleanup
        print("Closing connection")
        self.connection = None
        # Return False to propagate exceptions, True to suppress
        return False

# Usage
with DatabaseConnection("postgresql://localhost") as conn:
    print(f"Using {conn}")
    # Exception handling is automatic

# Context manager using decorator
from contextlib import contextmanager

@contextmanager
def timer_context(name):
    import time
    start = time.time()
    print(f"Starting {name}")
    try:
        yield  # Code in with block runs here
    finally:
        end = time.time()
        print(f"{name} took {end - start:.4f} seconds")

# Usage
with timer_context("Data Processing"):
    # Your code here
    time.sleep(1)

# Practical example: temporary directory
@contextmanager
def temporary_directory():
    import tempfile
    import shutil
    
    temp_dir = tempfile.mkdtemp()
    try:
        yield temp_dir
    finally:
        shutil.rmtree(temp_dir)

# Usage
with temporary_directory() as temp_dir:
    print(f"Working in {temp_dir}")
    # Create files in temp_dir
# temp_dir automatically deleted

# Multiple context managers
with open('input.txt') as infile, open('output.txt', 'w') as outfile:
    for line in infile:
        outfile.write(line.upper())
```

### 4. Descriptors

Control attribute access on objects.

```python
# Descriptor protocol: __get__, __set__, __delete__

class Validator:
    def __init__(self, min_value=None, max_value=None):
        self.min_value = min_value
        self.max_value = max_value
    
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.name, None)
    
    def __set__(self, obj, value):
        if self.min_value is not None and value < self.min_value:
            raise ValueError(f"{self.name} must be >= {self.min_value}")
        if self.max_value is not None and value > self.max_value:
            raise ValueError(f"{self.name} must be <= {self.max_value}")
        setattr(obj, self.name, value)

class Employee:
    rate = Validator(min_value=0, max_value=500)
    
    def __init__(self, name, rate):
        self.name = name
        self.rate = rate  # Uses descriptor's __set__

# Usage
emp = Employee("Willem", 105)
print(emp.rate)  # Uses descriptor's __get__
# emp.rate = -10  # Raises ValueError
# emp.rate = 600  # Raises ValueError

# Practical example: type checking descriptor
class TypedProperty:
    def __init__(self, expected_type):
        self.expected_type = expected_type
    
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.name, None)
    
    def __set__(self, obj, value):
        if not isinstance(value, self.expected_type):
            raise TypeError(f"{self.name} must be {self.expected_type}")
        setattr(obj, self.name, value)

class Project:
    name = TypedProperty(str)
    start_date = TypedProperty(str)
    team_size = TypedProperty(int)
    
    def __init__(self, name, start_date, team_size):
        self.name = name
        self.start_date = start_date
        self.team_size = team_size

project = Project("Atlas IDP", "2026-01-05", 5)
# project.team_size = "five"  # Raises TypeError
```

### 5. Metaclasses

Classes that create classes (advanced!).

```python
# Basic metaclass
class Meta(type):
    def __new__(mcs, name, bases, namespace):
        print(f"Creating class {name}")
        # Modify class before creation
        namespace['created_by'] = 'Meta'
        return super().__new__(mcs, name, bases, namespace)

class MyClass(metaclass=Meta):
    pass

print(MyClass.created_by)  # 'Meta'

# Practical example: singleton pattern
class Singleton(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=Singleton):
    def __init__(self):
        print("Initializing database connection")

# Always returns same instance
db1 = Database()
db2 = Database()
print(db1 is db2)  # True

# Practical example: auto-registration
class PluginRegistry(type):
    plugins = {}
    
    def __new__(mcs, name, bases, namespace):
        cls = super().__new__(mcs, name, bases, namespace)
        if name != 'Plugin':  # Don't register base class
            mcs.plugins[name] = cls
        return cls

class Plugin(metaclass=PluginRegistry):
    pass

class CloudPlugin(Plugin):
    pass

class SecurityPlugin(Plugin):
    pass

print(PluginRegistry.plugins)  # {'CloudPlugin': ..., 'SecurityPlugin': ...}
```

### 6. Advanced Decorators

```python
# Class decorators
def singleton(cls):
    """Class decorator to implement singleton"""
    instances = {}
    
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    
    return get_instance

@singleton
class Configuration:
    def __init__(self):
        self.settings = {}

config1 = Configuration()
config2 = Configuration()
print(config1 is config2)  # True

# Decorator factory with state
def count_calls(func):
    """Decorator that counts function calls"""
    count = 0
    
    @wraps(func)
    def wrapper(*args, **kwargs):
        nonlocal count
        count += 1
        print(f"{func.__name__} has been called {count} times")
        return func(*args, **kwargs)
    
    wrapper.call_count = lambda: count
    wrapper.reset_count = lambda: nonlocal count; count = 0  # Not valid, use class
    return wrapper

# Better implementation with class
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"{self.func.__name__} called {self.count} times")
        return self.func(*args, **kwargs)
    
    def reset(self):
        self.count = 0

@CountCalls
def greet(name):
    return f"Hello, {name}"

greet("Willem")
greet("Alice")
print(f"Total calls: {greet.count}")
```

### 7. Advanced Data Structures

```python
from collections import deque, defaultdict, OrderedDict, ChainMap
from collections.abc import MutableMapping

# Deque - efficient queue/stack
queue = deque([1, 2, 3])
queue.append(4)        # Add to right
queue.appendleft(0)    # Add to left
queue.pop()            # Remove from right
queue.popleft()        # Remove from left

# Practical: sliding window
def moving_average(numbers, window_size):
    window = deque(maxlen=window_size)
    for num in numbers:
        window.append(num)
        if len(window) == window_size:
            yield sum(window) / window_size

# defaultdict - never raises KeyError
from collections import defaultdict

word_freq = defaultdict(int)
for word in "the quick brown fox jumps over the lazy dog".split():
    word_freq[word] += 1

# ChainMap - combine multiple dicts
defaults = {'color': 'blue', 'user': 'guest'}
environment = {'user': 'admin'}
config = ChainMap(environment, defaults)
print(config['user'])   # 'admin' (from environment)
print(config['color'])  # 'blue' (from defaults)

# Custom mapping
class CaseInsensitiveDict(MutableMapping):
    def __init__(self):
        self._data = {}
    
    def __getitem__(self, key):
        return self._data[key.lower()]
    
    def __setitem__(self, key, value):
        self._data[key.lower()] = value
    
    def __delitem__(self, key):
        del self._data[key.lower()]
    
    def __iter__(self):
        return iter(self._data)
    
    def __len__(self):
        return len(self._data)

ci_dict = CaseInsensitiveDict()
ci_dict['Name'] = 'Willem'
print(ci_dict['name'])   # 'Willem'
print(ci_dict['NAME'])   # 'Willem'
```

### 8. Async/Await (Asyncio)

```python
import asyncio

# Basic async function
async def greet(name, delay):
    await asyncio.sleep(delay)
    return f"Hello, {name}!"

# Run async function
async def main():
    result = await greet("Willem", 1)
    print(result)

# asyncio.run(main())

# Concurrent execution
async def fetch_data(url, delay):
    print(f"Fetching {url}")
    await asyncio.sleep(delay)  # Simulate network request
    return f"Data from {url}"

async def main():
    # Run concurrently
    tasks = [
        fetch_data("api.example.com/users", 2),
        fetch_data("api.example.com/posts", 1),
        fetch_data("api.example.com/comments", 3)
    ]
    results = await asyncio.gather(*tasks)
    for result in results:
        print(result)

# asyncio.run(main())

# Async context manager
class AsyncDatabase:
    async def __aenter__(self):
        print("Connecting to database")
        await asyncio.sleep(1)
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print("Closing database connection")
        await asyncio.sleep(0.5)

async def main():
    async with AsyncDatabase() as db:
        print("Using database")

# asyncio.run(main())
```

## Practice Exercises

### Exercise 1: Custom Iterator for Binary Tree

```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

class BinaryTreeIterator:
    def __init__(self, root):
        self.stack = []
        self._push_left(root)
    
    def _push_left(self, node):
        while node:
            self.stack.append(node)
            node = node.left
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if not self.stack:
            raise StopIteration
        
        node = self.stack.pop()
        result = node.value
        
        if node.right:
            self._push_left(node.right)
        
        return result

# Build tree: 1 -> 2, 3
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)

# In-order traversal
for value in BinaryTreeIterator(root):
    print(value)  # 2, 1, 3
```

### Exercise 2: Resource Pool Context Manager

```python
@contextmanager
def resource_pool(resources):
    """Manages a pool of limited resources"""
    available = list(resources)
    allocated = []
    
    try:
        def acquire():
            if not available:
                raise RuntimeError("No resources available")
            resource = available.pop()
            allocated.append(resource)
            return resource
        
        def release(resource):
            allocated.remove(resource)
            available.append(resource)
        
        pool = {'acquire': acquire, 'release': release}
        yield pool
    finally:
        # Cleanup: return all allocated resources
        available.extend(allocated)
        allocated.clear()
        print(f"Pool cleanup: {len(available)} resources available")

# Usage
with resource_pool(['conn1', 'conn2', 'conn3']) as pool:
    conn1 = pool['acquire']()
    print(f"Using {conn1}")
    conn2 = pool['acquire']()
    print(f"Using {conn2}")
    pool['release'](conn1)
```

### Exercise 3: Caching Decorator with Expiration

```python
import time
from functools import wraps

def cache_with_expiration(expiration_seconds=60):
    def decorator(func):
        cache = {}
        
        @wraps(func)
        def wrapper(*args):
            now = time.time()
            
            # Check if cached and not expired
            if args in cache:
                result, timestamp = cache[args]
                if now - timestamp < expiration_seconds:
                    print(f"Cache hit for {args}")
                    return result
            
            # Compute and cache
            result = func(*args)
            cache[args] = (result, now)
            return result
        
        return wrapper
    return decorator

@cache_with_expiration(expiration_seconds=5)
def expensive_api_call(endpoint):
    print(f"Making API call to {endpoint}")
    time.sleep(1)  # Simulate network delay
    return f"Response from {endpoint}"

# First call - hits API
print(expensive_api_call("/users"))

# Second call within 5 seconds - from cache
print(expensive_api_call("/users"))

# Wait 6 seconds
time.sleep(6)

# Third call - cache expired, hits API again
print(expensive_api_call("/users"))
```

### Exercise 4: Pipeline Generator

```python
def pipeline(*functions):
    """Create a data processing pipeline using generators"""
    def process(data):
        result = data
        for func in functions:
            if hasattr(result, '__iter__') and not isinstance(result, str):
                result = func(result)
            else:
                result = func([result])
        return result
    return process

# Pipeline functions
def filter_positive(numbers):
    for n in numbers:
        if n > 0:
            yield n

def square(numbers):
    for n in numbers:
        yield n ** 2

def running_sum(numbers):
    total = 0
    for n in numbers:
        total += n
        yield total

# Create pipeline
process = pipeline(filter_positive, square, running_sum)

# Use pipeline
data = [-1, 2, -3, 4, 5]
result = list(process(data))
print(result)  # [4, 20, 45]
```

## Key Takeaways

1. **Generators** - Memory-efficient, lazy evaluation
1. **Iterators** - Custom iteration protocols
1. **Context managers** - Automatic resource management
1. **Descriptors** - Fine-grained attribute control
1. **Metaclasses** - Class creation control (use sparingly!)
1. **Async/await** - Concurrent I/O operations
1. **Advanced patterns** - Singleton, registry, pipeline

## Best Practices

- Use generators for large datasets or infinite sequences
- Prefer context managers for resource management
- Use descriptors for reusable validation logic
- Avoid metaclasses unless absolutely necessary
- Async for I/O-bound operations, not CPU-bound
- Document complex advanced features thoroughly

## Common Pitfalls

- Forgetting to call `next()` to start generators
- Not handling StopIteration in custom iterators
- Context manager `__exit__` returning True (suppresses exceptions)
- Overusing metaclasses (usually decorators or mixins are better)
- Mixing sync and async code incorrectly
- Creating memory leaks with descriptors

## Professional Development Patterns

```python
# 1. Registry pattern
class ServiceRegistry:
    _services = {}
    
    @classmethod
    def register(cls, name):
        def decorator(service_class):
            cls._services[name] = service_class
            return service_class
        return decorator
    
    @classmethod
    def get(cls, name):
        return cls._services.get(name)

@ServiceRegistry.register('cloud')
class CloudService:
    pass

# 2. Lazy property
class lazy_property:
    def __init__(self, func):
        self.func = func
        self.name = func.__name__
    
    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        value = self.func(obj)
        setattr(obj, self.name, value)
        return value

class DataLoader:
    @lazy_property
    def data(self):
        print("Loading data...")
        return "Heavy data"

# 3. Builder pattern with context
class QueryBuilder:
    def __init__(self):
        self._query = []
    
    def select(self, *fields):
        self._query.append(f"SELECT {', '.join(fields)}")
        return self
    
    def from_table(self, table):
        self._query.append(f"FROM {table}")
        return self
    
    def where(self, condition):
        self._query.append(f"WHERE {condition}")
        return self
    
    def build(self):
        return ' '.join(self._query)

query = (QueryBuilder()
         .select('name', 'age')
         .from_table('users')
         .where('age > 25')
         .build())
```

## Resources

- [Python Generators](https://realpython.com/introduction-to-python-generators/)
- [Context Managers and with Statement](https://realpython.com/python-with-statement/)
- [Descriptors Guide](https://docs.python.org/3/howto/descriptor.html)
- [Asyncio Documentation](https://docs.python.org/3/library/asyncio.html)
- [Python Metaclasses](https://realpython.com/python-metaclasses/)

## Conclusion

You’ve now covered all 5 layers of Python mastery:

1. ✅ Syntax & Basics
1. ✅ Data Structures
1. ✅ Object-Oriented Programming
1. ✅ Functional Programming
1. ✅ Advanced Topics

Continue practicing, building projects, and exploring specialized domains like web development, data science, or DevOps automation!
