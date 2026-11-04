# Day06｜Python 基础：函数、参数、返回值与作用域

## 1. 函数的定义与调用

函数将一段有明确用途的代码组织起来，方便重复调用。例如，实验程序可以分别用函数计算指标、处理数据、寻找最佳记录。

~~~python
def add(a, b):
    result = a + b
    return result


total = add(3, 5)
print(total)
~~~

输出：

~~~text
8
~~~

| 部分 | 含义 |
| --- | --- |
| `def` | 定义函数的关键字 |
| `add` | 函数名 |
| `a, b` | 函数接收的参数 |
| `result = a + b` | 函数体中的操作 |
| `return result` | 返回计算结果 |
| `add(3, 5)` | 调用函数 |

基本结构：

~~~text
def 函数名(参数):
    函数体
    return 返回值
~~~

- 函数定义末尾需要英文冒号。
- 函数体通常缩进四个空格。
- 本例不需要声明参数类型和返回值类型。
- 定义函数时，不会立即执行函数体；调用时才执行。
- 执行到调用语句时，函数名需要已经定义。初学时可将定义放在调用之前。

执行 `add(3, 5)` 时，参数 `a`、`b` 分别关联到 `3`、`5`，计算结果通过 `return` 返回，并赋给 `total`。

## 2. return 与 print() 的区别

| 操作 | 作用 |
| --- | --- |
| `print(value)` | 将内容输出到屏幕 |
| `return value` | 将值交给调用这个函数的代码，并结束当前调用 |

只打印、不返回计算结果：

~~~python
def add(a, b):
    print(a + b)


result = add(3, 5)
print(result)
~~~

输出：

~~~text
8
None
~~~

第一行来自函数内部的打印。函数没有返回计算结果，调用结束后返回 `None`，因此 `result` 也是 `None`。

如果后续还要使用结果，应通过 `return` 返回：

~~~python
def add(a, b):
    return a + b


result = add(3, 5)
doubled = result * 2

print(doubled)
~~~

输出：

~~~text
16
~~~

本课的普通函数，如果执行到函数体末尾而没有返回值，会返回 `None`。单独写 `return` 也会返回 `None`。

## 3. 位置参数与关键字参数

定义一个线性计算函数：

$$
y = wx + b
$$

~~~python
def linear(x, weight, bias):
    return weight * x + bias


print(linear(2.0, 0.5, 0.1))
print(linear(x=2.0, weight=0.5, bias=0.1))
print(linear(bias=0.1, x=2.0, weight=0.5))
print(linear(2.0, weight=0.5, bias=0.1))
~~~

四次调用都输出：

~~~text
1.1
~~~

| 传参方式 | 匹配规则 | 示例 |
| --- | --- | --- |
| 位置参数 | 按位置依次对应 | `linear(2.0, 0.5, 0.1)` |
| 关键字参数 | 按参数名称匹配 | `linear(x=2.0, weight=0.5, bias=0.1)` |
| 混合使用 | 普通位置参数在前 | `linear(2.0, weight=0.5, bias=0.1)` |

关键字参数可以调整排列顺序，但普通调用中的位置参数应放在关键字参数之前。

同一个参数不能重复提供。例如，`linear(2.0, x=3.0, weight=0.5, bias=0.1)` 会给 `x` 提供两次值，引发 `TypeError`。

## 4. 默认参数

定义函数时，可以为部分参数提供默认值：

~~~python
def linear(x, weight=1.0, bias=0.0):
    return weight * x + bias


print(linear(2.0))
print(linear(2.0, weight=0.5))
print(linear(2.0, weight=0.5, bias=0.1))
~~~

输出：

~~~text
2.0
1.0
1.1
~~~

- 调用时省略某个可选参数，就使用它的默认值。
- 调用时提供该参数，就使用本次提供的值。
- `x` 没有默认值，因此本例调用时必须提供。

对于本课这种普通参数定义，**没有默认值的参数应放在有默认值的参数之前**。

## 5. 提前返回与 None

`return` 会立即结束当前这次函数调用。

例如，计算平均值之前先处理空列表：

~~~python
def calculate_average(values):
    if not values:
        return None

    return sum(values) / len(values)


print(calculate_average([1, 2, 3]))
print(calculate_average([]))
~~~

输出：

~~~text
2.0
None
~~~

传入空列表时，执行 `return None` 后就离开函数，不会继续执行除法。

### 使用 is None 检查缺失的结果

~~~python
def calculate_average(values):
    if not values:
        return None

    return sum(values) / len(values)


average = calculate_average([])

if average is None:
    print("没有可计算的数据")
else:
    print(f"平均值：{average:.2f}")
~~~

输出：

~~~text
没有可计算的数据
~~~

`is None` 用来判断对象是不是 `None`。

这里不应简单地用 `if not average` 判断没有结果，因为合法的平均值也可能为 `0`，而 `0` 在条件中同样被视为 `False`。

### return 与 break 的控制范围

| 语句 | 作用 |
| --- | --- |
| `break` | 离开当前所在的循环 |
| `return` | 结束当前函数调用，即使它位于循环内部 |

## 6. 返回多个值与元组解包

~~~python
def summarize_losses(losses):
    average_loss = sum(losses) / len(losses)
    minimum_loss = min(losses)

    return average_loss, minimum_loss


average, minimum = summarize_losses([0.9, 0.7, 0.5])

print(f"平均损失：{average:.2f}")
print(f"最小损失：{minimum:.2f}")

