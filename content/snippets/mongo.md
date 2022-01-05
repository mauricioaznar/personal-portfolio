---
title: Mongo
---

## Intro

## Commands

### Mongo export collection

```shell
mongoexport --uri="mongodb+srv://emaily-cluster.dro2u.mongodb.net/nest-super-todo" -c cats --out cats.json --username admin --password admin
```


### Mongo dump database

```shell
mongodump --uri="mongodb+srv://admin:admin@emaily-cluster.dro2u.mongodb.net/nest-super-todo"
```


### Mongo restore

```shell
mongorestore "mongodb://localhost:27017"
```
