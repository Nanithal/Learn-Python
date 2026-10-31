# config = {

&#x20;   "model": "LeNet",

&#x20;   "epochs": 3,

&#x20;   "learning_rate": 0.01

}

history = [

&#x20;   {"epoch": 1, "val_accuracy": 0.82},

&#x20;   {"epoch": 2, "val_accuracy": 0.86},

&#x20;   {"epoch": 3, "val_accuracy": 0.84}

]

best_record = history[0]

for record in history:

&#x20;   if record["val_accuracy"] > best_record["val_accuracy"]:

&#x20;       best_record = record

print(f"模型：{config['model']}")

print(f"最佳轮次：{best_record['epoch']}")

print(f"最佳验证准确率：{best_record['val_accuracy']:.2%}")Day05｜Python 基础：字典与集合

## 1. 字典 dict：通过键组织数据

字典保存键值对，使用花括号 `{}`，每一项写作 `键: 值`。

```python
config = {
    "model": "LeNet",
    "epochs": 10,
    "learning_rate": 0.01,
    "use_gpu": False
}

print(type(config))
print(len(config))
```

输出：

```text
<class 'dict'>
4
```

例如，`"epochs": 10` 中：

- `"epochs"` 是键（key）。
- `10` 是对应的值（value）。
- `len(config)` 返回键值对的数量。

字典适合保存有名称的数据，例如实验配置、评价指标。其用途与 C++ 的 `unordered_map` 类似。

## 2. 读取字典中的值

### 2.1 使用方括号

```python
config = {
    "model": "LeNet",
    "epochs": 10,
    "learning_rate": 0.01
}

print(config["model"])
print(config["epochs"])
print(config["learning_rate"])
```

输出：

```text
LeNet
10
0.01
```

**字典通过键访问，列表主要通过位置索引访问。**

`config[0]` 查找的是整数键 `0`，不表示获取字典的第一项。

使用方括号访问不存在的键，会引发 `KeyError`。

### 2.2 get() 与默认值

```python
config = {"model": "LeNet", "epochs": 10}

batch_size = config.get("batch_size", 32)

print(batch_size)
print("batch_size" in config)
```

输出：

```text
32
False
```

基本形式：

```text
字典.get(键, 默认值)
```

| 情况         | 返回结果   |
| ---------- | ------ |
| 键存在        | 对应的值   |
| 键不存在，提供默认值 | 默认值    |
| 键不存在，省略默认值 | `None` |

**`get()` 返回默认值时，不会自动把该键加入字典。**

## 3. 添加、修改与删除

### 3.1 通过赋值添加或修改

```python
config = {"model": "LeNet", "epochs": 10}

config["epochs"] = 20
config["batch_size"] = 32

print(config)
```

输出：

```text
{'model': 'LeNet', 'epochs': 20, 'batch_size': 32}
```

- 键已经存在：修改对应的值。
- 键尚不存在：新增键值对。

**同一个字典不会重复保存相同的键。对同一个键再次赋值，会覆盖原值。**

### 3.2 使用 pop() 删除并返回值

```python
config = {
    "model": "LeNet",
    "epochs": 10,
    "use_gpu": False
}

removed = config.pop("use_gpu")

print(removed)
print(config)
```

输出：

```text
False
{'model': 'LeNet', 'epochs': 10}
```

`pop(key)` 在键不存在时引发 `KeyError`。可以提供默认值，避免因缺少该键而报错：

```python
config = {"model": "LeNet"}
removed = config.pop("batch_size", None)

print(removed)  # None
```

## 4. 成员检查与遍历

### 4.1 in 检查的是键

```python
config = {"model": "LeNet", "epochs": 10}

print("epochs" in config)      # True
print("batch_size" in config)  # False
print("LeNet" in config)       # False
```

直接对字典使用 `in`，检查的是键，不是值。

结合条件判断：

```python
config = {"model": "LeNet", "epochs": 10}

if "epochs" in config:
    print(f"训练轮数：{config['epochs']}")
```

外层字符串使用双引号，访问字符串键时使用单引号，便于区分。

### 4.2 同时遍历键和值

```python
config = {
    "model": "LeNet",
    "epochs": 10,
    "learning_rate": 0.01
}

for key, value in config.items():
    print(f"{key} = {value}")
```

输出：

```text
model = LeNet
epochs = 10
learning_rate = 0.01
```

`items()` 在遍历时提供键和值的配对，`key, value` 使用了解包。

| 写法                                 | 遍历时获得的内容 |
| ---------------------------------- | -------- |
| `for key in config`                | 键        |
| `for key in config.keys()`         | 键        |
| `for value in config.values()`     | 值        |
| `for key, value in config.items()` | 键和值      |

`keys()`、`values()` 和 `items()` 返回字典视图，可用于上述遍历，不是列表。

字典保留键的插入顺序，不会自动按键的大小排序。

## 5. 字典与列表、字典的嵌套

字典的值可以是列表，也可以是另一个字典。

```python
experiment = {
    "model": "LeNet",
    "losses": [0.90, 0.72, 0.55],
    "settings": {
        "epochs": 3,
        "learning_rate": 0.01
    }
}

print(experiment["losses"][-1])
print(experiment["settings"]["learning_rate"])
```

输出：

```text
0.55
0.01
```

按从左到右的顺序理解：

- `experiment["losses"][-1]`：先取出损失列表，再取最后一个元素。
- `experiment["settings"]["learning_rate"]`：先取出设置字典，再读取学习率。

这种组合方式可以把实验参数与指标记录组织在一起。

## 6. 集合 set：去重与成员检查

