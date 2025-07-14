---
date: '2025-07-02T17:06:18+08:00'
draft: false
title: 'Python 基础教程'
math: true
---

## 环境搭建

### IDE的选择

* PyCharm：受欢迎的重量级Python IDE，提供了丰富的功能和插件支持
  * 开箱即用
  * 专业版需要付费，社区版免费，目前已经合并为一个版本
* VSCode（推荐）：轻量级编辑器，支持完善的AI开发
  * 需要配置Python相关插件
  * 免费
  * 插件丰富

### Python版本

* Python 3.x：推荐使用
* Python 2.x：已不再维护，不推荐使用

### Python环境管理

由于不同项目所需的Python版本和依赖包可能不同，因此需要使用对应的工具管理Python环境。Python环境管理工具可以帮助你创建独立的环境，避免包冲突。

* virtualenv：用于创建独立的Python环境，避免包冲突
* conda/miniconda（推荐）：Anaconda的包管理工具，适用于数据科学和机器学习
* uv（推荐）：新一代Python包管理工具，支持多种语言和平台

### 切换环境

通常情况下，如果我们不指定Python版本，系统会使用默认的Python版本。我们需要在命令行中使用以下命令切换到指定的Python环境：

```bash
# virtualenv / uv
source .venv/bin/activate  # Linux/Mac
source .venv/Scripts/activate  # Windows
# conda
conda activate myenv  # 切换到名为 myenv 的环境
```

## Python 基础概念

* Python是一种解释型语言，这意味着你编写的代码会由解释器逐行执行
* Python支持面向对象（OOP）、函数式和命令式编程。
* Python是一个跨平台的语言，可以在Windows、Linux和macOS上运行。
* Python是强类型语言(Strongly Typed Language)，但是动态类型语言(Dynamically Typed Language)，变量不需要声明类型，在运行时（代码执行时）中确定具体的类型。

## 基础语法

### Python基础语法

#### 变量与数据类型

```python
x = 10  # 整数 (int)
pi = 3.14  # 浮点数 (float)
name = "Alice"  # 字符串 (str)
is_student = True  # 布尔值 (bool)
```

* **常用数据类型**：
  * **整数 (int)**: 正整数、负整数或零。
  * **浮点数 (float)**: 带有小数点的数字。
  * **字符串 (str)**: 由单引号 `' '` 或双引号 `" "` 括起来的文本。
  * **布尔值 (bool)**: 只有 `True` 和 `False` 两个值，常用于逻辑判断。

> 在 C++/Java 中，单引号代表是`char`类型，而在Python中，单引号和双引号都代表**字符串**类型。

#### 运算符

Python支持多种运算符，用于执行各种计算和操作。

* **算术运算符**: `+` (加), `-` (减), `*` (乘), `/` (除), `%` (取模), `**` (幂), `//` (整除)。

```python
print(10 + 3)  # 输出: 13
print(10 / 3)  # 输出: 3.3333333333333335
print(10 // 3) # 输出: 3
print(10 % 3)  # 输出: 1
print(2 ** 3)  # 输出: 8
```

* **比较运算符**: `==` (等于), `!=` (不等于), `>` (大于), `<` (小于), `>=` (大于等于), `<=` (小于等于)。比较结果为布尔值。

```python
print(5 == 5) # 输出: True
print(5 > 3)  # 输出: True
```

* **逻辑运算符**: `and` (与), `or` (或), `not` (非)。

```python
print(True and False) # 输出: False
print(True or False)  # 输出: True
print(not True)       # 输出: False
```

#### 控制流

控制流语句可以决定代码的执行顺序。

* **条件语句 (if-elif-else)**: 根据条件的真假来执行不同的代码块。

```python
score = 85
if score >= 90:
    print("优秀")
elif score >= 80:
    print("良好")
else:
    print("合格")
```

* **循环语句 (for 和 while)**: 用于重复执行一段代码。

  * **for循环**：通常用于遍历一个序列（如列表、字符串）。

    ```python
    for fruit in ["apple", "banana", "cherry"]:
        print(fruit)
    ```

  * **while循环**：只要条件为真，就会一直执行。

    ```python
    count = 0
    while count < 3:
        print("Hello")
        count += 1
    ```

### 小技巧

#### 使用 `f-string` 格式化字符串

Python 和 Java 一样可以使用 `+` 来连接字符串，但更推荐使用格式化字符串。

```python
"Hello, my name is " + name + " and I am " + str(age) + " years old."
```

Python 3.6及以上版本支持使用 `f-string` 来格式化字符串，这是一种更简洁和高效的方式。

```python
name = "Alice"
age = 30
# 使用 f-string 格式化字符串
greeting = f"Hello, my name is {name} and I am {age} years old."
```

#### for(int i = 0; i < 10; i++) 的Python写法

在Python中，`for` 循环的语法更简洁，不需要指定初始值、条件和增量。可以使用 `range()` 函数来生成一个数字序列。

```python
# 等价于 C/C++ 的 for(int i = 0; i < 10; i++)
for i in range(10):
    print(i)
```

## Python数据结构

Python内置了非常强大的数据结构

### 1. 列表 (List)

列表（List）是Python中使用最频繁的数据结构之一。它是一个**有序**且**可变**的集合，意味着你可以修改它的内容，比如添加、删除或更改元素。

#### 创建与基础操作

与JAVA不同，Python对于数据结构的创建不需要显式导入包

```python
# 创建一个包含不同数据类型的列表
my_list = [1, "hello", 3.14, True]
print(f"初始列表: {my_list}")

# 1. 访问元素 (通过索引)
# 索引从 0 开始
print(f"访问第二个元素: {my_list[1]}")  # 输出: hello

# 2. 修改元素
my_list[0] = 99
print(f"修改第一个元素后: {my_list}") # 输出: [99, 'hello', 3.14, True]

# 3. 添加元素到末尾 (append)
my_list.append("new_item")
print(f"append添加元素后: {my_list}") # 输出: [99, 'hello', 3.14, True, 'new_item']
```

