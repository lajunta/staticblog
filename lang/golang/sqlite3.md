Sqlite3 used by golang
===

### Create tables

```go

db = dbConn()
createSemester := `
    create table if not exists semesters(id integer primary key,
    name text not null unique, short_name text not null unique,
    begin_on integer not null unique,end_on integer not null unique);`
db.Exec(createSemester)

createApply := `
create table if not exists applys(id integer primary key,
applyer text not null,semester text not null,croom text not null,
banji text not null, course text not null,wday text not null,
jieci text not null, status text not null);`
db.Exec(createApply)

createSchedule := `
create table if not exists schedules(id integer primary key,
name text not null, semester text not null ,
created_on integer not null,data text not null,status integer);`

db.Exec(createSchedule)
```

### Connect the db

```go
func dbConn() (db *sql.DB) {
	db, err := sql.Open("sqlite3", dbPath+"?_busy_timeout=5000")
	if err != nil {
		panic(err.Error())
	}
	db.SetConnMaxLifetime(5 * time.Second)
	db.SetMaxIdleConns(0)
	db.SetMaxOpenConns(20)
	return db
}
```

### Insert record

```go
	name := r.FormValue("name")
	shortName := r.FormValue("short_name")
	beon, _ := time.Parse("2006-01-02", r.FormValue("begin_on"))
	enon, _ := time.Parse("2006-01-02", r.FormValue("end_on"))
	beginOn := int(beon.Unix())
	endOn := int(enon.Unix())
	stmt, err := db.Prepare("insert into semesters(name,short_name,begin_on,end_on) values(?,?,?,?)")
	checkErr(err)
	_, err = stmt.Exec(name, shortName, beginOn, endOn)
	checkErr(err)
	stmt.Close()
	noticeAndReturn(w, r, "学期添加成功", "/semesters")
```

### Show one record

```go
	rows, err := db.Query("select * from semesters where id=?", id)
	checkErr(err)
	for rows.Next() {
		err = rows.Scan(&semester.ID, &semester.Name, &semester.ShortName, &semester.BeginOn, &semester.EndOn)
		checkErr(err)
	}
```

### Update one record

```go
	vars := mux.Vars(r)
	id := vars["id"]
	name := r.FormValue("name")
	shortName := r.FormValue("short_name")
	beon, _ := time.Parse("2006-01-02", r.FormValue("begin_on"))
	enon, _ := time.Parse("2006-01-02", r.FormValue("end_on"))
	beginOn := int(beon.Unix())
	endOn := int(enon.Unix())
	stmt, err := db.Prepare("update semesters set name=?,short_name=?,begin_on=?,end_on=? where id=?")
	checkErr(err)
	_, err = stmt.Exec(name, shortName, beginOn, endOn, id)
	checkErr(err)
	stmt.Close()
	noticeAndReturn(w, r, "学期更新成功", "/semesters")
```

### Delete on record

```go
	vars := mux.Vars(r)
	id := vars["id"]
	stmt, err := db.Prepare("delete from semesters where id=?")
	checkErr(err)
	stmt.Exec(id)
	stmt.Close()
	noticeAndReturn(w, r, "学期删除成功", "/semesters")
```


### List all record

```go
	semester := Semester{}
	semesters := []Semester{}

	var rowcount int64
	db.QueryRow("select count(*) from semesters").Scan(&rowcount)
	page, _ := strconv.ParseInt(r.FormValue("page"), 10, 64)
	pn := paginate.New(page, 10, rowcount)
	offset := (page - 1) * 10
	rows, _ := db.Query("select * from semesters order by short_name desc limit 10 offset ?", offset)
	for rows.Next() {
		rows.Scan(&semester.ID, &semester.Name, &semester.ShortName, &semester.BeginOn, &semester.EndOn)
		semesters = append(semesters, semester)
	}
	rows.Close()
	idxData := &SemesterIndexData{PN: pn, Semesters: semesters}
	templ.ExecuteTemplate(w, "SemesterIndex", idxData)
```