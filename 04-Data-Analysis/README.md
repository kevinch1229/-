# 大數據分析基本應用
---
## 字串解析並存入字典結構
![圖片04](../assets/images/turtle04.png)
```python
import datetime

f = open('學生資料表.txt', 'r', encoding="big5")
# print(type(f)) # <class '_io.TextIOWrapper'>
# print(dir(f))
data_string = f.read() # 變成一個很大的字串
# print(repr(a))
f.close()

# 利用跳行轉義字符\n切割字串
lines = data_string.split('\n')
# print(lines)

# 创建字典列表
students = []
for line in lines:
    if line.strip():  # 如果是空行就跳過
        parts = line.split()
        # 处理姓名可能包含空格的情况（虽然本例中没有）
        # 假设格式固定为：姓名 学号 成绩一 成绩二 生日
        name = parts[0]
        student_id = parts[1]
        score1 = int(parts[2])
        score2 = int(parts[3])
        birthday = parts[4]

        student = {
            '姓名': name,
            '學號': student_id,
            '成績一': score1,
            '成績二': score2,
            '生日': birthday
        }
        students.append(student)

# 動態新增平均成績欄位
for student in students:
    student['平均'] = ( student['成績一'] + student['成績二'] ) / 2
    #print(student)

# 動態新增年齡欄位
for student in students:
    # step1. 把生日字串變成日期結構  'strftime'日期變字串, 'strptime'字串變日期
    我的生日 = datetime.datetime.strptime(student['生日'],'%Y/%m/%d')
    # step2. 得到今天日期結構
    今天日期 = datetime.datetime.now()
    # step3. 算年齡
    age = 今天日期.year - 我的生日.year
    # step4. 如果生日還沒過  年齡要減一
    if 今天日期 < datetime.datetime(今天日期.year,我的生日.month,我的生日.day) :
        age -= 1
    student['年齡'] = age

a = sorted(students,key=lambda x:(x['年齡'], x['平均']),reverse=True)
for student in a:
    print(student)
```
## Numpy-灰階影像輸出
![圖片05](../assets/images/turtle05.png)
```python
import matplotlib.pyplot as plt
import numpy as np

# 轉換為 NumPy 陣列，並指定為 uint8 類型（0-255）
array = np.array(s, dtype=np.uint8)

# 顯示灰階圖像
plt.imshow(array, cmap='gray', vmin=0, vmax=255)  # cmap='gray' 指定灰階
plt.title("Grayscale Image")
plt.show()
```
---
## Numpy-色階輸出
![圖片06](../assets/images/turtle06.png)
```python
hex_color = ["#FED172","#F3742B","#B83A14","#612E37","#231650"]

rgb_color = []
for item in hex_color :
    # 去除 # 並拆分通道
    r = int(item[1:3], 16)  # 紅色
    g = int(item[3:5], 16)  # 綠色
    b = int(item[5:7], 16)  # 藍色
    rgb_color.append((r, g, b))
    
arr = np.empty((300,500,3), dtype=np.uint8)
arr[ : ,     : 100 , : ] = rgb_color[0]
arr[ : , 100 : 200 , : ] = rgb_color[1]
arr[ : , 200 : 300 , : ] = rgb_color[2]
arr[ : , 300 : 400 , : ] = rgb_color[3]
arr[ : , 400 :     , : ] = rgb_color[4]

import matplotlib.pyplot as plt

# 顯示灰階圖像
plt.imshow(arr, vmin=0, vmax=255)  
plt.title("Color Image")
plt.show()
```
---

## Numpy-影像色階輸出
![圖片07](../assets/images/turtle07.png)
```python
# 利用numpy的陣列結構顯示彩色影像

from PIL import Image
import numpy as np

arr = Image.open('face10.jpg')

# 轉換為 NumPy 陣列，並指定為 uint8 類型（0-255）
array = np.array(arr, dtype=np.uint8)

arr_r = np.empty(array.shape, dtype=np.uint8)
arr_r[:,:,0] =  array[:,:,0]
arr_r[:,:,1] =  array[:,:,0]
arr_r[:,:,2] =  array[:,:,0]

arr_g = np.empty(array.shape, dtype=np.uint8)
arr_g[:,:,0] =  array[:,:,1]
arr_g[:,:,1] =  array[:,:,1]
arr_g[:,:,2] =  array[:,:,1]

arr_b = np.empty(array.shape, dtype=np.uint8)
arr_b[:,:,0] =  array[:,:,2]
arr_b[:,:,1] =  array[:,:,2]
arr_b[:,:,2] =  array[:,:,2]

a1 = np.concatenate((arr_r,arr_g,arr_b,array),axis=1)

import matplotlib.pyplot as plt

# 顯示灰階圖像
plt.imshow(a1, vmin=0, vmax=255)  
plt.title("Color Image")
plt.show()
```
---

