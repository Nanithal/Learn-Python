# Day07｜Python 基础：模块导入、文件读写与 JSON 实验记录

## 1. 模块与 import

模块提供可以导入使用的代码，例如函数、常量等。一个 `.py` 文件通常也可以作为模块导入。

~~~python
import math

result = math.sqrt(9)
print(result)
~~~

输出：

~~~text
3.0
~~~

- `import math`：导入模块。
- `math.sqrt`：访问模块提供的平方根函数。
- `math.sqrt(9)`：调用函数。

### 常见导入形式

| 写法 | 使用方式 |
| --- | --- |
| `import math` | `math.sqrt(9)` |
| `from math import sqrt` | `sqrt(9)` |
| `import math as m` | `m.sqrt(9)` |

`from ... import ...` 将指定名称导入当前命名空间，`as` 用来设置别名。

~~~python
from math import sqrt
import math as m

print(sqrt(9))
print(m.sqrt(9))
~~~

两行都输出 `3.0`。

本课的 `math`、`json` 和 `pathlib` 都属于 Python 标准库，不需要额外安装。

## 2. 文本文件的写入与读取

### 2.1 基本写法

~~~python
with open("note.txt", "w", encoding="utf-8") as f:
    f.write("这是一次实验记录")

with open("note.txt", "r", encoding="utf-8") as f:
    content = f.read()

print(content)
~~~

输出：

~~~text
这是一次实验记录
~~~

| 部分 | 含义 |
| --- | --- |
| `"note.txt"` | 文件路径 |
| `"w"` | 写入模式 |
| `"r"` | 读取模式 |
| `encoding="utf-8"` | 使用 UTF-8 编码处理文字 |
| `as f` | 将文件对象交给变量 f |
| `f.write(...)` | 写入字符串 |
| `f.read()` | 读取当前位置之后剩余的全部内容，得到字符串 |

`with` 会管理文件的关闭。离开缩进的代码块时，文件会自动关闭。

### 2.2 常用文件模式

| 模式 | 作用 | 文件不存在时 |
| --- | --- | --- |
| `"r"` | 读取 | 引发 FileNotFoundError |
| `"w"` | 写入，清空已有内容 | 创建文件 |
| `"a"` | 在文件末尾追加 | 创建文件 |

**`"w"` 会覆盖已有内容，`"a"` 用于末尾追加。**

创建文件不会自动创建缺失的父目录，需要先处理目录。

### 2.3 换行需要显式写入

~~~python
with open("training_log.txt", "w", encoding="utf-8") as f:
    f.write("第1轮：loss = 0.90\n")
    f.write("第2轮：loss = 0.72\n")
~~~

`\n` 表示换行。`write()` 不会像 `print()` 一样自动在末尾添加换行。

## 3. 当前工作目录与脚本目录

### 3.1 相对路径基于当前工作目录

`"note.txt"` 这样的相对路径，相对于程序的当前工作目录解析，不一定相对于 `.py` 文件所在目录。

~~~python
from pathlib import Path

print(Path.cwd())
~~~

`Path.cwd()` 返回当前工作目录。在不同目录启动同一个脚本时，相同的相对路径可能指向不同位置。

### 3.2 获取脚本所在目录

在普通 `.py` 脚本中，可以使用：

~~~python
from pathlib import Path

script_dir = Path(__file__).resolve().parent
output_path = script_dir / "outputs" / "result.json"

print(output_path)
~~~

| 写法 | 含义 |
| --- | --- |
| `__file__` | 当前脚本文件的路径 |
| `Path(...)` | 创建路径对象 |
| `.resolve()` | 将路径解析为绝对路径 |
| `.parent` | 获取父目录 |
| `路径 / "名称"` | 拼接路径 |

这里的 `/` 是 Path 对象提供的路径拼接操作。

`__file__` 适用于上述脚本场景；在交互式终端或 Notebook 中，通常不能直接使用它来定位脚本目录。

### 3.3 创建输出目录

~~~python
from pathlib import Path

output_path = Path("outputs") / "result.json"
output_path.parent.mkdir(parents=True, exist_ok=True)
~~~

- `parents=True`：需要时一起创建上级目录。
- `exist_ok=True`：目录已经存在时也能正常执行。

`open()` 可以直接接收 Path 对象，不必先手动转换为字符串。

**文件扩展名不会自动决定内容格式。** 将普通文字写入一个名为 `result.json` 的文件，不会自动生成 JSON 数据。

## 4. JSON 与对象的保存

JSON 是一种文本数据格式，适合保存实验配置和基本指标记录。

- 序列化：将对象转换为可保存的形式。
- 反序列化：从保存的内容恢复对象。

### 4.1 使用 json.dump() 写入

~~~python
import json

experiment = {
    "model": "LeNet",
    "epochs": 3,
    "learning_rate": 0.01,
    "losses": [0.90, 0.72, 0.55]
}

with open("experiment.json", "w", encoding="utf-8") as f:
    json.dump(experiment, f, ensure_ascii=False, indent=2)
~~~

| 参数 | 作用 |
| --- | --- |
| `experiment` | 要保存的对象 |
| `f` | 已打开的文件对象 |
| `ensure_ascii=False` | 让中文等非 ASCII 字符直接显示 |
| `indent=2` | 使用两个空格缩进，便于阅读 |

`encoding="utf-8"` 控制文件的文字编码；`ensure_ascii=False` 控制 JSON 中非 ASCII 字符的展示方式，两者作用不同。