---

#### pop() - 弹出元素

`pop()` 方法用于移除列表中的一个元素（默认是最后一个），并且会**返回**被移除的元素值。

**结合 `append()` 方法，我们可以把列表当做栈来使用。**

```python
# 我们继续使用上面的 my_list: [99, 'hello', 3.14, True, 'new_item']
# 不指定索引，默认弹出最后一个元素
popped_item = my_list.pop()
print(f"被pop出的元素是: '{popped_item}'") # 输出: 'new_item'
print(f"pop之后，列表变为: {my_list}") # 输出: [99, 'hello', 3.14, True]

# 指定索引，弹出指定位置的元素
# 比如，我们想弹出 "hello" (它的索引是 1)
popped_item_at_index_1 = my_list.pop(1)
print(f"从索引1 pop出的元素是: '{popped_item_at_index_1}'") # 输出: 'hello'
print(f"按索引pop后，列表变为: {my_list}") # 输出: [99, 3.14, True]
```

#### remove() - 按值删除

如果你不知道元素的位置，但知道它的值，可以使用 `remove()` 方法。它会移除**第一个**匹配到的元素。

```python
my_list.remove(3.14)
print(f"remove(3.14)后: {my_list}") # 输出: [99, True]
```

---

#### 切片 (Slicing) - 获取子列表

切片是Python中非常强大的一种操作，可以让你轻松地获取列表的一部分。它的语法是 `my_list[start:stop:step]`。

* `start`: 切片开始的索引（包含该位置）。如果留空，默认为 0。
* `stop`: 切片结束的索引（**不包含**该位置）。如果留空，默认为列表末尾。
* `step`: 步长，即每隔多少个元素取一个。如果留空，默认为 1。

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(f"\n用于切片的列表: {numbers}")

# 获取从索引2到索引5的元素 (不包含索引5)
sub_list = numbers[2:5]
print(f"numbers[2:5] -> {sub_list}") # 输出: [2, 3, 4]

# 从开头切到索引4 (不包含索引4)
first_part = numbers[:4]
print(f"numbers[:4] -> {first_part}") # 输出: [0, 1, 2, 3]

# 从索引6切到末尾
last_part = numbers[6:]
print(f"numbers[6:] -> {last_part}") # 输出: [6, 7, 8, 9]

# 使用步长，每隔一个元素取一个
every_other = numbers[::2]
print(f"numbers[::2] -> {every_other}") # 输出: [0, 2, 4, 6, 8]

# 负数索引：-1 表示最后一个元素
last_element = numbers[-1]
print(f"numbers[-1] -> {last_element}") # 输出: 9

# 技巧：使用切片来反转列表
reversed_list = numbers[::-1]
print(f"numbers[::-1] -> {reversed_list}") # 输出: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# 技巧：使用切片复制整个列表
list_copy = numbers[:]
print(f"列表的副本: {list_copy}")
```

---

#### insert() - 在指定位置插入元素

`insert()` 方法可以在你指定的任何位置插入一个元素。

```python
my_list = [99, True]
# 在索引 1 的位置插入 "world"
my_list.insert(1, "world")
print(f"\ninsert之后: {my_list}") # 输出: [99, 'world', True]
```

#### index() - 查找元素索引

`index()` 方法返回指定值在列表中第一次出现的索引。

```python
# 查找 'world' 的索引
world_index = my_list.index("world")
print(f"'world' 的索引是: {world_index}") # 输出: 1
```

#### sort() - 就地排序

`sort()` 方法会**就地**对列表进行排序（即直接修改原列表）。

```python
# 错误示范！不要这样做！
num_list = [34, 1, 19]
sorted_list = num_list.sort() # sort() 返回 None
print(f"错误的接收方式，得到: {sorted_list}") # 输出: None
print(f"原始列表已被修改: {num_list}")     # 输出: [1, 19, 34]
```

#### sorted() - 返回新排序的列表

> 注意，sort和sorted要求列表中的元素类型是可比较的（比如都是数字或都是字符串）。

sorted() 函数不会修改原始的输入对象。它会遍历输入的对象，然后返回一个全新的、排好序的列表。原始数据保持原样，这在需要保留原始数据顺序时至关重要。

```python
num_list = [34, 1, 19, 5, 87, -10]
print(f"排序前的原始列表: {num_list}") # 排序前的原始列表: [34, 1, 19, 5, 87, -10]

# 使用 sorted() 函数，并将其返回的新列表存入新变量
new_sorted_list = sorted(num_list)

print(f"sorted() 返回的新列表: {new_sorted_list}") # 输出: [-10, 1, 5, 19, 34, 87]
print(f"函数执行后，原始列表保持不变: {num_list}") # 输出: [34, 1, 19, 5, 87, -10]
```

---

#### len() - 获取列表长度

`len()` 是一个内置函数（不是方法），可以获取任何序列（包括列表）的长度。

```python
num_list = [1, 2, 3, 4, 5]
list_length = len(num_list)
print(f"列表的长度是: {list_length}") # 输出: 5
```

### 2. 元组 (Tuple)

元组（Tuple）可以看作是“只读”的列表。它是一个**有序**但**不可变**的集合。一旦创建，你就不能再修改它的内容。这个特性使得元组在某些场景下比列表更安全、更高效。

#### 创建与访问

元组使用圆括号 `()` 来创建。

```python
# 创建一个元组
my_tuple = (1, "hello", 3.14, True)
print(f"我的元组: {my_tuple}")

# 访问元素的方式和列表完全一样
print(f"访问第二个元素: {my_tuple[1]}") # 输出: hello

# 切片操作也和列表一样
sub_tuple = my_tuple[1:3]
print(f"元组切片[1:3]: {sub_tuple}") # 输出: ('hello', 3.14)

