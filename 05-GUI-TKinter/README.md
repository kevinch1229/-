# TKinter基本運用
---

## Pack物件佈局
![圖片24](../assets/images/turtle24.png)
```python
import tkinter as tk

root = tk.Tk()
root.title('Pack Demo')
root.geometry("1000x800")

frame1 = tk.Frame(root, pady=10, padx=10, bg='#aaaaaa')   # 第一個 Frame 元件
frame2 = tk.Frame(root, pady=10, padx=10, bg='#bbbbbb')   # 第二個 Frame 元件
frame3 = tk.Frame(root, pady=10, padx=10, bg='#cccccc')   # 第三個 Frame 元件
frame1.pack(expand=True, fill=tk.BOTH, padx=10, pady=10  )
frame2.pack(expand=True, fill=tk.BOTH, padx=10, pady=10  )
frame3.pack(expand=True, fill=tk.BOTH, padx=10, pady=10  )

# box 1
box1 = tk.Label(frame1, text="Box 1", bg="red", fg="white")
box1.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.NW,  expand=True)

# box 2
box2 = tk.Label(frame1, text="Box 2", bg="red", fg="white")
box2.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.N, expand=True)

# box 3
box3 = tk.Label(frame1, text="Box 3", bg="red", fg="white")
box3.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.NE, expand=True)

# box 4
box4 = tk.Label(frame2, text="Box 4", bg="green", fg="white")
box4.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.W,  expand=True)

# box 5
box5 = tk.Label(frame2, text="Box 5", bg="green", fg="white")
box5.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.CENTER, expand=True)

# box 6
box6 = tk.Label(frame2, text="Box 6", bg="green", fg="white")
box6.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.E, expand=True)

# box 7
box7 = tk.Label(frame3, text="Box 7", bg="blue", fg="white")
box7.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.SW,  expand=True)

# box 8
box8 = tk.Label(frame3, text="Box 8", bg="blue", fg="white")
box8.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.S, expand=True)

# box 9
box9 = tk.Label(frame3, text="Box 9", bg="blue", fg="white")
box9.pack(side=tk.LEFT, ipadx=20, ipady=20, anchor=tk.SE, expand=True)


root.mainloop()
```
---

## 使用者登入UI
![圖片25](../assets/images/turtle25.png)
```python
import tkinter as tk
from tkinter import ttk

root = tk.Tk()
root.title('Login')
root.geometry("320x200")


fields = {}

fields['username_label'] = ttk.Label(root, text='Username:')
fields['username'] = ttk.Entry(root)

fields['password_label'] = ttk.Label(root, text='Password:')
fields['password'] = ttk.Entry(root, show="*")


for field in fields.values():
    field.pack(anchor=tk.W, padx=10, pady=5, fill=tk.X)

ttk.Button(text='Login').pack(anchor=tk.W, padx=10, pady=10)

root.mainloop()
```
---

## Youtube下載器
![圖片26](../assets/images/turtle26.png)
```python
# 下載YT影片
import os
from yt_dlp import YoutubeDL

# 輸入 YouTube 影片的 URL
url = input("請輸入 YouTube 影片的 URL: ")
a = input("1)mp4   2)mp3   請輸入: ")

if a == '1' :
    # 設定下載選項
    ydl_opts = {
        'format': 'bestvideo[height<=1080][vcodec^=avc1]+bestaudio/best[height<=1080][vcodec^=avc1]',
        'outtmpl': '%(title)s.%(ext)s',  # 輸出檔案名稱格式
        # 'outtmpl': 'down.mp4',  # 固定輸出檔名為 down.mp4
        'merge_output_format': 'mp4',  # 合併影音時使用 mp4 格式
        'quiet': True,  # 避免輸出過多資訊
    }

    try:
        with YoutubeDL(ydl_opts) as ydl:
            info_dict = ydl.extract_info(url, download=False)  # 先獲取影片資訊
            filename = ydl.prepare_filename(info_dict)  # 取得預設檔名
            print(f"即將下載的檔案名稱: {filename}")

            ydl.download([url])  # 開始下載
            print(f"下載完成！檔案名稱: {filename}")
    except Exception as e:
        print(f"下載影片時發生錯誤: {e}")


    # https://youtu.be/XqJQlNGjdI8?si=5-he5Yzd-1cV2pVC

    # os.rename(filename,'down.mp4')
    # os.system('ffmpeg -i down.mp4 -c:v copy -c:a aac -b:a 192k output.mp4')
    # os.rename('output.mp4', filename)
    # os.remove('down.mp4')

else :
    ############################################################################################################

    # 下載 mp3 格式  只有聲音，音樂
    # 設定下載選項
    ydl_opts = {
        'format': 'bestaudio/best',  # 選擇最佳音訊品質
        'outtmpl': '%(title)s.%(ext)s',  # 輸出檔案名稱格式
        'postprocessors': [{  # 使用後處理器將音訊轉換為 MP3
            'key': 'FFmpegExtractAudio',  # 提取音訊
            'preferredcodec': 'mp3',       # 轉換為 MP3 格式
            'preferredquality': '192',     # 音訊品質 (可選，預設為 192kbps)
        }],
    }

    # 下載影片
    with YoutubeDL(ydl_opts) as ydl:
        ydl.download([url])
```
---

