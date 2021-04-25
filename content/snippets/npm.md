---
title: NPM
---

## Introduction

npm is the world's largest software registry ([npm docs](https://docs.npmjs.com/about-npm)). it
consists of three major components:
* the website to discover packages, set up profiles,
  and manage other aspects of your npm experience.
* The CLI runs from a terminal, and is how most developers interact with npm.
* The registry is a large public database of JavaScript
  software and the meta-information surrounding it.

<br />
  
## Common package configuration
*package that gets used among projects*

### easy exporting and importing
To get a better importing experience of the common package
(`import {requireAuth} from "@maguas/common";` instead of `import requireAuth from "@maguas/common/src/requireAuth";`)
the following is recommended:
 
* index.js with all files being exported
```typescript
// Re-export stuff from errors and middlewares

export * from './errors/bad-request-error'
export * from './errors/custom-error'
export * from './errors/database-connection-error'
export * from './errors/not-authorized-error'
export * from './errors/not-found-error'
export * from './errors/request-validation-error'

export * from './middlewares/current-user'
export * from './middlewares/error-handler'
export * from './middlewares/require-auth'
export * from './middlewares/validate-request'
```
* add the main file property and the declarations property
```json
{
  "main": "./build/index.js",
  "types": "./build/index.d.ts",
  "files": [
    "build/**/*"
  ]
}
```

* add this useful scripts to package.json
```json
{
  "scripts": {
    "clean": "rimraf ./build",
    "build": "npm run clean && tsc",
    "pub": "git add . && git commit -m \"Updates\" && npm version patch && npm run build && npm publish --access public "
  }
}
```

* set this properties in tsconfig.json
```json
{
  "declaration": true,
  "outDir": "./build",
  "esModuleInterop": true,
  "skipLibCheck": true,
  "forceConsistentCasingInFileNames": true
}

```

<br />

## Favorite libraries

<br />

### LERNA

A tool for managing JavaScript projects with multiple packages. [lerna docs](https://lerna.js.org/)

<br />

### PM2

PM2 is daemon process manager that will help you manage and keep
your application online. Getting started with PM2
is straightforward, it is offered as a simple
and intuitive CLI, installable via NPM. ([pm2 docs](https://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/))
<br/>

* command used for executing a npm script
```bash
pm2 start npm --name "{app_name}" -- run {script_name}
```
* command to log any pm2 application running
```bash
pm2 logs
```
* command to monitor an application (memory usage)
```bash
pm2 monit
```
