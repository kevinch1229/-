# Python 物件導向程式設計 (OOP)
---
## 核心概念掌握
四大物件導向原則：

封裝 (Encapsulation)：有效隱藏內部實現細節，透過方法控制資料存取，提升程式碼安全性

繼承 (Inheritance)：熟練運用類別繼承機制，實現程式碼重用和層次化設計

多型 (Polymorphism)：實現不同類別對相同方法的不同行為，增強程式擴展性

抽象 (Abstraction)：專注於核心功能設計，簡化複雜系統的接口暴露

---
## 技術實踐能力
類別與物件設計
精通類別藍圖設計與物件實例化流程

熟練運用 __init__ 建構子進行物件初始化

有效管理物件屬性 (Attribute) 和方法 (Method)

繼承與多型實現
```python
# 繼承實踐範例
class Bulldog(Dog):
    def __init__(self, name, age, strength):
        super().__init__(name, age)  # 父類別構造呼叫
        self.strength = strength
    
    def bark(self):  # 方法覆寫 (Override)
        print(f"{self.name} says: GRRR!")
```
---
封裝與資料保護
```python
# 封裝實踐範例
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # 私有屬性保護
    
    def deposit(self, amount):
        self.__balance += amount  # 透過方法控制存取
    
    def get_balance(self):
        return self.__balance  # 安全存取接口
```
---

## 進階特性應用
類別方法 (@classmethod)：操作類別層級資料與行為

靜態方法 (@staticmethod)：實現與類別相關但獨立於實例的功能

屬性裝飾器 (@property)：提供更優雅的屬性存取控制

---

## 實際應用場景
設計可重用、易維護的類別庫架構

實現複雜業務邏輯的模組化封裝

構建層次清晰的系統組件關係

提升程式碼的可測試性和擴展性

---

## 範例程式修改(Turtle色弱測驗)
```python
import turtle
import random
import colorsys

def givecolor(飽和度, 亮度, 色差):
    '''

    輸入 L S 色差  可以得到二組[R,G,B]串列資料 這兩個[R,G,B]有相同的色調 飽和度 但是亮度不同
    :param 飽和度: 0 到 1 之間的浮點數\n
    :param 亮度: 0 到 1 之間的浮點數\n
    :param 色差: 0 到 1 之間的浮點數\n 藉由此數字會修改亮度值 不要給太大的數
    :return: 二個[R.G.B]
    '''
    h = random.random()
    sa = 飽和度
    l = 亮度

    base_rgb = colorsys.hls_to_rgb(h, l, sa)

    nmcolor = []
    for item in base_rgb:
        nmcolor.append(int(item * 255))

    offset = random.choice([-色差, 色差])
    l = l + offset
    target_rgb = colorsys.hls_to_rgb(h, l, sa)

    spcolor = []
    for item in target_rgb:
        spcolor.append(int(item * 255))

    return (nmcolor, spcolor)
#
def draw_rectangle(t1,x,y,length,radius) :
    '''

    :param t1: turtle物件
    :param x: 座標
    :param y: 座標
    :param length: 矩形邊
    :param radius: 圓角半徑
    :return: 直接畫圖 無回傳值
    '''
    t1.goto(x,y) #方框中心點
    t1.setheading(270)
    t1.forward(length/2)
    t1.setheading(0)

    t1.down()
    t1.begin_fill()
    t1.forward(length/2-radius)
    t1.circle(radius,90)
    t1.forward(length-radius*2)
    t1.circle(radius, 90)
    t1.forward(length - radius*2)
    t1.circle(radius, 90)
    t1.forward(length - radius*2)
    t1.circle(radius, 90)
    t1.forward(length - radius*2)
    t1.end_fill()
    t1.up()

class visioncolor_test() :

    def __init__(self,s1,t1):

        self.Grid = 2
        self.x1 = 0
        self.y1 = 0
        self.x2 = 0
        self.y2 = 0
        self.screen = s1
        self.turtle = t1
        self.width = 600
        self.height = 600
        self.screen.setup(self.width, self.height)
        self.screen.bgcolor('white smoke')
        self.screen.colormode(255)
        self.screen.tracer(0)
        self.screen.onscreenclick(self.click_event)
        self.turtle.shape('turtle')
        self.turtle.turtlesize(2, 2)
        self.turtle.up()

    def draw_question(self):

        self.turtle.clear()

        spacing = 8
        radius = 60 / self.Grid
        length = (self.width - spacing * (self.Grid + 1)) / self.Grid
        target = random.randint(1, self.Grid ** 2)

        nmcolor, spcolor = givecolor(0.5,0.7,0.1)
        self.turtle.pencolor(nmcolor)
        self.turtle.fillcolor(nmcolor)

        counts = 1

        for i in range(self.Grid):  # 控制列
            for n in range(self.Grid):  # 控制行
                if counts == target:
                    self.turtle.pencolor(spcolor)
                    self.turtle.fillcolor(spcolor)
                else:
                    self.turtle.pencolor(nmcolor)
                    self.turtle.fillcolor(nmcolor)

                x = -self.width / 2 + spacing * (n + 1) + length / 2 * (2 * n + 1)
                y = self.height / 2 - spacing * (i + 1) - length / 2 * (2 * i + 1)

                draw_rectangle(self.turtle,x, y, length, radius)
                if counts == target :
                    self.x1 = x - length / 2 - radius #左上角x
                    self.y1 = y + length / 2 + radius  # 左上角y
                    self.x2 = x + length / 2 + radius  # 右下角x
                    self.y2 = y - length / 2 - radius  # 右下角y
                counts += 1

    def click_event(self, x, y):
        print(x, y)
        if self.x1 < x < self.x2 and self.y2 < y < self.y1:
            print('right answer')
            self.Grid += 1
            self.draw_question()

s1 = turtle.Screen()  # 建構式  產生一個視窗物件
t1 = turtle.Turtle()  # 建構式  產生一個小龜物件
t1.hideturtle()
app = visioncolor_test(s1,t1)
app.draw_question()
s1.mainloop()
```
