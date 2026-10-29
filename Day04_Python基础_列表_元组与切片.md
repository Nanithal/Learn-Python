# Day04｜Python 基础：列表、元组与切片

## 1. 列表 list

列表用于保存一组有顺序的数据，使用方括号 `[]`，元素之间用逗号分隔。

~~~python
losses = [0.9, 0.7, 0.5]

print(losses)
print(type(losses))
print(len(losses))
~~~

输出：

~~~text
[0.9, 0.7, 0.5]
<class 'list'>
3
~~~

- `losses` 是列表变量。
- 列表中的每项数据称为元素。
- `len(losses)` 返回元素数量。

创建空列表：

~~~python
losses = []
~~~

空列表常用于在程序运行过程中逐步收集数据，例如每一轮的训练损失。

## 2. 索引：访问和修改元素

### 2.1 正数与负数索引

索引从 `0` 开始，负数索引从末尾向前计数。

~~~python
losses = [0.9, 0.7, 0.5, 0.3, 0.1]

print(losses[0])   # 0.9，第一个元素
print(losses[2])   # 0.5，第三个元素
print(losses[-1])  # 0.1，最后一个元素
print(losses[-2])  # 0.3，倒数第二个元素
~~~

长度为 `n` 的非空列表：

- 非负索引范围为 `0～n-1`。
- 负数索引范围为 `-n～-1`。
- 单个索引超出范围会引发 `IndexError`。

例如，上面列表的 `losses[5]` 越界。空列表没有任何可直接访问的元素，`losses[-1]` 对空列表也会报错。

### 2.2 修改元素

~~~python
losses = [0.9, 0.7, 0.5]
losses[0] = 0.85

print(losses)
~~~

输出：

~~~text
[0.85, 0.7, 0.5]
~~~

列表支持通过索引替换元素。

## 3. 切片：一次取出多个元素

基本形式：

~~~text
列表[start:stop:step]
~~~

- `start`：起点。
- `stop`：终点。
- `step`：步长，默认为 `1`，不能为 `0`。
- **包含起点，不包含终点。**

以下示例使用：

~~~python
losses = [0.9, 0.7, 0.5, 0.3, 0.1]
~~~

| 写法 | 结果 | 含义 |
| --- | --- | --- |
| `losses[1:4]` | `[0.7, 0.5, 0.3]` | 取索引 1、2、3 |
| `losses[:3]` | `[0.9, 0.7, 0.5]` | 取前三个元素 |
| `losses[2:]` | `[0.5, 0.3, 0.1]` | 从索引 2 取到末尾 |
| `losses[-2:]` | `[0.3, 0.1]` | 取最后两个元素 |
| `losses[::2]` | `[0.9, 0.5, 0.1]` | 按步长 2 取元素 |
| `losses[::-1]` | `[0.1, 0.3, 0.5, 0.7, 0.9]` | 倒序取出全部元素 |
| `losses[:]` | `[0.9, 0.7, 0.5, 0.3, 0.1]` | 取出全部元素 |

读取列表切片会得到一个新列表，不会因这次读取而修改原列表。这里得到的是浅复制，嵌套对象的共享问题见后文。

### 单个索引与切片的越界区别

- `losses[100]`：引发 `IndexError`。
- `losses[:100]`：正常取到列表末尾。

切片边界超出列表范围时会进行调整，不会像单个索引那样直接报错。

## 4. 添加和删除元素

### 4.1 常用方法

| 写法 | 作用 |
| --- | --- |
| `values.append(x)` | 在末尾添加一个元素 x |
| `values.extend(other)` | 将另一个可迭代对象中的元素逐个添加到末尾 |
| `values.insert(i, x)` | 在位置 i 插入元素 x |
| `values.pop()` | 删除并返回最后一个元素 |
| `values.pop(i)` | 删除并返回索引 i 处的元素 |
| `values.remove(x)` | 删除第一个等于 x 的元素 |

