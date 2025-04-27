# WortPort General Programming Collaboration Guidelines v1.0

[English](README.md) | [中文](README-CN.md)

This is a set of guidelines for collaborating on code, aimed at helping developers in the WaterPort studio work together more effectively and write high-quality code.

---

## Table of Contents

- [1. Code Style](#1-code-style)
  - [1.1. Internal Functions/Classes](#11-internal-functionsclasses)
  - [1.2. Class Naming Conventions](#12-class-naming-conventions)
  - [1.3. Function Guidelines](#13-function-guidelines)
  - [1.4. Instantiating Classes](#14-instantiating-classes)
  - [1.5. Other Comments](#15-other-comments)
- [2. Code Management](#2-code-management)
  - [2.1. Storage & Sharing](#21-storage-sharing)
  - [2.2. File Directory Structure](#22-file-directory-structure)
  - [2.3. File Naming](#23-file-naming)
  - [2.4. Actions](#24-actions)

---

## 1. Code Style

### 1.1. Internal Functions/Classes
For internal functions (functions that are only used within the class and are not meant to be accessed by other members), use a `_` prefix, such as `_MyClass` or `_MyFunction`. Public functions that need to be accessed by other members should follow the general naming conventions (see 1.2, 1.3).

### 1.2. Class Naming Conventions
#### Class Naming Method:
- All class names must follow Pascal Case naming convention, i.e., capitalizing the first letter of each word, such as `MyClass`, `MyFunction`.
Example:
```python
# Pascal Case naming convention
class MyClass:
    pass
```
Not:
```python
# snake_case naming convention
class my_class:
    pass
```

- For classes that do not need to be accessed or known by other members, prefix the class name with `_`, such as `_MyClass` or `_MyFunction`.
This helps avoid confusion with public members and indicates that the class or function is intended for internal use only. This is a common convention used to signal that a class or function is "internal", i.e., not to be accessed directly by external code. Although Python’s single underscore does not enforce access restriction, it serves as a convention (not actual encapsulation).
Example:
```python
# Pascal Case naming convention
class _MyClass:
    pass
```

- Class names should not exceed a certain length. If necessary, abbreviations or nested classes can be used.
For instance, the following:
```python
class _DatabaseaManager_Account:
    pass
```
Can be refactored as:
```python
class _DBManager:
    class Account: # Subclasses or internal classes may not need a leading underscore.
        pass
```
- Additional conventions will be added in the future.

### 1.3. Function Guidelines
#### Function Naming Method:
Function names should generally follow the same conventions as class names, with the following differences:
- Function names should start with a lowercase letter.
- Functions can use either Pascal Case or snake_case naming conventions, such as `MyFunction` or `my_function`.
```python
def database_account_add():
    pass
def databaseAccountAdd():
    pass
```
However, this can make the code harder to read, so it's not recommended. A better approach would be:
```python
def DB_addAccount():
    pass
def add_user():
    pass
# Or add a parent class
class _DatabaseManager:
    def addAccount():
        pass
```
The most recommended approach:
```python
# file path: Project/common/addAccount.py
class AccountAdder:
    def __init__(self, connection: MySQLConnection, username: str, password: str) -> None:
        ...
    def addAccount(self) -> bool:
        ...

# External file call:
from Project.common.addAccount import AccountAdder

result = AccountAdder(
    connection=connection,
    username=username,
    password=password
).addAccount()
```

- Functions intended for use by other members (public functions) must have detailed string comments, including:
  - Description of functionality
  - Usage instructions
  - Return values
  - Args, etc.
```python
def screen_rect(self, screen, x: int, y: int, ..., RGB: tuple=(0,0,0)) -> None:
'''
This function draws a rectangle on a pygame screen object (Surface), excluding display refresh.
It simplifies rectangle drawing in pygame, without needing to manually call pygame.draw.rect().
No return value.

Example:
- screen_rect(screen, x, y, ...)

Args:
- screen: The pygame screen object
- x: The x-coordinate of the rectangle’s top-left corner
- y: The y-coordinate of the rectangle’s top-left corner
- ... # Additional parameters such as width, height, RGB, etc.
- RGB: The rectangle's color, default is black
'''
    ... # Function implementation
```
You may write the code first and then add comments later, but always remember to add comments.

- Function definitions must include parameter types and return types, such as `def my_function(my_parameter: int) -> None:`.
You can also define types using libraries:
```python
from mysql.connector import MySQLConnection
def my_function(Args: any) -> MySQLConnection.cursor:
    ...
```

- Function names should not exceed 40 characters, and should be kept as concise as possible.

### 1.4. Instantiating Classes
For classes or functions that are frequently called and not modified, instantiate them for better performance optimization.
Example:

Class object:
```python
class MyClass(FatherClass):
    def __init__(self, arg1, arg2) -> None:
        self.arg1 = arg1
        self.arg2 = arg2
    def objectA(self) -> type:
        ...
        return result
```

Before optimization:
```python
for arg1, arg2 in alist:
    # Every time this class is called, a new instance is created
    MyClass(arg1, arg2).objectA().doSomething()
```

After optimization:
```python
objectA_instance = MyClass(arg1, arg2).objectA()
for arg1, arg2 in alist:
    objectA_instance.doSomething()
```

### 1.5. Other Comments
For files like `main.py`, which are not referenced often and may have fewer imports, comments should provide an overall summary and clearly express the core logic of the program.

## 2. Code Management

### 2.1. Storage & Sharing
All code must be stored in a GitHub repository for sharing and collaboration among team members.

### 2.2. File Directory Structure
All files should be stored following the directory structure method shown below:

```
Project
├─ .env                  # Environment configuration file
├─ assets                # Static resources
│  ├─ audios             # Audio files
│  │  └─ audios.wav
│  ├─ fonts              # Font files
│  │  └─ fonts.ttf
│  └─ images             # Image resources
│     └─ ico
│        └─ icon.jpg
├─ config                # Configuration files
│  └─ config.ini
├─ docs                  # Project documentation
│  └─ How_to_use.txt
├─ src                   # Main application code
│  ├─ __init__.py        # Initialization file
│  ├─ common             # Common modules
│  │  └─ data.py
│  ├─ controller         # Controller modules
│  │  └─ flask.py
│  ├─ server             # Server-related modules
│  │  ├─ app.py
│  │  └─ model           # Model modules
│  │     ├─ addAccount.py
│  │     └─ checkAccount.py
│  ├─ examples           # Example code
│  │  └─ simple_example.py
│  ├─ spawn.py           # Startup/initialization script
│  ├─ utils              # Utility modules
│  │  ├─ realtime.py
│  │  ├─ tool.py
│  │  └─ __init__.py
├─ tests                 # Test files
│  └─ test.py
├─ versions              # Version management
│  └─ v1.0
│     └─ ...
├─ scripts               # Startup scripts
│  ├─ start.bat
│  └─ start.sh
├─ README.md             # Project README
├─ icon.ico              # Icon file
├─ LICENSE               # License file
├─ requirements.txt      # Dependency file
├─ setup.py              # Installation script
└─ main.py               # Main program entry
```

Before running and debugging, make sure to first run `pip install -r requirements.txt` to install dependencies, and `pip install -e .` to install the project.

### 2.3. File Naming
All files should use Pascal Case naming convention, such as `MyClass`, `MyFunction`.

### 2.4. Actions
All actions after testing should be uploaded to GitHub, and an automatic email notification should be sent to other members (future plans include notifications via WeChat, SMS, QQ, etc.).