---
title: Heroku
---


## Introduction

Heroku is a platform as a service (PaaS) that enables developers to build, run,
and operate applications entirely in the cloud. ([heroku docs](https://devcenter.heroku.com/categories/reference)) 

## CLI

### Post build

This command is run after a build in heroku

```json
{
  "scripts": {
    "heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix client && npm run build --prefix client"
  }
}
```
