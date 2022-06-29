Index and Match 
===

### Match 语法 

 ** [Match函数](https://support.office.com/zh-cn/article/match-%E5%87%BD%E6%95%B0-e8dffd45-c762-47d6-bf89-533f4a37673a)**

使用 MATCH 函数在 范围 单元格中搜索特定的项，然后返回该项在此区域中的相对位置。

MATCH(lookup_value, lookup_array, [match_type])

MATCH 函数语法具有下列参数：

```text
lookup_value    必需。 要在 lookup_array 中匹配的值。

lookup_value 参数可以为值（数字、文本或逻辑值）或对数字、文本或逻辑值的单元格引用。

lookup_array    必需。 要搜索的单元格区域。

match_type    可选。 数字 -1、0 或 1。  此参数的默认值为 1

```

#### Match_type 行为

|Match_type|行为|
|---|---|
|1 或省略|MATCH 查找小于或等于 lookup_value 的最大值。 lookup_array 参数中的值必须以升序排序，例如：...-2, -1, 0, 1, 2, ..., A-Z, FALSE, TRUE。|
|0|MATCH 查找完全等于 lookup_value 的第一个值。 lookup_array 参数中的值可按任何顺序排列。|
|-1|MATCH 查找大于或等于 lookup_value 的最小值。 lookup_array 参数中的值必须按降序排列，例如：TRUE, FALSE, Z-A, ...2, 1, 0, -1, -2, ... 等等。|


#### 例子

|农产品|计数||
|-|-|-|
|香蕉|二十五||
|橙子|38||
|苹果|40||
|梨|41||
|公式|说明|结果|
|=MATCH(39,B2:B5,1)|由于此处无精确匹配项，因此函数会返回单元格区域 B2:B5 中最接近的下个最小值 (38) 的位置。|2|
|=MATCH(41,B2:B5,0)|单元格区域 B2:B5 中值 41 的位置。|4|


### Index 语法

INDEX 函数返回表格或区域中的值或值的引用。

说明
返回表或数组中元素的值，由行号和列号索引选择。

当函数 INDEX 的第一个参数为数组常量时，使用数组形式。

语法
INDEX(array, row_num, [column_num])

INDEX 函数的数组形式具有下列参数：

```text

数组    必需。 单元格区域或数组常量。

如果数组只包含一行或一列，则相应的 row_num 或 column_num 参数是可选的。

如果数组具有多行和多列，并且仅使用 row_num 或 column_num，则 INDEX 返回数组中整个行或列的数组。

row_num    必需，除非存在 column_num。 选择数组中的某行，函数从该行返回数值。 如果省略 row_num，则需要 column_num。

column_num    可选。 选择数组中的某列，函数从该列返回数值。 如果省略 column_num，则需要 row_num。
```
#### 例子

|数据|数据||
|---|---|---|
|苹果|柠檬||
|香蕉|梨||
|公式|说明|结果|
|=INDEX(A2:B3,2,2)|位于区域 A2:B3 中第二行和第二列交叉处的数值。|梨|
|=INDEX(A2:B3,2,1)|位于区域 A2:B3 中第二行和第一列交叉处的数值。|香蕉|

