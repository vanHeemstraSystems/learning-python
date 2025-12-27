# 300 - Learning Our Subject

# 🚀 Building Your First Python App: A Beginner’s Guide

## Project Overview: Personal Task Manager

We’re going to build a **Task Manager** app where you can add, view, and complete tasks. Think of it like a simple to-do list! By the end, you’ll understand how real developers organize and build applications.

-----

## 📚 What We’re Building

**Files we’ll create:**

1. `app.py` - The main program (your app!)
1. `test_app.py` - Tests to make sure our code works
1. `requirements.txt` - Lists what extra tools our app needs
1. `config.conf` - Settings we can change without touching code
1. `.gitignore` - Tells Git what files to ignore

-----

## 🧪 Why Test-Driven Development (TDD)?

**Imagine building a bicycle:** Would you wait until it’s completely assembled to check if the brakes work? No way! You’d test each part as you go.

**That’s what TDD is:**

1. ✍️ Write a test (describe what you want the code to do)
1. ❌ Run it - it fails (because we haven’t written the code yet!)
1. ✅ Write just enough code to make the test pass
1. 🔄 Repeat!

**Why this is awesome:**

- You catch mistakes early (before they become big problems)
- You know exactly what to build next
- You can change code confidently (tests tell you if you broke something)
- It’s like having a safety net while learning!

-----

## 🏗️ Understanding SOLID Principles

**SOLID** is an acronym for five principles that help you write better, cleaner code. Think of them as building rules that make your code easier to understand, change, and grow.

### Why Should You Care About SOLID?

Imagine you built a treehouse, but:

- You can’t add a second floor without rebuilding the whole thing
- If one board breaks, the entire structure collapses
- You need a PhD to figure out how to add a new window
- Every time you fix something, something else breaks

**SOLID principles help you avoid this mess!**

-----

## 🎯 The SOLID Principles (Simple Version)

### **S - Single Responsibility Principle (SRP)**

**“Each piece should do ONE thing and do it well”**

**Real-world example:**
Think about your phone. The calculator app does math. The camera app takes pictures. Imagine if one app tried to do EVERYTHING - it would be confusing and buggy!

**In code:**

```python
# ❌ BAD: This function does too many things
def process_user(name, email, tasks):
    # Validates email
    if "@" not in email:
        return False
    # Saves to database
    save_to_db(name, email)
    # Sends welcome email
    send_email(email, "Welcome!")
    # Creates tasks
    for task in tasks:
        create_task(task)

# ✅ GOOD: Each function has ONE job
def validate_email(email):
    return "@" in email

def save_user(name, email):
    save_to_db(name, email)

def send_welcome_email(email):
    send_email(email, "Welcome!")

def create_user_tasks(tasks):
    for task in tasks:
        create_task(task)
```

**Why it matters:** When something breaks, you know exactly where to look!

-----

### **O - Open/Closed Principle (OCP)**

**“You should be able to add new features WITHOUT changing existing code”**

**Real-world example:**
Think about USB ports. You can plug in new devices (keyboards, mice, phones) without modifying the computer’s internal parts. The computer is “open for extension” but “closed for modification.”

**In code:**

```python
# ❌ BAD: Adding new task types means editing this function
def display_task(task):
    if task.type == "work":
        print(f"💼 {task.description}")
    elif task.type == "personal":
        print(f"🏠 {task.description}")
    # Uh oh, need to add "shopping"? Edit this function again!
    elif task.type == "shopping":
        print(f"🛒 {task.description}")

# ✅ GOOD: New task types don't require changing this
class Task:
    def display(self):
        print(f"{self.icon} {self.description}")

class WorkTask(Task):
    icon = "💼"

class PersonalTask(Task):
    icon = "🏠"

class ShoppingTask(Task):  # New type? Just add a new class!
    icon = "🛒"
```

**Why it matters:** Adding features doesn’t risk breaking existing code!

-----

### **L - Liskov Substitution Principle (LSP)**

**“If something works with a parent, it should work with any child”**

**Real-world example:**
If a recipe says “add a fruit,” you should be able to use an apple, orange, or banana - they all work as fruits. You shouldn’t add a tomato and have the recipe explode!

**In code:**

```python
# ❌ BAD: Car needs gas, but ElectricCar breaks this rule
class Car:
    def refuel(self):
        print("Adding gasoline")

class ElectricCar(Car):
    def refuel(self):
        raise Exception("I don't use gas!")  # BREAKS!

# ✅ GOOD: All vehicles can recharge, just differently
class Vehicle:
    def recharge(self):
        pass  # Parent defines the interface

class GasCar(Vehicle):
    def recharge(self):
        print("Adding gasoline")

class ElectricCar(Vehicle):
    def recharge(self):
        print("Plugging in to charger")
```

**Why it matters:** Your code is predictable and doesn’t surprise you!

-----

### **I - Interface Segregation Principle (ISP)**

**“Don’t force someone to have features they don’t need”**

**Real-world example:**
A basic flip phone shouldn’t be forced to have a camera, GPS, and app store just because smartphones have them. Each device should only have what it needs.

