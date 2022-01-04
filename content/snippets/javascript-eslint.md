---
title: Javascript ESLint
---

## Introduction


## Questions?

* .eslint.js
```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true,
  },
  extends: [
    '@nuxtjs/eslint-config-typescript',
    'plugin:prettier/recommended',
    'plugin:nuxt/recommended',
  ],
  plugins: [],
  // add your custom rules here
  rules: {
    'linebreak-style': ['error', 'unix'],
  },
}
```
* why linebreak-style?
