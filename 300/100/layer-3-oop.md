# Layer 3: Object-Oriented Programming (OOP)

## Overview

Object-Oriented Programming organizes code around objects and classes, enabling code reuse, encapsulation, and better organization of complex systems.

## Core Concepts

### 1. Classes and Objects

#### Basic Class Definition

```python
class Employee:
    """Represents an employee"""
    
    # Class variable (shared by all instances)
    company = "Team Rockstars IT"
    
    # Constructor/Initializer
    def __init__(self, name, role, rate):
        # Instance variables (unique to each instance)
        self.name = name
        self.role = role
        self.rate = rate
        self.projects = []
    
    # Instance method
    def introduce(self):
        return f"Hi, I'm {self.name}, a {self.role} at {self.company}"
    
    # Instance method
    def add_project(self, project_name):
        self.projects.append(project_name)
        return f"Added {project_name} to {self.name}'s projects"
    
    # Instance method
    def calculate_monthly_income(self, hours=160):
        return self.rate * hours

# Creating objects (instances)
willem = Employee("Willem van Heemstra", "Cloud Engineer", 105)
alice = Employee("Alice Johnson", "DevOps Engineer", 95)

# Using methods
print(willem.introduce())
print(willem.add_project("Atlas IDP"))
print(f"Monthly income: €{willem.calculate_monthly_income()}")
```

#### The `self` Parameter

```python
# 'self' represents the instance
# It's always the first parameter in instance methods
# Python automatically passes it when you call the method

class Counter:
    def __init__(self):
        self.count = 0  # self.count is an instance variable
    
    def increment(self):
        self.count += 1  # Access instance variable through self
        return self.count

counter1 = Counter()
counter2 = Counter()

counter1.increment()
counter1.increment()
print(counter1.count)  # 2
print(counter2.count)  # 0 (separate instance)
```

### 2. Encapsulation

Hiding internal implementation details and providing controlled access.

```python
class BankAccount:
    def __init__(self, account_holder, initial_balance=0):
        self.account_holder = account_holder
        self.__balance = initial_balance  # Private attribute (name mangling)
        self._transactions = []           # Protected attribute (convention)
    
    # Public method to access private data
    def get_balance(self):
        return self.__balance
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            self._transactions.append(f"Deposit: €{amount}")
            return f"Deposited €{amount}. New balance: €{self.__balance}"
        return "Invalid amount"
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            self._transactions.append(f"Withdrawal: €{amount}")
            return f"Withdrew €{amount}. New balance: €{self.__balance}"
        return "Insufficient funds or invalid amount"
    
    def get_transaction_history(self):
        return self._transactions.copy()  # Return copy to prevent modification

# Usage
account = BankAccount("Willem", 1000)
print(account.deposit(500))
print(account.withdraw(200))
print(account.get_balance())
# print(account.__balance)  # AttributeError - can't access directly
```

**Naming Conventions:**

- `public_attr` - Public, accessible from anywhere
- `_protected_attr` - Protected, should only be used within class and subclasses
- `__private_attr` - Private, name mangled to `_ClassName__private_attr`

### 3. Inheritance

Creating new classes based on existing classes.

```python
class Person:
    """Base class"""
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def introduce(self):
        return f"I'm {self.name}, {self.age} years old"

class Engineer(Person):
    """Derived class inheriting from Person"""
    def __init__(self, name, age, specialization, years_experience):
        # Call parent class constructor
        super().__init__(name, age)
        self.specialization = specialization
        self.years_experience = years_experience
    
    # Override parent method
    def introduce(self):
        # Can call parent method if needed
        base_intro = super().introduce()
        return f"{base_intro}, specializing in {self.specialization}"
    
    # New method specific to Engineer
    def describe_expertise(self):
        return f"{self.years_experience} years of experience in {self.specialization}"

# Usage
willem = Engineer("Willem", 55, "Cloud Security", 30)
print(willem.introduce())
print(willem.describe_expertise())
```

#### Multiple Inheritance

