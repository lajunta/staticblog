# VBA数组

## 数组类型

- 静态的 指定长度
  - Dim ArrayMonth(12) As String
- 动态的 不指定长度
  - Dim ArrayMonth() As Variant

## 定义数组

静态

- Dim arrayName (n) as datatype

动态

```vba
Dim arrayName() as datatype
ReDim arrayName(4)

```

## 使用数组

```vba
Sub LoadBeverages_Click()
    Dim Drinks(1 To 4) As String
     
    Drinks(1) = "Pepsi"
    Drinks(2) = "Coke"
    Drinks(3) = "Fanta"
    Drinks(4) = "Juice"
     
    Sheet1.Cells(1, 1).Value = "My Favorite Beverages"
    Sheet1.Cells(2, 1).Value = Drinks(1)
    Sheet1.Cells(3, 1).Value = Drinks(2)
    Sheet1.Cells(4, 1).Value = Drinks(3)
    Sheet1.Cells(5, 1).Value = Drinks(4)
End Sub
```