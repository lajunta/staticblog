# VBA 字符操作及其它操作符

## 字符连接

- "John" & "Doe"

## 其它函数

- Join: 连接二个字符串
- Left: 取字符左边的字符串
- Right: 取右边的字符串
- Mid: 取中间的字符串
- Len: 字符串的长度
- Instr: 子字符串在字符串中的位置

## VBA 比较运算符

= 是否相等
< 小于
`>` 大于
<> 不等于
<= 小于等于
`>=` 大于等于

举例

```vba
    If 2 = 1 Then
            MsgBox "True", vbOKOnly, "Equal Operator"
    Else
            MsgBox "False", vbOKOnly, "Equal Operator"
    End If
```

## VBA逻辑运算符

- AND 
- OR 
- NOT

```vba
Private Sub btnAND_Click()
    If (1 = 1) And (0 = 0) Then
            MsgBox "AND evaluated to TRUE", vbOKOnly, "AND operator"
        Else
            MsgBox "AND evaluated to FALSE", vbOKOnly, "AND operator"
    End If
End Sub
```