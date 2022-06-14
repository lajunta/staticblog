# 子程序和函数

VBA Subroutine 一段执行特定功能的代码，不返回值 

## 定义子程序

```vba
Private Sub mySubRoutine(ByVal arg1 As String, ByVal arg2 As String)
    'do something
End Sub
```

## 调用子程序

```vba
Private Sub btnDisplayFullName_Click()
    displayFullName "John", "Doe"
End Sub

Private Sub btnDisplayFullName_Click()
  displayFullName "John", "Doe"
End Sub

```

## 什么是函数 

能返回值的代码片段


### 定义函数

```vba
Private Function addNumbers(ByVal firstNumber As Integer, ByVal secondNumber As Integer)
    addNumbers = firstNumber + secondNumber
End Function
```

### 调用函数 

```vba
Private Sub btnAddNumbersFunction_Click()
    MsgBox addNumbers(2, 3)
End Sub
```