`values.append(...)` 这种写法表示调用列表对象提供的 `append` 方法。

### 4.2 逐步记录数据

~~~python
losses = []

losses.append(0.9)
losses.append(0.7)
losses.append(0.5)

print(losses)
~~~

输出：

~~~text
[0.9, 0.7, 0.5]
~~~

### 4.3 append 与 extend 的区别

~~~python
a = [1, 2]
a.append([3, 4])
print(a)

b = [1, 2]
b.extend([3, 4])
print(b)
~~~

输出：

~~~text
[1, 2, [3, 4]]
[1, 2, 3, 4]
~~~

- `append()` 将整个参数作为一个元素添加。
- `extend()` 遍历参数，将里面的元素逐个添加。

### 4.4 pop 按位置删除，remove 按值删除

~~~python
values = [10, 20, 30]
removed = values.pop(1)

print(removed)
print(values)
~~~

输出：

~~~text
20
[10, 30]
~~~

这里 `pop(1)` 的参数是索引。若改为 `remove(20)`，参数 `20` 则是要查找的值。

- `pop()` 在空列表上调用，或指定索引越界时，引发 `IndexError`。
- `remove(x)` 找不到指定值时，引发 `ValueError`。

### 4.5 不要将 append 的返回值赋给原变量

~~~python
losses = [0.9, 0.7]
result = losses.append(0.5)

print(losses)
print(result)
~~~

输出：

~~~text
[0.9, 0.7, 0.5]
None
~~~

`append()` 直接修改原列表，返回值为 `None`。

因此应写 `losses.append(0.5)`。如果写成 `losses = losses.append(0.5)`，变量 `losses` 最后会关联到 `None`。

## 5. 元组 tuple

元组也能保存一组有顺序的数据，通常使用圆括号表示。

~~~python
image_shape = (3, 32, 32)

print(image_shape)
print(type(image_shape))
print(image_shape[0])
print(image_shape[1:])
print(len(image_shape))
~~~

输出：

~~~text
(3, 32, 32)
<class 'tuple'>
3
(32, 32)
3
~~~

例如，可以约定这三个值分别表示图像的通道数、高度和宽度。

元组支持索引、切片和 `len()`，但**不支持通过索引重新赋值**，也没有 `append()` 这种原地增加元素的方法。

对上述元组执行 `image_shape[0] = 1`，会引发 `TypeError`。

可以重新赋值：

~~~python
image_shape = (3, 32, 32)
image_shape = (1, 28, 28)
~~~

这会让变量关联到一个新元组，并没有修改原来的元组。

这里的不可修改针对元组保存的元素引用；如果元组包含列表，那个列表对象本身仍然可以修改。

### 单元素元组需要逗号

~~~python
a = (32)
b = (32,)

print(type(a))  # <class 'int'>
print(type(b))  # <class 'tuple'>
~~~

**形成单元素元组的关键是逗号。** 空元组写作 `()`。

## 6. 解包

解包可以把序列中的多个元素分别赋给多个变量。

~~~python
image_shape = (3, 32, 32)
channels, height, width = image_shape

print(channels)  # 3
print(height)    # 32
print(width)     # 32
~~~

本例中，左边三个变量与右边三个元素一一对应。

普通解包要求两边数量匹配，否则会引发 `ValueError`。列表也支持相同的解包方式。

## 7. 遍历列表与 enumerate()

### 7.1 直接遍历元素

~~~python
losses = [0.9, 0.7, 0.5]

for loss in losses:
    print(loss)
~~~

每次循环，`loss` 都会获得列表中的一个元素。

### 7.2 同时获取编号和元素

~~~python
losses = [0.9, 0.7, 0.5]

for epoch, loss in enumerate(losses, start=1):
    print(f"第 {epoch} 轮，loss = {loss:.2f}")
~~~

输出：

~~~text
第 1 轮，loss = 0.90
第 2 轮，loss = 0.70
第 3 轮，loss = 0.50
~~~

