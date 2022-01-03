---
title: Postgresql
---

## Quick commands

### dump database into file

```shell
pg_dump -U postgres --format=c  note_dev > note_dev
```

<br />

### insert dump into database

```shell
 pg_restore -U postgres -v -d note_prod note_dev
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

## what tools does postgres offer?

* 