**In code:**

```python
# ❌ BAD: Basic task forced to implement complex features
class TaskInterface:
    def set_priority(self): pass
    def set_deadline(self): pass
    def set_tags(self): pass
    def set_subtasks(self): pass

class BasicTask(TaskInterface):
    # Has to implement ALL of these, even if it doesn't need them
    def set_priority(self): raise NotImplementedError
    def set_deadline(self): raise NotImplementedError
    def set_tags(self): raise NotImplementedError
    def set_subtasks(self): raise NotImplementedError

# ✅ GOOD: Pick only what you need
class BasicTaskInterface:
    def set_description(self): pass

class AdvancedTaskInterface(BasicTaskInterface):
    def set_priority(self): pass
    def set_deadline(self): pass

class BasicTask(BasicTaskInterface):
    def set_description(self): 
        # Only implements what it needs
        pass
```

**Why it matters:** Simple things stay simple!

-----

### **D - Dependency Inversion Principle (DIP)**

**“Depend on ideas, not specific implementations”**

**Real-world example:**
Your lamp works with any lightbulb (LED, incandescent, halogen) because it depends on the IDEA of “a thing that produces light,” not a specific bulb type.

**In code:**

```python
# ❌ BAD: TaskManager locked to specific storage type
class TaskManager:
    def __init__(self):
        self.storage = JSONFileStorage()  # Locked to JSON files!
    
    def save_tasks(self, tasks):
        self.storage.save(tasks)

# ✅ GOOD: TaskManager works with ANY storage
class TaskManager:
    def __init__(self, storage):
        self.storage = storage  # Could be JSON, SQL, Cloud, anything!
    
    def save_tasks(self, tasks):
        self.storage.save(tasks)

# Now you can use any storage type
task_manager = TaskManager(JSONFileStorage())
# Or later switch to:
task_manager = TaskManager(SQLiteStorage())
# Or even:
task_manager = TaskManager(CloudStorage())
```

**Why it matters:** You can swap out parts without rewriting everything!

-----

## 🎨 SOLID in One Picture

Think of building with LEGO:

- **S**: Each brick has one job (connect to other bricks)
- **O**: You can add new bricks without modifying existing ones
- **L**: Any 2x4 brick works where a brick is needed
- **I**: A simple brick doesn’t need to be a motorized gear
- **D**: Bricks connect via the standard pegs, not specific brick types

-----

## 🌳 Understanding Git & Gitflow

Before we start coding, let’s understand how professional developers save and organize their code.

### What is Git?

**Git is like a time machine for your code!** It:

- Saves snapshots of your project at different points in time
- Lets you go back if you mess something up
- Helps multiple people work on the same project without interfering with each other
- Keeps a history of every change you made

**Real-world example:** You’re writing an essay. With Git, you can save version 1, make changes, save version 2, and if you don’t like version 2, you can go back to version 1. It’s like “Save As” but way more powerful!

### What is Gitflow?

**Gitflow is a strategy for organizing your work with Git.** Think of it like traffic lanes on a highway:

- **Main lane (main branch)**: The finished, working code that people actually use
- **Development lane (develop branch)**: Where features come together for testing
- **Feature lanes (feature branches)**: Individual lanes where you build new things

**Why use Gitflow?**

- You can work on new features without breaking the working app
- Multiple people can work on different features at the same time
- You can review code before it becomes “official”
- If something goes wrong, the main version is always safe

-----

## Step 1: Setting Up Git and Your Repository

### Install Git (if you haven’t already)

**Check if Git is installed:**

```bash
git --version
```

**If not installed:**