## Numpy-陣列轉置
![圖片08](../assets/images/turtle08.png)
```python
import numpy as np
import matplotlib.pyplot as plt

# 隨機生成一個 3x5 的 uint8 陣列
# arr = np.random.randint(256, size=(3, 5), dtype=np.uint8)

from PIL import Image

# 讀取彩色影像
array = Image.open('643458.jpg')

# 方法 1: 使用 convert('L') 直接轉灰階
gray_img = array.convert('L')  # 'L' 表示 8-bit 灰階
arr= np.array(gray_img, dtype=np.uint8)  # 轉為 NumPy 陣列

# 定義各種變換
a1 = arr.T                     # 轉置
a2 = np.rot90(arr, k=1)        # 預設逆時旋轉 90 度 (axes=(0,1))
a3 = np.rot90(arr, k=1, axes=(1, 0))  # 順時旋轉 90 度 (axes=(1,0))
a4 = np.rot90(arr, k=2)        # 旋轉 180 度
a5 = np.flip(arr, axis=0)      # 上下翻轉
a6 = np.flip(arr, axis=1)      # 左右翻轉

# 設定畫布大小
plt.figure(figsize=(12, 6))

# --- 原圖 (占用位置 1 ) ---
# ax_original = plt.subplot(2, 4, 1)
# ax_original.imshow(arr, cmap='gray', vmin=0, vmax=255)
# ax_original.set_title("Original Image")
# ax_original.axis('off')

# 由於 subplot(2,4,5) 會放在第二列第一行，我們讓它空白（或合併）
# 這裡直接讓原圖佔用位置 1，並在視覺上延伸（但實際不佔用 5）
# 或者改用 subplot2grid 更精確控制

# --- 改用 subplot2grid 精確控制 ---
plt.subplot2grid((2, 4), (0, 0), rowspan=2)  # 原圖佔用 (0,0) 和 (1,0)
plt.imshow(arr, cmap='gray', vmin=0, vmax=255)
plt.title("Original Image")
plt.axis('off')

# --- a1 (轉置) -> 位置 2 ---
plt.subplot(2, 4, 2)
plt.imshow(a1, cmap='gray', vmin=0, vmax=255)
plt.title("Transposed (a1)")
plt.axis('off')

# --- a5 (上下翻轉) -> 位置 3 ---
plt.subplot(2, 4, 3)
plt.imshow(a5, cmap='gray', vmin=0, vmax=255)
plt.title("Flip Axis=0 (a5)")
plt.axis('off')

# --- a6 (左右翻轉) -> 位置 4 ---
plt.subplot(2, 4, 4)
plt.imshow(a6, cmap='gray', vmin=0, vmax=255)
plt.title("Flip Axis=1 (a6)")
plt.axis('off')

# --- a2 (旋轉 90 度) -> 位置 6 ---
plt.subplot(2, 4, 6)
plt.imshow(a2, cmap='gray', vmin=0, vmax=255)
plt.title("Rot90 k=1 (a2)")
plt.axis('off')

# --- a3 (旋轉 90 度 axes=(1,0)) -> 位置 7 ---
plt.subplot(2, 4, 7)
plt.imshow(a3, cmap='gray', vmin=0, vmax=255)
plt.title("Rot90 axes=(1,0) (a3)")
plt.axis('off')

# --- a4 (旋轉 180 度) -> 位置 8 ---
plt.subplot(2, 4, 8)
plt.imshow(a4, cmap='gray', vmin=0, vmax=255)
plt.title("Rot90 k=2 (a4)")
plt.axis('off')

# 自動調整佈局
plt.tight_layout()
plt.show()
```
---
![圖片09](../assets/images/turtle09.png)
```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image

# 讀取彩色影像
array = Image.open('face10.jpg')
arr= np.array(array, dtype=np.uint8)  # 轉為 NumPy 陣列

# 定義各種變換
a1 = arr.T                                  # 轉置
a1 = np.transpose(a1, (1, 2, 0))
a2 = np.rot90(arr, k=1, axes=(0, 1))        # 預設逆時旋轉 90 度 (axes=(0,1))
a3 = np.rot90(arr, k=1, axes=(1, 0))        # 順時旋轉 90 度 (axes=(1,0))
a4 = np.rot90(arr, k=2, axes=(0, 1))        # 旋轉 180 度
a5 = np.flip(arr, axis=0)                   # 上下翻轉
a6 = np.flip(arr, axis=1)                   # 左右翻轉

# 設定畫布大小
plt.figure(figsize=(12, 6))

# --- 原圖 (占用位置 1 ) ---
# ax_original = plt.subplot(2, 4, 1)
# ax_original.imshow(arr, cmap='gray', vmin=0, vmax=255)
# ax_original.set_title("Original Image")
# ax_original.axis('off')

# 由於 subplot(2,4,5) 會放在第二列第一行，我們讓它空白（或合併）
# 這裡直接讓原圖佔用位置 1，並在視覺上延伸（但實際不佔用 5）
# 或者改用 subplot2grid 更精確控制

# --- 改用 subplot2grid 精確控制 ---
plt.subplot2grid((2, 4), (0, 0), rowspan=2)  # 原圖佔用 (0,0) 和 (1,0)
plt.imshow(arr, vmin=0, vmax=255)
plt.title("Original Image")
plt.axis('off')

# --- a1 (轉置) -> 位置 2 ---
plt.subplot(2, 4, 2)
plt.imshow(a1, vmin=0, vmax=255)
plt.title("Transposed (a1)")
plt.axis('off')

# --- a5 (上下翻轉) -> 位置 3 ---
plt.subplot(2, 4, 3)
plt.imshow(a5, vmin=0, vmax=255)
plt.title("Flip Axis=0 (a5)")
plt.axis('off')

# --- a6 (左右翻轉) -> 位置 4 ---
plt.subplot(2, 4, 4)
plt.imshow(a6, vmin=0, vmax=255)
plt.title("Flip Axis=1 (a6)")
plt.axis('off')

# --- a2 (旋轉 90 度) -> 位置 6 ---
plt.subplot(2, 4, 6)
plt.imshow(a2, vmin=0, vmax=255)
plt.title("Rot90 k=1 (a2)")
plt.axis('off')

# --- a3 (旋轉 90 度 axes=(1,0)) -> 位置 7 ---
plt.subplot(2, 4, 7)
plt.imshow(a3, vmin=0, vmax=255)
plt.title("Rot90 axes=(1,0) (a3)")
plt.axis('off')

# --- a4 (旋轉 180 度) -> 位置 8 ---
plt.subplot(2, 4, 8)
plt.imshow(a4, vmin=0, vmax=255)
plt.title("Rot90 k=2 (a4)")
plt.axis('off')

# 自動調整佈局
plt.tight_layout()
plt.show()
```
## Tidy Data
![圖片10](../assets/images/turtle10.png)
```python
# 字串解析並存入字典結構  最後轉成 DF

import pandas as pd

f = open(r'C:\Users\Hcedu\Downloads\學生資料表.txt', 'r', encoding="utf-8")
data_string = f.read() # 變成一個很大的字串
f.close()

# 利用跳行轉義字符\n切割字串
lines = data_string.split('\n')

students = {}
姓名 = []
學號 = []
成績一 = []
成績二 =  []
生日 = []
for line in lines:
    if line.strip():  # 如果是空行就跳過
        parts = line.split()
        姓名.append(parts[0])
        學號.append(parts[1])
        成績一.append(int(parts[2]))
        成績二.append(int(parts[3]))
        生日.append(parts[4])
      
students['姓名'] = 姓名
students['學號'] = 學號
students['成績一'] = 成績一
students['成績二'] = 成績二
students['生日'] = 生日

df = pd.DataFrame( students )
df
```
---