stats = summarize_losses([0.9, 0.7, 0.5])
print(type(stats))
~~~

输出：

~~~text
平均损失：0.70
最小损失：0.50
<class 'tuple'>
~~~

`return average_loss, minimum_loss` 实际返回一个包含两个元素的元组。

调用处的 `average, minimum = ...` 使用解包，分别接收两个元素。也可以只用一个变量接收整个元组。

本例中的平均值和最小值计算要求 `losses` 非空。

## 7. 局部变量与重新绑定参数

函数参数以及函数内部创建的普通局部变量，属于该次函数调用。

~~~python
def change_number(value):
    value = 100
    print(f"函数内部：{value}")


number = 10
change_number(number)

print(f"函数外部：{number}")
~~~

输出：

~~~text
函数内部：100
函数外部：10
~~~

调用开始时，参数名 `value` 与外部变量 `number` 都关联到整数 `10`。

执行 `value = 100` 后，只是重新绑定了局部名称 `value`，没有重新绑定外部的 `number`。

函数内部的普通局部变量不能直接通过那个名称在函数外访问。需要交给外部使用的结果，可以通过 `return` 返回。

## 8. 可变对象参数：修改列表会影响外部

~~~python
def append_loss(values, loss):
    values.append(loss)


losses = [0.9, 0.7]
append_loss(losses, 0.5)

print(losses)
~~~

输出：

~~~text
[0.9, 0.7, 0.5]
~~~

参数 `values` 和外部变量 `losses` 关联到同一个列表对象。通过参数修改这个对象，外部也能看到变化。

| 操作 | 含义 |
| --- | --- |
| `values = 新对象` | 重新绑定函数内的参数名 |
| `values.append(...)` | 修改参数所关联的列表对象 |

Python 不会因为把列表传入函数就自动复制它。是否修改传入的对象，应根据函数的用途明确设计。

## 9. 可变默认参数与 None 写法

**默认参数表达式在执行函数定义时求值，得到的对象会在后续调用之间复用。**

如果把会被修改的 `[]` 直接写成默认值，省略该参数的多次调用就可能共享同一个列表。

希望每次省略参数时创建新列表，可以使用 `None`：

~~~python
def record_loss(loss, history=None):
    if history is None:
        history = []

    history.append(loss)
    return history


print(record_loss(0.9))
print(record_loss(0.7))

losses = [0.9]
record_loss(0.7, history=losses)
print(losses)
~~~

输出：

~~~text
[0.9]
[0.7]
[0.9, 0.7]
~~~

两种情况：

- 省略 `history`：本次调用在函数体内创建一个新列表。
- 明确传入列表：修改传入的那个列表，并返回它。

因此，`history=None` 并不是要求函数只能接收 `None`；它只是设置省略参数时采用的默认值。

## 10. 综合示例：封装最佳验证记录查找

~~~python
def find_best_record(history, metric="val_accuracy"):
    if not history:
        return None

    best_record = history[0]

    for record in history:
        if record[metric] > best_record[metric]:
            best_record = record

    return best_record


history = [
    {"epoch": 1, "val_accuracy": 0.82},
    {"epoch": 2, "val_accuracy": 0.86},
    {"epoch": 3, "val_accuracy": 0.84}
]

best = find_best_record(history)

if best is None:
    print("没有可比较的记录")
else:
    print(f"最佳轮次：{best['epoch']}")
    print(f"最佳验证准确率：{best['val_accuracy']:.2%}")
~~~

输出：

~~~text
最佳轮次：2
最佳验证准确率：86.00%
~~~

| 代码 | 作用 |
| --- | --- |
| `history` 参数 | 接收记录列表 |
| `metric="val_accuracy"` | 设置默认比较指标 |
| `if not history` | 处理空列表 |
| `return None` | 没有记录时提前返回 |
| `record[metric]` | 读取每条记录中的指定指标 |
| `return best_record` | 将最佳记录交给调用处 |

`find_best_record(history)` 使用默认指标，也可以明确指定 `metric="val_accuracy"`。

这个函数按照**指标越大越好**进行比较，并要求每条记录都包含指定指标。它不能直接用于寻找最小损失。

最高指标出现多次时，因为使用严格大于号，会保留最先遇到的记录。

返回的字典就是原列表中对应的记录对象，没有自动复制。

## 11. 常见易错点

| 易错点 | 正确理解 |
| --- | --- |
| 认为定义函数后就会执行函数体 | 调用时才执行函数体 |
| 把 print 当作返回计算结果 | print 负责输出，return 将值交给调用处 |
| 漏写 return 后仍期待获得计算结果 | 本课普通函数走到末尾会返回 None |
| 把必填参数放在默认参数后面 | 本课普通参数定义中，必填参数应在前 |
| 普通调用中将位置参数放在关键字参数后面 | 位置参数应在前 |
| 给同一参数提供两次值 | 会引发 TypeError |
| 认为 return 只结束所在循环 | return 结束当前函数调用 |
| 用 `if not result` 判断结果一定不存在 | 合法结果可能为 0，缺失结果可用 is None 检查 |
| 认为返回多个值是多个独立返回对象 | 逗号分隔的返回值会组成元组 |
| 认为修改局部参数名会重新绑定外部变量 | 重新绑定参数名只影响局部名称 |
| 认为传入列表时会自动复制 | 参数可以关联到原列表对象 |
| 使用会被修改的列表作为默认值 | 默认对象会被复用，可用 None 在函数体内创建新列表 |