# 注意：创建只有一个元素的元组时，必须加一个逗号
single_element_tuple = (99,)
print(f"只有一个元素的元组: {single_element_tuple}")
```

#### 不可变性 (Immutability)

这是元组最核心的特点。任何试图修改元组的操作都会导致错误。

```python
# 试图修改元组的元素，会引发 TypeError
# my_tuple[0] = 99 # 取消这行代码的注释会报错: TypeError: 'tuple' object does not support item assignment

# 元组没有 append(), pop(), remove() 等会修改自身的方法
# my_tuple.append("new") # 取消注释会报错: AttributeError: 'tuple' object has no attribute 'append'
```

**什么时候使用元组？**

1. **函数返回多个值**：函数可以方便地返回一个元组，调用者可以轻松地接收。
2. **作为字典的键**：因为元组是不可变的（因此是“可哈希的”），所以它可以作为字典的键，而列表不行。
3. **保护数据**：当你有一组不希望在程序运行中被意外修改的数据时，使用元组可以增加代码的安全性。

#### 元组 (Tuple) 与多重赋值 (Unpacking)

元组不仅可以存储多个值，还可以用于多重赋值（unpacking），这在函数返回多个值时非常有用。

```python
# 定义一个函数，返回多个值
def get_user_info():
    return "Alice", 30, "New York"

# 使用多重赋值接收返回的元组
name, age, city = get_user_info()
print(f"姓名: {name}, 年龄: {age}, 城市: {city}")
# 输出: 姓名: Alice, 年龄: 30, 城市: New York

# 或者使用元组直接接收
user_info = get_user_info()
print(type(user_info))  
# 输出: <class 'tuple'>
print(f"用户信息: {user_info}")  
# 输出: 用户信息: ('Alice', 30, 'New York')
```

#### 在循环中解包

当你有一个包含元组的列表时，可以在 for 循环中直接解包，让代码可读性飙升。

```python
points = [(1, 5), (2, 8), (3, 4)]

# C++/Java 思维的 Python 代码
for point in points:
    x = point[0]
    y = point[1]
    print(f"Processing point at x={x}, y={y}")

# 利用解包的 Pythonic 代码
for x, y in points:
    print(f"Processing point at x={x}, y={y}")
```

---

### 3. 字典 (Dictionary)

字典（Dict）是Python中另一个非常重要的数据结构。它存储的是 **键值对 (key-value pairs)** 的集合。字典是**无序的**（在Python 3.7+版本中变为插入有序）且**可变**的。

字典最大的特点是查找速度极快，因为它不通过索引，而是通过唯一的 **键(key)** 来直接找到对应的值（value）。

字典可以看做是一个关联数组或哈希表。

#### 创建与操作

字典使用花括号 `{}` 来创建，每个元素都是 `key: value` 的形式。

```python
# 创建一个字典，存储一个人的信息
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}
print(f"我的字典: {person}")

# 1. 访问元素 (通过键)
print(f"姓名: {person['name']}") # 输出: Alice

# 2. 使用 get() 方法安全地访问
# 如果键不存在，'get'方法不会报错，而是返回None或指定的默认值
print(f"职业: {person.get('job')}") # 输出: None
print(f"职业 (带默认值): {person.get('job', 'Unemployed')}") # 输出: Unemployed

# 3. 修改或添加元素
person['age'] = 26 # 如果键已存在，就是修改
person['email'] = "alice@example.com" # 如果键不存在，就是添加
print(f"修改后的字典: {person}")

# 4. 删除元素
# 使用 pop() 删除并返回对应的值
city = person.pop('city')
print(f"被删除的城市: {city}")
print(f"删除城市后的字典: {person}")
# 使用 del 删除键值对
del person['email']
print(f"删除email后的字典: {person}")
```

两种不同的删除元素的方法比较：

| 特性 | `dict.pop(key[, default])` | `del dict[key]` |
| :--- | :--- | :--- |
| **主要目的** | 删除键并**返回**对应的值 | 从字典中**删除**键值对 |
| **返回值** | 返回被删除键的值 | 无返回值 (`None`) |
| **处理不存在的键** | 如果键不存在，会抛出 `KeyError` 异常，但可以提供一个默认值来避免异常并返回该默认值。 | 如果键不存在，总是会抛出 `KeyError` 异常。 |
| **语法** | 方法调用 (`my_dict.pop(...)`) | 语句 (`del my_dict[...]`) |


#### 遍历字典

遍历字典有几种常见的方式。

```python
person = {'name': 'Alice', 'age': 26, 'email': 'alice@example.com'}

# 1. 遍历所有的键 (key)
print("\n遍历键:")
for key in person.keys():
    print(key)

# 2. 遍历所有的值 (value)
print("\n遍历值:")
for value in person.values():
    print(value)

# 3. 遍历所有的键值对 (item)
print("\n遍历键值对:")
for key, value in person.items():
    print(f"{key}: {value}")
```

---

### 4. 集合 (Set)

集合（Set）是一个**无序**且**不含重复元素**的集合。它也是**可变**的。

集合最强大的功能是快速判断一个元素是否存在于集合中，以及进行数学上的集合运算（如并集、交集、差集等）。

#### 创建与基本操作

集合也可以使用花括号 `{}` 创建，但要注意与字典的区别。空集合必须使用 `set()` 来创建。

```python
# 从列表创建集合，可以自动去重
numbers = [1, 2, 2, 3, 4, 4, 4, 5]
unique_numbers = set(numbers)
print(f"从列表创建的集合 (自动去重): {unique_numbers}") # 输出: {1, 2, 3, 4, 5}

# 直接创建集合
my_set = {10, 20, "hello"}
print(f"直接创建的集合: {my_set}")

# 创建空集合，必须用 set()
empty_set = set()
# empty_dict = {} # 注意：这是创建一个空字典

# 1. 添加元素 (add)
my_set.add(30)
my_set.add("hello") # 重复添加不会有任何效果
print(f"添加元素后: {my_set}")

