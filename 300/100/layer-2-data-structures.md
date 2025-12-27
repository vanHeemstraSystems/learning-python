# Layer 2: Data Structures

## Overview

Master Python’s built-in data structures: lists, tuples, dictionaries, and sets. Understanding these is crucial for efficient data manipulation.

## Core Concepts

### 1. Lists

Ordered, mutable collections that can contain mixed data types.

```python
# Creating lists
technologies = ["Python", "Docker", "Kubernetes", "Azure"]
numbers = [1, 2, 3, 4, 5]
mixed = ["ASML", 2018, True, 3.14]
empty = []

# Accessing elements
print(technologies[0])    # "Python"
print(technologies[-1])   # "Azure" (last element)
print(technologies[1:3])  # ["Docker", "Kubernetes"] (slicing)

# Modifying lists
technologies.append("Terraform")           # Add to end
technologies.insert(0, "Git")             # Insert at index
technologies.remove("Docker")             # Remove by value
popped = technologies.pop()               # Remove and return last
technologies[0] = "GitHub"                # Replace element

# List methods
technologies.sort()                       # Sort in place
technologies.reverse()                    # Reverse in place
count = technologies.count("Python")      # Count occurrences
index = technologies.index("Kubernetes")  # Find index

# List comprehensions (powerful!)
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
upper_techs = [tech.upper() for tech in technologies]
```

### 2. Tuples

Ordered, immutable collections - like read-only lists.

```python
# Creating tuples
coordinates = (51.4695, 5.4714)  # Veldhoven coordinates
person = ("Willem", 55, "Security Engineer")
single = (42,)  # Note the comma!

# Accessing elements
lat, lon = coordinates  # Tuple unpacking
name, age, role = person

# Why use tuples?
# 1. Immutable - can't be changed accidentally
# 2. Faster than lists
# 3. Can be used as dictionary keys
# 4. Good for related data that shouldn't change

# Tuple methods (limited due to immutability)
numbers = (1, 2, 3, 2, 4, 2)
count = numbers.count(2)  # Count occurrences
index = numbers.index(3)  # Find first occurrence
```

### 3. Dictionaries

Unordered key-value pairs - extremely useful for structured data.

```python
# Creating dictionaries
employee = {
    "name": "Willem van Heemstra",
    "age": 55,
    "role": "Cloud Engineer",
    "company": "Team Rockstars IT",
    "location": "Eersel"
}

# Accessing values
print(employee["name"])           # Direct access
print(employee.get("salary"))     # Returns None if key doesn't exist
print(employee.get("salary", 0))  # Default value if key doesn't exist

# Modifying dictionaries
employee["rate"] = 105                    # Add or update
employee.update({"start_date": "2026-01-01"})
del employee["age"]                       # Delete key
popped_value = employee.pop("location")   # Remove and return

# Dictionary methods
keys = employee.keys()       # Get all keys
values = employee.values()   # Get all values
items = employee.items()     # Get key-value pairs

# Iterating
for key in employee:
    print(f"{key}: {employee[key]}")

for key, value in employee.items():
    print(f"{key}: {value}")

# Dictionary comprehensions
squared_dict = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

filtered_dict = {k: v for k, v in employee.items() if isinstance(v, str)}
```

### 4. Sets

Unordered collections of unique elements.

```python
# Creating sets
certifications = {"AZ-104", "CompTIA Security+", "AZ-700"}
skills = set(["Python", "Docker", "Python", "Azure"])  # Duplicates removed
empty = set()  # Note: {} creates an empty dict, not set!

# Set operations
certifications.add("AZ-305")           # Add element
certifications.remove("CompTIA Security+")  # Remove (raises error if not found)
certifications.discard("NonExistent")  # Remove (no error if not found)

# Mathematical set operations
team_a_skills = {"Python", "Docker", "Kubernetes"}
team_b_skills = {"Python", "Terraform", "AWS"}

# Union (all skills)
all_skills = team_a_skills | team_b_skills
# or: team_a_skills.union(team_b_skills)

# Intersection (common skills)
common_skills = team_a_skills & team_b_skills
# or: team_a_skills.intersection(team_b_skills)

# Difference (in A but not B)
unique_to_a = team_a_skills - team_b_skills
# or: team_a_skills.difference(team_b_skills)

# Symmetric difference (in A or B but not both)
unique_skills = team_a_skills ^ team_b_skills

# Set comprehensions
even_squares = {x**2 for x in range(10) if x % 2 == 0}
```

## Advanced Techniques

### Nested Data Structures

