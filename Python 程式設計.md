# Turtle應用
## 繪圖工具實作

```python 
# 龜圖學練習畫紅心
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