# 2. 删除元素 (remove vs discard)
# remove: 如果元素不存在会报错
my_set.remove(20)
# discard: 如果元素不存在，什么也不做，不会报错
my_set.discard("world") # "world" 不在集合中，但不会报错
print(f"删除元素后: {my_set}")
```

#### 集合运算

```python
set_a = {1, 2, 3, 4}
set_b = {3, 4, 5, 6}

# 1. 并集 (Union): 两者中所有的元素
union_set = set_a.union(set_b)
# 或者使用 | 操作符
union_set_op = set_a | set_b
print(f"并集: {union_set}") # 输出: {1, 2, 3, 4, 5, 6}

# 2. 交集 (Intersection): 两者共有的元素
intersection_set = set_a.intersection(set_b)
# 或者使用 & 操作符
intersection_set_op = set_a & set_b
print(f"交集: {intersection_set}") # 输出: {3, 4}

# 3. 差集 (Difference): 在 set_a 中但不在 set_b 中的元素
difference_set = set_a.difference(set_b)
# 或者使用 - 操作符
difference_set_op = set_a - set_b
print(f"差集 (A-B): {difference_set}") # 输出: {1, 2}

print(f"差集 (B-A): {set_b - set_a}") # 输出: {5, 6}
```

### 小技巧2

#### 交换两个变量的值

这是展示 Python 多重赋值优势最经典的例子

C++/Java: 需要一个临时变量

```java
int a = 5, b = 10;
int temp = a;
a = b;
b = temp;
System.out.println("a: " + a + ", b: " + b); // 输出
// a: 10, b: 5
```

Python: 一行代码搞定，无需临时变量

```python
a = 5
b = 10
a, b = b, a 
# 发生了什么？
# 1. 右边先创建了一个元组 (10, 5)
# 2. 然后将这个元组解包，把 10 赋给 a，5 赋给 b
print(f"a: {a}, b: {b}")  # 输出: a: 10, b: 5
```

#### 快速翻转列表

使用切片可以非常简洁地翻转一个列表。

```python
# 使用切片翻转列表
reversed_list = my_list[::-1]
print(f"翻转后的列表: {reversed_list}") # 输出: ['new_item', True, 99]
```

#### 循环：不止 range，遍历一切

在Python中，`for` 循环可以遍历任何可迭代对象（iterable），包括列表、字符串、字典等。

```python
# 遍历列表
for item in my_list:
    print(f"列表中的元素: {item}")
# 遍历字符串
for char in "hello":
    print(f"字符串中的字符: {char}")# 遍历字典
my_dict = {"name": "Alice", "age": 30}
for key, value in my_dict.items():
    print(f"字典中的键: {key}, 值: {value}")
```

#### 数据结构不严格对应

* std::vector / ArrayList  => `list`
* std::map / HashMap       => `dict`
* std::set / HashSet       => `set`
* const 数组 / 不可变集合   => `tuple`

案例：统计词频：

Java/C++ 需要使用 Map 来统计词频

```java
Map<String, Integer> counts = new HashMap<>();
String[] words = {"apple", "banana", "apple"};
for (String word : words) {
    counts.put(word, counts.getOrDefault(word, 0) + 1);
}
```

Python 可以使用字典或 `collections.Counter` 来实现同样的功能

```python
counts = {}  # 创建一个空字典
words = ["apple", "banana", "apple"]
for word in words:
    counts[word] = counts.get(word, 0) + 1
print(counts)  # 输出: {'apple': 2, 'banana': 1}

# 更 Pythonic 的方式是使用 collections.Counter
from collections import Counter
counts = Counter(words)
print(counts)  # 输出: Counter({'apple': 2, 'banana': 1})
```

Ref: [collections --- 容器数据类型](https://docs.python.org/zh-cn/3.13/library/collections.html)

#### 列表/字典推导式

能用一行代码完成 C++/Java 需要一个循环才能完成的任务

案例：创建一个 0-9 的平方数列表

```Java
List<Integer> squares = new ArrayList<>();
for (int i = 0; i < 10; i++) {
    squares.add(i * i);
}
```

Python 列表推导式：

```python
squares = [i * i for i in range(10)]
print(squares)  # 输出: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

案例：创建一个**带条件的** 0-9 的平方数列表, 只包含偶数的平方数

```Java
List<Integer> even_squares = new ArrayList<>();
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        even_squares.add(i * i);
    }
}
```

Python 列表推导式：

```python
even_squares = [i * i for i in range(10) if i % 2 == 0]
print(even_squares)  # 输出: [0, 4, 16, 36, 64]
```

#### 使用`enumerate()` 获取索引和值

在遍历列表时，`enumerate()` 函数可以同时获取元素的索引和值。

```python
fruits = ["apple", "banana", "cherry"]
for index, fruit in enumerate(fruits):
    print(f"索引: {index}, 水果: {fruit}")
# 输出:
# 索引: 0, 水果: apple
# 索引: 1, 水果: banana
# 索引: 2, 水果: cherry
```

#### 使用`zip()` 同时遍历多个列表

`zip()` 函数可以将多个可迭代对象（如列表）打包成一个元组的迭代器，常用于同时遍历多个列表。

```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
for name, age in zip(names, ages):
    print(f"{name} 的年龄是 {age}")
# 输出:
# Alice 的年龄是 25
# Bob 的年龄是 30
# Charlie 的年龄是 35
```

#### 使用`map()` 函数应用函数到列表

`map()` 函数可以将一个函数应用到一个可迭代对象的每个元素上，返回一个迭代器。

```python
# 定义一个函数
def square(x):
    return x * x
# 使用 map() 应用函数到列表
numbers = [1, 2, 3, 4, 5]
squared_numbers = list(map(square, numbers))
print(squared_numbers)  # 输出: [1, 4, 9, 16, 25]
```

