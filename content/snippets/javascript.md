---
title: Javascript
---

## Introduction

## functions

### validate-emails

validate-emails.js
```javascript
export default function validateEmail(emails)  {
  const invalidEmails = emails
    .split(',')
    .map(email => email.trim())
    .filter(email => {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(email) === false
    })
  if (invalidEmails.length) {
    return `These emails are invalid: ${invalidEmails}`
  }

  return
}
```

<br />

## Object

### is object empty

[stackoverflow answer #1](https://stackoverflow.com/questions/679915/how-do-i-test-for-an-empty-javascript-object)

```javascript
if (obj && Object.keys(obj).length === 0 && obj.constructor === Object) {
    // is empty
}
```

<br />

## DOM

### send download to tab

* getDownloadDocumentUrl gets the full url for the download
* fetch document reuses the previous function to download send the user to another tab and download the document
```javascript
export const getDownloadDocumentUrl = async (file_name) => {
    const result = await MYSQL_API.getUri( {
        url: '/file',
        params: {
            file_name,
        }
    })
    return MYSQL_API.defaults.baseURL + result
} 
export const fetchDocument = async (filename) => {
  this.downloadingDocument = true
  try {
    const result = await getDownloadDocumentUrl(filename, this.getActiveUser())
    window.open(result, '_blank').focus();
  } catch (e) {
    this.error = e
  }
  this.downloadingDocument = false
}

```

<br />


