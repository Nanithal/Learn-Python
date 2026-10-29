# Day02｜Python 基础：输入、类型转换与条件判断

## 1. 使用 input() 接收输入

`input()` 会显示提示文字，等待用户输入并按回车，然后返回输入内容。

~~~python
model_name = input("请输入模型名称：")
print(f"你选择的模型是：{model_name}")
~~~

运行示例：

~~~text
请输入模型名称：LeNet
你选择的模型是：LeNet
~~~

**`input()` 返回的一定是字符串，即使输入内容看起来是数字。**

~~~python
epochs = input("请输入训练轮数：")

print(epochs)
print(type(epochs))
~~~

输入 `10` 后，输出：

~~~text
10
<class 'str'>
~~~

此时 `epochs` 保存的是 `"10"`，而不是整数 `10`。作为数字参与计算之前，需要先转换类型。

## 2. 类型转换

### 2.1 常用转换函数

| 函数 | 用途 | 示例 | 结果 |
| --- | --- | --- | --- |
| `int()` | 转换为整数 | `int("10")` | `10` |
| `float()` | 转换为浮点数 | `float("0.01")` | `0.01` |
| `str()` | 转换为字符串 | `str(10)` | `"10"` |
| `bool()` | 按真值规则转换为布尔值 | `bool(0)` | `False` |

例如，把输入的训练轮数转换成整数：

~~~python
epochs = input("请输入训练轮数：")
epochs = int(epochs)

print(epochs + 1)
~~~

输入 `10` 后，输出 `11`。

也可以合并为：

~~~python
epochs = int(input("请输入训练轮数："))
~~~

执行顺序是：

1. `input()` 得到字符串，例如 `"10"`。
2. `int()` 得到整数 `10`。
3. 将整数赋给 `epochs`。

读取浮点数时使用 `float()`：

~~~python
learning_rate = float(input("请输入学习率："))

print(f"学习率：{learning_rate}")
print(type(learning_rate))
~~~

### 2.2 转换需要满足格式要求

| 表达式 | 结果或行为 |
| --- | --- |
| `int("10")` | 得到整数 `10` |
| `float("3.14")` | 得到浮点数 `3.14` |
| `int(3.9)` | 得到 `3`，向零截断 |
| `int(-3.9)` | 得到 `-3`，向零截断 |
| `int("3.9")` | 引发 `ValueError` |
| `int("hello")` | 引发 `ValueError` |

`int(3.9)` 的参数是浮点数，而 `int("3.9")` 的参数是字符串，不能将两者混为一谈。

### 2.3 bool() 不按字符串的文字含义判断真假

~~~python
print(bool(0))        # False
print(bool(""))       # False
print(bool("False"))  # True
print(bool("0"))      # True
~~~

对于本课涉及的常见值：

- 数值零转换为 `False`，非零数值转换为 `True`。
- 空字符串转换为 `False`，非空字符串转换为 `True`。

因此，不能直接用 `bool(input(...))` 解析用户输入的 `"True"` 或 `"False"`。

## 3. 比较运算

对于本课使用的普通数值，比较表达式的结果是 `True` 或 `False`。

| 运算符 | 含义 | 示例 |
| --- | --- | --- |
| `>` | 大于 | `10 > 5` |
| `<` | 小于 | `3 < 5` |
| `>=` | 大于或等于 | `10 >= 10` |
| `<=` | 小于或等于 | `3 <= 5` |
| `==` | 等于 | `10 == 10` |
| `!=` | 不等于 | `10 != 5` |

~~~python
epochs = 10

print(epochs > 5)    # True
print(epochs == 10)  # True
print(epochs != 10)  # False
~~~

**`=` 是赋值，`==` 是比较。**

~~~python
epochs = 10    # 将 10 赋给变量
epochs == 10   # 比较，表达式的结果是 True
~~~

Python 支持链式比较：

~~~python
accuracy = 0.92

print(0 <= accuracy <= 1)  # True
~~~

这里要求 `accuracy` 同时满足大于等于 `0` 和小于等于 `1`。这种区间判断写法不能直接照搬到 C++ 中。

## 4. if 与缩进

### 4.1 基本结构

~~~python
accuracy = 0.92

if accuracy >= 0.90:
    print("准确率达到要求")
~~~

先判断条件是否成立，再决定是否执行代码块。

Python 的写法与 C++ 有两个关键区别：

- 条件后面使用英文冒号 `:`。
- 使用缩进表示代码块，通常每一级使用四个空格。

### 4.2 缩进决定代码属于哪个代码块

~~~python
accuracy = 0.85

if accuracy >= 0.90:
    print("准确率达到要求")
    print("这一行也属于 if")

print("判断结束")
~~~

输出：

~~~text
判断结束
~~~

两条缩进的语句属于 `if` 代码块，只有条件成立时才执行。

最后一条语句没有缩进，已经离开 `if` 代码块，不受这个条件控制。