注意`map()` 返回的是一个迭代器，所以通常需要用 `list()` 转换为列表。

#### 使用`filter()` 函数过滤列表

`filter()` 函数可以根据一个函数的返回值过滤可迭代对象

```python
# 定义一个函数，判断是否为偶数
def is_even(x):
    return x % 2 == 0
# 使用 filter() 过滤列表
numbers = [1, 2, 3, 4, 5, 6]
even_numbers = list(filter(is_even, numbers))
print(even_numbers)  # 输出: [2, 4, 6]
```

#### 使用`reduce()` 函数进行累积计算

`reduce()` 函数来自 `functools` 模块，可以对一个可迭代对象进行累积计算。

```python
from functools import reduce
# 定义一个函数，计算两个数的和
def add(x, y):
    return x + y
# 使用 reduce() 计算列表的总和
numbers = [1, 2, 3, 4, 5]
total = reduce(add, numbers)
print(total)  # 输出: 15
```

## 函数

### 函数定义与调用

使用 `def` 关键字来定义函数。

```python
def greet(name):
    """这是一个向指定名字的人问好的函数"""
    return f"Hello, {name}!"

# 调用函数
message = greet("Alice")
print(message) # 输出: Hello, Alice!
```

### 函数参数

函数可以接受多种类型的参数：

* **位置参数**：按顺序传递的参数。
* **默认参数**：可以为参数设置默认值，如果调用时没有提供该参数，则使用默认值。
* **可变参数**：使用 `*args` 来接收任意数量的位置参数，使用 `**kwargs` 来接收任意数量的关键字参数。
  * 对于 `*args`，函数内部会将所有**位置参数**打包成一个元组。
  * 对于 `**kwargs`，函数内部会将所有**关键字参数**打包成一个字典。
  
```python
# 定义一个函数，演示各种参数类型
def example_function(pos1, pos2, default_param="default", *args, **kwargs):
    print(f"位置参数1: {pos1}")
    print(f"位置参数2: {pos2}")
    print(f"默认参数: {default_param}")
    print(f"可变参数: {args}")
    print(f"关键字参数: {kwargs}")

# 允许调用的方式
# 1. 只传递位置参数
example_function(1, 2)  
# default_param 使用默认值 "default"

# 2. 传递位置参数和默认参数
example_function(1, 2, "custom")  

# 3. 传递位置参数、默认参数和可变参数
example_function(1, 2, "custom", 3, 4, 5) 
# 其中 args 为tuple (3, 4, 5)

# 4. 传递位置参数、默认参数、可变参数和关键字参数
example_function(1, 2, "custom", 3, 4, 5, key1="value1", key2="value2")  
# 其中 args 为tuple (3, 4, 5)
# 其中 kwargs 为字典 {'key1': 'value1', 'key2': 'value2'}
```

### Lambda (匿名) 函数

有时候需要一个功能非常简单的临时函数，但又不想用 def 去专门定义一个，这时 lambda 函数就派上用场了。它是一种小型的、匿名的、单行函数。

它的语法是：`lambda arguments: expression`

```python
# 使用 def 定义一个加法函数
def add_def(x, y):
    return x + y

# 使用 lambda 定义一个等效的加法函数
add_lambda = lambda x, y: x + y

print(f"\ndef 定义的函数结果: {add_def(3, 5)}")       # 输出: 8
print(f"lambda 定义的函数结果: {add_lambda(3, 5)}") # 输出: 8

# Lambda最常见的用途是作为高阶函数（如 sorted, map, filter）的参数
students = [
    {'name': 'Alice', 'score': 85},
    {'name': 'Bob', 'score': 92},
    {'name': 'Charlie', 'score': 78}
]

# 使用 lambda 指定 sorted 函数按每个字典的 'score' 值进行排序
sorted_students = sorted(students, key=lambda student: student['score'])
print(f"\n按分数排序后的学生: {sorted_students}")
```

### 变量作用域 (Scope)

作用域决定了代码中哪个部分可以访问一个变量。Python主要有两种作用域：

* 局部作用域 (Local Scope)：变量在函数内部定义，只能在该函数内部访问。
* 全局作用域 (Global Scope)：变量在所有函数外部定义，可以在程序的任何地方访问。

```python
x = 10  # 全局变量

def my_function():
    y = 5  # 局部变量
    print(f"函数内部: y = {y}")
    print(f"函数内部: x = {x}") # 可以访问全局变量 x

my_function()

print(f"\n函数外部: x = {x}")
# print(f"函数外部: y = {y}") # 取消此行注释会报错: NameError: name 'y' is not defined
```

**修改全局变量**：如果在函数内部想修改一个全局变量的值，需要使用 global 关键字声明。

```python
count = 0 # 全局变量

def increment():
    global count # 声明 count 是全局变量
    count += 1
    print(f"函数内 count = {count}")

increment()
increment()

print(f"函数外 count = {count}") # 输出: 2
```

**变量遮蔽 (Variable Shadowing)**：如果函数内部定义了与外部全局变量同名的变量，那么在函数内部，这个局部变量会“遮蔽”或“隐藏”同名的全局变量。

```python
x = 10  # 全局变量
def shadowing_example():
    x = 5  # 局部变量，遮蔽了全局变量 x
    print(f"函数内部: x = {x}")  # 输出: 5
shadowing_example()
print(f"函数外部: x = {x}")  # 输出: 10
```

## 模块和包

* 模块 (Module): 一个单独的 Python 文件，例如 utils.py。你可以把它看作一份文档。
* 包 (Package): 一个包含了**多个模块的文件夹**。这个特殊的文件夹里必须有一个 `__init__.py` 文件
  
### 安装包

Python 有一个强大的包管理工具 `pip`，可以用来安装第三方模块。`poetry`和`uv`等依赖管理工具可以声明式方式管理依赖。

例如安装 pytorch 依赖

```bash
# pip
pip3 install torch torchvision torchaudio
# OR uv
uv add torch torchvision torchaudio
```

