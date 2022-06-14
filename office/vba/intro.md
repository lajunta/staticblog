# 什么是VBA(Visual Basic for Application)

VBA是微软开发的事件驱动的开发office应用的编程语言

VBA帮助开发自动化过程，用户自定义的函数，可以让你操作用户界面等特性

要使用VBA,你需要调出开发者面板,选项-定制化面板-开发者

## VBA Hello World!

所有的vba程序都放在以下的结构中

```vba
Sub name()


End Sub
```

- 创建一个工作簿
- 保存为带宏的文件 *.xlsm
- 点击开发者面板
- 插入按钮,指定相应的函数名称(宏)
- 进入代码编辑,在函数下写相应的代码

```vba
Dim name As String
name = InputBox("Enter your name")
MsgBox "Hello " + name
```

输入名称如果匹配，就在名字后写入注册

```vba
Sub name_Click()

Dim name As String

Dim row As Variant

Dim regcell As String

name = InputBox("输入姓名：")

row = Application.Match(name, Range("B:B"), 0)

If IsError(row) Then

    MsgBox "没有这个人"

Else

    regcell = "D" & row
    Range(regcell).Value = "注册"
    MsgBox "注册成功"

End If


End Sub
```

## VBA变量

- 长度不能大于255字符
- 不能有空格
- 不能用数字开头
- 不能使用点

#### 定义变量

- 隐式定义
  - label = guru
  - volume = 4
- 显式定义(使用Dim)
  - Dim Num As Integer
  - Dim Password As String

## Excel VBA 数据类型

数字类型

类型|字节|范围
---|---|---
Byte|1字节|0-255
Integer|2字节|-32,768 - 32,767
Long|4字节|
Single|4字节|
Double|8字节|
Decimal|12字节|

非数字类型

类型|字节|范围
String|字符的长度|
Boolean|2字节|True or False
Date|8字节|January 1, 100 to December 31, 9999
Object|4字节|
Variant|16字节|

__如果类型没有指定,自动指定为Variant__

定义和赋值

```vba
Sub Btn_Click()
Dim YourName as String,JoiningDay as Date,Income as Currency
YourName = "James"
JoiningDay = "1 April 2020"
Income = 1000
Range("A1") = YourName
Range("A2") = JoiningDay
Range("A3") = Income
End Sub

```

## VBA中的常量

常量不能修改，用`Const`来定义

有二种类型的常量

- 应用的内置内在常量
- 符号或者用户定义

可以指定scope

```vba
Public Const DaysInYear=365

Private Const Workdays=250
```