```python
# Real-world example: Project structure
projects = {
    "Atlas IDP": {
        "client": "NATO",
        "start_date": "2026-01-05",
        "technologies": ["Azure", "Terraform", "Kubernetes"],
        "team_size": 5
    },
    "Code Smell Detective": {
        "type": "Open Source",
        "language": "Python",
        "status": "Active",
        "contributors": ["Willem"]
    }
}

# Accessing nested data
print(projects["Atlas IDP"]["technologies"][0])  # "Azure"

# Nested list comprehension
matrix = [[i*j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]
```

### Collections Module

```python
from collections import defaultdict, Counter, namedtuple

# defaultdict - never raises KeyError
word_count = defaultdict(int)
for word in ["python", "is", "python", "amazing"]:
    word_count[word] += 1

# Counter - count hashable objects
tech_stack = ["Python", "Docker", "Python", "Kubernetes", "Docker", "Python"]
counter = Counter(tech_stack)
print(counter.most_common(2))  # [('Python', 3), ('Docker', 2)]

# namedtuple - tuple with named fields
Employee = namedtuple('Employee', ['name', 'role', 'rate'])
willem = Employee(name="Willem", role="Cloud Engineer", rate=105)
print(willem.name)  # More readable than willem[0]
```

## Practice Exercises

### Exercise 1: Contact Manager

```python
contacts = {}

def add_contact(name, email, phone):
    contacts[name] = {"email": email, "phone": phone}

def get_contact(name):
    return contacts.get(name, "Contact not found")

def remove_contact(name):
    if name in contacts:
        del contacts[name]
        return f"{name} removed"
    return "Contact not found"

# Test
add_contact("Rianne", "rianne@example.com", "+31612345678")
print(get_contact("Rianne"))
```

### Exercise 2: Unique Word Counter

```python
def count_unique_words(text):
    # Remove punctuation and convert to lowercase
    words = text.lower().replace(".", "").replace(",", "").split()
    unique_words = set(words)
    return len(unique_words), unique_words

text = "Python is great. Python is powerful. Python is versatile."
count, words = count_unique_words(text)
print(f"Unique words: {count}")
print(f"Words: {words}")
```

### Exercise 3: Grade Book

```python
grade_book = {}

def add_student(name, grades):
    grade_book[name] = grades

def calculate_average(name):
    if name in grade_book:
        grades = grade_book[name]
        return sum(grades) / len(grades)
    return None

def get_top_student():
    if not grade_book:
        return None
    averages = {name: calculate_average(name) for name in grade_book}
    return max(averages, key=averages.get)

# Test
add_student("Alice", [85, 90, 88, 92])
add_student("Bob", [78, 85, 80, 83])
add_student("Charlie", [95, 98, 96, 99])

print(f"Top student: {get_top_student()}")
```

### Exercise 4: Data Transformation

```python
# Transform list of tuples to dictionary
employees = [
    ("Willem", "Cloud Engineer"),
    ("Alice", "DevOps Engineer"),
    ("Bob", "Security Analyst")
]

employee_dict = {name: role for name, role in employees}
print(employee_dict)

# Group by first letter
from collections import defaultdict

def group_by_first_letter(words):
    groups = defaultdict(list)
    for word in words:
        groups[word[0].upper()].append(word)
    return dict(groups)

cities = ["Amsterdam", "Eindhoven", "Eersel", "Veldhoven", "Arnhem"]
grouped = group_by_first_letter(cities)
print(grouped)
```

## Key Takeaways

1. **Lists** - Use for ordered, changeable collections
1. **Tuples** - Use for ordered, unchangeable data
1. **Dictionaries** - Use for key-value mappings
1. **Sets** - Use for unique elements and set operations
1. **Comprehensions** - Concise way to create data structures
1. **Choose wisely** - Right data structure = better performance

## When to Use What?

|Use Case                       |Data Structure                   |
|-------------------------------|---------------------------------|
|Ordered items that change      |List                             |
|Ordered items that don’t change|Tuple                            |
|Key-value lookups              |Dictionary                       |
|Unique items only              |Set                              |
|Fast membership testing        |Set or Dictionary                |
|JSON-like nested data          |Dictionary with nested structures|

## Common Pitfalls

- Modifying a list while iterating over it
- Using `[]` as default argument in functions (mutable default!)
- Forgetting that dictionaries were unordered in Python < 3.7
- Not handling missing dictionary keys
- Trying to modify tuples

## Next Steps

Master **Layer 3: Object-Oriented Programming** to organize code using classes and objects.

## Resources

- [Python Data Structures Official Docs](https://docs.python.org/3/tutorial/datastructures.html)
- [Real Python - Lists and Tuples](https://realpython.com/python-lists-tuples/)
- [Real Python - Dictionaries](https://realpython.com/python-dicts/)
- [Understanding Python Sets](https://realpython.com/python-sets/)
