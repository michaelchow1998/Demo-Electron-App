# How to create demo Electron React App

## Set up 
___
### Create-react-app


```
cd ~/ your-prefered-location

npx create-react-app electron-react-demo

```

### Install electron
* electron-is-dev: The command also installed a useful npm package called used for checking whether our electron app is in development or production.
```
cd electron-react-demo

npm i -D electron electron-is-dev

```
### Install related package 
* Concurently: allows us to run mutliple commands within one script
* will-on: will wait for port 3000 which is the default CRA port, to launch the app.
* cross-env: env variable

```
npm i -D concurrently wait-on cross-env
```

### Configuring package.json
___

Add this before your scripts in package.json
```
  "main": "public/electron.js",
```

Change yuor scripts
```
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "dev": "concurrently \"cross-env BROWSER=none npm start\" \"wait-on http://localhost:3000 && electron .\"",
    "electron": "wait-on tcp:3000 && electron ."
  },
```

## Main 
---
create electron.js in public folder, the path will be /public/electron.js
* doc: https://www.electronjs.org/docs/latest/tutorial/quick-start
```
const path = require('path');

const { app, BrowserWindow } = require('electron');
const isDev = require('electron-is-dev');

function createWindow() {
  // Create the browser window.
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
    },
  });

  // and load the index.html of the app.
  // win.loadFile("index.html");
  win.loadURL(
    isDev
      ? 'http://localhost:3000'
      : `file://${path.join(__dirname, '../build/index.html')}`
  );
  // Open the DevTools.
  if (isDev) {
    win.webContents.openDevTools({ mode: 'detach' });
  }
}
```

## Final
* Run the application 
```
npm run dev
```