![圖片11](../assets/images/turtle11.png)
```python
# 字串解析並存入串列結構   最後轉成 DF  使用 pandas 自己的日期處理

f = open(r'C:\Users\Hcedu\Downloads\學生資料表.txt', 'r', encoding="utf-8")
data_string = f.read()
f.close()

# 分割字符串為行
lines = data_string.split('\n')

# 創建學生列表（每筆資料是一個子列表）
students = []

for line in lines:
    if line.strip():  # 確保行不為空
        parts = line.split()
        # 將每行數據轉換為列表並添加到students中
        student = [
            parts[0],        # 姓名
            parts[1],        # 學號
            int(parts[2]),   # 成績一
            int(parts[3]),   # 成績二
            parts[4]         # 生日
        ]
        students.append(student)

df = pd.DataFrame( students, columns=['姓名','學號','成績一','成績二','生日'] )

df['姓名'] = df['姓名'].astype('string')
df['學號'] = df['學號'].astype('string')
df['生日'] = df['生日'].astype('datetime64[ns]')

# 使用 pandas 的向量運算計算年齡
today = pd.Timestamp.today()
df['年齡'] = today.year - df['生日'].dt.year - ( 
    (today.month < df['生日'].dt.month) | ((today.month == df['生日'].dt.month) & (today.day < df['生日'].dt.day))
    ).astype(int)

# 這樣做的好處：

# * 不需使用 `datetime.datetime` 模組，完全依賴 Pandas。
# * 計算方式是「向量化」，速度比 `apply()` 快很多，對大數據特別有效。
df
```
---
![圖片15](../assets/images/turtle15.png)
![圖片16](../assets/images/turtle16.png)
```python
df = pd.read_excel('新竹市重要遊憩據點遊客人次統計.xlsx')
df = df.drop(columns=['Countycode', 'YYYMM'])
df = df.astype('int32')
df['民國年月']=df['民國年月'].astype('string')
df_unpivoted = pd.melt(df, 
                      id_vars=['民國年月'],
                      value_vars=df.columns.values[1:],
                      var_name='據點',
                      value_name='人數')
df_unpivoted['據點']=df_unpivoted['據點'].astype('string')

# 統計歷年來各據點的來客總人數
# 統計108年各據點的來客總人數

# 1. 統計歷年來各據點的來客總人數
total_by_branch_year = df_unpivoted.groupby(['據點'])['人數'].sum().reset_index()
print("歷年各據點來客總人數:")
print(total_by_branch_year.to_string(index=False))

# 2. 統計108年各據點的來客總人數
year_108_stats = df_unpivoted[df_unpivoted['民國年月'].str.slice(0,3) == '108'].groupby('據點')['人數'].sum().reset_index()
print("\n108年各據點來客總人數:")
print(year_108_stats.to_string(index=False))           
```
---

