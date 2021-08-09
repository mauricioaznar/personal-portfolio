---
title: POSTMAN
---


<br />

## Config

##W Allow postman to use files in collection runners

1. go to settings > general > location > check "allow reading files outside working directory"


<br />

## Scripts

### Set enviroment variable with response

```javascript
// Check if the request was succesfull (pm object - short for postman)
if (pm.response.code === 201) {
    // Set the environment variable
    pm.environment.set('token', pm.response.json().access_token)
}
```
