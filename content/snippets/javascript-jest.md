---
title: Javascript jest
---

## Introduction

Jest is a delightful JavaScript Testing Framework with a focus on simplicity.
([Jest docs](https://jestjs.io/docs/getting-started))

## Mocks

### Setup

The following instructions use the @sendgrid package as an example `"@sendgrid/mail": "^7.4.0",`

1. name the mocks folder `__mocks__`
2. use the same folder structure where the mockfile that is getting mocked (e.g. `__mocks__/@sendgrid/mail.js`)

### Example #1

mail.js

```javascript
module.exports = {
  setApiKey() {
    console.log('set api key mock')
  },
  send() {
    console.log('send mock')
  }
}
```

<br />

## Base package.json config

### With global setup


```json
{
  "jest": {
    "globalSetup": "<rootDir>/__tests__/setup.ts",
    "moduleFileExtensions": [
      "js",
      "json",
      "ts"
    ],
    "rootDir": "src",
    "testRegex": ".*\\.test\\.ts$",
    "transform": {
      "^.+\\.(t|j)s$": "ts-jest"
    },
    "collectCoverageFrom": [
      "**/*.(t|j)s"
    ],
    "coverageDirectory": "../coverage",
    "testEnvironment": "node"
  }
}

```


## Expectations

### To equal array with object containing

```javascript
expect(response.body.operations).toEqual(
  expect.arrayContaining([
    expect.objectContaining({ id: operation1.id }),
  ]),
);
```

<br />

### To equal array with string matching

```javascript
expect(response.body.message).toEqual(
  expect.arrayContaining([expect.stringMatching(/banned/)]),
);


```

<br />



## Moxios

integration.test.js

```javascript
import React from 'react'
import { mount } from 'enzyme'
import moxios from 'moxios'
import Root from 'Root'
import App from 'components/App'
import { createMemoryHistory } from "history";

beforeEach(() => {
  moxios.install()
  moxios.stubRequest('http://jsonplaceholder.typicode.com/comments', {
    status: 200,
    response: Array(5).fill({ name: 'Comment' })
  })
})

afterEach(() => {
  moxios.uninstall()
})

it('can fetch a list of comments and display them', (done) => {
  const wrapped = mount(
    <Root initialRoute={['/post']}>
      <App/>
    </Root>
  )
  wrapped.find('li').at(1).simulate('click')
  wrapped.find('.fetch-comments').simulate('click')
  wrapped.find('li').at(0).simulate('click')

  // introduce a tiny little pause
  moxios.wait(() => {
    wrapped.update()
    expect(wrapped.find('li').length).toEqual(3)
    wrapped.unmount()
    done()
  })
})
```