## 簡易塗鴉畫板
![圖片27](../assets/images/turtle27.png)
```python
# 塗鴉_使用_canvas

import tkinter as tk
from tkinter import colorchooser


class SimpleDrawingApp:
    def __init__(self, root):
        self.root = root
        self.root.title("簡易塗鴉畫板")
        self.root.geometry("800x600")

        # 繪圖變數
        self.drawing = False
        self.last_x = None
        self.last_y = None
        self.brush_size = 5
        self.brush_color = "black"
        self.eraser_mode = False

        # 創建主框架
        self.main_frame = tk.Frame(root)
        self.main_frame.pack(fill=tk.BOTH, expand=True)

        # 創建工具列
        self.create_toolbar()

        # 創建畫布
        self.canvas = tk.Canvas(self.main_frame, bg="white",
                                width=800, height=550, cursor="cross")
        self.canvas.pack(fill=tk.BOTH, expand=True, padx=10, pady=10)

        # 綁定滑鼠事件
        self.canvas.bind("<Button-1>", self.start_drawing)
        self.canvas.bind("<B1-Motion>", self.draw)
        self.canvas.bind("<ButtonRelease-1>", self.stop_drawing)

    def create_toolbar(self):
        """創建簡單的工具列"""
        toolbar = tk.Frame(self.main_frame, bg="lightgray", height=40)
        toolbar.pack(side=tk.TOP, fill=tk.X, padx=5, pady=5)

        # 顏色選擇按鈕
        colors = ["black", "red", "green", "blue", "yellow", "purple", "orange"]
        for color in colors:
            btn = tk.Button(toolbar, bg=color, width=2, height=1,
                            command=lambda c=color: self.set_color(c))
            btn.pack(side=tk.LEFT, padx=2)

        # 自訂顏色按鈕
        custom_btn = tk.Button(toolbar, text="更多顏色", command=self.choose_custom_color)
        custom_btn.pack(side=tk.LEFT, padx=5)

        # 畫筆/橡皮擦切換
        self.eraser_btn = tk.Button(toolbar, text="畫筆", command=self.toggle_eraser)
        self.eraser_btn.pack(side=tk.LEFT, padx=5)

        # 畫筆大小調整
        size_label = tk.Label(toolbar, text="大小:")
        size_label.pack(side=tk.LEFT, padx=5)

        self.size_scale = tk.Scale(toolbar, from_=1, to=20, orient=tk.HORIZONTAL,
                                   length=100, command=self.update_brush_size)
        self.size_scale.set(self.brush_size)
        self.size_scale.pack(side=tk.LEFT, padx=5)

        # 清除按鈕
        clear_btn = tk.Button(toolbar, text="清除畫布", command=self.clear_canvas)
        clear_btn.pack(side=tk.LEFT, padx=5)

    def set_color(self, color):
        """設置畫筆顏色"""
        self.brush_color = color
        self.eraser_mode = False
        self.eraser_btn.config(text="畫筆")

    def choose_custom_color(self):
        """選擇自訂顏色"""
        color = colorchooser.askcolor(title="選擇顏色", initialcolor=self.brush_color)
        if color[1]:
            self.set_color(color[1])

    def update_brush_size(self, value):
        """更新畫筆大小"""
        self.brush_size = int(value)

    def toggle_eraser(self):
        """切換橡皮擦模式"""
        self.eraser_mode = not self.eraser_mode
        if self.eraser_mode:
            self.eraser_btn.config(text="橡皮擦")
        else:
            self.eraser_btn.config(text="畫筆")

    def start_drawing(self, event):
        """開始繪圖"""
        self.drawing = True
        self.last_x = event.x
        self.last_y = event.y

    def draw(self, event):
        """繪圖過程"""
        if self.drawing and self.last_x is not None and self.last_y is not None:
            if self.eraser_mode:
                # 橡皮擦模式 - 用白色畫筆覆蓋
                self.canvas.create_line(
                    self.last_x, self.last_y, event.x, event.y,
                    fill="white", width=self.brush_size * 2,
                    capstyle=tk.ROUND, smooth=True
                )
            else:
                # 畫筆模式
                self.canvas.create_line(
                    self.last_x, self.last_y, event.x, event.y,
                    fill=self.brush_color, width=self.brush_size,
                    capstyle=tk.ROUND, smooth=True
                )

            self.last_x = event.x
            self.last_y = event.y

    def stop_drawing(self, event):
        """停止繪圖"""
        self.drawing = False
        self.last_x = None
        self.last_y = None

    def clear_canvas(self):
        """清除畫布"""
        self.canvas.delete("all")


# 啟動應用程式
if __name__ == "__main__":
    root = tk.Tk()
    app = SimpleDrawingApp(root)
    root.mainloop()
```
---