集合保存不重复的元素，不提供按位置索引的访问方式，也不保证遍历顺序。

```python
labels = [0, 1, 1, 2, 0, 2]
unique_labels = set(labels)

print(len(unique_labels))
print(1 in unique_labels)
print(3 in unique_labels)
```

输出：

```text
3
True
False
```

虽然原列表有六个元素，但只出现三种不同的标签。

### 创建集合

```python
classes = {0, 1, 2}
empty_set = set()
empty_dict = {}

print(type(classes))     # <class 'set'>
print(type(empty_set))   # <class 'set'>
print(type(empty_dict))  # <class 'dict'>
```

**`{}` 创建空字典；创建空集合使用 `set()`。**

集合不支持 `classes[0]` 这样的按位置访问。

### 添加和删除

```python
classes = {0, 1, 2}

classes.add(3)
classes.add(3)

print(len(classes))  # 4

classes.discard(2)
print(2 in classes)  # False
```

重复添加相同元素，不会产生重复项。

| 写法             | 作用                     |
| -------------- | ---------------------- |
| `s.add(x)`     | 添加元素                   |
| `s.remove(x)`  | 删除元素，不存在时引发 `KeyError` |
| `s.discard(x)` | 删除元素，不存在时也能正常执行        |
| `x in s`       | 检查元素是否存在               |
| `len(s)`       | 获取元素数量                 |

## 7. 交集、并集与差集

```python
a = {0, 1, 2}
b = {1, 2, 3}

print(sorted(a & b))
print(sorted(a | b))
print(sorted(a - b))
```

输出：

```text
[1, 2]
[0, 1, 2, 3]
[0]
```

| 运算       | 含义                  |
| -------- | ------------------- |
| `a & b`  | 交集：两边都包含的元素         |
| `a \| b` | 并集：至少一边包含的元素        |
| `a - b`  | 差集：在 a 中，但不在 b 中的元素 |

`sorted()` 返回排序后的新列表，可以让本例的整数结果按固定顺序展示，不会修改原集合。

### 检查样本编号是否重叠

```python
train_ids = {101, 102, 103, 104}
val_ids = {104, 105, 106}

overlap = train_ids & val_ids

if overlap:
    print(f"发现重复的样本编号：{sorted(overlap)}")
else:
    print("两个集合没有重叠")
```

输出：

```text
发现重复的样本编号：[104]
```

这里检查两个集合是否包含相同的样本编号。前提是编号能够准确标识样本。

## 8. 可哈希要求与容器的真假

### 8.1 字典的键、集合的元素需要可哈希

现阶段可以先使用字符串、整数等作为字典的键或集合的元素。

- 字典的键需要可哈希。
- 集合的元素需要可哈希。
- 列表不能直接作为字典的键或集合的元素，否则会引发 `TypeError`。
- 字典的值没有必须可哈希的要求，因此可以是列表或字典。

### 8.2 空容器在条件中视为 False

```python
print(bool([]))     # False
print(bool({}))     # False
print(bool(set()))  # False

print(bool([0]))          # True
print(bool({"loss": 0}))  # True
```

判断的是容器是否为空。只要容器非空，即使里面保存的是 `0`，容器本身也会被视为 `True`。

因此，可以使用 `if overlap:` 检查集合是否非空。

## 9. 综合示例：寻找最佳验证记录

```python
config = {
    "model": "LeNet",
    "epochs": 3,
    "learning_rate": 0.01
}

history = [
    {"epoch": 1, "val_accuracy": 0.82},
    {"epoch": 2, "val_accuracy": 0.86},
    {"epoch": 3, "val_accuracy": 0.84}
]

best_record = history[0]

for record in history:
    if record["val_accuracy"] > best_record["val_accuracy"]:
        best_record = record

print(f"模型：{config['model']}")
print(f"最佳轮次：{best_record['epoch']}")
print(f"最佳验证准确率：{best_record['val_accuracy']:.2%}")
```

输出：

```text
模型：LeNet
最佳轮次：2
最佳验证准确率：86.00%
```

数据结构：

| 对象               | 类型 | 用途        |
| ---------------- | -- | --------- |
| `config`         | 字典 | 保存实验参数    |
| `history`        | 列表 | 按顺序保存多轮记录 |
| `history` 中的每个元素 | 字典 | 保存某一轮的指标  |

`best_record` 最初关联到第一条记录，发现更高的验证准确率时，就让它关联到对应记录。这里的赋值不会自动复制字典。

该示例要求 `history` 非空，才能访问 `history[0]`。由于使用严格大于号，最高准确率出现多次时，会保留最先遇到的记录。

## 10. 常见易错点

| 易错点                    | 正确理解                         |
| ---------------------- | ---------------------------- |
| 将 `config[0]` 理解为获取第一项 | 它查找的是整数键 0                   |
| 用方括号读取不存在的键            | 会引发 KeyError，可按需求使用 get      |
| 认为 get 会插入默认值          | get 只读取，不会自动新增键              |
| 认为同一个键可以重复保存多份值        | 再次赋值会覆盖原值                    |
| 认为 `x in config` 会搜索值  | 它检查键                         |
| 认为字典会自动按键排序            | 字典保留插入顺序                     |
| 用 `{}` 创建空集合           | 它创建空字典，空集合使用 set()           |
| 依赖集合的索引或遍历顺序           | 集合不提供按位置索引，也不保证遍历顺序          |
| 混淆 remove 和 discard    | remove 在元素不存在时报错，discard 不报错 |
| 用列表作为字典键或集合元素          | 列表不可哈希，会引发 TypeError         |
| 将 `[0]` 当作空容器          | 它有一个元素，在条件中视为 True           |