`enumerate()` 每次提供一对值：编号和元素。通过 `epoch, loss` 进行解包，分别接收这两个值。

- 编号默认从 `0` 开始。
- `start=1` 表示编号从 `1` 开始。
- 改变编号起点不会改变列表中的元素。

## 8. 赋值、复制与共享对象

### 8.1 赋值不会自动复制列表

~~~python
a = [1, 2, 3]
b = a

b[0] = 99

print(a)
print(b)
~~~

输出：

~~~text
[99, 2, 3]
[99, 2, 3]
~~~

`a` 和 `b` 关联到同一个列表对象。通过其中一个变量修改该对象，另一个变量也能看到变化。

### 8.2 copy() 创建新的外层列表

~~~python
a = [1, 2, 3]
b = a.copy()

b[0] = 99

print(a)
print(b)
~~~

输出：

~~~text
[1, 2, 3]
[99, 2, 3]
~~~

此时 `a` 和 `b` 是两个不同的外层列表，替换 `b[0]` 不会替换 `a[0]`。

`copy()` 是浅复制：外层列表是新的，里面引用的对象没有递归复制。如果元素本身也是列表，修改那个内层列表，仍可能影响双方看到的内容。`a[:]` 也具有相同的浅复制特征。

## 9. 列表的加号表示拼接

~~~python
print([1, 2] + [3, 4])
~~~

输出：

~~~text
[1, 2, 3, 4]
~~~

列表相加表示把两个列表连接起来，不会进行逐元素数值相加。

以后使用 NumPy 数组或 PyTorch 张量时，应根据对象类型理解运算规则。

## 10. 综合示例：统计训练损失

~~~python
losses = [0.90, 0.72, 0.55, 0.43]

for epoch, loss in enumerate(losses, start=1):
    print(f"第 {epoch} 轮，loss = {loss:.2f}")

average_loss = sum(losses) / len(losses)
best_loss = min(losses)
last_loss = losses[-1]

print(f"平均损失：{average_loss:.2f}")
print(f"最小损失：{best_loss:.2f}")
print(f"最后一轮损失：{last_loss:.2f}")
~~~

输出：

~~~text
第 1 轮，loss = 0.90
第 2 轮，loss = 0.72
第 3 轮，loss = 0.55
第 4 轮，loss = 0.43
平均损失：0.65
最小损失：0.43
最后一轮损失：0.43
~~~

| 写法 | 作用 |
| --- | --- |
| `len(losses)` | 获取元素数量 |
| `sum(losses)` | 对数值元素求和 |
| `min(losses)` | 获取最小值 |
| `losses[-1]` | 获取最后一个元素 |

该示例要求列表非空。空列表长度为 `0`，不能用于这里的平均值计算；`min([])` 引发 `ValueError`，访问空列表的 `[-1]` 引发 `IndexError`。

## 11. 常见易错点

| 易错点 | 正确理解 |
| --- | --- |
| 将第一个元素的索引写成 1 | 索引从 0 开始 |
| 认为切片包含终点 | 切片包含起点，不包含终点 |
| 混淆单个索引和切片的越界行为 | 单个索引越界报错，切片边界会调整 |
| 将 append 和 extend 混淆 | 前者添加一个元素，后者逐个添加元素 |
| 将 pop 和 remove 混淆 | pop 按索引，remove 按值 |
| 写成 `values = values.append(x)` | append 返回 None，应直接调用 |
| 将 `(32)` 当作元组 | 单元素元组应写成 `(32,)` |
| 认为元组可以通过索引替换元素 | 这种操作会引发 TypeError |
| 解包时变量数量与元素数量不一致 | 普通解包需要数量匹配 |
| 认为 `b = a` 会复制列表 | 两个变量关联同一个对象 |
| 认为浅复制会递归复制所有内层对象 | 浅复制创建新外层，内层对象仍可能共享 |
| 将列表的加号理解为逐元素加法 | 列表的加号表示拼接 |

