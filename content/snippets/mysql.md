---
title: mysql
---

## Quick commands

### dump database into file

```shell
mysqldump -u root -p backend > backend.sql
```

<br />

### insert dump into database

```shell
 mysql -u root -p inopack < backend.sql
```

<br />

### create database 

```sql
 create database inopack;
```

<br />

### drop database

```sql
drop database inopack;
```

### list all foreign keys for a table

```sql
SELECT
  TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
FROM
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
  REFERENCED_TABLE_SCHEMA = '<database>' AND
  REFERENCED_TABLE_NAME = '<table>';
```

### List all foreign keys for a column

```sql
SELECT 
  TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
FROM
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
  REFERENCED_TABLE_SCHEMA = '<database>' AND
  REFERENCED_TABLE_NAME = '<table>' AND
  REFERENCED_COLUMN_NAME = '<column>';
```


### Drop column in table

```sql
ALTER TABLE table_name
  DROP COLUMN column_name;
```


### run script

```sql
source D:/Projects/backend.sql;
```

<br />


## MAC (OSx)

### installment

Uninstall Mysql or any other mysql isntallations

[Uninstall mysql](https://gist.github.com/vitorbritto/0555879fe4414d18569d)

Brew update

```shell
brew update
```


Brew install

```shell
brew install mysql
```

Start mysql service 

```shell
brew services start mysql
```

mysql secure installation

```shell
mysql_secure_installation
```

Brew fix

[Brew fix](https://stackoverflow.com/questions/9695362/macosx-homebrew-mysql-root-password)

