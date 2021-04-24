---
title: Article 2
---

Lear how to use @nuxt/content

<!--more-->

# Article 2

```typescript
const http = require('http')
const bodyParser = require('body-parser')

http
  .createServer((req, res) => {
    bodyParser.parse(req, (error, body) => {
      res.end(body)
    })
  })
  .listen(3000)
```
