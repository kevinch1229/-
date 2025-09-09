# 🧱 Python 基礎語法

---

## 📖 目錄
- 資料類型  
- 關鍵字  
- 模組導入  
- 序列型資料  
- 程式邏輯  
- 字串操作  
- 檔案讀寫  
- 函數定義與呼叫  
- 內建函數  
- 運算子優先級  
- 跳脫序列  
- 集合操作  
- 變數作用域  
- 字典操作  
- 字串格式化  
- 數學函式  
- 排序函式  
- 字串處理  
- 時間處理  
- 檔案讀取方法  
- 函數參數  
- 可變參數  
- 文件字串  
- 異常處理  

---
## 📊 資料類型
9種基礎資料類型  

| 類型 | 分類 | 說明 | 範例 |
|------|------|------|------|
| int, float | 原生 | 數值類型 | `10`, `3.14` |
| str | 原生 | 字串類型 | `"hello"` |
| list | 原生 | 可變序列 | `[1, 2, 3]` |
| tuple | 原生 | 不可變序列 | `(1, 2, 3)` |
| dict | 原生 | 鍵值對映射 | `{"a": 1, "b": 2}` |
| set | 原生 | 無序不重複集合 | `{1, 2, 3}` |
| Ndarray | 外來 (numpy) | 多維陣列 | `np.array([1, 2, 3])` |
| Series | 外來 (pandas) | 帶標籤的一維陣列 | `pd.Series([1, 2, 3])` |
| DataFrame | 外來 (pandas) | 二維表格數據結構 | `pd.DataFrame({"col": [1, 2, 3]})` |

---
## 🔑 Python 關鍵字
Python 的關鍵字是編程語言中保留的詞彙，具有特定的語法意義，不能用作變數名稱。

```python
# Python 3.10+ 關鍵字列表
keywords = [
    'False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await',
    'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except',
    'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is',
    'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try',
    'while', 'with', 'yield'
]
```
---
## 📦 模組導入

Python 使用 import 來導入模組，支援多種方式：

```python
# 導入整個模組
import math
print(math.sqrt(16))  # 4.0

# 導入模組中的特定函數
from math import sqrt
print(sqrt(25))  # 5.0

# 導入並取別名
import numpy as np
print(np.array([1, 2, 3]))

# 導入所有內容（不建議）
from math import *
print(cos(0))  # 1.0
```
---
## 🔢 序列型資料

Python 的序列型資料：list, tuple, range, str

```python
# list
nums = [1, 2, 3]
nums.append(4)

# tuple
coords = (10, 20)

# range
r = range(5)  # 0 ~ 4

# str
s = "hello"
print(s[1])  # 'e'
```
---
## 🧠 程式邏輯

條件判斷與迴圈語法：

```python
# if 判斷
x = 10
if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")

# for 迴圈
for i in range(3):
    print(i)

# while 迴圈
n = 3
while n > 0:
    print(n)
    n -= 1
```
---
## 📝 字串操作

```python
s = "python"
print(s.upper())   # PYTHON
print(s.lower())   # python
print(s.title())   # Python
print(s.replace("p", "P"))  # Python
print(" ".join(["I", "love", "Python"]))  # I love Python
```
---

## 📁 檔案讀寫

```python
# 寫入檔案
with open("test.txt", "w", encoding="utf-8") as f:
    f.write("Hello World")

# 讀取檔案
with open("test.txt", "r", encoding="utf-8") as f:
    print(f.read())
```
---
## 📞 函數定義與呼叫

```python
def add(a, b=0):
    return a + b

print(add(2, 3))  # 5
print(add(5))     # 5
```
---
## 🏗️ 內建函數

常用內建函數：
| 函數         | 功能   | 範例                  |
| ---------- | ---- | ------------------- |
| `len()`    | 計算長度 | `len("abc")`        |
| `max()`    | 最大值  | `max([1, 5, 2])`    |
| `min()`    | 最小值  | `min([1, 5, 2])`    |
| `sum()`    | 總和   | `sum([1, 2, 3])`    |
| `sorted()` | 排序   | `sorted([3, 1, 2])` |

---
## 🧮 運算子優先級
1.括號 ()

2.指數 **

3.乘除 * / // %

4.加減 + -

5.比較 == != < > <= >=

6.邏輯運算 not > and > or

---
## 🔤 跳脫序列
| 跳脫序列 | 功能   |
| ---- | ---- |
| `\n` | 換行   |
| `\t` | 水平定位 |
| `\\` | 反斜線  |
| `\'` | 單引號  |
| `\"` | 雙引號  |

---
## 🔄 集合操作

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # 聯集 {1,2,3,4,5}
print(a & b)  # 交集 {3}
print(a - b)  # 差集 {1,2}
```
---
## 🌐 變數作用域

```python
x = 10  # 全域變數

def foo():
    global x
    x = 20
foo()
print(x)  # 20
```
---
## 📚 字典操作

```python
d = {"a": 1, "b": 2}
print(d["a"])       # 1
print(d.get("c", 0))  # 0 (預設值)
d["c"] = 3
```
---
## 🎨 字串格式化

```python
name = "Tom"
age = 20
print(f"My name is {name}, age {age}")  
```
---
## 🧮 數學函式

```python
import math
print(math.sqrt(16))   # 4.0
print(math.ceil(3.2))  # 4
print(math.floor(3.8)) # 3
```
---
## 🔄 排序函式

```python
nums = [3, 1, 2]
print(sorted(nums))       # [1, 2, 3]
print(sorted(nums, reverse=True))  # [3, 2, 1]
```
---
## ✂️ 字串處理

```python
s = "   hello world   "
print(s.strip())     # "hello world"
print(s.split())     # ["hello", "world"]
```
---
## ⏰ 時間處理

```python
import time, datetime

print(time.time())  # 當前時間戳
print(datetime.date.today())  # 今天日期
```
---
## 📖 檔案讀取方法

```python
# 逐行讀取
with open("test.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())
```
---
## 🎯 函數參數

```python
def greet(name, msg="Hello"):
    print(f"{msg}, {name}")
greet("Tom")
```
---
## 🔄 可變參數

```python
def add_all(*args):
    return sum(args)
print(add_all(1,2,3))

def print_info(**kwargs):
    for k, v in kwargs.items():
        print(k, v)
print_info(name="Tom", age=20)
```
---
## 📘 文件字串

```python
def foo():
    """這是文件字串 (docstring) 範例"""
    pass
print(foo.__doc__)
```
---
## 🚨 異常處理

Python 的錯誤處理機制，避免程式因錯誤而中斷。

基本語法
```python
try:
    # 可能出錯的代碼
except 異常類型 as e:
    # 錯誤處理
else:
    # 沒錯誤時執行
finally:
    # 總會執行
```
---
常見異常
| 異常類型                | 說明     |
| ------------------- | ------ |
| `ValueError`        | 值錯誤    |
| `TypeError`         | 類型錯誤   |
| `IndexError`        | 索引超出範圍 |
| `KeyError`          | 字典鍵不存在 |
| `ZeroDivisionError` | 除以零    |
| `FileNotFoundError` | 檔案不存在  |

---
範例
```python
try:
    x = 10 / int(input("輸入數字: "))
except (ValueError, ZeroDivisionError) as e:
    print(f"錯誤: {e}")
else:
    print("運算成功")
finally:
    print("程式結束")
```
---
自訂與拋出異常
```python
class MyError(Exception):
    pass

raise MyError("這是自訂錯誤！")
```
