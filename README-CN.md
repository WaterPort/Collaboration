# WortPort通用编程合作规范v1.0

[English](README.md) | [中文](README-CN.md)

这是一个关于如何合作编写代码的规范，旨在帮助WaterPor工作室的开发者们更好地协作和编写高质量的代码。

---

## 目录

- [1. 代码风格](#1-代码风格)
  - [1.1. 内部函数/类](#11-内部函数/类)
  - [1.2. 类命名规范](#12-类命名规范)
  - [1.3. 函数规范](#13-函数规范)
  - [1.4. 创建实例](#14-创建实例)
  - [1.5. 其它注释](#15-其它注释)
- [2. 代码管理](#2-代码管理)
  - [2.1. 存储与共享](#21-存储与共享)
  - [2.2. 文件目录结构](#22-文件目录结构)
  - [2.3. 文件命名](#23-文件命名)
  - [2.4. Actions](#24-Actions)

---

## 1. 代码风格

### 1.1. 内部函数/类
在内部函数（其它成员不需要了解的函数，只在类内部使用）中，使用`_`开头，如`_MyClass`或`_MyFunction`。允许公共调用并且需要其它部分开发人员了解的使用一般命名规范（见1.2、1.3）。

### 1.2. 类命名规范
#### 类命名方法：
- 所以类命名必须遵循Pascal Case命名法，即首字母大写，如`MyClass`、`MyFunction`。 
例如：
```python
# Pascal Case命名法
class MyClass:
    pass
```
而不是：
```python
# snake_case命名法
class my_class:
    pass
```

- 对于其它成员不需要了解或调用的类，在类的名称前加上`_`，如`_MyClass`、`_MyFunction`。
这样可以避免与公共成员混淆，并且表示该类或函数是内部使用的。
这是一种常见的约定，用来表示类或函数是“内部的”，即不希望外部直接访问。尽管 Python 中的单个下划线并不会强制性地限制访问，只是一种约定（并非真正的封装），但它在很多代码库中都有使用。
例如：
```python
# Pascal Case命名法
class _MyClass:
    pass
```

- 类名称长度原则上不能超过字符，如果需要，可以使用缩写或者分成多个类。
例如对于这种情况：
```python
class _DatabaseaManager_Account:
    pass
```
可以改成：
```python
class _DBManager:
    class Account: # 对于内部类/函数的子类，可以不再使用下划线开头
        pass
```
- 其它规范待补充

### 1.3. 函数规范
#### 函数命名方法：
大体上与类命名方法相同，但存在以下差异：
- 首字母原则上小写。
- 所有函数命名既可以用Pascal命名法，也可以用snake_case命名法，如`MyFunction`或`my_function`。
```python
def database_account_add():
    pass
def databaseAccountAdd():
    pass
```
但这种方式显然看着很难看，代码可阅读性低。
因此建议写为：
```python
def DB_addAccount():
    pass
def add_user():
    pass
#或者添加父类
class _DatabaseManager:
    def addAccount():
        pass
```
最推荐的写法：
```python
# file path: Project/common/addAccount.py
class AccountAdder:
    def __init__(self, connection: MySQLConnection, username: str, password: str) -> None:
        ...
    def addAccount(self) -> bool:
        ...

# 外部文件调用方法：
from Project.common.addAccount import AccountAdder

result = AccountAdder(
    connection=connection,
    username=username,
    password=password
).addAccount()

```

- 开发给其它成员的函数接口（开放函数）必须使用长字符串注释说明，包括：功能说明、用法说明、返回值、Args等。
```python
def screen_rect(self, screen, x: int, y: int, ..., RGB: tuple=(0,0,0)) -> None:
'''
这个函数用于在pygame的screen对象绘制Surface对象矩形，但并不包括刷新display。
这个函数可以让你更快更便捷地在pygame中绘制矩形，而不需要手动调用pygame.draw.rect()函数。无返回值。

example:
- screen_rect(screen, x, y, ...)

Args:
- screen: pygame的screen对象
- x: 矩形左上角的x坐标
- y: 矩形左上角的y坐标
- ... # 其它参数，如width、height、RGB等，实际使用中需要补充完整
- RGB: 矩形的颜色，默认为黑色
'''
    ... # 函数实现

```
你也完全可以先写完代码再补充注释，但一定要记得补充注释。

- 函数定义处必须标注函数参数类型与返回值类型，如`def my_function(my_parameter: int) -> None:`。
也可以利用库来定义类型：
```python
from mysql.connector import MySQLConnection
def my_function(Args: any) -> MySQLConnection.cursor:
    ...

```

- 函数命名不可超过40个字符，尽量简短

### 1.4. 创建实例
无需更改且调用频繁的类/函数，通过创建实例来引用以此优化性能，例：

类对象
```python
class MyClass(FatherClass):
    def __init__(self, arg1, arg2) -> None:
        # 一般来说__init__()都不可能会返回值，但为了统一美观，还是在其后加上了"-> None"
        self.arg1 = arg1
        self.arg2 = arg2
    def objectA(self) -> type:
        ...
        return result
```
优化前：
```python
for arg1, arg2 in alist:
    # 这样以来每次调用该类都会创建一个新的实例
    MyClass(arg1, arg2).objectA().doSomething()
```
优化后：
```python
objectA_instance = MyClass(arg1, arg2).objectA()
for arg1, arg2 in alist:
    objectA_instance.doSomething()
```

### 1.5. 其它注释
通常在main.py等源文件中，被引用的部分相对较少甚至没有，因此此类源代码的注释需要概括全局，清晰地表达程序运行的核心逻辑。



## 2. 代码管理
### 2.1. 存储与共享
所有代码必须存储在Github仓库中，以供团队成员共享和协作。

### 2.2. 文件目录结构
所有文件尽量按照以下目录结构方法进行存储：

```
Project
├─ .env                  # 环境配置文件
├─ assets                # 静态资源
│  ├─ audios             # 音频文件
│  │  └─ audios.wav
│  ├─ fonts              # 字体文件
│  │  └─ fonts.ttf
│  └─ images             # 图片资源
│     └─ ico
│        └─ icon.jpg
├─ config                # 配置文件
│  └─ config.ini
├─ docs                  # 项目文档
│  └─ How_to_use.txt
├─ src                   # 主要的应用代码
│  ├─ __init__.py        # 初始化文件
│  ├─ common             # 公共模块
│  │  └─ data.py
│  ├─ controller         # 控制器模块
│  │  └─ flask.py
│  ├─ server             # 服务器相关模块
│  │  ├─ app.py
│  │  └─ model           # 模型模块
│  │     ├─ addAccount.py
│  │     └─ checkAccount.py
│  ├─ examples           # 示例代码
│  │  └─ simple_example.py
│  ├─ spawn.py           # 启动/初始化脚本
│  ├─ utils              # 工具模块
│  │  ├─ realtime.py
│  │  ├─ tool.py
│  │  └─ __init__.py
├─ tests                 # 测试文件
│  └─ test.py
├─ versions              # 版本管理
│  └─ v1.0
│     └─ ...
├─ scripts               # 启动脚本
│  ├─ start.bat
│  └─ start.sh
├─ README.md             # 项目说明文档
├─ icon.ico              # 图标文件
├─ LICENSE               # 许可证
├─ requirements.txt      # 依赖文件
├─ setup.py              # 项目安装脚本
└─ main.py               # 主程序入口
```

在运行调试项目前，需要先运行`pip install -r requirements.txt`安装依赖，
以及`pip install -e .`安装项目。


### 2.3. 文件命名
所有文件命名尽量使用Pascal Case命名法，如`MyClass`、`MyFunction`。

### 2.4. Actions
所以的操作测试后都必须上传至Github，并且自动发送邮件通知其它成员。（未来会增加微信、短信、QQ通知等功能）