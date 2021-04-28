---
title: Javascript
---


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

## DOM

### Check for device type

```jsx
const iOS = process.browser && /iPad|iPhone|iPod/.test(navigator.userAgent);

const drawer = (
  <React.Fragment>
    <SwipeableDrawer
      disableBackdropTransition={!iOS}
      disableDiscovery={iOS}
      open={openDrawer}
      onClose={() => {setOpenDrawer(false)}}
      onOpen={() => {setOpenDrawer(true)}}
      classes={{paper: classes.drawer}}
    >
      Drawer
    </SwipeableDrawer>
  </React.Fragment>
)
```