### 4.2 使用 json.load() 读取

运行前面的写入示例后，再运行：

~~~python
import json

with open("experiment.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

print(type(loaded))
print(loaded["model"])
print(loaded["losses"][-1])
~~~

输出：

~~~text
<class 'dict'>
LeNet
0.55
~~~

读取后得到的是 Python 对象，可以继续索引、遍历和计算。

| 写法 | 返回内容 |
| --- | --- |
| `f.read()` | 文件中的字符串 |
| `json.load(f)` | 根据 JSON 内容恢复的 Python 对象 |

## 5. JSON 支持的基本数据

| Python 数据 | JSON 表示 | 读取后常见的 Python 类型 |
| --- | --- | --- |
| 使用字符串键的字典 | 对象 | dict |
| 列表或元组 | 数组 | list |
| 字符串 | 字符串 | str |
| 普通整数、浮点数 | 数字 | int、float |
| `True`、`False` | `true`、`false` | bool |
| `None` | `null` | None |

注意：

- JSON 数组读取后是列表，不会保留 Python 元组这一特有类型。
- 保存配置时，字典键统一使用字符串最清楚。
- 不能假设任意 Python 对象都能直接通过 JSON 保存；本课使用基本类型及其组合。
- `json.dump()` 会将一次完整 JSON 数据写入文件。记录一份实验结果时，使用写入模式保存完整对象即可。

## 6. 综合示例：保存并读取实验结果

将下面的程序保存为 `day07.py`：

~~~python
import json
from pathlib import Path


def save_experiment(data, output_path):
    output_path.parent.mkdir(parents=True, exist_ok=True)

    with open(output_path, "w", encoding="utf-8") as f:
        json.dump(data, f, ensure_ascii=False, indent=2)


def load_experiment(input_path):
    with open(input_path, "r", encoding="utf-8") as f:
        return json.load(f)


def main():
    experiment = {
        "model": "LeNet",
        "epochs": 3,
        "learning_rate": 0.01,
        "losses": [0.90, 0.72, 0.55]
    }

    script_dir = Path(__file__).resolve().parent
    output_path = script_dir / "outputs" / "experiment.json"

    save_experiment(experiment, output_path)

    loaded = load_experiment(output_path)
    average_loss = sum(loaded["losses"]) / len(loaded["losses"])

    print(f"模型：{loaded['model']}")
    print(f"平均损失：{average_loss:.4f}")
    print(f"结果已保存到：{output_path}")


if __name__ == "__main__":
    main()
~~~

前两行输出：

~~~text
模型：LeNet
平均损失：0.7233
~~~

第三行显示实际保存路径。程序会在脚本同级的 `outputs` 目录中保存 `experiment.json`。

| 函数 | 职责 |
| --- | --- |
| `save_experiment()` | 创建输出目录并保存数据 |
| `load_experiment()` | 读取文件并返回对象 |
| `main()` | 组织整个程序的执行过程 |

本例保存函数的 `output_path` 参数需要传入 Path 对象，因为函数内部使用了它的 `.parent` 属性。

平均值计算要求损失列表非空。本例已经提供了三个损失值。

## 7. __name__ 与入口判断

综合示例末尾的：

~~~python
if __name__ == "__main__":
    main()
~~~

用于判断当前文件是否作为入口脚本直接运行。

| 运行方式 | 本例中的 __name__ |
| --- | --- |
| 直接运行 `day07.py` | `"__main__"` |
| 通过 `import day07` 导入 | `"day07"` |

- 直接运行时，条件成立，调用 `main()`。
- 作为模块导入时，条件不成立，不会通过这个代码块调用 `main()`。
- 导入后，其他代码仍可访问 `day07.save_experiment` 等函数。

**首次导入模块会执行其顶层代码。** 入口判断保护的是该条件代码块，并不代表整个文件的顶层代码都不会执行。综合示例把具体的保存与打印操作放在 `main()` 内，因此导入时不会自动执行这些操作。

`main` 是自己起的函数名，Python 不要求入口函数必须叫这个名字。真正控制调用的是末尾的条件判断。

## 8. 常见易错点

| 易错点 | 正确理解 |
| --- | --- |
| 导入方式改变后仍使用原来的名称 | 根据 import、from import、as 的写法访问 |
| 认为相对路径总是基于脚本目录 | 文件相对路径基于当前工作目录 |
| 把当前工作目录与脚本目录混为一谈 | 分别用 Path.cwd() 和脚本中的 __file__ 理解 |
| 认为 w 模式会追加内容 | w 会清空已有内容，a 用于末尾追加 |
| 认为创建文件会自动创建所有父目录 | 需要先创建缺失的父目录 |
| 认为 write 会自动换行 | 需要显式写入换行符 |
| 认为 json 扩展名会自动生成 JSON | 应使用 json.dump 等方式生成内容 |
| 把 f.read 的结果当作字典使用 | read 返回字符串，JSON 对象可用 json.load 恢复 |
| 认为 JSON 会保留元组类型 | JSON 数组读取后得到列表 |
| 在交互式环境中直接依赖 __file__ | 该名称通常不可用，应根据运行环境选择路径 |
| 认为导入模块不会执行任何代码 | 首次导入会执行模块顶层代码 |
| 认为 Python 自动寻找名为 main 的函数 | 本例通过入口判断显式调用 main |

