Emmet语法
===

### 下一代 >

`nav>ul>li`

```html
<nav>
  <ul>
    <li></li>
  </ul>
</nav>
```

### 同一代 +

`div+p+bq`

```html
<div></div>
<p></p>
<blockquote></blockquote>
```

### 上升一级 ^

`div+div>p>span+em^bq`

```html
<div></div>
<div>
  <p><span></span><em></em></p>
  <blockquote></blockquote>
</div>
```


`div+div>p>span+em^^bq`

```html
<div></div>
<div>
  <p><span></span><em></em></p>
</div>
<blockquote></blockquote>
```

### 成组 ()

`div>(header>ul>li*2>a)+footer>p`

```html
<div>
  <header>
    <ul>
      <li><a href=""></a></li>
      <li><a href=""></a></li>
    </ul>
  </header>
  <footer>
    <p></p>
  </footer>
</div>
```

`(div>dl>(dt+dd)*3)+footer>p`
```html
<div>
  <dl>
    <dt></dt>
    <dd></dd>
    <dt></dt>
    <dd></dd>
    <dt></dt>
    <dd></dd>
  </dl>
</div>
<footer>
  <p></p>
</footer>
```

### 重复

`ul>li*5`
```html
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

### 命名和编号

`ul>li.sample$*5`
```html
<ul>
  <li class="sample1"></li>
  <li class="sample2"></li>
  <li class="sample3"></li>
  <li class="sample4"></li>
  <li class="sample5"></li>
</ul>
```

`ul>li.item$$$*5`

```html
<ul>
  <li class="item001"></li>
  <li class="item002"></li>
  <li class="item003"></li>
  <li class="item004"></li>
  <li class="item005"></li>
</ul>
```

`ul>li.item$@3*5`

```html
<ul>
  <li class="item3"></li>
  <li class="item4"></li>
  <li class="item5"></li>
  <li class="item6"></li>
  <li class="item7"></li>
</ul>
```

### id 和 class 属性

`#header`

    <div id="header"></div>

`.title`

    <div class="title"></div>

`form#search.wide` 

    <form id="search" class="wide"></form>

`p.class1.class2.class3`

    <p class="class1 class2 class3"></p>

### 自定义属性

`p[title="Hello world"]`

    <p title="Hello world"></p>

`td[rowspan=2 colspan=3 title]` 

    <td rowspan="2" colspan="3" title=""></td>

`[a=‘value1‘ b="value2"]`

    <div a="value1" b="value2"></div>

### 文本: {} 

`a{Click me}`

    <a href="">Click me</a>

`p>{Click }+a{here}+{ to continue}` 

    <p>Click <a href="">here</a> to continue</p>


### 隐式标签 

`names.class`

    <div class="class"></div>

`em>.class`

    <em><span class="class"></span></em>

`ul>.class`

    <ul>
      <li class="class"></li>
    </ul>

`table>.row>.col`

    <table>
      <tr class="row">
        <td class="col"></td>
      </tr>
    </table>