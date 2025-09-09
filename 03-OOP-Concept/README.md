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
