---
title: Heroku
---


## Introduction

Heroku is a platform as a service (PaaS) that enables developers to build, run,
and operate applications entirely in the cloud. ([heroku docs](https://devcenter.heroku.com/categories/reference)) 

* Started in 2007
* They handle 26+ billion requests per day
* Good security/easy to scale
* Low config
* Free options

<br />

## Heroku dashboard

### Create new app

1. click on create new app
2. go to settings
3. add buildpack and select nodejs

<br />

### Custom url



<br />

## CLI


### Install heroku on a project

Heroku instructions link ([Heroku cli instructions](https://devcenter.heroku.com/articles/heroku-cli))

Instructions summary

1. install npm package globally 
2. login with heroku
3. Add heroku remote to project
4. push to heroku master `git push heroku master`

Optionally we could add the post build script and a port to the 'start' script

<br />

### Post build

This command is run after a build in heroku

```json
{
  "scripts": {
    "heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix client && npm run build --prefix client"
  }
}
```

<br />

### Add custom domain

1. set billing information (you dont get charged)
2. go to project ./example-project
3. run next script


```html
heroku domains:add www.example.com -a heroku-example-project-name
```

4. configure dns provider
