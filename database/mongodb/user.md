Mongodb User role operation
===

`https://www.guru99.com/mongodb-create-user.html`

### 添加超级管理员

```json
db.createUser(

{	user: "Guru99",

	pwd: "password",

	roles:[{role: "userAdminAnyDatabase" , db:"admin"}]})

```

### 添加单个数据库管理员

```text
db.createUser(
{
	user: "Employeeadmin",

	pwd: "password",

	roles:[{role: "userAdmin" , db:"Employee"}]})
```

### 添加普通用户

```text
db.createUser(
{
	user: "Mohan",

	pwd: "password",

	roles:[
	{
		role: "read" , db:"Marketing"},

		role: "readWrite" , db:"Sales"}
	}
       ]
})
```