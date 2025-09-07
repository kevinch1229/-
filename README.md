# -
職訓局 尖兵-智慧機械與AI人工智慧技術應用
# 大數據資料分析技術課程筆記

## 使用 Jupyter 寫程式

### 1. 先檢查並安裝模組
- pandas
- numpy
- matplotlib
- pillow

開發環境：IDLE / PyCharm / VSCode

### 2. 新的實驗介面
- Google Colab
- Jupyter Notebook

安裝指令：
```bash
pip install jupyter
pip install openpyxl
pip install lxml
pip list   # 查看已安裝模組
```

![模組安裝示意](images/jupyter_setup.png)

---

## 學生成績處理範例（版本 1）

```python
import datetime

f = open("學生資料表.txt", "r", encoding="big5")
data_string = f.read()
f.close()

lines = data_string.split("\n")
students = []

for line in lines:
    parts = line.split()
    student = {
        "姓名": parts[0],
        "學號": parts[1],
        "成績一": int(parts[2]),
        "成績二": int(parts[3]),
        "生日": parts[4]
    }
    students.append(student)

# 計算平均
for student in students:
    student["平均"] = (student["成績一"] + student["成績二"]) / 2

# 計算年齡
for student in students:
    birthday = datetime.datetime.strptime(student["生日"], "%Y/%m/%d")
    today = datetime.datetime.now()
    age = today.year - birthday.year
    if (today.month, today.day) < (birthday.month, birthday.day):
        age -= 1
    student["年齡"] = age

a = sorted(students, key=lambda x: (x["年齡"], x["平均"]), reverse=True)
for student in a:
    print(student)
```

---

## 學生成績處理範例（版本 2）

```python
import datetime

f = open("學生資料表.txt", "r", encoding="big5")
data_string = f.read()
f.close()

lines = data_string.split("\n")
students = []

for line in lines:
    if line.strip():
        parts = line.split()
        student = [
            parts[0],
            parts[1],
            int(parts[2]),
            int(parts[3]),
            parts[4]
        ]
        students.append(student)

# 計算平均
for student in students:
    student.append((student[2] + student[3]) / 2)

# 計算年齡
for student in students:
    birthday = datetime.datetime.strptime(student[4], "%Y/%m/%d")
    today = datetime.datetime.now()
    age = today.year - birthday.year
    if (today.month, today.day) < (birthday.month, birthday.day):
        age -= 1
    student.append(age)

# 輸出
for student in students:
    print(student)
```

![程式執行結果](images/student_scores.png)
