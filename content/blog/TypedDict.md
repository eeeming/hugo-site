---
date: '2025-07-10T10:52:54+08:00'
draft: true
title: 'TypedDict'
math: true
---

在 langGraph 中，经常可以见到 `TypedDict` 的使用。它是 Python 3.8 引入的一个特性，允许我们定义具有特定键和类型的字典。`TypedDict` 可以帮助我们在处理字典时提供更好的类型检查和代码可读性。

`TypedDict` 是 `typing` 模块的一个类，所以**主要的目的是进行静态检查，而不是进行字典和类之间的转换**。

所以在函数参数使用 `TypedDict` 时，在**运行时依然是传递一个字典对象**，而不是一个类实例。静态检查器1会检查传入的字典是否符合 `TypedDict` 的定义。

如果需要使用字典和类之间在运行时的转换，可以使用 `Pydantic`。

### 使用案例

```python
from typing import TypedDict

class Point(TypedDict):
    x: int
    y: int

p: Point = {"x": 1, "y": 2}

# 在运行时检查类型
print(type(p))
# 输出: <class 'dict'>  <-- 看这里，它就是一个普通的字典

# 你仍然需要使用字典的方式访问数据
print(p['x']) 
# 输出: 1

# 你不能使用属性（attribute）的方式访问，因为它不是一个类实例
# 下面这行代码会直接引发 AttributeError
# print(p.x)
```

### 特性

可以利用 `total` 和 `typing.NotRequired` 来定义可选字段：

```python
from typing import TypedDict, NotRequired
# 方法一: 所有键都是可选的
class UserPreferences(TypedDict, total=False):
    theme: str
    language: str

config1: UserPreferences = {}  # OK
config2: UserPreferences = {"theme": "dark"} # OK

# 方法二: 精确指定可选键 (更推荐)
class UserAccount(TypedDict):
    username: str
    email: NotRequired[str] # email 是可选的

user1: UserAccount = {"username": "admin"} # OK
user2: UserAccount = {"username": "guest", "email": "guest@example.com"} # OK
```