```python
class Swimmer:
    def swim(self):
        return "Swimming!"

class Flyer:
    def fly(self):
        return "Flying!"

class Duck(Swimmer, Flyer):
    def quack(self):
        return "Quack!"

duck = Duck()
print(duck.swim())   # Inherited from Swimmer
print(duck.fly())    # Inherited from Flyer
print(duck.quack())  # Duck's own method
```

### 4. Polymorphism

Objects of different classes can be used interchangeably through a common interface.

```python
class CloudProvider:
    def deploy(self, app_name):
        raise NotImplementedError("Subclass must implement deploy()")

class Azure(CloudProvider):
    def deploy(self, app_name):
        return f"Deploying {app_name} to Azure"

class AWS(CloudProvider):
    def deploy(self, app_name):
        return f"Deploying {app_name} to AWS"

class GCP(CloudProvider):
    def deploy(self, app_name):
        return f"Deploying {app_name} to Google Cloud"

# Polymorphism in action
def deploy_application(provider: CloudProvider, app_name: str):
    """Works with any CloudProvider subclass"""
    return provider.deploy(app_name)

# Usage
providers = [Azure(), AWS(), GCP()]
for provider in providers:
    print(deploy_application(provider, "AtlasIDP"))
```

### 5. Special Methods (Magic Methods)

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation
    def __str__(self):
        """Called by str() and print()"""
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        """Official string representation"""
        return f"Vector(x={self.x}, y={self.y})"
    
    # Operator overloading
    def __add__(self, other):
        """Enables v1 + v2"""
        return Vector(self.x + other.x, self.y + other.y)
    
    def __mul__(self, scalar):
        """Enables v * 3"""
        return Vector(self.x * scalar, self.y * scalar)
    
    def __eq__(self, other):
        """Enables v1 == v2"""
        return self.x == other.x and self.y == other.y
    
    def __len__(self):
        """Enables len(v)"""
        return int((self.x**2 + self.y**2)**0.5)

# Usage
v1 = Vector(2, 3)
v2 = Vector(1, 1)
print(v1)              # Calls __str__
print(v1 + v2)         # Calls __add__
print(v1 * 3)          # Calls __mul__
print(v1 == v2)        # Calls __eq__
```

**Common Magic Methods:**

- `__init__` - Constructor
- `__str__` - String representation (informal)
- `__repr__` - String representation (official)
- `__len__` - Length
- `__getitem__` - Index access `obj[key]`
- `__setitem__` - Index assignment `obj[key] = value`
- `__add__`, `__sub__`, `__mul__` - Arithmetic operators
- `__eq__`, `__lt__`, `__gt__` - Comparison operators

### 6. Class Methods and Static Methods

```python
class Project:
    project_count = 0
    
    def __init__(self, name, start_date):
        self.name = name
        self.start_date = start_date
        Project.project_count += 1
    
    @classmethod
    def from_string(cls, project_string):
        """Alternative constructor"""
        name, start_date = project_string.split(',')
        return cls(name.strip(), start_date.strip())
    
    @classmethod
    def get_project_count(cls):
        """Access class variables"""
        return f"Total projects: {cls.project_count}"
    
    @staticmethod
    def is_valid_date(date_string):
        """Utility function that doesn't need instance or class"""
        try:
            year, month, day = map(int, date_string.split('-'))
            return 1 <= month <= 12 and 1 <= day <= 31
        except:
            return False

# Usage
project1 = Project("Atlas IDP", "2026-01-05")
project2 = Project.from_string("Code Smell Detective, 2024-06-01")
print(Project.get_project_count())
print(Project.is_valid_date("2026-01-05"))
```

### 7. Properties and Decorators

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """Getter for celsius"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """Setter with validation"""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """Computed property"""
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        self._celsius = (value - 32) * 5/9

# Usage
temp = Temperature(25)
print(temp.celsius)     # 25 (uses getter)
print(temp.fahrenheit)  # 77.0 (computed)
temp.celsius = 30       # Uses setter
print(temp.fahrenheit)  # 86.0
```