### 使用模块和包

在 Python 中使用模块和包非常简单，通过 `import` 语句来导入模块或包。

```python
# 1. 导入整个模块
import math

# 使用模块中的函数或变量，需要通过 `模块名.` 的方式调用
print(f"π 的值是: {math.pi}")
print(f"16的平方根是: {math.sqrt(16)}")

# 2. 导入模块并使用别名 (当模块名太长时很有用)
import random as rd

# 使用别名来调用
print(f"生成一个 0 到 1 之间的随机数: {rd.random()}")

# 3. 从模块中只导入需要的特定部分
from datetime import datetime

# 直接使用导入的函数或类名，无需加模块名前缀
now = datetime.now()
print(f"当前时间是: {now}")

# 4. 从模块中导入所有内容 (不推荐)
# from math import *
# print(sqrt(25)) # 可以直接调用 sqrt，但可能会与你自定义的函数名冲突
```

### 自定义包

假设我们正在开发一个Web应用，一个典型的项目结构可能如下：

```sh
my_web_app/
├── main.py              # 主程序入口
└── my_app/              # <-- 这是我们的包
    ├── __init__.py      # 告诉Python 'my_app' 是一个包
    ├── database.py      # 处理数据库连接的模块
    ├── models.py        # 定义数据模型的模块 (e.g., User, Product)
    └── utils/           # <-- 包里还可以有子包 (嵌套包)
        ├── __init__.py
        └── formatters.py # 格式化数据的工具模块
```

在 `main.py` 中使用这个包：

```python
# main.py

# 方式1: 导入包中的特定模块
import my_app.database
my_app.database.connect()

# 方式2: 使用 from...import 导入模块，更常用
from my_app import models
user1 = models.User("Alice")

# 方式3: 导入模块并使用别名
from my_app import database as db
db.connect()

# 方式4: 从模块中直接导入变量或函数
from my_app.models import User
user2 = User("Bob")

# 方式5: 从子包中导入
from my_app.utils import formatters
formatted_name = formatters.capitalize_name("charlie")
```

### 相对导入与绝对导入

假设我们正在开发一个Web应用，一个典型的项目结构可能如下：

```sh
my_web_app/
├── main.py              # 主程序入口
└── my_app/              # <-- 这是我们的包
    ├── __init__.py      # 告诉Python 'my_app' 是一个包
    ├── database.py      # 处理数据库连接的模块
    ├── models.py        # 定义数据模型的模块 (e.g., User, Product)
    └── utils/           # <-- 包里还可以有子包 (嵌套包)
        ├── __init__.py
        └── formatters.py # 格式化数据的工具模块
```

* 在 `database.py` 中，需要使用 `models.py` 里定义的类（比如一个 `User` 类）。

```python
# my_app/database.py
# 绝对导入
from my_app.models import User
# 相对导入
from .models import User 
```

* 在 `formatters.py` 中，需要使用 `database.py` 里的函数。

```python
# my_app/utils/formatters.py
# 绝对导入
from my_app.database import connect
# 相对导入
from ..database import connect  
```

### 常见的坑

#### 导入包？导入模块？

很多情况下，我们可以直接导入包而不是单独的模块，但是本质上导入包时，Python 会自动执行包中的 `__init__.py` 文件，这个文件可以包含初始化代码或导入子模块。

例子：

假设我们有如下的文件结构：

```sh
project/
├── main.py
└── my_app/
    ├── __init__.py # 空文件
    └── utils.py
```

并且 utils.py 的内容是：

```python
# my_app/utils.py
def greet(name):
    return f"Hello, {name}!"
```

以下代码在现在是会报错的：

```python
# main.py
import my_app  # 导入包!?
print(my_app.utils.greet("Alice"))  # 报错: “utils”不是模块“testt”的已知属性 / AttributeError: module 'my_app' has no attribute 'utils'
```

事实上，我们的import后面应该是模块而不是包 —— 但如果import后面是包，那么Python会尝试在包的 `__init__.py` 文件中查找 `utils`，但如果没有在 `__init__.py` 中导入 `utils`，就会报错。

我们可以在 `__init__.py` 中添加以下内容来解决这个问题：

```python
# my_app/__init__.py
from . import utils  # 导入 utils 模块
```

现在，`main.py` 中的代码就可以正常工作了：

```python
import my_app  # 导入包
print(my_app.utils.greet("Alice"))  # 输出: Hello, Alice!
```

#### 为什么相对导入会失败？

很有可能是在顶级脚本中进行了相对导入

问题场景：一个文件夹，两个文件

```sh
my_project/
├── main.py
└── helper.py # 有一个 greet()函数
```

错误的尝试：

```python
# main.py
from . import helper # <-- 很多初学者会这样尝试

message = helper.greet()
print(message)
```

**WHY?** 失败的根本原因在于Python的**模块身份**问题 —— “顶层脚本”没有包身份：

* 当执行 python main.py 时，Python将 main.py 视作程序的入口或顶层脚本。在这种模式下，main.py 的“身份”是特殊的：
  * 它的 `__name__` 属性被设为 `'__main__'`。
  * 它的 `__package__` 属性被设为 `None`。

* 相对导入需要“包身份”：相对导入的语法（如 . 和 ..）是为包（Package）内部的模块互相引用而设计的。它需要通过当前模块的 `__package__` 属性来确定自己的“位置”，从而计算出相对路径。

* 当 main.py（其 `__package__` 为 None）尝试执行 `from . import helper` 时，Python不知道 `.` 指的是哪个包。它没有计算相对位置的“锚点”，因此只能报错，错误信息 `attempted relative import with no known parent package` 的意思就是：“你想用相对导入，但我不知道你的父包是谁”。

正确的代码

```python
# main.py
import helper # 直接写模块名即可

message = helper.greet()
print(message) # 输出: Hello from helper!
```

## 面向对象编程(OOP)

