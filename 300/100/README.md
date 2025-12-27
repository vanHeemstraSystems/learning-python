# 100 - 5 Layers of Learning Python

Based on the systematic approach to mastering Python through progressive layers of complexity.

## Overview

This learning path is structured around 5 progressive layers, each building upon the previous one. The approach emphasizes deep understanding over breadth, ensuring solid foundations before advancing.

## The 5 Layers

### [Layer 1: Syntax & Basics](./layer-1-syntax-basics.md)

**Foundation Level**

Core Python fundamentals including:

- Variables and data types
- Basic operations
- Control flow (if/else, loops)
- Functions basics
- Input/output

**Goal**: Write simple programs and scripts
**Time estimate**: 1-2 weeks for basics

### [Layer 2: Data Structures](./layer-2-data-structures.md)

**Intermediate Level**

Master Python’s built-in collections:

- Lists and list operations
- Tuples and immutability
- Dictionaries and key-value pairs
- Sets and set operations
- Comprehensions
- Collections module

**Goal**: Efficiently manipulate and organize data
**Time estimate**: 2-3 weeks

### [Layer 3: Object-Oriented Programming](./layer-3-oop.md)

**Intermediate-Advanced Level**

Organize code with classes and objects:

- Classes and objects
- Encapsulation
- Inheritance
- Polymorphism
- Special methods (magic methods)
- Class/static methods
- Properties and decorators

**Goal**: Design and implement object-oriented systems
**Time estimate**: 3-4 weeks

### [Layer 4: Functional Programming](./layer-4-functional-programming.md)

**Advanced Level**

Functional programming paradigm:

- First-class functions
- Lambda functions
- Map, filter, reduce
- Decorators
- Closures
- Partial functions
- Pure functions and immutability

**Goal**: Write elegant, reusable, functional code
**Time estimate**: 2-3 weeks

### [Layer 5: Advanced Topics](./layer-5-advanced-topics.md)

**Expert Level**

Advanced Python features:

- Generators and iterators
- Context managers
- Descriptors
- Metaclasses
- Advanced decorators
- Advanced data structures
- Async/await (asyncio)

**Goal**: Master Python’s most powerful features
**Time estimate**: 4-6 weeks

## Learning Strategy

### 1. Sequential Learning

Complete each layer before moving to the next. Don’t skip ahead even if you’re familiar with some concepts.

### 2. Practice-Driven

Each layer includes:

- Concept explanations with examples
- Practical exercises
- Real-world applications
- Common pitfalls to avoid

### 3. Build Projects

After completing each layer:

- **Layer 1-2**: Simple scripts and utilities
- **Layer 3**: Small applications with classes
- **Layer 4**: Data processing pipelines
- **Layer 5**: Production-ready tools

### 4. Progressive Complexity

```
Layer 1: "Hello World" → Simple calculator
Layer 2: Data analysis scripts → File processors
Layer 3: Class-based applications → Game development
Layer 4: Functional pipelines → Data transformations
Layer 5: Framework development → Production tools
```

## Recommended Project Path

### After Layer 1-2:

- CLI tools and scripts
- File processors
- Simple calculators
- Text-based games

### After Layer 3:

- Inventory systems
- Contact managers
- Simple game engines
- Data models for applications

### After Layer 4:

- Data processing pipelines
- Functional utilities library
- ETL scripts
- Code analysis tools

### After Layer 5:

- Web frameworks
- Testing frameworks
- Development tools
- Cloud automation

## Your Learning Roadmap (Willem’s Context)

Based on your background and goals:

### Immediate Focus (Atlas IDP Project)

- **Layer 1-2**: Quick review (you likely know this)
- **Layer 3**: OOP patterns for infrastructure code
- **Layer 4**: Decorators for automation scripts
- **Layer 5**: Generators for log processing, context managers for cloud resources

### Cloud Engineering Applications

#### Infrastructure as Code

