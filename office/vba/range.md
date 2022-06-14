# Range 对象

- 单个单元格
- 一行或者一列
- 单元格区域
- 3D范围

## 如何引用Range

```vba
Application.Workbooks("Book1.xlsm").Worksheets("Sheet1").Range("A1")
```

上面是完全引用

- 属性 
- 方法


Range 属性可以用在二个不同的对象上

- Worksheet对象
- Range 对象
### 引用示例
范围|引用
---|---
单行	|Range(“1:1”)
单列	|Range(“A: A”)
区域	|Range(“A1:C5”)
不连续区域	|Range(“A1:C5, F1:F5”)
交叉区域	|Range(“A1:C5 F1:F5”)
合并	|Range(“A1:C5”).merge()

## Cell属性

可以在循环中使用, 它有`item`属性,可以引用单元格

Cells.item(Row, Column),下面都引用A1单元格

- Cells.item(1,1)
- Cells.item(1,"A")

## Ranges偏移属性

`offset`可以在原位置的基础上选择偏移的位置

```vba
Range("A1").offset(Rowoffset:=1, Columnoffset:=1).Select
```

将会选择B2

## 总结