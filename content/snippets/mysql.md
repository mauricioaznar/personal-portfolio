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