## Practice Exercises

### Exercise 1: Library System

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_checked_out = False
    
    def checkout(self):
        if not self.is_checked_out:
            self.is_checked_out = True
            return f"{self.title} checked out"
        return f"{self.title} is already checked out"
    
    def return_book(self):
        if self.is_checked_out:
            self.is_checked_out = False
            return f"{self.title} returned"
        return f"{self.title} wasn't checked out"

class Library:
    def __init__(self, name):
        self.name = name
        self.books = []
    
    def add_book(self, book):
        self.books.append(book)
    
    def find_book(self, isbn):
        for book in self.books:
            if book.isbn == isbn:
                return book
        return None
    
    def available_books(self):
        return [book for book in self.books if not book.is_checked_out]

# Test
library = Library("Tech Library")
book1 = Book("Clean Code", "Robert Martin", "978-0132350884")
book2 = Book("Python Tricks", "Dan Bader", "978-1775093305")
library.add_book(book1)
library.add_book(book2)
print(book1.checkout())
print(f"Available: {len(library.available_books())} books")
```

### Exercise 2: Shape Hierarchy

```python
from abc import ABC, abstractmethod
import math

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        return 2 * math.pi * self.radius

# Test
shapes = [
    Rectangle(5, 3),
    Circle(4)
]

for shape in shapes:
    print(f"Area: {shape.area():.2f}, Perimeter: {shape.perimeter():.2f}")
```

### Exercise 3: Employee Management

```python
class Department:
    def __init__(self, name):
        self.name = name
        self.employees = []
    
    def add_employee(self, employee):
        self.employees.append(employee)
        employee.department = self
    
    def total_salary(self):
        return sum(emp.annual_salary() for emp in self.employees)
    
    def __str__(self):
        return f"{self.name} Department ({len(self.employees)} employees)"

class Employee:
    def __init__(self, name, role, hourly_rate):
        self.name = name
        self.role = role
        self.hourly_rate = hourly_rate
        self.department = None
    
    def annual_salary(self, hours_per_month=160):
        return self.hourly_rate * hours_per_month * 12
    
    def __str__(self):
        dept = self.department.name if self.department else "No Department"
        return f"{self.name} - {self.role} ({dept})"

# Test
cloud_dept = Department("Cloud Engineering")
security_dept = Department("Security")

willem = Employee("Willem", "Cloud Engineer", 105)
alice = Employee("Alice", "Security Engineer", 100)

cloud_dept.add_employee(willem)
security_dept.add_employee(alice)

print(cloud_dept)
print(f"Total salary: €{cloud_dept.total_salary():,.2f}")
```

## Key Takeaways

1. **Classes** are blueprints; **Objects** are instances
1. **Encapsulation** - Hide internal details, expose interface
1. **Inheritance** - Reuse code through class hierarchies
1. **Polymorphism** - Same interface, different implementations
1. **Magic methods** - Customize object behavior
1. **Properties** - Controlled attribute access
1. **Class/Static methods** - Methods not tied to instances

## OOP Principles (SOLID)

- **S**ingle Responsibility - One class, one purpose
- **O**pen/Closed - Open for extension, closed for modification
- **L**iskov Substitution - Subclasses should be substitutable
- **I**nterface Segregation - Many specific interfaces > one general
- **D**ependency Inversion - Depend on abstractions, not concretions

## Common Pitfalls

- Forgetting `self` in instance methods
- Not calling `super().__init__()` in derived classes
- Overusing inheritance (favor composition)
- Making everything a class (sometimes functions are better)
- Not understanding the difference between class and instance variables

## Next Steps

Progress to **Layer 4: Functional Programming** to learn about functions as first-class citizens, decorators, and functional patterns.

## Resources

- [Python OOP Official Tutorial](https://docs.python.org/3/tutorial/classes.html)
- [Real Python - OOP in Python 3](https://realpython.com/python3-object-oriented-programming/)
- [Corey Schafer - Python OOP Tutorial](https://www.youtube.com/watch?v=ZDa-Z5JzLYM)
