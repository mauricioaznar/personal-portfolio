---
title: postgresql
---

## Quick commands

### dump database into file

```shell
pg_dump -U postgres mydb > mydb.pgsql
```

<br />

### insert dump into database

```shell
 mysql -u root -p inopack < backend.sql
```

<br />

### create database 

```sql
 create database m;
```

<br />

### drop database

```sql
drop database m;
```

### list all foreign keys for a table


### List all foreign keys for a column


### List all databases

```sql
\list
```

<br /> 

```sql
\l
```

### Drop column in table


### run script


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