- Windows: Download from [git-scm.com](https://git-scm.com)
- Mac: Install via Homebrew `brew install git` or it may already be installed
- Linux: `sudo apt-get install git` (Ubuntu/Debian)

### Configure Git (first time only)

Tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Create Your Project Folder and Initialize Git

```bash
# Create the project folder
mkdir task_manager
cd task_manager

# Initialize Git
git init
```

### Create the Main Branch Structure

```bash
# Create the main branch
git branch -M main
```

-----

## Step 2: Create .gitignore FIRST

**Create a file named `.gitignore` and add:**

```
# Python stuff
__pycache__/
*.py[cod]
*$py.class
*.so

# Virtual environment
venv/
env/
ENV/

# Testing
.pytest_cache/
.coverage
htmlcov/

# Personal files
*.log
.DS_Store
.vscode/
.idea/

# Config files with sensitive data (optional)
# config.conf
```

**Commit it:**

```bash
git add .gitignore
git commit -m "Initial commit: Add .gitignore"
```

-----

## Step 3: Create requirements.txt

**Create a file named `requirements.txt` and add:**

```txt
colorama==0.4.6
pytest==7.4.3
```

**Commit it:**

```bash
git add requirements.txt
git commit -m "Add requirements.txt with dependencies"
```

-----

## Step 4: Create config.conf

**Create a file named `config.conf` and add:**

```conf
[Settings]
max_tasks = 50
show_colors = true
app_name = My Task Manager

[Display]
completed_symbol = ✓
incomplete_symbol = ○
```

**Why separate config from code:**
Settings users might want to change without editing the actual code.

**Commit it:**

```bash
git add config.conf
git commit -m "Add configuration file with default settings"
```

-----

## Step 5: Create Development Branch

```bash
git checkout -b develop
```

-----

## Step 6: Create Feature Branch and Write Tests

```bash
git checkout -b feature/add-tests
```

**Create `test_app.py`:**

```python
# test_app.py
# Tests for our Task Manager

import pytest
from app import Task, TaskManager, ConfigManager, TaskDisplay


class TestTask:
    """Tests for the Task class (Single Responsibility)"""
    
    def test_create_task(self):
        """Test creating a basic task"""
        task = Task("Buy groceries")
        assert task.description == "Buy groceries"
        assert task.completed == False
    
    def test_complete_task(self):
        """Test marking a task as complete"""
        task = Task("Do homework")
        task.mark_complete()
        assert task.completed == True
    
    def test_task_string_representation(self):
        """Test how task appears as string"""
        task = Task("Write code")
        assert "Write code" in str(task)


class TestTaskManager:
    """Tests for TaskManager class (Single Responsibility)"""
    
    def test_add_task(self):
        """Test adding a task to the manager"""
        manager = TaskManager(max_tasks=50)
        result = manager.add_task("Test task")
        
        assert result == True
        assert len(manager.get_all_tasks()) == 1
    
    def test_max_tasks_limit(self):
        """Test that max tasks limit is enforced"""
        manager = TaskManager(max_tasks=2)
        manager.add_task("Task 1")
        manager.add_task("Task 2")
        result = manager.add_task("Task 3")  # Should fail
        
        assert result == False
        assert len(manager.get_all_tasks()) == 2
    
    def test_complete_task_by_index(self):
        """Test completing a task by its index"""
        manager = TaskManager(max_tasks=50)
        manager.add_task("Task 1")
        manager.add_task("Task 2")
        
        result = manager.complete_task(0)
        
        assert result == True
        tasks = manager.get_all_tasks()
        assert tasks[0].completed == True
        assert tasks[1].completed == False
    
    def test_complete_invalid_index(self):
        """Test that invalid index returns False"""
        manager = TaskManager(max_tasks=50)
        manager.add_task("Task 1")
        
        result = manager.complete_task(5)  # Invalid index
        assert result == False
    
    def test_get_all_tasks(self):
        """Test retrieving all tasks"""
        manager = TaskManager(max_tasks=50)
        manager.add_task("Task 1")
        manager.add_task("Task 2")
        
        tasks = manager.get_all_tasks()
        assert len(tasks) == 2


class TestConfigManager:
    """Tests for ConfigManager (Single Responsibility)"""
    
    def test_load_config(self):
        """Test loading configuration"""
        config = ConfigManager('config.conf')
        settings = config.get_settings()
        
        assert 'max_tasks' in settings
        assert 'show_colors' in settings
        assert 'app_name' in settings
    
    def test_get_specific_setting(self):
        """Test getting a specific setting"""
        config = ConfigManager('config.conf')
        max_tasks = config.get_setting('max_tasks', default=10)
        
        assert isinstance(max_tasks, int)
        assert max_tasks > 0
```

**Commit the tests:**

```bash
git add test_app.py
git commit -m "Add comprehensive tests following SOLID principles"
```

-----

## Step 7: Write the App with SOLID Principles

**Create `app.py` with SOLID in mind:**

```python
# app.py
# Task Manager Application - Built with SOLID Principles

import configparser
from colorama import init, Fore, Style
from typing import List, Optional

# Initialize colorama
init()


# ============================================================================
# SINGLE RESPONSIBILITY PRINCIPLE (SRP)
# Each class has ONE clear job and ONE reason to change
# ============================================================================

class Task:
    """
    Represents a single task.
    
    RESPONSIBILITY: Manage task data and state.
    This class ONLY deals with what a task IS, not how to store it,
    display it, or manage multiple tasks.
    """
    
    def __init__(self, description: str):
        """
        Create a new task.
        
        Args:
            description: What the task is about
        """
        self.description = description
        self.completed = False
    
    def mark_complete(self):
        """Mark this task as completed."""
        self.completed = True
    
    def mark_incomplete(self):
        """Mark this task as incomplete."""
        self.completed = False
    
    def __str__(self):
        """String representation of the task."""
        status = "✓" if self.completed else "○"
        return f"[{status}] {self.description}"


class ConfigManager:
    """
    Handles configuration file loading.
    
    RESPONSIBILITY: Load and provide access to configuration settings.
    If we need to change how config is loaded (maybe use JSON instead?),
    we only change this class.
    """
    
    def __init__(self, config_file: str = 'config.conf'):
        """
        Initialize the config manager.
        
        Args:
            config_file: Path to the configuration file
        """
        self.config_file = config_file
        self.config = configparser.ConfigParser()
        self.config.read(config_file)
    
    def get_setting(self, key: str, section: str = 'Settings', default=None):
        """
        Get a specific setting.
        
        Args:
            key: Setting name
            section: Config section
            default: Default value if not found
        """
        try:
            value = self.config.get(section, key)
            # Try to convert to int or bool if possible
            if value.lower() in ('true', 'false'):
                return value.lower() == 'true'
            try:
                return int(value)
            except ValueError:
                return value
        except:
            return default
    
    def get_settings(self) -> dict:
        """Get all settings as a dictionary."""
        settings = {}
        for section in self.config.sections():
            for key, value in self.config.items(section):
                settings[key] = self.get_setting(key, section)
        return settings


class TaskManager:
    """
    Manages a collection of tasks.
    
    RESPONSIBILITY: Business logic for task operations (add, remove, complete).
    This class doesn't know about display, storage, or configuration - just
    managing the task collection.
    """
    
    def __init__(self, max_tasks: int = 50):
        """
        Initialize the task manager.
        
        Args:
            max_tasks: Maximum number of tasks allowed
        """
        self.tasks: List[Task] = []
        self.max_tasks = max_tasks
    
    def add_task(self, description: str) -> bool:
        """
        Add a new task.
        
        Args:
            description: Task description
            
        Returns:
            True if added successfully, False if at max capacity
        """
        if len(self.tasks) >= self.max_tasks:
            return False
        
        task = Task(description)
        self.tasks.append(task)
        return True
    
    def get_all_tasks(self) -> List[Task]:
        """
        Get all tasks.
        
        Returns:
            List of all tasks
        """
        return self.tasks
    
    def get_task(self, index: int) -> Optional[Task]:
        """
        Get a specific task by index.
        
        Args:
            index: Task index
            
        Returns:
            Task if found, None otherwise
        """
        if 0 <= index < len(self.tasks):
            return self.tasks[index]
        return None
    
    def complete_task(self, index: int) -> bool:
        """
        Mark a task as complete.
        
        Args:
            index: Task index
            
        Returns:
            True if successful, False if invalid index
        """
        task = self.get_task(index)
        if task:
            task.mark_complete()
            return True
        return False
    
    def delete_task(self, index: int) -> bool:
        """
        Delete a task.
        
        Args:
            index: Task index
            
        Returns:
            True if successful, False if invalid index
        """
        if 0 <= index < len(self.tasks):
            self.tasks.pop(index)
            return True
        return False


# ============================================================================
# OPEN/CLOSED PRINCIPLE (OCP) & DEPENDENCY INVERSION PRINCIPLE (DIP)
# Display is open for extension (can create new display styles)
# but closed for modification (don't need to change TaskDisplay)
# We depend on abstractions (DisplayStrategy) not concrete implementations
# ============================================================================

class DisplayStrategy:
    """
    Abstract base class for display strategies.
    
    This defines the INTERFACE that all display strategies must follow.
    This is the "abstraction" that high-level code depends on (DIP).
    """
    
    def display_task(self, index: int, task: Task) -> str:
        """
        Format a task for display.
        
        Args:
            index: Task number (for display)
            task: The task to display
            
        Returns:
            Formatted string
        """
        raise NotImplementedError("Subclasses must implement this")
    
    def display_header(self, title: str) -> str:
        """Format the header."""
        raise NotImplementedError("Subclasses must implement this")


class ColoredDisplayStrategy(DisplayStrategy):
    """
    Display tasks with colors.
    
    OPEN/CLOSED: New display style without modifying existing code!
    """
    
    def display_task(self, index: int, task: Task) -> str:
        """Display task with color based on completion status."""
        status = "✓" if task.completed else "○"
        color = Fore.GREEN if task.completed else Fore.YELLOW
        return f"{color}{index + 1}. [{status}] {task.description}{Style.RESET_ALL}"
    
    def display_header(self, title: str) -> str:
        """Display colored header."""
        line = "=" * 50
        return f"\n{Fore.CYAN}{line}\n  {title}\n{line}{Style.RESET_ALL}"


class PlainDisplayStrategy(DisplayStrategy):
    """
    Display tasks without colors (for terminals that don't support it).
    
    OPEN/CLOSED: Another display style, no changes to existing code!
    """
    
    def display_task(self, index: int, task: Task) -> str:
        """Display task in plain text."""
        status = "✓" if task.completed else "○"
        return f"{index + 1}. [{status}] {task.description}"
    
    def display_header(self, title: str) -> str:
        """Display plain header."""
        line = "=" * 50
        return f"\n{line}\n  {title}\n{line}"


class TaskDisplay:
    """
    Handles displaying tasks to the user.
    
    RESPONSIBILITY: Format and display task information.
    DEPENDENCY INVERSION: Depends on DisplayStrategy (abstraction),
    not specific display implementations.
    """
    
    def __init__(self, strategy: DisplayStrategy, app_name: str = "Task Manager"):
        """
        Initialize the display handler.
        
        Args:
            strategy: The display strategy to use (DIP - we depend on abstraction!)
            app_name: Name of the application
        """
        self.strategy = strategy
        self.app_name = app_name
    
    def show_tasks(self, tasks: List[Task]):
        """
        Display all tasks.
        
        Args:
            tasks: List of tasks to display
        """
        print(self.strategy.display_header(self.app_name))
        
        if len(tasks) == 0:
            print("No tasks yet! Add one to get started.")
            return
        
        for index, task in enumerate(tasks):
            print(self.strategy.display_task(index, task))


# ============================================================================
# INTERFACE SEGREGATION PRINCIPLE (ISP)
# UserInterface only exposes methods the main app needs
# We don't force it to have methods for things it doesn't do
# ============================================================================

class UserInterface:
    """
    Handles user interaction.
    
    RESPONSIBILITY: Get input from user and show messages.
    INTERFACE SEGREGATION: Only the methods needed for user interaction.
    """
    
    def show_message(self, message: str):
        """Display a message to the user."""
        print(message)
    
    def show_error(self, message: str):
        """Display an error message."""
        print(f"❌ {message}")
    
    def show_success(self, message: str):
        """Display a success message."""
        print(f"✅ {message}")
    
    def get_input(self, prompt: str) -> str:
        """
        Get text input from user.
        
        Args:
            prompt: The prompt to show
            
        Returns:
            User's input
        """
        return input(prompt)
    
    def get_number_input(self, prompt: str) -> Optional[int]:
        """
        Get number input from user.
        
        Args:
            prompt: The prompt to show
            
        Returns:
            Number if valid, None if invalid
        """
        try:
            return int(self.get_input(prompt))
        except ValueError:
            return None
    
    def show_menu(self, options: List[str]):
        """
        Display a menu.
        
        Args:
            options: List of menu options
        """
        print("\nWhat would you like to do?")
        for i, option in enumerate(options, 1):
            print(f"{i}. {option}")


# ============================================================================
# APPLICATION CLASS
# Coordinates all the pieces following SOLID principles
# ============================================================================

class TaskManagerApp:
    """
    Main application class that coordinates everything.
    
    DEPENDENCY INVERSION: This high-level class depends on abstractions
    (interfaces/base classes), not concrete implementations.
    """
    
    def __init__(
        self,
        config_manager: ConfigManager,
        task_manager: TaskManager,
        display: TaskDisplay,
        ui: UserInterface
    ):
        """
        Initialize the application.
        
        Args:
            config_manager: Handles configuration
            task_manager: Manages tasks
            display: Handles task display
            ui: Handles user interaction
            
        Notice: We're INJECTING dependencies (DIP)!
        The app doesn't create these - they're given to it.
        This makes testing easier and the code more flexible.
        """
        self.config = config_manager
        self.manager = task_manager
        self.display = display
        self.ui = ui
    
    def run(self):
        """Run the main application loop."""
        settings = self.config.get_settings()
        self.ui.show_message(f"\n🎯 Welcome to {settings['app_name']}!")
        
        menu_options = [
            "Add a task",
            "View tasks",
            "Complete a task",
            "Delete a task",
            "Quit"
        ]
        
        while True:
            self.ui.show_menu(menu_options)
            choice = self.ui.get_input("\nEnter your choice (1-5): ")
            
            if choice == "1":
                self._handle_add_task()
            elif choice == "2":
                self._handle_view_tasks()
            elif choice == "3":
                self._handle_complete_task()
            elif choice == "4":
                self._handle_delete_task()
            elif choice == "5":
                self.ui.show_message("👋 Goodbye!")
                break
            else:
                self.ui.show_error("Invalid choice! Please enter 1-5.")
    
    def _handle_add_task(self):
        """Handle adding a new task."""
        description = self.ui.get_input("What's the task? ")
        
        if not description.strip():
            self.ui.show_error("Task description cannot be empty!")
            return
        
        if self.manager.add_task(description):
            self.ui.show_success(f"Added: {description}")
        else:
            max_tasks = self.config.get_setting('max_tasks', default=50)
            self.ui.show_error(f"Cannot add more tasks! Maximum is {max_tasks}")
    
    def _handle_view_tasks(self):
        """Handle viewing all tasks."""
        tasks = self.manager.get_all_tasks()
        self.display.show_tasks(tasks)
    
    def _handle_complete_task(self):
        """Handle completing a task."""
        tasks = self.manager.get_all_tasks()
        
        if len(tasks) == 0:
            self.ui.show_error("No tasks to complete!")
            return
        
        self.display.show_tasks(tasks)
        task_num = self.ui.get_number_input("\nWhich task number? ")
        
        if task_num is None:
            self.ui.show_error("Please enter a valid number!")
            return
        
        if self.manager.complete_task(task_num - 1):
            self.ui.show_success("Task marked as complete!")
        else:
            self.ui.show_error("Invalid task number!")
    
    def _handle_delete_task(self):
        """Handle deleting a task."""
        tasks = self.manager.get_all_tasks()
        
        if len(tasks) == 0:
            self.ui.show_error("No tasks to delete!")
            return
        
        self.display.show_tasks(tasks)
        task_num = self.ui.get_number_input("\nWhich task number to delete? ")
        
        if task_num is None:
            self.ui.show_error("Please enter a valid number!")
            return
        
        if self.manager.delete_task(task_num - 1):
            self.ui.show_success("Task deleted!")
        else:
            self.ui.show_error("Invalid task number!")


# ============================================================================
# MAIN ENTRY POINT
# This is where we "wire up" all the components
# ============================================================================

def main():
    """
    Main entry point - creates and wires up all components.
    
    This demonstrates DEPENDENCY INJECTION - we create all the parts
    and pass them to the classes that need them.
    """
    # Load configuration
    config_manager = ConfigManager('config.conf')
    settings = config_manager.get_settings()
    
    # Create task manager with settings
    task_manager = TaskManager(max_tasks=settings['max_tasks'])
    
    # Create display strategy based on config
    if settings['show_colors']:
        display_strategy = ColoredDisplayStrategy()
    else:
        display_strategy = PlainDisplayStrategy()
    
    # Create display handler
    task_display = TaskDisplay(
        strategy=display_strategy,
        app_name=settings['app_name']
    )
    
    # Create user interface
    ui = UserInterface()
    
    # Create and run the app (Dependency Injection!)
    app = TaskManagerApp(
        config_manager=config_manager,
        task_manager=task_manager,
        display=task_display,
        ui=ui
    )
    
    app.run()


if __name__ == "__main__":
    main()
```

**Understanding SOLID in This Code:**

1. **Single Responsibility (S):**

- `Task` - Only handles task data
- `ConfigManager` - Only handles configuration
- `TaskManager` - Only manages task collection
- `TaskDisplay` - Only handles display
- `UserInterface` - Only handles user interaction

1. **Open/Closed (O):**

- Want a new display style? Create a new `DisplayStrategy` subclass
- No need to modify existing `TaskDisplay` code

1. **Liskov Substitution (L):**

- Any `DisplayStrategy` works in `TaskDisplay`
- `ColoredDisplayStrategy` and `PlainDisplayStrategy` are interchangeable

1. **Interface Segregation (I):**

- `UserInterface` only has methods for user interaction
- `DisplayStrategy` only has display methods
- No bloated interfaces with unused methods

1. **Dependency Inversion (D):**

- `TaskDisplay` depends on `DisplayStrategy` (abstraction), not concrete classes
- `TaskManagerApp` is given its dependencies, doesn’t create them
- Easy to swap out parts for testing or different implementations

**Commit the app:**

```bash
git add app.py
git commit -m "Implement task manager with SOLID principles"
```

-----

## Step 8: Test Everything

**Install dependencies:**

```bash
pip install -r requirements.txt
```

**Run the tests:**

```bash
pytest test_app.py -v
```

All tests should pass!

**Run the app:**

```bash
python app.py
```

-----

## Step 9: Merge to Develop

```bash
# Make sure all changes are committed
git status

# Switch to develop
git checkout develop

# Merge the feature
git merge feature/add-tests

# Delete the feature branch (optional)
git branch -d feature/add-tests
```

-----

## Step 10: Push to GitHub

```bash
# Add remote (first time only)
git remote add origin https://github.com/YOUR_USERNAME/task-manager.git

# Push main branch
git checkout main
git merge develop
git push -u origin main

# Push develop branch
git checkout develop
git push -u origin develop
```

-----

## 🎓 What You’ve Learned

### Programming Concepts:

- **Variables & Data Types**: Strings, integers, booleans, lists
- **Functions**: Reusable code blocks
- **Classes & Objects**: Creating your own data types
- **Type Hints**: Making code clearer (`str`, `int`, `List[Task]`)
- **Loops & Conditionals**: Control flow
- **Imports & Modules**: Organizing code

### SOLID Principles:

- ✅ **Single Responsibility**: One class, one job
- ✅ **Open/Closed**: Extend without modifying
- ✅ **Liskov Substitution**: Interchangeable components
- ✅ **Interface Segregation**: Small, focused interfaces
- ✅ **Dependency Inversion**: Depend on abstractions

### Design Patterns:

- ✅ **Strategy Pattern**: Swappable display strategies
- ✅ **Dependency Injection**: Passing dependencies to classes
- ✅ **Separation of Concerns**: Each part has a clear role

### Best Practices:

- ✅ **Test-Driven Development**: Write tests first
- ✅ **Configuration Files**: Separate settings from code
- ✅ **Type Hints**: Document expected types
- ✅ **Docstrings**: Explain what code does
- ✅ **Meaningful Names**: Clear, descriptive names
- ✅ **Git & Gitflow**: Professional version control

-----

## 🚀 Challenge: Add a Feature Using SOLID!

Let’s add **Task Priority** using SOLID principles!

### Step 1: Create Feature Branch

```bash
git checkout develop
git checkout -b feature/task-priority
```

### Step 2: Write Tests First (TDD!)

Add to `test_app.py`:

```python
class TestTaskWithPriority:
    """Test tasks with priority levels"""
    
    def test_task_default_priority(self):
        """Test that tasks have default priority"""
        task = Task("Test task")
        assert hasattr(task, 'priority')
        assert task.priority == "medium"
    
    def test_set_task_priority(self):
        """Test setting task priority"""
        task = Task("Important task")
        task.set_priority("high")
        assert task.priority == "high"
    
    def test_priority_display(self):
        """Test that priority appears in string representation"""
        task = Task("Urgent task")
        task.set_priority("high")
        task_str = str(task)
        assert "high" in task_str.lower() or "!" in task_str
```

### Step 3: Implement the Feature

Modify the `Task` class in `app.py`:

```python
class Task:
    """
    Represents a single task with priority.
    
    SINGLE RESPONSIBILITY: Still only manages task data.
    We're extending functionality without breaking existing code.
    """
    
    VALID_PRIORITIES = ["low", "medium", "high"]
    
    def __init__(self, description: str, priority: str = "medium"):
        """
        Create a new task.
        
        Args:
            description: What the task is about
            priority: Priority level (low, medium, high)
        """
        self.description = description
        self.completed = False
        self.priority = priority if priority in self.VALID_PRIORITIES else "medium"
    
    def set_priority(self, priority: str):
        """
        Set task priority.
        
        Args:
            priority: New priority level
        """
        if priority in self.VALID_PRIORITIES:
            self.priority = priority
    
    def mark_complete(self):
        """Mark this task as completed."""
        self.completed = True
    
    def mark_incomplete(self):
        """Mark this task as incomplete."""
        self.completed = False
    
    def __str__(self):
        """String representation of the task."""
        status = "✓" if self.completed else "○"
        priority_symbol = "!" if self.priority == "high" else ""
        return f"[{status}] {priority_symbol}{self.description}"
```

### Step 4: Update Display Strategy (Open/Closed!)

Add a new display strategy without modifying existing ones:

```python
class PriorityColoredDisplayStrategy(DisplayStrategy):
    """
    Display tasks with colors based on priority AND completion.
    
    OPEN/CLOSED: We're EXTENDING functionality without MODIFYING
    existing ColoredDisplayStrategy or PlainDisplayStrategy!
    """
    
    PRIORITY_COLORS = {
        "high": Fore.RED,
        "medium": Fore.YELLOW,
        "low": Fore.BLUE
    }
    
    def display_task(self, index: int, task: Task) -> str:
        """Display task with priority-based coloring."""
        status = "✓" if task.completed else "○"
        
        if task.completed:
            color = Fore.GREEN
        else:
            color = self.PRIORITY_COLORS.get(task.priority, Fore.WHITE)
        
        priority_label = f"[{task.priority.upper()}]"
        return f"{color}{index + 1}. [{status}] {priority_label} {task.description}{Style.RESET_ALL}"
    
    def display_header(self, title: str) -> str:
        """Display colored header."""
        line = "=" * 50
        return f"\n{Fore.CYAN}{line}\n  {title}\n{line}{Style.RESET_ALL}"
```

### Step 5: Update App to Support Priority

Add a method to `TaskManagerApp`:

```python
def _handle_set_priority(self):
    """Handle setting task priority."""
    tasks = self.manager.get_all_tasks()
    
    if len(tasks) == 0:
        self.ui.show_error("No tasks available!")
        return
    
    self.display.show_tasks(tasks)
    task_num = self.ui.get_number_input("\nWhich task number? ")
    
    if task_num is None or not (0 < task_num <= len(tasks)):
        self.ui.show_error("Invalid task number!")
        return
    
    self.ui.show_message("\nPriority levels: 1=Low, 2=Medium, 3=High")
    priority_choice = self.ui.get_number_input("Choose priority: ")
    
    priority_map = {1: "low", 2: "medium", 3: "high"}
    priority = priority_map.get(priority_choice)
    
    if priority:
        task = self.manager.get_task(task_num - 1)
        task.set_priority(priority)
        self.ui.show_success(f"Priority set to {priority}!")
    else:
        self.ui.show_error("Invalid priority choice!")
```

Add to menu in the `run` method:

```python
menu_options = [
    "Add a task",
    "View tasks",
    "Complete a task",
    "Delete a task",
    "Set task priority",  # NEW!
    "Quit"
]
```

### Step 6: Run Tests

```bash
pytest test_app.py -v
```

### Step 7: Commit Changes

```bash
git add app.py test_app.py
git commit -m "Add task priority feature following SOLID principles"
```

### Step 8: Push and Create Pull Request

```bash
git push -u origin feature/task-priority
```

Then create a Pull Request on GitHub, review it, and merge to `develop`!

-----

## 📊 Comparing Code: With vs Without SOLID

### ❌ WITHOUT SOLID (Bad)

```python
# Everything in one giant function - nightmare to maintain!
def do_everything():
    tasks = []
    config = load_config()  # What if we want to test without files?
    
    while True:
        print("1. Add task")
        # ... 200 lines of menu handling ...
        
        if choice == "1":
            desc = input("Task: ")
            # Validation logic mixed with business logic
            if len(desc) > 0:
                if len(tasks) < 50:
                    # Display logic mixed with business logic
                    if config['colors']:
                        print(f"\033[92m✓ Added\033[0m")
                    else:
                        print("✓ Added")
                    tasks.append({'desc': desc, 'done': False})
        # ... everything else crammed in here ...

# Try changing the display color? Good luck finding that code!
# Try testing just task addition? Can't - it's all tangled together!
# Try adding a new feature? Might break 10 other things!
```

### ✅ WITH SOLID (Good)

```python
# Clean, organized, testable, maintainable!

# Each class has ONE job
task = Task("Buy milk")
task.mark_complete()

# Easy to test
manager = TaskManager(max_tasks=10)
manager.add_task("Test task")
assert len(manager.get_all_tasks()) == 1

# Easy to change display without touching business logic
colored_display = ColoredDisplayStrategy()
plain_display = PlainDisplayStrategy()
priority_display = PriorityColoredDisplayStrategy()

# Easy to swap components
app = TaskManagerApp(
    config_manager=ConfigManager(),
    task_manager=TaskManager(),
    display=TaskDisplay(colored_display),  # Change this easily!
    ui=UserInterface()
)
```

-----

## 🎯 SOLID Principles: Real-World Benefits

### For This Project:

1. **Want to change how tasks are displayed?**

- WITHOUT SOLID: Hunt through 500 lines of code
- WITH SOLID: Create new `DisplayStrategy` class (5 minutes)

1. **Want to save tasks to a database instead of memory?**

- WITHOUT SOLID: Rewrite the entire app
- WITH SOLID: Create new `StorageStrategy` class, inject it

1. **Want to test the task logic?**

- WITHOUT SOLID: Can’t - it’s all tangled with display and input
- WITH SOLID: `manager = TaskManager(); manager.add_task("test")`

1. **Found a bug in task completion?**

- WITHOUT SOLID: Could be anywhere in 500 lines
- WITH SOLID: Check `TaskManager.complete_task()` method only

1. **Want to add a new feature (priorities)?**

- WITHOUT SOLID: Edit core logic, risk breaking everything
- WITH SOLID: Extend `Task` class, add display strategy

### For Your Career:

- **Companies LOVE developers who write SOLID code**
- **Your future self will thank you** (reading your own code in 6 months)
- **Works better in teams** (others understand your code easily)
- **Less bugs** (isolated components = isolated problems)
- **Faster development** (add features without fear)

-----

## 🔄 Complete Gitflow Workflow Summary

### Starting a New Feature

```bash
# 1. Update develop
git checkout develop
git pull origin develop

# 2. Create feature branch
git checkout -b feature/awesome-new-feature

# 3. Write tests first! (TDD)
# ... edit test_app.py ...
git add test_app.py
git commit -m "Add tests for awesome new feature"

# 4. Implement feature
# ... edit app.py ...
git add app.py
git commit -m "Implement awesome new feature"

# 5. Run tests
pytest test_app.py -v

# 6. Push and create PR
git push -u origin feature/awesome-new-feature

# 7. Merge via GitHub PR
# 8. Clean up
git checkout develop
git pull origin develop
git branch -d feature/awesome-new-feature
```

-----

## 💡 Quick Reference

### SOLID Principles

|Principle                |Simple Rule           |Example                                                     |
|-------------------------|----------------------|------------------------------------------------------------|
|**S**ingle Responsibility|One class, one job    |`Task` manages task data, not display                       |
|**O**pen/Closed          |Extend, don’t modify  |Add `PriorityDisplayStrategy` without changing existing code|
|**L**iskov Substitution  |Swappable parts       |Any `DisplayStrategy` works in `TaskDisplay`                |
|**I**nterface Segregation|Small interfaces      |`UserInterface` only has input/output methods               |
|**D**ependency Inversion |Depend on abstractions|`TaskDisplay` uses `DisplayStrategy`, not specific class    |

### Git Commands

```bash
git status                    # Check what's changed
git add file                  # Stage file
git commit -m "message"       # Save snapshot
git push origin branch        # Upload to GitHub
git pull origin branch        # Download from GitHub
git checkout -b branch        # Create & switch to branch
git merge branch              # Merge branch into current
git log --oneline             # See history
```

### Running Your App

```bash
pip install -r requirements.txt    # Install dependencies
pytest test_app.py -v              # Run tests
python app.py                      # Run application
```

-----

## 🎉 Congratulations!

You’ve learned:

- ✅ Python programming fundamentals
- ✅ Object-Oriented Programming (OOP)
- ✅ Test-Driven Development (TDD)
- ✅ SOLID Principles (professional code quality!)
- ✅ Git & GitHub (version control)
- ✅ Gitflow (professional workflow)
- ✅ Project organization & best practices

**You’re not just coding - you’re engineering software like a professional!** 🚀

These principles apply to ANY programming language and ANY project. You now have the foundation that senior developers use every day.

**Next steps:**

1. Practice by adding more features
1. Refactor existing code to follow SOLID
1. Build a new project from scratch using these principles
1. Read other people’s code and spot SOLID violations
1. Keep learning - you’re on the right path! 🌟​​​​​​​​​​​​​​​​

# 100 - 5 Layers of Learning Python

See [README.md](./100/README.md)
