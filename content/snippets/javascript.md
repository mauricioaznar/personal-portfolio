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


<br />

## notifications

* [push notifications](https://developers.google.com/web/fundamentals/codelabs/push-notifications/)
* [web push](https://github.com/web-push-libs/web-push)
* [pushjs](https://pushjs.org/)
* [MDN notifications](https://developer.mozilla.org/en-US/docs/Web/API/notification)

<br />

## Copy to clipboard

```javascript
function copy(text) {
    var input = document.createElement('textarea');
    input.innerHTML = text;
    document.body.appendChild(input);
    input.select();
    var result = document.execCommand('copy');
    document.body.removeChild(input);
    return result;
}
```

<br />


## Exercises


### Soduku


#### Problem

```javascript
function sudoku(grid) {
    
}


console.log('---------- TESTS ----------')

function test (testName, result, expectedResult) {
    console.log(testName, expectedResult === result ? 'passed' : 'failed')
}

test('#1', sudoku([[1,3,2,5,4,6,9,8,7],
    [4,6,5,8,7,9,3,2,1],
    [7,9,8,2,1,3,6,5,4],
    [9,2,1,4,3,5,8,7,6],
    [3,5,4,7,6,8,2,1,9],
    [6,8,7,1,9,2,5,4,3],
    [5,7,6,9,8,1,4,3,2],
    [2,4,3,6,5,7,1,9,8],
    [8,1,9,3,2,4,7,6,5]]), true);
test('#2', sudoku([[1,3,2,5,4,6,9,2,7],
    [4,6,5,8,7,9,3,8,1],
    [7,9,8,2,1,3,6,5,4],
    [9,2,1,4,3,5,8,7,6],
    [3,5,4,7,6,8,2,1,9],
    [6,8,7,1,9,2,5,4,3],
    [5,7,6,9,8,1,4,3,2],
    [2,4,3,6,5,7,1,9,8],
    [8,1,9,3,2,4,7,6,5]]), false)
test('#3', sudoku([[1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9],
    [1,2,3,4,5,6,7,8,9]]), false)
test('#4', sudoku([[1,3,4,2,5,6,9,8,7],
    [4,6,8,5,7,9,3,2,1],
    [7,9,2,8,1,3,6,5,4],
    [9,2,3,1,4,5,8,7,6],
    [3,5,7,4,6,8,2,1,9],
    [6,8,1,7,9,2,5,4,3],
    [5,7,6,9,8,1,4,3,2],
    [2,4,5,6,3,7,1,9,8],
    [8,1,9,3,2,4,7,6,5]]), false)
test('#5',sudoku([[1,3,2,5,4,6,9,8,7],
    [4,6,5,8,7,9,3,2,1],
    [7,9,8,2,1,3,6,5,4],
    [9,2,1,4,3,5,8,7,6],
    [3,5,4,7,6,8,2,1,9],
    [6,8,7,1,9,2,5,4,3],
    [5,4,6,9,8,1,4,3,2],
    [2,7,3,6,5,7,1,9,8],
    [8,1,9,3,2,4,7,6,5]]), false)

```

#### Solution 
```javascript

function sudoku(grid) {
    const rows = Array.from(Array(grid.length), () => { return Array() })
    const columns = Array.from(Array(grid.length), () => { return Array() })
    const grids = Array.from(Array(grid.length), () => { return Array() })
    
    
    for (let y = 0; y < grid.length; y++) {
        for (let x = 0; x < grid[y].length; x++) {
            const number = grid[y][x]
            
            if (!rows[y].includes(number)) {
                rows[y].push(number)
            }
            
            if (!columns[x].includes(number)) {
                columns[x].push(number)
            }
            
            const pos = Math.floor(x / 3) + (Math.floor(y / 3) * 3)
            if (!grids[pos].includes(number)) {
                grids[pos].push(number)
            }
        }
    }
    
    const gridsOk = grids.map(g => g.length).filter(g => g === 9).length === 9
    const rowsOk = rows.map(g => g.length).filter(g => g === 9).length === 9
    const columnsOk = columns.map(g => g.length).filter(g => g === 9).length === 9
    
    return gridsOk && rowsOk && columnsOk
    
}




```
