# 職訓局 尖兵-智慧機械與AI人工智慧技術應用
--- 
## Python 程式設計


- ## turtle 應用

 1. 繪圖工具實作
  
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

[程式實作示意] (Python 程式設計.md)

---

  2. 色弱測試
```python
# 老師模組.py
import random
import colorsys

def 給我顏色(飽和度, 亮度, 色差):
    '''輸入 L S 色差  可以得到二組[R,G,B]串列資料  這二個[R,G,B]有相同的色調  相同的飽和度  但是亮度不同 \n
    飽和度 :  0 到 1 之間的浮點數 \n
    亮度 : 0 到 1 之間的浮點數 \n
    色差 : 0 到 1 之間的浮點數 藉由此數字會去修改亮度值  所以不要給太大的數 \n
    return : 二個[R,G,B]包在一個元祖裡回傳給你  ([R,G,B],[R,G,B])
    '''

    # 產生接近的顏色
    h = random.random()
    sa = 飽和度
    l = 亮度

    base_rgb = colorsys.hls_to_rgb(h, l, sa)

    一般色 = []
    for item in base_rgb:
        一般色.append(int(item * 255))

    offset = random.choice([-色差, 色差])
    l = l + offset
    target_rgb = colorsys.hls_to_rgb(h, l, sa)

    特殊色 = []
    for item in target_rgb:
        特殊色.append(int(item * 255))

    return (一般色, 特殊色)

def 繪製圓角矩形(小烏龜,x,y,邊長,半徑) :
    '''
    小烏龜 : Turtle 物件 \n
    x : 座標 \n
    y : 座標 \n
    邊長 : 矩形的邊 \n
    半徑 : 圓角的半徑 \n
    return : 直接畫圖  沒有回傳東西
    '''
    小烏龜.goto(x, y)
    小烏龜.setheading(270)
    小烏龜.forward(邊長 / 2)
    小烏龜.setheading(0)

    小烏龜.down()
    小烏龜.begin_fill()
    小烏龜.forward(邊長 / 2 - 半徑)
    小烏龜.circle(半徑, 90)
    小烏龜.forward(邊長 - 半徑 * 2)
    小烏龜.circle(半徑, 90)
    小烏龜.forward(邊長 - 半徑 * 2)
    小烏龜.circle(半徑, 90)
    小烏龜.forward(邊長 - 半徑 * 2)
    小烏龜.circle(半徑, 90)
    小烏龜.forward(邊長 / 2 - 半徑)
    小烏龜.end_fill()
    小烏龜.up()
    return None
```
```python
import turtle
import random
import 老師模組

s1 = (turtle.Screen())  # 建構式  產生一個視窗物件
視窗寬度 = 視窗高度 = 600
s1.setup(視窗寬度, 視窗高度)
s1.bgcolor('white smoke')
s1.colormode(255)
s1.tracer(0)

t1 = turtle.Turtle()  # 建構式  產生一個小龜物件
t1.shape('turtle')
t1.turtlesize(2, 2)
t1.up()

def 畫出考題(個數):

    global x1,y1,x2,y2
    t1.clear()

    間格 = 8
    目標 = random.randint(1,個數**2)
    半徑 = 60 / 個數
    邊長 = (視窗寬度-間格*(個數+1))/個數

    一般色, 特殊色 = 老師模組.給我顏色(0.5,0.7,0.2)
    t1.pencolor(一般色)
    t1.fillcolor(一般色)

    計數 = 1
    for m in range(個數):  # 控制列
        for n in range(個數):  # 控制行
            if 計數 == 目標 :
                t1.pencolor(特殊色)
                t1.fillcolor(特殊色)
            else :
                t1.pencolor(一般色)
                t1.fillcolor(一般色)
            x = - 視窗寬度 / 2 + 間格 * (n + 1) + 邊長 / 2 * (2 * n + 1)
            y = - 視窗高度 / 2 + 間格 * (m + 1) + 邊長 / 2 * (2 * m + 1)
            老師模組.繪製圓角矩形(t1,x, y, 邊長, 半徑)
            if 計數 == 目標 :
                x1 = x - 邊長/2 - 半徑  # 左上角 x
                y1 = y + 邊長/2 + 半徑  # 左上角 y
                x2 = x + 邊長/2 + 半徑  # 右下角 x
                y2 = y - 邊長/2 - 半徑  # 右下角 y
            計數 += 1   #   -= 1  *= 2  /= 2

def 點擊事件(x,y) :
    global 個數
    print(x,y)
    if x1<x<x2 and y2<y<y1 :
        print('答對了')
        個數 += 1
        畫出考題(個數)

個數 = 2
x1 = x2 = y1 = y2 = 0
畫出考題(個數)
t1.hideturtle()
s1.onscreenclick(點擊事件)
s1.mainloop()
```
   3. 動畫實作
```python
import turtle

def 畫座標十字線(a) :
    '''
    畫座標十字線
    :param a: 是一個元組 (width,height)
    :return: 沒有回傳
    '''
    turtle.goto(0,0)
    turtle.setheading(0)
    turtle.pendown()
    turtle.forward(a[0]/2)
    turtle.left(180)
    turtle.forward(a[0])
    turtle.left(180)
    turtle.forward(a[0]/2)
    turtle.left(90)
    turtle.forward(a[1]/2)
    turtle.left(180)
    turtle.forward(a[1])
    turtle.left(180)
    turtle.forward(a[1]/2)
    turtle.left(270)
    turtle.penup()

w = 1000  # 視窗寬度
h = 700   # 視窗高度
turtle.setup(w,h)
turtle.bgcolor('royal blue')
turtle.hideturtle()
turtle.color("white", "white")
turtle.tracer(0)

x = 0
y = 0
xd = 0.5
yd = 0.7
r = 150
while True :
    turtle.clear()  #  螢幕擦乾淨
    畫座標十字線((w, h))
    turtle.penup()  #  筆拿起來
    turtle.goto(x,y)  #  走到指定的位置
    turtle.pendown()
    turtle.begin_fill()
    turtle.circle(r)
    turtle.end_fill()
    turtle.penup()  # 筆拿起來
    turtle.update()
    if not -w/2+r <= x <= w/2-r:
        xd = -xd
    x = x + xd
    if not  -h/2 <= y <= h/2-2*r:
        yd = -yd
    y = y + yd
turtle.done()
```
---


![程式執行結果](images/student_scores.png)