## Turtle-彈跳球動畫
![圖片28](../assets/images/turtle28.png)
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

## Canvas-彈跳球動畫
![圖片29](../assets/images/turtle29.png)
```python
# 動畫簡介_使用canvas

import tkinter as tk
import math


def draw_coordinate_cross(canvas, width, height):
    """
    在畫布上繪製座標十字線
    :param canvas: Tkinter 畫布對象
    :param width: 畫布寬度
    :param height: 畫布高度
    """
    # 繪製水平線
    canvas.create_line(0, height / 2, width, height / 2, fill="white", width=1)
    # 繪製垂直線
    canvas.create_line(width / 2, 0, width / 2, height, fill="white", width=1)


def update_animation():
    """
    更新動畫幀
    """
    global x, y, xd, yd

    # 清除畫布
    canvas.delete("all")

    # 繪製背景
    canvas.create_rectangle(0, 0, WIDTH, HEIGHT, fill="royal blue", outline="")

    # 繪製座標十字線
    draw_coordinate_cross(canvas, WIDTH, HEIGHT)

    # 繪製圓形
    canvas.create_oval(x - 50, y - 50, x + 50, y + 50, fill="white", outline="")

    # 更新位置
    x += xd
    y += yd

    # 檢查邊界碰撞
    if not (WIDTH / 2 - 550) <= x <= (WIDTH / 2 + 550):
        xd = -xd

    if not (HEIGHT / 2 - 300) <= y <= (HEIGHT / 2 + 300):
        yd = -yd

    # 安排下一幀更新
    root.after(10, update_animation)


# 設定視窗大小
WIDTH, HEIGHT = 1200, 800

# 初始化視窗
root = tk.Tk()
root.title("彈跳球動畫")
root.resizable(False, False)

# 創建畫布
canvas = tk.Canvas(root, width=WIDTH, height=HEIGHT, bg="royal blue")
canvas.pack()

# 初始化球的位置和速度
x = WIDTH / 2  # 初始位置在中心
y = HEIGHT / 2
xd = 2
yd = 1

# 啟動動畫
update_animation()

# 啟動主循環
root.mainloop()
```
---

## Canvas-版面安排
![圖片30](../assets/images/turtle30.png)
```python
import tkinter as tk# 導入圖形界面庫

root = tk.Tk()# 創建主視窗
root.geometry('700x700')# 設定視窗大小
root.title('Canvas Demo - Binding Event')# 設定視窗標題
root.config(bg='gray80')# 設定視窗背景色# 創建頂部框架容器（用於放置按鈕）
frame1 = tk.Frame(root, bg='gray80')# 框架背景與視窗一致
frame1.pack(side=tk.TOP, anchor=tk.W)# 放置於頂部，靠左對齊# 創建第一個按鈕
button1 = tk.Button(
    frame1,# 放在frame1框架內
    text='AAA',# 按鈕文字
    font=("Helvetica", 14)# 字體設定)
button1.pack(side=tk.LEFT, padx=10, pady=10)# 水平排列，添加邊距# 創建第二個按鈕
button2 = tk.Button(
    frame1,
    text='BBB',
    font=("Helvetica", 14)
)
button2.pack(side=tk.LEFT, padx=10, pady=10)# 繼續水平排列# 創建第三個按鈕
button3 = tk.Button(
    frame1,
    text='CCC',
    font=("Helvetica", 14)
)
button3.pack(side=tk.LEFT, padx=10, pady=10)# 繼續水平排列# 創建畫布元件
canvas = tk.Canvas(root,
                   width=500,# 畫布內部繪圖區域寬度
                   height=500,# 畫布內部繪圖區域高度
                   bg='gray80')# 畫布背景色
canvas.pack(
            expand=True# 允許畫布擴展填滿剩餘空間)

root.mainloop()# 啟動主循環
```
---