## Mplfinance資料圖形化
![圖片12](../assets/images/turtle12.png)
```python
#使用mplfinance將資料圖形化

df = pd.read_csv(r'C:\Users\Hcedu\Downloads\STOCK_DAY_2330_202507.csv',
                 encoding='big5',
                 thousands=',',
                 header=1) 
df = df.drop(columns=['成交金額', '漲跌價差', '成交筆數', 'Unnamed: 9'])
df = df[   df['日期'].str.contains(r'[0-9][0-9]/[0-9][0-9]/[0-9][0-9]', regex=True)    ]
df['國曆年'] = df['日期'].str.slice(start=0, stop=3, step=1) 
df['月'] = df['日期'].str.slice(start=4, stop=6, step=1) 
df['日'] = df['日期'].str.slice(start=7, stop=9, step=1) 
df['西元年'] = df['國曆年'].astype('int32') + 1911
df['西元日期'] = df['西元年'].astype('string') + '/' + df['月'] + '/' + df['日']
df = df.drop(columns=['日期', '國曆年', '月', '日', '西元年'])
df['成交股數'] = df['成交股數'].astype('int32')
df['開盤價'] = df['開盤價'].astype('float64')
df['最高價'] = df['最高價'].astype('float64')
df['最低價'] = df['最低價'].astype('float64')
df['收盤價'] = df['收盤價'].astype('float64')
df['西元日期'] = df['西元日期'].astype('datetime64[ns]')
df = df.rename(columns = {'成交股數':'Volume',
                          '開盤價':'Open',
                          '最高價':'High',
                          '最低價':'Low', '收盤價':'Close',
                          '西元日期':'Date'})
df = df.set_index('Date')

import mplfinance as mpf
mpf.plot(df,type='candle')
```
---

