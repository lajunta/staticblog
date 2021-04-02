# Add and Grand user


```bash

CREATE DATABASE database_name;

CREATE USER 'newuser'@'10.8.0.5' IDENTIFIED BY 'user_password';

GRANT ALL PRIVILEGES ON database_name.* TO 'database_user'@'localhost';

```