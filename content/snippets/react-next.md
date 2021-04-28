---
title: React next.js
---

## Introduction

Next.js is an open-source React front-end development web
framework created by Vercel that enables functionality such
as server-side rendering and generating static websites
for React based web applications. ([nextjs docs](https://nextjs.org/docs/getting-started))

<br />

### Benefits of nextjs

* built-in router
* Code splitting: load code depending on the active page. Decreases load time.
* Server rendering. Helps users with slower devices.

<br />

## Folders

### /public

Every file that is inside this folder, will be available at baseUrl + / + 'file_name'

* Files should have blank spaces.

<br />

### /pages

* Every file must be lowercase
* Include next/head per page. 
    * title tags with key property to ensure that the tag is unique per page (the pipe looks professional '|')

<br />
  
## Migration from material ui

1. Install project 

```shell
curl https://codeload.github.com/mui-org/material-ui/tar.gz/master | tar -xz --strip=2  material-ui-master/examples/nextjs
```

2. cd into project and install

3. Remove CssBaseline

4. add `margin: 0` to <body></ body>
   
<br />

## Routing

* Use next/Link instead of react-router-dom link.
    * replace href for to.

<br />

## Layout

Insert repeated components in _app.js

<br />


## Image optimization

1. Install next-optimized-images ([link](https://github.com/cyrilwanner/next-optimized-images))
2. Download necessary plugins (e.g. jpeg) `npm install imagemin-mozjpeg`
3. Enable the plugin in your nextjs configuraton file:

next.config.js
```js
// next.config.js
const withPlugins = require('next-compose-plugins');
const optimizedImages = require('next-optimized-images');

module.exports = withPlugins([
  [optimizedImages, {
    /* config for next-optimized-images */
  }],

  // your other plugins here

]);
```
