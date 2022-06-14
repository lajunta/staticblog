# VBA Loop

## For ... Next

单循环

```vba
For i = 1 To 6
    Cells(i, 1).Value = 100
Next i
```

双循环

```vba
For i = 1 To 6
    For j = 1 To 2
        Cells(i, j).Value = 100
    Next j
Next i
```

Step

```vba
Sub AddEvenNumbers()
Dim Total As Integer
Dim Count As Integer
Total = 0
For Count = 2 To 10 Step 2
  Total = Total + Count
Next Count
MsgBox Total
End Sub
```

## For Each

可以循环以下对象

- 所有打开的工作簿
- 一个工作簿中所有的工作表
- 所有选择的单元格

```vba
Sub HighlightNegativeCells()
Dim Cll As Range
For Each Cll In Selection
  If Cll.Value < 0 Then
  Cll.Interior.Color = vbRed
  End If
Next Cll
End Sub
```

## DO .. While

```vba
Sub AddFirst10PositiveIntegers()
Dim i As Integer
i = 1
Do While i <= 10
  Result = Result + i
  i = i + 1
Loop
MsgBox Result
End Sub
```

## DO .. Until

```vba
Sub AddFirst10PositiveIntegers()
Dim i As Integer
i = 1
Do Until i > 10
  Result = Result + i
  i = i + 1
Loop
MsgBox Result
End Sub
```
