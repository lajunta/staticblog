# Add and Grand user


```bash

CREATE DATABASE database_name;

CREATE USER 'newuser'@'10.8.0.5' IDENTIFIED BY 'user_password';

GRANT ALL PRIVILEGES ON database_name.* TO 'database_user'@'localhost';

```

## Another way

```bash
mysql> create role "zxy"@"localhost";
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT ALL PRIVILEGES  ON page_center.* TO 'zxy'@'localhost';
Query OK, 0 rows affected (0.00 sec)

mysql> CREATE USER 'zxy' IDENTIFIED BY 'user_password';
Query OK, 0 rows affected (0.00 sec)

```