![圖片13](../assets/images/turtle13.png)
```python
#合併兩組資料圖形

df = pd.read_csv(r'C:\Users\Hcedu\Downloads\STOCK_DAY_2330_202507.csv',
                 encoding='big5',
                 thousands=',',
                 header=1) 
df = df.drop(columns=['成交金額', '漲跌價差', '成交筆數', 'Unnamed: 9'])
df = df[   df['日期'].str.contains(r'[0-9][0-9]/[0-9][0-9]/[0-9][0-9]', regex=True)    ]
df['國曆年'] = df['日期'].str.slice(start=0, stop=3, step=1) 
df['月'] = df['日期'].str.slice(start=4, stop=6, step=1) 
df['日'] = df['日期'].str.slice(start=7, stop=9, step=1) 
df['西元年'] = df['國曆年'].astype('int32') + 1911
df['西元日期'] = df['西元年'].astype('string') + '/' + df['月'] + '/' + df['日']
df = df.drop(columns=['日期', '國曆年', '月', '日', '西元年'])
df['成交股數'] = df['成交股數'].astype('int32')
df['開盤價'] = df['開盤價'].astype('float64')
df['最高價'] = df['最高價'].astype('float64')
df['最低價'] = df['最低價'].astype('float64')
df['收盤價'] = df['收盤價'].astype('float64')
df['西元日期'] = df['西元日期'].astype('datetime64[ns]')
df = df.rename(columns = {'成交股數':'Volume',
                          '開盤價':'Open',
                          '最高價':'High',
                          '最低價':'Low', '收盤價':'Close',
                          '西元日期':'Date'})
df2507 = df.set_index('Date')

df = pd.read_csv(r'C:\Users\Hcedu\Downloads\STOCK_DAY_2330_202506.csv',
                 encoding='big5',
                 thousands=',',
                 header=1) 
df = df.drop(columns=['成交金額', '漲跌價差', '成交筆數', 'Unnamed: 9'])
df = df[   df['日期'].str.contains(r'[0-9][0-9]/[0-9][0-9]/[0-9][0-9]', regex=True)    ]
df['國曆年'] = df['日期'].str.slice(start=0, stop=3, step=1) 
df['月'] = df['日期'].str.slice(start=4, stop=6, step=1) 
df['日'] = df['日期'].str.slice(start=7, stop=9, step=1) 
df['西元年'] = df['國曆年'].astype('int32') + 1911
df['西元日期'] = df['西元年'].astype('string') + '/' + df['月'] + '/' + df['日']
df = df.drop(columns=['日期', '國曆年', '月', '日', '西元年'])
df['成交股數'] = df['成交股數'].astype('int32')
df['開盤價'] = df['開盤價'].astype('float64')
df['最高價'] = df['最高價'].astype('float64')
df['最低價'] = df['最低價'].astype('float64')
df['收盤價'] = df['收盤價'].astype('float64')
df['西元日期'] = df['西元日期'].astype('datetime64[ns]')
df = df.rename(columns = {'成交股數':'Volume',
                          '開盤價':'Open',
                          '最高價':'High',
                          '最低價':'Low', '收盤價':'Close',
                          '西元日期':'Date'})
df2506 = df.set_index('Date')

daily = pd.concat([df2507,df2506])

import mplfinance as mpf
mpf.plot(daily,type='candle',mav=(3,6,9))
```
---

