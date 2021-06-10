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

