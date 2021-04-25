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

lerna.json
```json
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^4.0.0"
  },
  "scripts": {
    "start": "lerna run start --parallel"
  }
}
```

<br />

package.json
```json
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "^4.0.0"
  },
  "scripts": {
    "start": "lerna run start --parallel"
  }
}

```

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

### Commander

Here is an example of how to use commander

<br />

package.json
```json
{
  "name": "@maguas/jsnote",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "start": "tsc --watch --preserveWatchOutput",
    "test": "echo \"Error: no test specified\" && exit 1",
    "prepublishOnly": "esbuild src/index.ts --platform=node --outfile=dist/index.js --bundle --minify --define:process.env.NODE_ENV=\\\"production\\\""
  },
  "files": [
    "dist"
  ],
  "bin": "dist/index.js",
  "publishConfig": {
    "access": "public"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@maguastupaguas/local-client": "^1.0.0"
  },
  "devDependencies": {
    "@maguastupaguas/local-api": "^1.0.0",
    "@types/node": "^14.14.31",
    "commander": "^7.1.0",
    "esbuild": "0.8.26",
    "typescript": "^4.2.2"
  }
}
```

<br />

tsconfig.json
```json
{
  "compilerOptions": {
    "target": "es5",  
    "module": "commonjs",
     "outDir": "./dist", 
    "strict": true,     
    "esModuleInterop": true,
    "skipLibCheck": true,                       
    "forceConsistentCasingInFileNames": true
  }
}
```

<br />

```ts
#!/usr/bin/env node

import {program} from 'commander'
import {serveCommand} from "./commands/serve";

// comment

program
    .addCommand(serveCommand)

program.parse(process.argv)
```

<br />

./src/commands/serve
```ts
import {Command} from "commander";
import path from "path";
import {serve} from "@maguastupaguas/local-api";


const isProduction = process.env.NODE_ENV === 'production'

export const serveCommand = new Command()
    .command('serve [filename]')
    .description('Open a file for edting')
    .option('-p, --port <number>', 'port to run server on', '4005')
    .action(async (filename = 'notebook.js', options: { port: string }) => {
        try {
            const dir = path.join(process.cwd(), path.dirname(filename))
            await serve(parseInt(options.port), path.basename(filename), dir, !isProduction)
            console.log(`Opened ${filename}, Navigate to http://localhost:${options.port} to edit the file.`)
        } catch (err) {
            if (err.code === 'EADDRINUSE') {
                console.error('Port is in use. Try running on a different port.')
            } else {
                console.log('Here the problem', err.message)
            }
            process.exit(1)
        }
    })
```

<br />

@maguastupaguas/local-api serve
index.ts
```ts
import express from 'express'
import {createProxyMiddleware} from "http-proxy-middleware";
import path from "path";
import {createCellsRouter} from "./routes/cells";


export const serve = (port: number, filename: string, dir: string, useProxy: boolean) => {
    const app = express()

    app.use(createCellsRouter(filename, dir))

    if (useProxy){
        app.use(createProxyMiddleware({
            target: 'http://localhost:3000',
            ws: true,
            logLevel: 'silent'
        }))
    } else {
        // WHY resolve???
        const packagePath = require.resolve('@maguastupaguas/local-client/build/index.html')
        app.use(express.static(path.dirname(packagePath)))
    }

    return new Promise<void>((resolve, reject) => {
        app.listen(port, resolve).on('error', reject)
    })
}
```

<br />

### Concurrently

Run multiple commands concurrently. Like npm run watch-js & npm run watch-less but better. ([concurrently docs](https://github.com/kimmobrunfeldt/concurrently#readme))

```json
{
  "scripts": {
    "start": "node index.js",
    "server": "nodemon index.js",
    "client": "npm run start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  }
}
```

### Ngrok

Ngrok exposes local servers behind NATs
and firewalls to the public internet over secure tunnels. ([ngrok docs](https://ngrok.com/docs))

Expose a web server on port 80 of your local machine to the internet

```shell
ngrok http 80
```

<br />

### Nodemon

## questions?

### What is `"bin": "dist/index.js",`?

### what is this script used for `"prepublishOnly": "npm run build"`?

### What is the use of `"files": ["dist"],` in package.json? 

### What is the use of `  "engines": { "node": "14.15.0", "npm": "6.14.8"},` in package.json?
