模板
===


`html/template`包提供了丰富的模板语言，它通常能显示结构化的数据。它最大的好处是能自动对数据进行转义，不用担心 `XSS`攻击

### 创建模板

先准备数据:

```go
data := TodoPageData{
    PageTitle: "My TODO list",
    Todos: []Todo{
        {Title: "Task 1", Done: false},
        {Title: "Task 2", Done: true},
        {Title: "Task 3", Done: true},
    },
}
```

模板文件:

```html
{{ define "Index" }}
<h1>{{.PageTitle}}</h1>
<ul>
    {{range .Todos}}
        {{if .Done}}
            <li class="done">{{.Title}}</li>
        {{else}}
            <li>{{.Title}}</li>
        {{end}}
    {{end}}
</ul>
{{ end }}
```

### 从文件中获取模板

模板可以是字符串也可以是文件，从文件中获取

```go
tmpl, err := template.ParseFiles("layout.html")
//or
tmpl := template.Must(template.ParseFiles("layout.html"))
//or parse multiple files
tmpl := template.Must(template.ParseGlob("*.html"))
```

### 遍历目录获取模板

```go
func parseTmpl() {
	fileary := []string{}
	filepath.Walk("./views",
		func(path string, info os.FileInfo, err error) error {
			if strings.Contains(path, "layout") {
				return nil
			}
			if !info.IsDir() {
				fileary = append(fileary, path)
			}
			return nil
		})
	tmpl, _ = template.New("*").Funcs(funcs).ParseFiles(fileary...)
}
```

### 设置模板函数

```go
	funcs = template.FuncMap{
		"to_date":  utime.ToDate,
		"to_date1": utime.ToDate1,
		"to_date2": utime.ToDate2,
		"htmlSafe": uhtml.Safe,
		"urlSafe":  uhtml.URLSafe,
		"hex":      utils.Hex,
		"page_num": utils.PageNum,
	}
```


### 执行模板

一旦模板被解析，它就可以在处理函数中使用

```go
func(w http.ResponseWriter, r *http.Request){
    tmpl.Execute(w, data)
    // execute named tempate
    tmpl.ExecuteTemplate(w, "Index", data)
}
```

网页的`Content-Type`被自动设置为 `Content-Type: text/html;charset=utf-8`