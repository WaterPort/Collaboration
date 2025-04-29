# WortPort General Programming Collaboration Guidelines v1.1

[![English](https://img.shields.io/badge/English-README-blue?style=for-the-badge)](README.md)
[![中文](https://img.shields.io/badge/中文-README--CN-red?style=for-the-badge)](README-CN.md)

This is a guideline on how to collaborate on writing code, designed to help developers in the WaterPort studio better collaborate and produce high-quality code.

---

## Table of Contents

- [1. Code Style](#1-code-style)
  - [1.1. Internal Functions/Classes](#11-internal-functionsclasses)
  - [1.2. Class Naming Conventions](#12-class-naming-conventions)
  - [1.3. Function Naming Conventions](#13-function-naming-conventions)
  - [1.4. Variable Naming Conventions](#14-variable-naming-conventions)
  - [1.5. Other Comments](#15-other-comments)
  - [1.6. Instantiating Objects](#16-instantiating-objects)
  - [1.7. Function/Variable Naming Suggestions](#17-functionvariable-naming-suggestions)
  - [1.8. Encapsulating Code in Classes and Functions](#18-encapsulating-code-in-classes-and-functions)
- [2. Code Management](#2-code-management)
  - [2.1. Storage and Sharing](#21-storage-and-sharing)
  - [2.2. File Directory Structure](#22-file-directory-structure)
  - [2.3. File Naming](#23-file-naming)
  - [2.4. Actions](#24-actions)
  - [2.5. main.py](#25-main-py)
- [3. Github Guidelines](#3-github-guidelines)
  - [3.1. Commit Guidelines](#31-commit-guidelines)
  - [3.2. Branch Management](#32-branch-management)
  - [3.3. Source Code Commit](#33-source-code-commit)
  - [3.4. Pull Request](#34-pull-request)
  - [3.5. Issue](#35-issue)
- [4. Collaboration Methods](#4-collaboration-methods)
  - [4.1. Scope](#41-scope)
  - [4.2. Integration Guidelines](#42-integration-guidelines)

---

## 1. Code Style

### 1.1. Internal Functions/Classes
For internal functions (functions that other members do not need to know about, and are used only within the class), use an underscore prefix, such as `_MyClass` or `_MyFunction`. Public functions that need to be understood by other developers should follow the general naming conventions (see 1.2, 1.3).

### 1.2. Class Naming Conventions
#### Class Naming Method:
- All class names must follow Pascal Case naming conventions, with the first letter capitalized, such as `MyClass`, `MyFunction`.
For example:
```python
# Pascal Case Naming Convention
class MyClass:
    pass
```
Instead of:
```python
# snake_case Naming Convention
class my_class:
    pass
```

- For classes that do not need to be accessed by other members, add an underscore before the class name, such as `_MyClass`, `_MyFunction`. This avoids confusion with public members and signifies that the class or function is for internal use. This is a common convention to indicate that a class or function is "internal," meaning it should not be accessed externally. Although Python does not enforce access restrictions with a single underscore, it is commonly used in many codebases.
For example:
```python
# Pascal Case Naming Convention
class _MyClass:
    pass
```

- Class names should not exceed a certain length. If necessary, abbreviate or split the class into multiple parts.
For example, for this case:
```python
class _DatabaseaManager_Account:
    pass
```
It can be changed to:
```python
class _DBManager:
    class Account: # For internal subclasses, the underscore prefix is not necessary
        pass
```

- Further rules will be added as needed.

### 1.3. Function Naming Conventions
#### Function Naming Method:
Generally, function naming follows the same rules as class naming, but with the following differences:
- The first letter should be lowercase.
- Function names can use either Pascal Case or snake_case, such as `MyFunction` or `my_function`.
```python
def database_account_add():
    pass
def databaseAccountAdd():
    pass
```
However, this style tends to look unappealing and reduces code readability.
Thus, it is recommended to write like:
```python
def DB_addAccount():
    pass
def add_user():
    pass
# Or add a parent class
class _DatabaseManager:
    def add_Account():
        pass
```
The most recommended approach:
```python
# file path: Project/common/addAccount.py
class AccountAdder:
    def __init__(self, connection: MySQLConnection, username: str, password: str) -> None:
        ...
    def add_Account(self) -> bool:
        ...

# External file usage:
from Project.common.addAccount import AccountAdder

result = AccountAdder(
    connection=connection,
    username=username,
    password=password
).add_Account()
```

- Functions exposed to other members must include a detailed string comment, including: function description, usage, return values, Args, etc.
```python
def screen_rect(self, screen, x: int, y: int, ..., RGB: tuple=(0,0,0)) -> None:
'''
This function draws a rectangle on the pygame screen object, but does not refresh the display.
It allows you to quickly and easily draw rectangles without manually calling pygame.draw.rect().
No return value.

Example:
- screen_rect(screen, x, y, ...)

Args:
- screen: pygame screen object
- x: x-coordinate of the rectangle's top-left corner
- y: y-coordinate of the rectangle's top-left corner
- ... # Other parameters, like width, height, RGB, etc., should be completed in actual use
- RGB: Rectangle color, default is black
'''
    ... # Function implementation
```
You can write the code first and then add comments, but always remember to add the comments.

- Function definitions must include parameter and return type annotations, such as `def my_function(my_parameter: int) -> None:`.
You can also use libraries to define types:
```python
from mysql.connector import MySQLConnection
def my_function(Args: any) -> MySQLConnection.cursor:
    ...
```

- Function names should not exceed 40 characters, and should be as concise as possible.

### 1.4. Variable Naming Conventions
Variable naming conventions are similar to function naming, but when the following situations occur:
- A variable is assigned to another variable.
- A variable is assigned a value from a function/class without a clearly defined return type.
If the type of the assigned variable is a common type (such as int, str, list, dict, etc.), it should be annotated, for example:
```python
var1: int = a_number
var2: str = a_fun_return_str()
```

### 1.5. Other Comments
Generally, in source files like `main.py`, the referenced parts are less frequent or even absent, so comments in these files should provide an overview and clearly express the core logic of the program.

### 1.6. Instantiating Objects
For classes/functions that are frequently called and do not need to be changed, instantiate objects to optimize performance. For example:

Class object:
```python
class MyClass(FatherClass):
    def __init__(self, arg1, arg2) -> None:
        self.arg1 = arg1
        self.arg2 = arg2
    def object_A(self) -> type:
        ...
        return result
```
Before optimization:
```python
for arg1, arg2 in alist:
    MyClass(arg1, arg2).object_A().doSomething()
```
After optimization:
```python
objectA_instance = MyClass(arg1, arg2).object_A()
for arg1, arg2 in alist:
    objectA_instance.doSomething()
```

### 1.7. Function/Variable Naming Suggestions
For functions and variables, the following naming methods are recommended:
- Single-word variables should be lowercase or abbreviated (e.g., `number`, `num`).
- For functions/variables composed of 2-3 words, use Pascal case, such as `MyFunction`, `MyFirstVariable`.
- For repeated naming like object + process, use snake_case, such as `screen_draw`, `screen_delete`.
- For instance variables (bound to the object instance, like `self`) or global variables, avoid using overly simple or generic names like `cat`, `num`, `manager`, as they can be unclear, especially for external functions/variables.

### 1.8. Encapsulating Code in Classes and Functions
**Never put this kind of chaotic, unorganized code in the WaterPort library :)**
```python
import math, pygame, random, time
import sys, os, json, shutil, datetime

x = 0
y = 1
z = 2
a = 3
b = 4
c = 5

if x == 0:
    if y == 1:
        while z < 10:
            for i in range(5):
                if a == 3:
                    if b == 4:
                        while c < 7:
                            for j in range(3):
                                if x == 0:
                                    if y == 1:
                                        while z < 8:
                                            for k in range(4):
                                                if a == 3:
                                                    if b == 4:
                                                        while c < 6:
                                                            for l in range(2):
                                                                if x == 0:
                                                                    if y == 1:
                                                                        while z < 9:
                                                                            for m in range(3):
                                                                                if a == 3:
                                                                                    if b == 4:
                                                                                        while c < 5:
                                                                                            print("Deep Loop!")
                                                                                            c += 1
                                                                                        c -= 1
                                                                                    
                                                c += 1
                                            z += 1
                                        z -= 1
                                    y += 1
                                a += 1
                            z -= 1
                        b += 1
                    a -= 1
                y -= 1
            z -= 1
        x += 1
```
**If you see this chaotic, unstructured code without using classes or functions, you should address it immediately:)**
__Except for flask__.

For code that needs to be reused multiple times, encapsulate it into classes or functions, such as:
```python
class DatabaseManager:
    def __init__(self, connection: MySQLConnection) -> None:
        self.connection = connection
    def addAccount(self, username: str, password: str) -> bool:
        ...
    def checkAccount(self, username: str, password: str) -> bool:
        ...
```
This way, in other files, you can directly call its methods without repeating the same code. The same applies to functions exposed to other members.

---

## 2. Code Management
### 2.1. Storage and Sharing
All code must be stored in a Github repository for team members to share and collaborate.

### 2.2. File Directory Structure
All files should follow the directory structure method as much as possible:

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
├─ README.md             # Project description document
├─ icon.ico              # Icon file
├─ LICENSE               # License
├─ requirements.txt      # Dependencies file
├─ setup.py              # Project installation script
└─ main.py               # Main program entry
```

Before running the project, first run `pip install -r requirements.txt` to install dependencies,
and then `pip install -e .` to install the project.

### 2.3. File Naming
All file names should follow Pascal Case naming conventions, such as `MyClass`, `MyFunction`.

### 2.4. Actions
All operation tests must be uploaded to Github, and an automatic email should notify other members. (In the future, we will add features such as WeChat, SMS, and QQ notifications.)

### 2.5. main.py
The content of the `main.py` file should be kept as simple as possible, containing only startup and initialization code for easy future expansion, maintenance, and code management.
For example, a program with both UI and backend (very simple case) can import `index.py` into `main.py`, and import some modules from `backend.py` into `index.py` (to avoid circular imports):
```
src
├─ main.py               # Main program entry
├─ index.py              # UI part
├─ backend.py            # Backend part
└─ common                # Common modules
```

## 3. Github Guidelines
### 3.1. Commit Guidelines
After local testing passes, upload the code to the Github repository, create a new branch, and submit the code with detailed commit messages for easy future tracking. After automatic testing via Github Actions, you can ensure double safety.

### 3.2. Branch Management
When committing code to the Github repository, create a new branch, such as `feature-xxx`, `bugfix-xxx`, `hotfix-xxx`, etc., to facilitate team collaboration and code management.
Once the code is confirmed to be correct, merge the branch into the main branch `main`.
Tips: Currently, there is no strict requirement for branch management because I am too lazy to do it lol:)

### 3.3. Source Code Commit
For changes to the `src` directory, include a brief description of the changes in the external interface in the commit message. Commits without descriptions will be considered internal modifications or changes that do not need to be understood by others.

### 3.4. Pull Request
If a new branch is created, before merging into the main branch, create a Pull Request for other members to review the code and ensure its quality.

### 3.5. Issue
For external bugs, suggestions, requests, etc., create an Issue for team discussion and resolution.

---

## 4. Collaboration Methods
### 4.1. Scope
In the `README.md` file in the project root directory, list each person's basic work scope, such as:
```markdown
# Project Member Responsibilities
## Nabil
- Main UI (Main screen, login screen, level selection screen, settings screen, etc.)
## Vue
- Game content UI and generation algorithm
- Network backend and maintenance
- Some utils
```
Responsibility areas are not mandatory but are meant to help team members understand each other's work to avoid redundant work and logical conflicts. Areas not considered or new areas can be discussed and assigned together.

### 4.2. Integration Guidelines
To facilitate code integration between developers, create an integration document in the project's `doc` directory to detail the interfaces and calling methods of each module. The document should include external interfaces for each part/module and the main program logic, like:
```
doc
├─ main.md
├─ backend.md
├─ utils.md
├─ server.md
└─ utils.md
```
All should be in markdown format, which can be viewed directly on Github. (Shouldn't be too hard to write markdown, right?)
