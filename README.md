# WortPort General Programming Collaboration Specification v1.1

[English](README.md) | [中文](README-CN.md)

This document outlines coding collaboration guidelines to help WaterPor Studio developers work together efficiently and produce high-quality code.

---

## Table of Contents

- [1. Code Style](#1-code-style)
  - [1.1. Internal Functions/Classes](#11-internal-functionsclasses)
  - [1.2. Class Naming Conventions](#12-class-naming-conventions)
  - [1.3. Function Standards](#13-function-standards)
  - [1.4. Variable Naming Conventions](#14-variable-naming-conventions)
  - [1.5. Other Comments](#15-other-comments)
  - [1.6. Creating Instances](#16-creating-instances)
  - [1.7. Function/Variable Naming Suggestions](#17-functionvariable-naming-suggestions)
  - [1.8. Encapsulating Code with Classes/Functions](#18-encapsulating-code-with-classesfunctions)
- [2. Code Management](#2-code-management)
  - [2.1. Storage and Sharing](#21-storage-and-sharing)
  - [2.2. Directory Structure](#22-directory-structure)
  - [2.3. File Naming](#23-file-naming)
  - [2.4. Actions](#24-actions)
  - [2.5. main.py](#25-mainpy)
- [3. GitHub Standards](#3-github-standards)
  - [3.1. Commit Standards](#31-commit-standards)
  - [3.2. Branch Management](#32-branch-management)
  - [3.3. Source Code Submissions](#33-source-code-submissions)
  - [3.4. Pull Requests](#34-pull-requests)
  - [3.5. Issues](#35-issues)
- [4. Collaboration Methods](#4-collaboration-methods)
  - [4.1. Scope](#41-scope)
  - [4.2. Integration Guidelines](#42-integration-guidelines)

---

## 1. Code Style

### 1.1. Internal Functions/Classes
- Prefix internal functions/classes (meant only for internal use within a class) with `_`, e.g., `_MyClass` or `_MyFunction`.  
- Public functions/classes (needed by other developers) follow standard naming conventions (see [1.2](#12-class-naming-conventions), [1.3](#13-function-standards)).

### 1.2. Class Naming Conventions
- Use **PascalCase** (e.g., `MyClass`, `MyFunction`).  
  ```python
  # Correct
  class MyClass:
      pass

  # Incorrect (snake_case)
  class my_class:
      pass
  ```
- For internal classes, prefix with `_`:  
  ```python
  class _MyClass:
      pass
  ```
- Avoid excessively long names (>40 chars). Split or abbreviate if needed:  
  ```python
  # Bad
  class _DatabaseManager_Account:
      pass

  # Good
  class _DBManager:
      class Account:  # Subclasses need no underscore
          pass
  ```

### 1.3. Function Standards
- **Naming**:  
  - Use **snake_case** (e.g., `add_account`) or **camelCase** (e.g., `addAccount`).  
  - Avoid ambiguous names like `databaseAccountAdd`; prefer:  
    ```python
    def db_add_account():
        pass
    ```
  - For reusable logic, encapsulate in classes:  
    ```python
    class AccountAdder:
        def __init__(self, connection: MySQLConnection, ...) -> None:
            ...
        def add_account(self) -> bool:
            ...
    ```
- **Documentation**:  
  - Public functions **must** include docstrings:  
    ```python
    def draw_rect(screen, x: int, y: int, rgb: tuple = (0, 0, 0)) -> None:
        """
        Draws a rectangle on a Pygame screen without refreshing the display.

        Args:
            screen: Pygame screen object.
            x (int): Top-left x-coordinate.
            y (int): Top-left y-coordinate.
            rgb (tuple): RGB color (default: black).
        """
    ```
- **Type Hints**: Always annotate parameters and return types:  
  ```python
  def get_user(id: int) -> User | None:
      ...
  ```

### 1.4. Variable Naming Conventions
- Use **snake_case** for variables (e.g., `user_count`).  
- Annotate types when assigning from dynamic sources:  
  ```python
  count: int = get_total()
  name: str = "Alice"
  ```

### 1.5. Other Comments
- In `main.py` or similar entry files, use high-level comments to explain core logic.

### 1.6. Creating Instances
- Cache frequently used class instances to optimize performance:  
  ```python
  # Bad: Creates new instance per loop
  for x, y in data:
      MyClass(x, y).do_something()

  # Good: Reuse instance
  processor = MyClass(x, y)
  for x, y in data:
      processor.do_something()
  ```

### 1.7. Function/Variable Naming Suggestions
- Single-word names: lowercase (`count`, `num`).  
- Multi-word names:  
  - **PascalCase** for 2–3 words (e.g., `CalculateScore`).  
  - **snake_case** for action-based names (e.g., `draw_screen`).  
- Avoid generic names like `manager` or `temp`.

### 1.8. Encapsulating Code with Classes/Functions
- **Never** write spaghetti code like this:  
  ```python
  # BAD: Unstructured nested loops/conditions
  if x == 0:
      while y < 10:
          for i in range(5):
              ...
  ```
- **Do** encapsulate logic:  
  ```python
  class DatabaseManager:
      def __init__(self, connection):
          self.connection = connection
      def add_user(self, username: str) -> bool:
          ...
  ```

---

## 2. Code Management

### 2.1. Storage and Sharing
- All code must be stored in **GitHub** for team access.

### 2.2. Directory Structure
```
Project/
├─ .env                  # Environment config
├─ assets/               # Static files (images, fonts, etc.)
├─ src/                  # Main code
│  ├─ common/            # Shared modules
│  ├─ server/            # Backend logic
│  └─ ...
├─ tests/                # Unit tests
└─ requirements.txt      # Dependencies
```

### 2.3. File Naming
- Use **PascalCase** for filenames (e.g., `AccountManager.py`).

### 2.4. Actions
- All CI/CD workflows (e.g., testing) must run via GitHub Actions.

### 2.5. main.py
- Keep `main.py` minimal (initialization only). Delegate logic to other modules.

---

## 3. GitHub Standards

### 3.1. Commit Standards
- Test locally before pushing.  
- Write descriptive commit messages.

### 3.2. Branch Management
- Prefix branches with purpose:  
  - `feature/login`  
  - `bugfix/header-align`  
- Merge to `main` after review.

### 3.3. Source Code Submissions
- Document public API changes in commit messages.

### 3.4. Pull Requests
- Require PR reviews before merging to `main`.

### 3.5. Issues
- Use GitHub Issues for bugs/feature requests.

---

## 4. Collaboration Methods

### 4.1. Scope
- Define responsibilities in `README.md`:  
  ```markdown
  ## Team Roles
  - **Alice**: UI (login, settings)
  - **Bob**: Backend APIs
  ```

### 4.2. Integration Guidelines
- Document interfaces in `docs/`:  
  ```
  docs/
  ├─ api.md              # Backend endpoints
  ├─ ui.md               # UI components
  └─ ...
  ```
