---
title: Electron
---

## Introduction


## Native node modules

Recommended to use electron-build (e.g. bycrypt)

[Native node modules](https://www.electronjs.org/docs/tutorial/using-native-node-modules)

<br />

## Debugging

[Debugging electron link](https://www.electronjs.org/docs/tutorial/debugging-main-process)

`mainWindow.webContents.openDevTools()`


<br />

## App object

[app](https://www.electronjs.org/docs/api/app)

### Prevent app from quitting

```javascript
app.on('before-quit', (e) => {
  e.preventDefault()
})
```

<br />

## Browser window

[browser window](https://www.electronjs.org/docs/api/browser-window)


setup

*use `loadFile` instead of` loadURL`*

```javascript
const {app, BrowserWindow} = require('electron')

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html') 
}

app.on('ready', createWindow)
```

<br />


### Load content

```javascript
window.loadURL('https://myseite.com/index.html')
window.loadFile('index.html')
window.loadURL('file://Users/me/app/index.html')
```

<br />

### Get user data (information of the user logged in)

`app.getPath('userData')`

<br />

### Showing window gracefully

```javascript
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true },
    show: false,
  })

  mainWindow.once('ready-to-show', mainWindow.show) 
}
```

<br />

Or...

```javascript

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true },
    backgroundColor: '#2b2b3b'
  })
}
```

<br />

### parent & child

[parent & child](https://www.electronjs.org/docs/api/browser-window#parent-and-child-windows)

```javascript
let mainW

function createWindow () {
  mainW = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  secondaryW = new BrowserWindow({
    width: 600, height: 300,
    webPreferences: { ndoeIntegration: true },
    parent: mainW,
    modal: true,
    show: false,
  })

  mainW.loadFile('index.html')
  secondaryW.loadFile('secondary.html')


  setTimeout(() => {
    secondaryW.show()
    setTimeout(() => {
      secondaryW.close()
      secondaryW = null
    }, 3000)
  }, 2000)

  mainW.on('closed', () => {
    mainW = null
  }) 
}
```   

<br />

### frameless window

[frameless](https://www.electronjs.org/docs/api/frameless-window)


```javascript
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true },
    frame: false
  })

  mainWindow.loadFile('index.html') 
}
```

<br />

### browser window properties and methods (options)

[browser windows optios](https://www.electronjs.org/docs/api/browser-window#new-browserwindowoptions)


## State management on windows

`electron-window-state` package

````shell
npm install --save-dev electron-window-state
````

<br />

```javascript
const windowStateKeeper = require('electron-window-state')

let mainWindow

function createWindow () {
  let winState = windowStateKeeper({
    defaultWidth: 1000,
    defaultHeight: 800
  })
  
  mainWindow = new BrowserWindow({
    width: winState.width,
    height: winState.height,
    x: winState.x,
    y: winState.y,
    webPreferences: { ndoeIntegration: true },
  })

  mainWindow.loadFile('index.html')
  
  winState.manage(mainWindow)
}
```

<br />

## Web contents (broweser window)

[Web contents](https://www.electronjs.org/docs/api/web-contents)

### Instance vs static

```javascript

const { app, BrowserWindow, webContents } = require('electron')

let mainWindow

function createWindow () {
  
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    x: 100,
    y: 100,
    webPreferences: { ndoeIntegration: true },
  })

  mainWindow.loadFile('index.html')
  
  // mainWindow.webContents.openDevTools()
  
  // web content instance
  let wc = mainWindow.webContents
  
  wc.on('did-finish-load', () => {
    console.log('All content (images or others content included)')
  })

  wc.on('dom-ready', () => {
    console.log('Dom loaded (all tags)')
  })
  
  // static method (from all web content instatnces)
  webContents.getAllWebContents()
  
  mainWindow.on('closed', () => {
    mainWindow = null
  })
}
```

<br />

### Prevent window creation

```javascript
wc.on('new-window', (e, url) => {
  e.preventDefault()
  console.log(`Preventing new window for: ${url}`)
})
```

<br />

### Before event

```javascript
wc.on('before-input-event', (e, input) => {
  e.preventDefault()
  console.log(`Preventing new window for: ${input}`)
})
```



<br />

### Login

```javascript
wc.on('login', (e, request, authInfo, callback) => {
  console.log('Logging in: ')
  callback('user', 'password')
})

wc.on('did-navigate', (e, url, statusCode, message) => {
  console.log(`Navigated to: ${url}`)
  console.log(statusCode)
})
```

<br />

### Did navigate

```javascript
wc.on('did-navigate', (e, url, statusCode, message) => {
  console.log(`Navigated to: ${url}`)
  console.log(statusCode)
})
```

<br />

### Context menu

```javascript
wc.on('context-menu', (e, params) => {
  console.log(`Context menu opened on: ${params.mediaType} at x:${params.x}, y:${params.y}`)
})

wc.on('context-menu', (e, params) => {
  console.log(`User selected text: ${params.selectionText}`)
  console.log(`Selection can be copied: ${params.editFlags.canCopy}`)
})

```

<br />

### Execute javascript

`wc.executeJavascript()`

<br />

## Session

[Session](https://www.electronjs.org/docs/api/session)

* Session share sessions

```javascript
let ses = mainWindow.webContents.session
```

<br />

Default session is also used

```javascript
let defaultsSes = session.defaultSession
```

<br />

Custom session

```javascript
const {session} = require('electron')

let customSession = session.fromPartition('part1')

let mainWindow
let secondWindow
let thirdWindow

const mainWindow = new BrowserWindow({
  width: 1000,
  height: 1000,
  x: 100,
  y: 100,
  webPreferences: {
    ndoeIntegration: true, 
    session: customSession 
  },
})


let persistedCustomSession = session.fromPartition('persist:part2')

const secondWindow = new BrowserWindow({
  width: 1000,
  height: 1000,
  x: 100,
  y: 100,
  webPreferences: {
    ndoeIntegration: true,
    session: persistedCustomSession
  },
})


// asiggned inside of browser window

const thirdWindow = new BrowserWindow({
  width: 1000,
  height: 1000,
  x: 100,
  y: 100,
  webPreferences: {
    ndoeIntegration: true,
    partition: 'persist:part3'
  },
})

```

<br />

### clear session

clear on instance

`ses.clearStorageData()`

<br />

### Cookies

[Cookies](https://www.electronjs.org/docs/api/cookies)

```javascript
const {session} = require('electron')

let mainWindow

let customSession = session.fromPartition('part1')

function createWindow () {

  let ses = session.defaultSession
  
  // read cookies
  ses.cookies.get({})
    .then(cookies => {
      
    })
    .catch(e => {})
  
  let cookie = {
    url: "https://myappdomain.com",
    name: 'cookie1',
    value: 'electron',
    expirationDate: 1622818789
  }
  
  // set cookie
  ses.cookies.set(cookie)
    .then(() => {
      console.log('Cookie 1 set')
    })
  
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    x: 100,
    y: 100,
    webPreferences: { ndoeIntegration: true },
  })
  
  // get by name
  set.cookies.get({
    name: 'cookie1'
  })
  
  // remove cookie (url and name of cookie)
  ses.cookies.remove("https://myappdomain.com", 'cookie1')
    .then(() => {})
    
  
}


```

<br />

### Download item

[Download item](https://www.electronjs.org/docs/api/download-item)

Use html attribute property `download` on a `a` tag


```javascript
const {session} = require('electron')

let mainWindow

let customSession = session.fromPartition('part1')

function createWindow () {

  let ses = session.defaultSession
  
  // read cookies
  ses.cookies.get({})
    .then(cookies => {
      
    })
    .catch(e => {})
  
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    x: 100,
    y: 100,
    webPreferences: { ndoeIntegration: true },
  })
  
  // download item without any prompt (small file)
  ses.on(`will-download`, (e, downloadItem, webContents) => {
    console.log('starting download')
    let filename = downloadItem.getFilename()
    let filesize = downloadItem.getTotalBytes()
    
    // save to desktop
    downloadItem.setSavePath(app.getPath('desktop') + `/${filename}`)
    
    downloadItem.on('updated', (e, state) => {
      
      let received = downloadItem.getReceivedBytes()
      
      if (state === 'progressing' && received) {
        let progress = Math.Round((received/filesize) * 100)
        webContents.executeJavascript(`window.progress.value = ${progress}`)
      }
      
    })
  })
    
  
}


```

<br />

## Dialog

[Dialog](https://www.electronjs.org/docs/api/dialog)

```javascript
const {Dialog} = require('electron')

let mainWindow


function createWindow () {
  
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    x: 100,
    y: 100,
    webPreferences: { ndoeIntegration: true },
  })
  
  mainWindow.loadFile('index.html')
  

  // file explorer dialog
  
  mainWindow.webContents.on('did-finish-load', () => {
    
    Dialog.showOpenDialog(mainWindow, {
      buttonLabel: 'Select a photo',
      defaultPath: app.getPath('home'),
      properties: [
        'multiSelections',
        'createDirectory',
        'openFile',
        'openDirectory'
      ]
    })
      .then(result => {

      })


    Dialog.showSaveDialog({})
      .then(result => {

      })

    Dialog.showMessageBox({
      title: 'title',
      message: 'Please select an option',
      detail: 'Message details',
      buttons: ['yes', 'no', 'maybe']
    })
      .then(result => {
        console.log('button selected index ' + result.response)
      })
  })
}


```

<br />

## accelerators & global shortcuts

```javascript
const {globalShortcut} = require('electron')

globalShortcut.register('G', () => {
  console.log('user pressed g')
})

globalShortcut.register('CommandOrControl+G', () => {
  console.log('user pressed g with a combination key')
})

// once
globalShortcut.register('CommandOrControl+G', () => {
  console.log('user pressed g with a combination key')
  globalShortcut.unregister('CommandOrControl+G')
})
```

<br />

## Menu

* menu is like a system bar
* could be exported to a separate file
* roles (already integrated menu actions)
* shortcuts are only triggered when app is in focus

```javascript
const {app, BrowserWindow, Menu, MenuItem} = require('electron')

let mainWindow

let mainMenu = new Menu()

// in mac the first menu item is the name of the app
let menuItem1 = new MenuItem({
  label: 'Electron',
  submenu: [
    { 
      label: 'Item 1',
      click: () => {
        console.log('item clicked')
      },
      accelerator: 'Shift + Alt + g'
    },
    { 
      label: 'Item 2',
      submenu: {
        label: "Sub item 1"
      }
    },
    {
      role: 'toggleFullScreen'
    },
    {
      role: 'undo'
    },
    {
      role: 'redo'
    },
    {
      role: 'copy'
    },
    {
      role: 'paste'
    },
    { 
      label: 'Item 3',
      enabled: false
    }
  ]
})

mainMenu.append(menuItem1)


let mainMenu2 = Menu.buildFromTemplate([
  {
    label: 'Electron',
    submenu: [
      { label: 'Item 1'},
      {
        label: 'Item 2',
        submenu: {
          label: "Sub item 1"
        }
      },
      { label: 'Item 3'}
    ]
  },
  {
    label: 'Action 2'
  },
  {
    label: 'Action 3'
  }
])


function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')
  
  Menu.setApplicationMenu(mainMenu)
}

app.on('ready', createWindow)

```

<br />


### Context menu

```javascript
const {app, BrowserWindow, Menu, MenuItem} = require('electron')

let mainWindow

let contextMenu = new Menu.buildFromTemplate([
  {
    label: 'item 1'
  },
  {
    label: 'item 2'
  }
])

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')
  
  mainWindow.webContents.on('context-menu', () => {
    contextMenu.popup()
  })
}

app.on('ready', createWindow)

```

<br />

## Tray

[Tray](https://www.electronjs.org/docs/api/tray)
[Native images](https://www.electronjs.org/docs/api/native-image)

```javascript
const {app, BrowserWindow, Menu, MenuItem, Tray} = require('electron')

let tray
let mainWindow


let trayMenu = Meny.buildFromTemplate([
  { label: 'Item 1'},
  { role: 'quit'}
])

function createTray () {
  tray = new Tray('trayTemplate@2x.png')
  tray.setTooltip('Tray details')
  
  tray.on('click', (e) => {
    
    if (e.shiftKey) {
      app.quit()
    } else {
      mainWindow.isVisible ? mainWindow.hide() : mainWindow.show() 
    }
  })
  
  tray.setContextMeny(trayMenu)
}

let contextMenu = new Menu.buildFromTemplate([
  {
    label: 'item 1'
  },
  {
    label: 'item 2'
  }
])

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')
  
  mainWindow.webContents.on('context-menu', () => {
    contextMenu.popup()
  })
}

app.on('ready', createWindow)

```

<br />

## Power Monitor

[Power monitor](https://www.electronjs.org/docs/api/power-monitor)


```javascript
const electron = require('electron')
const {app, BrowserWindow, Menu, MenuItem, Tray} = electron

let tray
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000, height: 800,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')


  electron.powerMonitor.on('resume', (e) => {
    if (!mainWindow) {
      createWindow()
    }
  })

  electron.powerMonitor.on('suspend', (e) => {
    console.log('Saving some data')
  })
}

app.on('ready', createWindow)

```

<br />

## Screen

* used only when app is ready

[Screen](https://www.electronjs.org/docs/api/screen)

```javascript
const electron = require('electron')
const {app, BrowserWindow, screen} = electron

let primaryDisplay = screen.getPrimaryDisplay()

let tray
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: primaryDisplay.size.width / 2,
    height: primaryDisplay.size.height,
    x: primaryDisplay.bounds.x,
    y: primaryDisplay.bounds.y,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')
  
}

app.on('ready', createWindow)

```

<br />


## Renderer process

### Browser window proxy

[Browser window proxy](https://www.electronjs.org/docs/api/browser-window-proxy)

* we can control window properties

```javascript
let win
const newWin = () => {
  win = window.open('https://developer.mozilla.org')  
}

const closeWin = () => {
  win.close()
}

<br />

```

### Web frame

* zoom
* spelling and grammar
* resources

[Web frame](https://www.electronjs.org/docs/api/web-frame)

```javascript
const electron = require('electron')
const {webFrame} = electron

webFrame.getZoomFactor() // 1 === 100%, 2 === 200%
```

<br />

### Desktop capturer

```javascript
const electron = require('electron')
const {desktopCapturer} = electron


desktopCapturer,getSources({
  type
})

```

<br />

## IPC communication

[IPC main](https://www.electronjs.org/docs/api/ipc-main)
[IPC renderer](https://www.electronjs.org/docs/api/ipc-renderer)

* inter process communication

main.js
```javascript
const electron = require('electron')
const {app, BrowserWindow, ipcMain} = electron

let primaryDisplay = screen.getPrimaryDisplay()

let tray
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: primaryDisplay.size.width / 2,
    height: primaryDisplay.size.height,
    x: primaryDisplay.bounds.x,
    y: primaryDisplay.bounds.y,
    webPreferences: { ndoeIntegration: true }
  })

  mainWindow.loadFile('index.html')
  
  
  mainWindow.webContents.on('did-finish-load', (e) => {
    mainWindow.webContents.send('mailbox', 'you have mail')
  })
  
}

ipcMain.on('channel1', (e, args) => {
  e.sender.send('channel1-response', 'Message received on channeld 1')
})

cMain.on('sync-message', (e, args) => {
  e.returnValue = 'return value'
})


app.on('ready', createWindow)

```


<br />

renderer.js

```javascript
const electron = require('electron')
const {ipcRenderer} = electron

ipcRenderer.send('channel1', 'Message')

const response = ipcRenderer.sendSync('sync-message', 'Message')

ipcRenderer.on('channel1-response', (e, args) => {
  
})

ipcRenderer.on('mailbox', (e, args) => {
  console.log(args)
})

app.on('ready', createWindow)
```

<br />

### Remote module

[Remote](https://www.electronjs.org/docs/api/remote)

* risky exposing node js to users
* performance penalty
* accessing electron node modules in renderer process

main.js
```javascript
const electron = require('electron')
const {app, BrowserWindow, ipcMain} = electron

let primaryDisplay = screen.getPrimaryDisplay()

let tray
let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: primaryDisplay.size.width / 2,
    height: primaryDisplay.size.height,
    x: primaryDisplay.bounds.x,
    y: primaryDisplay.bounds.y,
    webPreferences: {
      ndoeIntegration: true,
      enableRemoteModule: true
    }
  })

  mainWindow.loadFile('index.html')
  
  
  mainWindow.webContents.on('did-finish-load', (e) => {
    mainWindow.webContents.send('mailbox', 'you have mail')
  })
  
}


app.on('ready', createWindow)

```

<br />

renderer.js

```javascript
const electron = require('electron')
const {remote} = electron
const {dialog, BrowserWindow} = remote

setTimeout(() => {
  dialog.showMessageBox({
    message: 'Dialog from renderer',
    buttons: ['One', 'Two']
  }).renderer( res => {
    console.log(res)
  })
  
  let win = new BrowserWindow({
    x: 50,
    y: 50,
    width: 300,
    height: 250
  })
  
  const app = remote.app
  
  let mainWindow = remote.getCurrentWindow()
  
  mainWindow.maximize()
},2000)

```

<br />

### Disabled remote and calling electron modules from renderer

main.js
```javascript
const electron = require('electron')
const {app, BrowserWindow, ipcMain, dialog} = electron

let primaryDisplay = screen.getPrimaryDisplay()

let tray
let mainWindow


ipcMain.on('ask-fruit', (e) => {
  callDialog().then(answer => {
    e.reply('answer-fruit', answer)
  })
})

ipcMain.handle('ask-fruit2', (e) => {
  return callDialog()
})

async function callDialog () {
  const buttons = ['One', 'Two']
  
  const index = await  dialog.showMessageBox({
    message: 'Dialog from renderer',
    buttons: buttons
  }).renderer( res => {
    console.log(res)
  })
  
  return buttons[index]
}

function createWindow () {
  mainWindow = new BrowserWindow({
    width: primaryDisplay.size.width / 2,
    height: primaryDisplay.size.height,
    x: primaryDisplay.bounds.x,
    y: primaryDisplay.bounds.y,
    webPreferences: {
      ndoeIntegration: true,
      enableRemoteModule: false
    }
  })

  mainWindow.loadFile('index.html')
  
  
  mainWindow.webContents.on('did-finish-load', (e) => {
    mainWindow.webContents.send('mailbox', 'you have mail')
  })
  
}


app.on('ready', createWindow)

```

<br />

renderer.js

```javascript
const electron = require('electron')
const {ipcRenderer} = electro

ipcRenderer.send('ask-fruit')

ipcRenderer.on('answer-fruit', (e, arg) => {
  console.log(answer)
})

ipcRenderer.invoke('ask-fruit2').then(answer => {
  console.log(answer)
})


```

<br />

## Shared api

### Process

[electron js process](https://www.electronjs.org/docs/api/process)

* available at main process
* available at renderer process when node integration is true
* available process methods
    * hang()
    * crash()
  
<br />


### Shell 

[shell](https://www.electronjs.org/docs/api/shell)

use default applications for resource usage

* openExternal
* openPath
* showItemInFolder
* moveItemToTrash

```javascript
const {shell} = require('electron')
```

<br />

### Native images

[native images](https://www.electronjs.org/docs/api/native-image)

main.js
```javascript
const {nativeImage, ipcMain} = require('electron')

let mainWindow


ipcMain.handle('app-path', () => {
  return app.getPath('desktop')
})

```

<br />

renderer.js
```javascript
const {nativeImage, ipcRenderer} = require('electron')
const fs = require('fs')
const splash = nativeImage.createFromPath(`${__dirname}/splash.png`)
console.log(splash.getSize())
const toPng = e => {
  let pngSplash = splash.toPNG()
}
const toJpg = e => {
  let pngSplash = splash.toJPEG(100)
}
const toTag = e => {
  
  let size = splash.getSize()
  
  const resizedImage = splash.resize({ width: Math.round(size.width / 4), height: Math.round(size.height / 4)})
  
  let splashUrl = resizedImage.getDataUrl()
  document.getElementById('preview').src = splashUrl 
  
}
const saveToDesktop = async (data, extension) => {
  let desktopPath = await ipcRenderer.invoke('app-path')
  fs.writeFile(desktopPath + '/' + extension, data, console.log)
} 

```


<br />


### Clipboard

[Clipboard](https://www.electronjs.org/docs/api/clipboard)

```javascript
const {clipboard} = require('electron')

console.log(clipboard.readText())

const contentOfClipboard = clipboard.readText()

clipboard.writeText('some text')

const image = clipboard.readImage()

image.toDataUrl()
```

<br />


## Features & techniques


### offscreen rendering

* The "paint" event will fire each time a section of the webContents is
  rendered to a BrowserWindow. This happens regardless of the webContents being visible or 
  not and is useful for handling off-screen content rendering.

[offscreen rendering](https://www.electronjs.org/docs/tutorial/offscreen-rendering)

main.js
```javascript
const electron = require('electron')
const {app, BrowserWindow} = electron
const fs = require('fs')


// dont use gpu, use cpu instead
app.disableHardwareAcceleration()

let primaryDisplay = screen.getPrimaryDisplay()

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: primaryDisplay.size.width / 2,
    height: primaryDisplay.size.height,
    x: primaryDisplay.bounds.x,
    y: primaryDisplay.bounds.y,
    show: false,
    webPreferences: {
      ndoeIntegration: true,
      offscreen: true
    }
  })

  mainWindow.loadUrl('https://electronjs.org')
  
  
  let i = 1
  mainWindow.webContents.on('paint', (e, dirty, image) => {
    let screenshot = image.toPNG()
    fs.writeFile(app.getPath('desktop') + `/screenshot_${i}.png`, screenshot, console.log)
    i++
  })
  
  
  mainWindow.webContents.on('did-finish-load', () => {
    console.log(mainWindow.getTitle())
    mainWindow.close()
    mainWindow = null
  })
  
}


app.on('ready', createWindow)

```


<br />

### Network detection

[Online & offline](https://developer.mozilla.org/en-US/docs/Web/API/NavigatorOnLine/Online_and_offline_events)

* exclusive to the renderer process
* there are events that report if app is online or offline

```javascript

const isOnline = navigator.onLine ? 'online' : 'offline'

```

<br />


### Notifications
[Notifications](https://www.electronjs.org/docs/tutorial/notifications)

* clicking the notification focuses the app

```javascript
const {remote} = require('electron')
const self = remote.getCurrentWindow()

let notification = new Notification('Electron app', {
  body: 'Some notification info'
})

notification.onclick = (e) => {
  if (!self.isVisible()) {
    self.show()
  }
}
```

<br />


### Preload scripts

[security](https://www.electronjs.org/docs/tutorial/security)


preload.js
```javascript
const fs = require('fs') 

window.versions = {
  electron_v: process.versions.electron,
  writeContent: function (text) {
    fs.write(__dirname, '/a_file.txt', text, console.log)
  }
}
```

<br />

main.js
```javascript
const electron = require('electron')
const {app, BrowserWindow} = electron
const fs = require('fs')

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    show: false,
    webPreferences: {
      ndoeIntegration: false,
      preload: __dirname + 'preload.js'
    }
  })

  mainWindow.loadUrl('https://electronjs.org')
  
}


app.on('ready', createWindow)
```

<br />


### Progress bar

* loading bar for an app depending on the operating system
* `mainWindow.setProgressBar(-1)` remove the progress bar


[Progress bar](https://www.electronjs.org/docs/tutorial/progress-bar)



```javascript
const electron = require('electron')
const {app, BrowserWindow} = electron
const fs = require('fs')

let mainWindow

function createWindow () {
  mainWindow = new BrowserWindow({
    width: 1000,
    height: 1000,
    show: false,
    webPreferences: {
      ndoeIntegration: false,
      preload: __dirname + 'preload.js'
    }
  })
  
  mainWindow.setProgressBar(0.25)

  mainWindow.loadUrl('https://electronjs.org')

}


app.on('ready', createWindow)
```

<br />

## Application distribution

### Electron builder

[electron builder](https://www.electron.build/)

CloudConvert is an online file converter. We support nearly all audio,
video, document, ebook, archive, image, spreadsheet, and presentation 
formats. To get started, use the button below and select files to convert from your computer.
[Cloud convert](https://cloudconvert.com/)

<br />

### Code signing

Code Signing Certificates allow you to add digital signatures to your executables,
enable software developers to include information about themselves and the integrity
of their code with their software. The end users that download digitally signed 32-bit or 64-bit 
executable files (.exe, .ocx, .dll, .cab, and more) can be confident that the code really comes 
from you and has not been altered or corrupted since it was signed.
[Comodo ssl store](https://comodosslstore.com/code-signing)
[Apple Developer](https://developer.apple.com/programs/)

<br />

### Publishing releases

Electron builder: A complete solution to package and build a ready for distribution Electron
app for macOS, Windows and Linux with “auto update” support out of the box.

[Auto update](https://www.electron.build/auto-update)

[Semantic versioning](https://www.geeksforgeeks.org/introduction-semantic-versioning/)

<br />


### AutoUpdater module

[Auto update](https://www.electron.build/auto-update)
[Electron log](https://github.com/megahertz/electron-log)

<br />

## MacOs

[Apple id](https://appleid.apple.com/)

### Notarize 

[Electron notarize](https://github.com/electron/electron-notarize)

<br />

### Hardened runtime
The Hardened Runtime, along with System Integrity Protection (SIP), protects the runtime
integrity of your software by preventing certain classes of exploits, like code injection,
dynamically linked library (DLL) hijacking, and process memory space tampering. To enable
the Hardened Runtime for your app, navigate in Xcode to your target’s Signing & Capabilities
information and click the + button. In the window that appears, choose Hardened Runtime.

[Hardened runtime](https://developer.apple.com/documentation/security/hardened_runtime)

<br />

### Touch bar

[Apple touch bar](https://www.electronjs.org/docs/api/touch-bar)
[Touch bar simulator](https://sindresorhus.com/touch-bar-simulator/)
