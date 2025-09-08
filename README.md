# 職訓局 尖兵-智慧機械與AI人工智慧技術應用
--- 
## Python 程式設計
<details>
    
<summary>turtle 應用</summary>

- 繪圖工具實作
```python
import turtle
import random
# 1. 創建 Screen 物件（視窗）
s1 = turtle.Screen()
s1.title("Python Turtle - 紅心 ❤️")  # 設定視窗標題
s1.bgcolor("white")  # 背景顏色
s1.setup(width=800, height=600)  # 設定畫布大小


# 2. 創建 Turtle 物件（畫筆）
pen = turtle.Turtle()
pen.speed(0)  # 畫筆速度（1=最慢，10=最快）
pen.color("violet")  # 畫筆顏色
pen.fillcolor("pink")  # 填充顏色
pen.pensize(3)  # 畫筆粗細
for n in range(20):
    
    scale = random.uniform(0.2, 0.8)
    x = random.randint(-400, 400)
    y = random.randint(-400, 400)
    pen.up()
    pen.goto(x, y)
    pen.setheading(0)
    pen.right(90)
    pen.fd(100*scale)
    pen.lt(90)
    pen.down()

    # 3. 開始繪製紅心
    pen.begin_fill()  # 開始填充

    # 繪製左半邊的曲線
    pen.left(47)
    pen.forward(250*scale)
    pen.circle(100*scale, 200)  # 畫半圓（半徑40，角度200）

    # 繪製右半邊的曲線
    pen.right(136)
    pen.circle(100*scale, 200)
    pen.forward(250*scale)

    pen.end_fill()  # 結束填充

    # 4. 隱藏畫筆並顯示結果
pen.hideturtle()

    # 5. 點擊視窗關閉程式
s1.exitonclick()

```
</details>

[程式實作示意](images/jupyter_setup.png)

---

### 色弱測試

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

### 學生成績處理範例（版本 2）

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