**缩进是 Python 语法的一部分。** 同一个代码块中的语句应保持相同的缩进层级，避免混用制表符和空格。

## 5. if、elif 与 else

### 5.1 两个分支

~~~python
accuracy = 0.85

if accuracy >= 0.90:
    print("准确率达到要求")
else:
    print("需要继续改进")
~~~

条件成立时执行 `if` 分支，否则执行 `else` 分支。`else` 后面不写条件，但仍需要冒号。

### 5.2 多个分支

`elif` 对应 C++ 中的 `else if`。

~~~python
accuracy = 0.85

if accuracy >= 0.90:
    print("表现优秀")
elif accuracy >= 0.80:
    print("表现良好")
else:
    print("需要改进")
~~~

输出：

~~~text
表现良好
~~~

执行规则：

1. 从上到下检查条件。
2. 遇到第一个成立的条件，执行对应代码块。
3. 跳过同一组中剩余的分支。
4. 如果条件都不成立，有 `else` 就执行它。

**同一组 `if / elif / else` 最多执行一个分支。** 如果不写 `else`，也可能一个分支都不执行。

条件的排列顺序会影响结果。例如，判断准确率等级时，应先检查更高的阈值；否则 `0.95` 也会先满足 `accuracy >= 0.80`。

## 6. 逻辑运算

| Python | 对应 C++ | 含义 |
| --- | --- | --- |
| `and` | `&&` | 两个条件都成立 |
| `or` | `\|\|` | 至少一个条件成立 |
| `not` | `!` | 对条件取反 |

### 6.1 and：同时满足

~~~python
epochs = 10
learning_rate = 0.01

if epochs > 0 and learning_rate > 0:
    print("两个参数都大于零")
~~~

### 6.2 or：至少满足一个

~~~python
accuracy = 1.2

if accuracy < 0 or accuracy > 1:
    print("准确率超出范围")
~~~

### 6.3 not：取反

~~~python
use_gpu = False

if not use_gpu:
    print("当前未启用 GPU")
~~~

这里 `use_gpu` 是 `False`，所以 `not use_gpu` 是 `True`。

## 7. f-string 的百分数格式

~~~python
accuracy = 0.923456

print(f"{accuracy:.2f}")
print(f"{accuracy:.2%}")
~~~

输出：

~~~text
0.92
92.35%
~~~

| 格式 | 显示方式 |
| --- | --- |
| `:.2f` | 以小数形式显示，保留两位小数 |
| `:.2%` | 以百分数形式显示，保留两位小数 |

百分数格式会在显示时将数值乘以 `100`，并添加 `%`。

这两种写法都只影响显示格式，不会修改 `accuracy` 原来的值。

## 8. 综合示例：更新最佳验证准确率

~~~python
best_accuracy = 0.90

accuracy = float(input("请输入本次验证集准确率（0～1）："))

if accuracy < 0 or accuracy > 1:
    print("输入不合法，准确率应在 0 到 1 之间")
elif accuracy > best_accuracy:
    print("超过之前的最佳准确率")
    best_accuracy = accuracy
else:
    print("没有超过之前的最佳准确率")

print(f"当前最佳准确率：{best_accuracy:.2%}")
~~~

关键逻辑：

- `float(input(...))`：读取并转换输入。
- 第一个分支：检查输入数值是否超出范围。
- 第二个分支：只有超过原记录时，才更新最佳准确率。
- `else` 分支：输入未超过最佳记录时，保留原值。
- 最后一行：在所有分支结束后，输出当前最佳记录。

| 输入 | 执行情况 | 最终显示的最佳准确率 |
| --- | --- | --- |
| `0.95` | 超过原记录，更新最佳准确率 | `95.00%` |
| `0.85` | 未超过原记录 | `90.00%` |
| `1.2` | 超出合法范围 | `90.00%` |

每次重新运行程序，`best_accuracy` 都会从代码中设置的 `0.90` 开始，之前运行的结果不会自动保存。

如果输入 `hello` 等无法转换为浮点数的文字，会在 `float()` 处引发 `ValueError`，不会执行到后面的范围判断。

## 9. 常见易错点

| 易错点 | 正确理解 |
| --- | --- |
| 输入 `10` 就自动得到整数 | `input()` 返回字符串，需要显式转换 |
| 将 `int(3.9)` 和 `int("3.9")` 混淆 | 前者可以转换，后者引发 `ValueError` |
| 认为 `bool("False")` 是 `False` | 非空字符串转换为 `True` |
| 用 `=` 判断是否相等 | 比较使用 `==` |
| 忘记条件后面的冒号 | `if`、`elif`、`else` 后均需要冒号 |
| 将缩进当作排版习惯 | 缩进决定代码块的归属 |
| 将 `elif` 写成 `else if` | Python 使用 `elif` |
| 认为多个条件成立就会执行多个 `elif` 分支 | 同一组中只执行第一个匹配的分支 |
| 将 C++ 的 `&&`、`\|\|`、`!` 直接写进 Python | 使用 `and`、`or`、`not` |

