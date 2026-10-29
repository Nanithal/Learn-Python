# Day01｜Python 基础：变量、数据类型与输出

## 1. 运行第一个 Python 程序

在 VS Code 中新建 `day01.py`，使用现有的 Python 环境运行：

~~~python
print("Hello, Python!")
print("开始学习机器学习编程")
~~~

输出：

~~~text
Hello, Python!
开始学习机器学习编程
~~~

- `print()` 是 Python 的内置函数，用于输出内容。
- 引号中的文字是字符串。
- `print()` 默认在输出结束后换行。
- 对于这样的简单脚本，顶层语句按从上到下的顺序执行。

与 C/C++ 相比，简单 Python 脚本不需要写 `#include` 或 `main()`，语句末尾一般也不写分号。

## 2. 注释

`#` 后面直到本行末尾的内容是注释，不参与程序执行。

~~~python
# 设置训练轮数
epochs = 10

print(epochs)  # 输出训练轮数
~~~

注释用于解释代码的目的，方便自己和别人阅读。

## 3. 变量与赋值

### 3.1 定义变量

~~~python
epochs = 10
learning_rate = 0.01
model_name = "LeNet"
use_gpu = False
~~~

| 变量名 | 值 | 示例含义 |
| --- | --- | --- |
| `epochs` | `10` | 训练轮数 |
| `learning_rate` | `0.01` | 学习率 |
| `model_name` | `"LeNet"` | 模型名称 |
| `use_gpu` | `False` | 是否使用 GPU |

这里的机器学习术语只是用来举例，当前先理解怎样保存这些数据。

赋值语句的一般形式：

~~~text
变量名 = 表达式
~~~

执行时，先计算右边的表达式，再让左边的变量名关联到结果对象。`=` 表示赋值，不表示判断两边是否相等。

### 3.2 不需要先声明变量类型

C++ 通常写作：

~~~cpp
int epochs = 10;
double learning_rate = 0.01;
~~~

Python 直接写：

~~~python
epochs = 10
learning_rate = 0.01
~~~

**Python 不要求事先声明变量类型，但数据对象本身仍然有类型。**

### 3.3 重新赋值

~~~python
epochs = 10
epochs = 20

print(epochs)
~~~

输出：

~~~text
20
~~~

变量名也可以重新关联到不同类型的对象：

~~~python
value = 10
value = "hello"

print(value)
~~~

此时输出 `hello`。这里不是把整数 `10` 转成了字符串，而是让 `value` 关联到了另一个对象。

实际写代码时，尽量让同一个变量保持明确的用途，避免反复改变它所表示的数据种类。

### 3.4 变量与文字的区别

~~~python
epochs = 10

print(epochs)
print("epochs")
~~~

输出：

~~~text
10
epochs
~~~

- `epochs`：变量名，读取它关联的值。
- `"epochs"`：字符串，表示这几个字符本身。

## 4. 四种常用数据类型

| 类型 | Python 名称 | 示例 |
| --- | --- | --- |
| 整数 | `int` | `10`、`0`、`-3` |
| 浮点数 | `float` | `0.01`、`3.14` |
| 字符串 | `str` | `"LeNet"`、`"你好"` |
| 布尔值 | `bool` | `True`、`False` |

注意：

- `10` 是整数，`10.0` 是浮点数。
- `10` 与 `"10"` 不同：前者是整数，后者是字符串。
- Python 中的布尔值写作 `True` 和 `False`，首字母必须大写。

使用 `type()` 查看对象类型：

~~~python
epochs = 10
learning_rate = 0.01
model_name = "LeNet"
use_gpu = False

print(type(epochs))
print(type(learning_rate))
print(type(model_name))
print(type(use_gpu))
~~~

输出：

~~~text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
~~~

初学时，先关注其中的 `int`、`float`、`str` 和 `bool` 即可。

## 5. 基本算术运算

设 `a = 7`、`b = 2`：

| 运算 | 写法 | 结果 |
| --- | --- | --- |
| 加法 | `a + b` | `9` |
| 减法 | `a - b` | `5` |
| 乘法 | `a * b` | `14` |
| 普通除法 | `a / b` | `3.5` |
| 向下取整除法 | `a // b` | `3` |
| 取余 | `a % b` | `1` |
| 乘方 | `a ** b` | `49` |

可以运行下面的代码验证：

~~~python
a = 7
b = 2

print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a // b)
print(a % b)
print(a ** b)
~~~

### 5.1 普通除法与 C++ 整数除法的区别

Python 中，两个整数使用 `/` 相除，也会得到浮点数：

~~~python
print(7 / 2)  # 3.5
print(8 / 2)  # 4.0
~~~

### 5.2 向下取整不等于向零截断

~~~python
print(7 // 2)   # 3
print(-7 // 2)  # -4
~~~

`-7 / 2` 是 `-3.5`，向下取整得到 `-4`。这与 C++ 整数除法向零截断的规则不同。

### 5.3 乘方使用两个星号

~~~python
print(2 ** 3)  # 8
~~~

不要把 `^` 当作乘方；在 Python 中，它表示按位异或。

## 6. 将文字与变量一起输出

### 6.1 使用逗号分隔多个参数

~~~python
epochs = 10

print("训练轮数：", epochs)
~~~

输出：

~~~text
训练轮数： 10
~~~

`print()` 默认在多个参数之间添加一个空格。

### 6.2 使用 f-string

~~~python
model_name = "LeNet"
epochs = 10
learning_rate = 0.01

print(f"模型：{model_name}")
print(f"训练轮数：{epochs}")
print(f"学习率：{learning_rate}")
~~~

输出：

~~~text
模型：LeNet
训练轮数：10
学习率：0.01
~~~

基本格式：

~~~text
f"说明文字：{变量或表达式}"
~~~

记住两点：

1. 在字符串前加上 `f`。
2. 将变量或表达式放进 `{}` 中。

花括号里也可以直接计算：

~~~python
epochs = 10

print(f"下一次设置的训练轮数：{epochs + 5}")
~~~

输出：

~~~text
下一次设置的训练轮数：15
~~~

### 6.3 保留两位小数

~~~python
accuracy = 0.923456

print(f"准确率：{accuracy:.2f}")
print(accuracy)
~~~

输出：

~~~text
准确率：0.92
0.923456
~~~

`:.2f` 表示按固定小数格式显示，保留两位小数。

**格式化只改变显示方式，不会修改变量原来的值。** 这里的 `0.92` 也没有转换成百分数；它仍然是小数形式。

## 7. 常见易错点

| 容易混淆的地方 | 正确理解 |
| --- | --- |
| `print(epochs)` 与 `print("epochs")` | 前者读取变量，后者输出文字 |
| `10` 与 `"10"` | 前者是整数，后者是字符串 |
| 写成 `true` 或 `false` | 布尔字面量必须写成 `True`、`False` |
| 认为 `7 / 2` 得到 `3` | Python 中结果是 `3.5` |
| 认为 `//` 总是向零截断 | 它是向下取整，`-7 // 2` 得到 `-4` |
| 把 `^` 当作乘方 | 乘方使用 `**` |
| 写 `"{epochs}"` 时忘记前面的 `f` | 会直接输出文字 `{epochs}` |
| 认为 `:.2f` 修改了变量值 | 它只控制本次输出的显示格式 |