![圖片14](../assets/images/turtle14.png)
```python
# 合併csv

import pandas as pd
import mplfinance as mpf
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D

def csv_to_df(file):
    df = pd.read_csv(file,
                     encoding='big5',
                     thousands=',',
                     header=1) 
    df = df.drop(columns=['成交金額', '漲跌價差', '成交筆數', 'Unnamed: 9'])
    df = df[   df['日期'].str.contains(r'[0-9][0-9]/[0-9][0-9]/[0-9][0-9]', regex=True)    ]
    df['國曆年'] = df['日期'].str.slice(start=0, stop=3, step=1) 
    df['月'] = df['日期'].str.slice(start=4, stop=6, step=1) 
    df['日'] = df['日期'].str.slice(start=7, stop=9, step=1) 
    df['西元年'] = df['國曆年'].astype('int32') + 1911
    df['西元日期'] = df['西元年'].astype('string') + '/' + df['月'] + '/' + df['日']
    df = df.drop(columns=['日期', '國曆年', '月', '日', '西元年'])
    df['成交股數'] = df['成交股數'].astype('int32')
    df['開盤價'] = df['開盤價'].astype('float64')
    df['最高價'] = df['最高價'].astype('float64')
    df['最低價'] = df['最低價'].astype('float64')
    df['收盤價'] = df['收盤價'].astype('float64')
    df['西元日期'] = df['西元日期'].astype('datetime64[ns]')
    # Open	High	Low	Close	Volume  Date	
    df = df.rename(columns = {'成交股數':'Volume',
                              '開盤價':'Open',
                              '最高價':'High',
                              '最低價':'Low','收盤價':'Close',
                              '西元日期':'Date'})
    return df.set_index('Date')

import os

def get_all_files(directory):
    file_paths = []
    for root, directories, files in os.walk(directory):
        for filename in files:
            file_path = os.path.join(root, filename)
            file_paths.append(file_path)
    return file_paths

# 使用範例
directory_path = r'C:\Users\Hcedu\Downloads\2330'
all_files = get_all_files(directory_path)

df_list = []
for file in all_files :
    df = csv_to_df(file)
    df_list.append(df)

df = pd.concat(df_list)

# 計算布林帶
df['20SMA'] = df['Close'].rolling(window=20).mean()  # 20日簡單移動平均線
df['20STD'] = df['Close'].rolling(window=20).std()  # 20日標準差
df['UpperB'] = df['20SMA'] + (df['20STD'] * 2)  # 上布林帶
df['LowerB'] = df['20SMA'] - (df['20STD'] * 2)  # 下布林帶

custom_style = mpf.make_mpf_style(
    base_mpf_style='yahoo',  # 使用傳入的 style_name
    mavcolors=['gray', 'black', 'red'],  # 均線顏色：5日紅, 10日綠, 20日藍
    rc={
        'font.sans-serif': ['Microsoft JhengHei'],  # 確保顯示繁體中文
        'axes.labelsize': 12,   # X、Y 軸標籤字體大小
        'axes.titlesize': 28,   # 標題字體大小
        'xtick.labelsize': 8,   # X 軸刻度字體大小
        'ytick.labelsize': 8    # Y 軸刻度字體大小
    }
)

# 定義附加圖形（布林帶）
apdict = [
    mpf.make_addplot(df['LowerB'], label="LowerB", color='black'),
    mpf.make_addplot(df['UpperB'], label="UpperB", color='black')
]

fig, axlist = mpf.plot(
    df,
    type='candle',
    volume=True,
    style=custom_style,
    mav=(5, 20, 60),   # 均線 (5日, 20日, 60日)
    figratio=(12, 5),
    title='台積電',  # 標題
    ylabel='股價',          # 左側 Y 軸標籤
    ylabel_lower='成交量',   # 成交量子圖的 Y 軸標籤
    xrotation=20,            # 日期標籤旋轉角度
    addplot=apdict,          # 添加布林帶
    returnfig=True           # 返回 fig 和 axlist，以便後續自訂
)

# 自訂圖例 (Legend)
legend_labels = ['5日均線', '20日均線', '60日均線']
legend_colors = ['gray', 'black', 'red']  # 與 mavcolors 對應
legend_lines = [Line2D([0], [0], color=color, linewidth=1) for color in legend_colors]

# 將圖例放置於圖表外部右上角
axlist[0].legend(
    legend_lines,               # 自訂曲線樣式
    legend_labels,              # 圖例標籤
    loc='lower right',            # 位置：左上角
    fontsize=10,                # 字體大小
    labelcolor='red'          # 圖例字體顏色
)

plt.show()  # 顯示圖表
```
---