### 定义类

```python
# 定义一个最简单的“汽车”蓝图
class Car:
    pass # pass 是一个占位符，表示这个类是空的

# --- 对象的创建（实例化）---
# 根据 Car 类的蓝图，创建了两个具体的汽车对象
my_car = Car()
another_car = Car()

print(f"my_car 的类型是: {type(my_car)}")
# 输出: my_car 的类型是: <class '__main__.Car'>
```

### 类的初始化、`self`、与方法

* 类的初始化方法 `__init__` 是一个特殊的方法，用于在创建对象时初始化对象的属性。
* `self` 是一个指向当前对象的引用，用于访问对象的属性和方法，类似于C++/JAVA中的`this`。
* `self` 关键字**必须**作为类方法的第一个参数。
* 如果没有`self`，为静态方法，不能访问实例属性。

```python
class Car:
    def __init__(self, make, model, year):
        """初始化汽车的品牌、型号和年份"""
        self.make = make  # 品牌
        self.model = model  # 型号
        self.year = year  # 年份
    def display_info(self):
        """显示汽车信息"""
        print(f"{self.year} {self.make} {self.model}")
```

### 动态类型与鸭子类型 (Dynamic Typing & Duck Typing)

* Python是动态类型语言，变量没有固定类型，类型附着于对象本身。

```python
class Profile:
    def __init__(self, name, age):
        self.name = name  # name可以是str, 也可以在之后被赋值为任何类型
        self.age = age
```

* Duck Typing: "如果一个东西走起来像鸭子，叫起来也像鸭子，那它就是鸭子。"

```python
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

def make_sound(animal):
    # 不关心animal是Dog还是Cat, 只要它有speak()方法就行
    print(animal.speak())

make_sound(Dog()) # 输出: Woof!
make_sound(Cat()) # 输出: Meow!
```

Duck Typing 导致 Python 的多态与 C++/Java 有所不同。C++/Java 强制要求类型匹配，而 Python 只关心对象是否具有所需的方法或属性。

### 类的访问控制

Python没有像Java/C++中 private, protected, public 这样的强制访问控制关键字。其访问控制完全基于命名约定和程序员的自觉。

* `_single_underscore` (单下划线前缀):

  * **约定为“受保护” (Protected)**。它提示其他开发者：“这是一个内部属性/方法，请不要从外部直接访问”，但解释器**不作任何限制**。
  * `from module import *` 不会导入单下划线开头的名称。

* `__double_underscore` (双下划线前缀):

  * **实现“伪私有” (Pseudo-private)**，通过一种叫做 **名称改写 (Name Mangling)** 的机制。
  * 解释器会将 `__my_var` 这样的属性名改写为 `_ClassName__my_var`。
  * 其主要目的是**避免在继承中发生意外的名字冲突**，而不是为了实现真正的私有化。你仍然可以通过改写后的名字从外部访问它。

### 魔法方法 (Magic Methods)

以双下划线开头和结尾的方法（如 `__init__`, `__str__`, `__len__`, `__add__`）是Python的“魔术方法”。它们能让你的自定义对象与Python的内置语法和函数无缝集成。这类似于C++中的操作符重载。

* `__str__(self)`: 定义对实例调用 `str()` 或 `print()` 时的用户友好输出 (类似Java的 `toString()`)。
* `__repr__(self)`: 定义对实例调用 `repr()` 时的开发者友好、无歧义的输出，目标是让输出内容能被 `eval()` 执行后重现对象。
* `__len__(self)`: 让对象可以响应 `len()` 函数。
* `__add__(self, other)`: 让对象支持 `+` 操作符。

<!-- end list -->

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __str__(self):
        return f"({self.x}, {self.y})"
        
    def __add__(self, other):
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1)         # 调用 __str__ -> (1, 2)
print(repr(v1))   # 调用 __repr__ -> Vector(1, 2)
print(v1 + v2)    # 调用 __add__ -> (4, 6)
```

### 属性访问: `@property` 实现优雅的Getter/Setter

Java/C++开发者习惯于为私有字段编写 `getX()` 和 `setX()` 方法。Python推崇更直接的方式。

* **直接访问**: 通常，你应该将属性设为公开的，直接访问。
* **`@property`**: 如果未来你需要为属性的读取、设置或删除添加逻辑（如验证），你可以使用 `@property` 装饰器将其“升级”为一个具有getter/setter/deleter逻辑的属性，而**无需改变调用方的代码**。

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self._salary = salary # 使用_前缀表示内部变量

    @property
    def salary(self):
        """这是 salary 属性的 getter"""
        return f"${self._salary:,.2f}"

    @salary.setter
    def salary(self, value):
        """这是 salary 属性的 setter"""
        if value < 0:
            raise ValueError("Salary cannot be negative.")
        self._salary = value

emp = Employee("Alice", 50000)
emp.salary = 60000        # 调用 setter (emp.salary(60000)的语法糖)
print(emp.name, emp.salary) # 调用 getter -> Alice $60,000.00
```

### 继承

Python支持**多重继承**，这一点更像C++，而与Java的单继承+接口模型不同。在使用多重继承时，需要注意**方法解析顺序 (Method Resolution Order, MRO)**，Python使用C3线性化算法来确定调用哪个父类的方法。你可以通过 `ClassName.mro()` 或 `help(ClassName)` 查看MRO。

假设我们希望创建一个神经网络，同时我们希望它具备两种额外的、可复用的能力：

* 权重初始化 (Weight Initialization): 提供一种特定的方法来初始化网络中的线性层和卷积层权重。
* 日志记录 (Logging): 提供简单的方法来记录和展示训练过程中的损失或其他指标。

我们将把这两种能力分别封装在两个 Mixin 类中，然后让我们的主模型同时继承 torch.nn.Module 和这两个 Mixin 类。