```python
# Layer 3 + 4: OOP + Decorators
class AzureResource:
    @validate_config
    def deploy(self): ...

# Layer 5: Context managers
with azure_resource_group() as rg:
    deploy_vm(rg)
```

#### Security Automation

```python
# Layer 4: Functional pipelines
security_scan = compose(
    scan_vulnerabilities,
    filter_high_severity,
    generate_report
)

# Layer 5: Async for parallel scans
async def scan_multiple_repos():
    tasks = [scan_repo(r) for r in repos]
    await asyncio.gather(*tasks)
```

#### DevOps Tools

```python
# Layer 2: Data structures for config
config = {
    "environments": ["dev", "staging", "prod"],
    "services": [...],
    "deployments": {...}
}

# Layer 3 + 5: Registry pattern
@DeploymentRegistry.register('kubernetes')
class K8sDeployment:
    ...
```

## Integration with Your Existing Projects

### Code Smell Detective

- Layer 2: Parsing code into data structures
- Layer 3: AST visitor pattern (OOP)
- Layer 4: Decorators for smell detection
- Layer 5: Generators for large codebases

### Atlas IDP

- Layer 3: Resource abstractions
- Layer 4: Configuration transformations
- Layer 5: Async Azure API calls

### Security Automation

- Layer 2: Security findings data
- Layer 3: Scanner base classes
- Layer 4: Pipeline composition
- Layer 5: Concurrent scanning

## Assessment Checklist

After each layer, ensure you can:

### ✅ Layer 1

- [ ] Write functions with parameters and return values
- [ ] Use if/else and loops correctly
- [ ] Work with basic data types
- [ ] Handle user input and output

### ✅ Layer 2

- [ ] Choose appropriate data structures
- [ ] Use list/dict comprehensions
- [ ] Manipulate nested structures
- [ ] Understand mutability vs immutability

### ✅ Layer 3

- [ ] Design class hierarchies
- [ ] Implement encapsulation
- [ ] Use inheritance appropriately
- [ ] Override magic methods

### ✅ Layer 4

- [ ] Write and use decorators
- [ ] Understand closures
- [ ] Use map/filter/reduce effectively
- [ ] Create pure functions

### ✅ Layer 5

- [ ] Build generators for lazy evaluation
- [ ] Create context managers
- [ ] Understand when to use advanced features
- [ ] Write production-ready async code

## Resources by Layer

### Universal Resources

- [Official Python Documentation](https://docs.python.org/3/)
- [Real Python Tutorials](https://realpython.com/)
- [Python PEPs](https://www.python.org/dev/peps/)

### Layer-Specific

Each layer’s markdown file contains curated resources specific to that topic.

## Time Investment

**Minimum**: 12-16 weeks (3-4 months)
**Comfortable**: 16-24 weeks (4-6 months)
**Mastery**: Ongoing practice with real projects

## Next Steps

1. **Start with Layer 1**: Even if familiar, use it as a quick review
1. **Work through exercises**: Don’t just read, code along
1. **Build small projects**: After each layer
1. **Document learning**: Keep notes on insights and patterns
1. **Apply to work**: Integrate concepts into Atlas IDP and security automation

## Contributing

This is your personal learning repository. Feel free to:

- Add notes and insights
- Include project-specific examples
- Document patterns you discover
- Track progress and milestones

## Maintenance

Update these files as you:

- Discover new patterns
- Apply concepts to projects
- Learn advanced techniques
- Find better examples

-----

**Remember**: The goal isn’t speed, it’s depth. Master each layer before advancing. The time invested in solid foundations pays exponential dividends in advanced work.

**Your advantages**:

- 30 years IT experience provides context
- Security background informs best practices
- Real projects (Atlas IDP) for immediate application
- Teaching/documentation skills enhance learning

Good luck with your Python mastery journey! 🐍

-----

*Last updated: December 27, 2025*
*Learning path based on: [5 Layers to Learning Python](https://m.youtube.com/watch?v=mC4nyib2sfg)*