## Matplotlib-折線圖
![圖片17](../assets/images/turtle17.png)
```python
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rcParams

# 設定中文字體和圖表大小
plt.rcParams['font.sans-serif'] = ['Microsoft JhengHei']  # 使用微軟正黑體
plt.rcParams['axes.unicode_minus'] = False  # 解決負號顯示問題
rcParams['figure.figsize'] = 12, 8  # 設定圖表大小（寬12吋，高8吋）

# 讀取資料並預處理
df = pd.read_excel(r'C:\Users\Hcedu\Downloads\新竹遊憩據點人次統計.xlsx')
df = df.drop(columns=['Countycode', 'YYYMM'])
df = df.round() # 將數字四捨五入
df = df.astype('int32')
df['民國年月'] = df['民國年月'].astype('string')

# 取消樞紐
df_unpivoted = pd.melt(df, 
                      id_vars=['民國年月'],
                      value_vars=df.columns.values[1:],
                      var_name='據點',
                      value_name='人數')

# 新增年月欄位
df_unpivoted['年'] = df_unpivoted['民國年月'].str.slice(0,3)
df_unpivoted['月'] = df_unpivoted['民國年月'].str.slice(3,5)

# 建立樞紐分析表
table = pd.pivot_table(df_unpivoted, values='人數', index=['年'],
                      columns=['據點'], aggfunc="sum")

# 繪製趨勢圖
ax = table.plot(kind='line', 
                marker='o', 
                linewidth=2,
                markersize=8,
                figsize=(14, 8))  # 進一步放大圖表

# 設定圖表標題和標籤
plt.title('新竹市重要遊憩據點歷年來客趨勢', fontsize=16, pad=20)
plt.ylabel('來客人數', fontsize=12)
plt.xlabel('年度', fontsize=12)

# 調整圖例位置和字體大小
plt.legend(bbox_to_anchor=(1.05, 1),  # 將圖例放在圖表右側
           loc='upper left',
           borderaxespad=0.,
           title='遊憩據點',
           title_fontsize=12,
           fontsize=10)

# 調整刻度標籤大小
ax.tick_params(axis='both', which='major', labelsize=10)

# 自動調整版面以防止標籤被截斷
plt.tight_layout()

# 顯示圖表
plt.show()

# 可選：儲存圖表
# plt.savefig('新竹市遊憩據點來客趨勢.png', dpi=300, bbox_inches='tight')   
```
---

## Matplotlib-長條圖
![圖片18](../assets/images/turtle18.png)
```python
#長條圖

table.plot(kind='bar', 
           color='skyblue',  # 長條顏色
           edgecolor='black',  # 邊框顏色
           alpha=0.7,  # 透明度
           figsize=(10, 6),
           title='類別比較',
           rot=45)  # 旋轉x軸標籤
```
---

![圖片19](../assets/images/turtle19.png)
```python
#水平長條圖

table.plot(kind='barh', 
           color=['red', 'blue'],  # 不同顏色列表
           legend=True,  # 顯示圖例
           grid=True)  # 顯示格線
```
---

## 直方圖
![圖片20](../assets/images/turtle20.png)
```python
#直方圖

table.plot(kind='hist', 
           bins=20,  # 分箱數量
           density=True,  # 顯示密度而非計數
           alpha=0.5,  # 透明度
           edgecolor='black')  # 邊框顏色
```
---

## 區域圖
![圖片21](../assets/images/turtle21.png)
```python
#區域圖

table.plot(kind='area', 
           stacked=True,  # 是否堆疊
           alpha=0.8,  # 透明度
           figsize=(12, 6))
```
---