```python
import torch
import torch.nn as nn

# Mixin 1: 提供权重初始化功能
class WeightInitMixin:
    def _initialize_weights(self):
        """
        遍历模型的所有子模块，并应用特定的权重初始化。
        """
        print("=> 调用 WeightInitMixin 的 _initialize_weights()...")
        for m in self.modules():
            if isinstance(m, nn.Conv2d):
                nn.init.kaiming_normal_(m.weight, mode='fan_out', nonlinearity='relu')
                if m.bias is not None:
                    nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.Linear):
                nn.init.normal_(m.weight, 0, 0.01)
                nn.init.constant_(m.bias, 0)
        print("=> 权重初始化完成.")


# Mixin 2: 提供日志记录功能
class LoggingMixin:
    def __init__(self):
        # 注意：在多继承中，super().__init__()非常重要
        super().__init__() 
        self.logs = {}
        print("=> LoggingMixin 初始化完成，创建 self.logs 字典.")

    def log(self, key, value):
        """记录一个指标"""
        if key not in self.logs:
            self.logs[key] = []
        self.logs[key].append(value)
        
    def show_logs(self, n=5):
        """展示最近的n条日志"""
        print("\n--- 模型日志 ---")
        for key, values in self.logs.items():
            recent_values = ", ".join([f"{v:.4f}" for v in values[-n:]])
            print(f"- {key}: [{recent_values}]")
        print("----------------\n")

# 主模型，同时继承 nn.Module 和我们的两个 Mixin
class ComplexNet(nn.Module, WeightInitMixin, LoggingMixin):
    def __init__(self, in_features, num_classes):
        # --- 关键：调用super().__init__() ---
        # 这会根据Python的MRO(方法解析顺序)正确地调用所有父类的__init__方法
        # 在本例中，它会调用 LoggingMixin.__init__ 和 nn.Module.__init__
        super().__init__()
        
        print(f"\n开始构建 ComplexNet...")
        
        # 1. 定义模型的核心架构 (来自 nn.Module)
        self.features = nn.Sequential(
            nn.Linear(in_features, 128),
            nn.ReLU(),
            nn.Linear(128, 64),
            nn.ReLU()
        )
        self.classifier = nn.Linear(64, num_classes)
        
        # 2. 调用权重初始化方法 (来自 WeightInitMixin)
        self._initialize_weights()

    def forward(self, x):
        x = self.features(x)
        x = self.classifier(x)
        return x
```

## 错误处理

在Python中，错误处理是通过异常（Exception）来实现的。异常是程序运行时发生的错误，可以通过 `try` 和 `except` 语句来捕获和处理。

```python
try:
    # 尝试执行可能会引发异常的代码
    result = 10 / 0  # 故意引发一个除以零
except ZeroDivisionError as e:
    # 捕获特定的异常类型
    print(f"发生了除以零错误: {e}")
except Exception as e:
    # 捕获所有其他类型的异常
    print(f"发生了其他错误: {e}")
else:
    # 如果没有异常发生，执行这部分代码
    print(f"计算结果是: {result}")
finally:
    # 无论是否发生异常，都会执行这部分代码
    print("清理资源或执行结束操作。")
```

## 文件操作

Python可以轻松地读取和写入文件。

### 读取文件

```python
with open('pi_digits.txt') as file_object:
    contents = file_object.read()
print(contents)
```

也可以一行一行读取文件内容

```python
with open('pi_digits.txt') as file_object:
    for line in file_object:
        print(line.strip())  # 使用 strip() 去除行末的换行符
```

#### 写入文件

* **写入模式 ('w')**: 会覆盖文件原有内容。
* **追加模式 ('a')**: 会在文件末尾添加新内容。

```python
with open('programming.txt', 'w') as file_object:
    file_object.write("I love programming.\n")
    file_object.write("I love creating new games.\n")

with open('programming.txt', 'a') as file_object:
    file_object.write("I also love finding meaning in large datasets.\n")
```

### with 关键字

`with` 关键字用于简化资源管理，确保资源在使用后正确关闭。它会自动处理文件的打开和关闭，避免忘记关闭文件导致的资源泄漏。

工作原理：  
当使用 `with` 语句时，Python会自动调用文件对象的 `__enter__()` 方法来打开文件，并在代码块结束后调用 `__exit__()` 方法来关闭文件。

### Python 与 JSON 数据交互

Python与JSON的类型对应关系：

| Python | JSON |
| :--- | :--- |
| `dict` | `object` |
| `list`, `tuple` | `array` |
| `str` | `string` |
| `int`, `float` | `number` |
| `True` / `False` | `true` / `false` |
| `None` | `null` |

`json` 模块提供了四个主要的方法，可以分为两类：

| 函数 | 作用 | 输入 | 输出 |
| :--- | :--- | :--- | :--- |
| `json.dumps()` | **序列化**到**字符串** | Python 对象 (dict, list) | JSON 格式的字符串 |
| `json.loads()` | 从**字符串**中**反序列化** | JSON 格式的字符串 | Python 对象 (dict, list) |
| `json.dump()` | **序列化**到**文件** | Python 对象 和 文件对象 | 无 (写入文件) |
| `json.load()` | 从**文件**中**反序列化** | 文件对象 | Python 对象 (dict, list) |

> **记忆技巧**: 函数名中带 `s` 的 (`dumps`, `loads`) 是用来处理**s**tring（字符串）的

```python
import json

# 1. 创建一个Python字典
python_data = {
    "name": "Alice",
    "age": 25,
    "isStudent": True,
    "courses": ["Computer Science", "Physics"],
    "address": None
}

# 2. 使用 dumps() 转换为JSON字符串
json_string = json.dumps(python_data)

print("--- 转换后的JSON字符串 ---")
print(json_string) 
# 输出: {"name": "Alice", "age": 25, "isStudent": true, "courses": ["Computer Science", "Physics"], "address": null}
print(f"\n类型: {type(json_string)}")
# 输出: 类型: <class 'str'>
```