## 圓餅圖
![圖片22](../assets/images/turtle22.png)
```python
#圓餅圖

import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rcParams

# 設定中文字體和圖表大小
plt.rcParams['font.sans-serif'] = ['Microsoft JhengHei']  # 使用微軟正黑體
plt.rcParams['axes.unicode_minus'] = False  # 解決負號顯示問題
rcParams['figure.figsize'] = 10, 8  # 設定圖表大小

# 讀取資料並預處理（使用您之前的代碼）
df = pd.read_excel(r'C:\Users\Hcedu\Downloads\新竹遊憩據點人次統計.xlsx')
df = df.drop(columns=['Countycode', 'YYYMM'])
df = df.round().astype('int32')
df['民國年月'] = df['民國年月'].astype('string')

# 取消樞紐
df_unpivoted = pd.melt(df, 
                      id_vars=['民國年月'],
                      value_vars=df.columns.values[1:],
                      var_name='據點',
                      value_name='人數')

# 計算各據點總人數
location_totals = df_unpivoted.groupby('據點')['人數'].sum().sort_values(ascending=False)

# 設定美觀的顏色（根據 據點 數量自動生成）
colors = plt.cm.Paired.colors[:len(location_totals)]

# 創建圓餅圖
plt.figure(figsize=(12, 10))

# 繪製圓餅圖（增加陰影效果和起始角度）
wedges, texts, autotexts = plt.pie(
    location_totals,
    colors=colors,
    autopct='%1.1f%%',  # 顯示百分比
    startangle=90,       # 起始角度
    shadow=True,         # 陰影效果
    pctdistance=0.85,    # 百分比位置
    textprops={'fontsize': 10}  # 文字大小
)

# 設定標題
plt.title('新竹市重要遊憩據點遊客分布比例', fontsize=16, pad=20)

# 調整百分比文字顏色為白色（在深色區域更清晰）
for autotext in autotexts:
    autotext.set_color('white')
    autotext.set_weight('bold')

# 添加圖例（放在圖表右側）
plt.legend(
    wedges,
    location_totals.index,
    title="遊憩據點",
    loc="center left",
    bbox_to_anchor=(1, 0, 0.5, 1),
    fontsize=10
)

# 確保圖形是正圓形
plt.axis('equal')

# 自動調整版面
plt.tight_layout()

# 顯示圖表
plt.show()

# 可選：儲存圖表
# plt.savefig('新竹市遊憩據點遊客比例.png', dpi=300, bbox_inches='tight')
```
---

## 圓環圖
![圖片23](../assets/images/turtle23.png)
```python
#圓環圖

# library
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rcParams

# 設定中文字體和圖表大小
plt.rcParams['font.sans-serif'] = ['Microsoft JhengHei']  # 使用微軟正黑體
plt.rcParams['axes.unicode_minus'] = False  # 解決負號顯示問題
rcParams['figure.figsize'] = 10, 8  # 設定圖表大小

# create data

# 讀取資料並預處理（使用您之前的代碼）
df = pd.read_excel(r'C:\Users\Hcedu\Downloads\新竹遊憩據點人次統計.xlsx')
df = df.drop(columns=['Countycode', 'YYYMM'])
df = df.round().astype('int32')
df['民國年月'] = df['民國年月'].astype('string')

# 取消樞紐
df_unpivoted = pd.melt(df, 
                      id_vars=['民國年月'],
                      value_vars=df.columns.values[1:],
                      var_name='據點',
                      value_name='人數')

# 計算各據點總人數
location_totals = df_unpivoted.groupby('據點')['人數'].sum().sort_values(ascending=False)

names = location_totals.index
size = location_totals

# 計算百分比
total = sum(size)
percentages = [(s/total)*100 for s in size]

# 合併標籤與百分比 (格式: 據點名稱 XX.X%)
labels = [f'{name}\n{perc:.1f}%' for name, perc in zip(names, percentages)]


# 設定美觀的顏色（根據 據點 數量自動生成）
# colors = plt.cm.hsv.colors[:len(location_totals)]

# 獲取HSV色彩映射
_colormap = plt.cm.twilight_shifted

# 生成n個均勻分布的HSV顏色
n = len(location_totals)  # 據點數量
colors = [_colormap(i/n) for i in range(n)]  # 在0-1之間均勻取值

# 設定標題
plt.title('新竹市重要遊憩據點遊客分布比例', fontsize=16, pad=20)

# Create a circle at the center of the plot
my_circle = plt.Circle( (0,0), 0.7, color='white')

# Custom wedges
plt.pie(
        size, 
        labels=labels, 
        colors=colors, 
        startangle=0,       # 起始角度
        wedgeprops = { 'linewidth' : 7, 'edgecolor' : 'white' }
       )
p = plt.gcf()
p.gca().add_artist(my_circle)
plt.show()
```
