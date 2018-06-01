# My React Electron Boilerplate app

Check out this [Article link](https://medium.com/@kitze/%EF%B8%8F-from-react-to-an-electron-app-ready-for-production-a0468ecb1da3), it details what we are doing here.

The basics are you first use create-react-app to create your application. Then you add some npm modules to get electron, electron builder and some helper modules.

```
$ npx create-react-app appname
$ yarn add electron electron-builder concurrently wait-on --dev
$ yarn add electron-is-dev
```

The article also has you install **wait-on**, but I couldn't get it to work on windows and didn't see any adverse effects.

**Concurrently** is used in *package.json* scripts to run multiple commands in one script.

Now we have a react app, but we want it to run in Electron.  To do this we need to add **electron.js** to the **public** directory.  This is so that during build time, it will get moved to the build directory.

```javascript
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

const path = require('path');
const url = require('url');
const isDev = require('electron-is-dev');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({width: 900, height: 680});
  mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
  mainWindow.on('closed', () => mainWindow = null);
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});

```

You can see that we are using the **electron-is-dev** module to determine if we are in dev mode or build mode.

We don't want a browser to open automatically, so we need to set the env variable BROWSER to none, however, I couldn't get this to work on Windows in a *Package.json* script.

To make it work, I created the **.env.development** file.  create-react-app will read this file.

```
BROWSER="NONE"
```

Now we need to fix up *package.json* file so electron knows where to find its entry point and to update the scripts.

```json
"homepage": "./",
"main": "public/electron.js",
"scripts": {
    "start": "react-scripts start",
    "electron-dev": "concurrently  \"yarn start\" \"wait-on http://localhost:3000 && electron .",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    "electron-pack": "build --em.main=build/electron.js",
    "pack": "electron-builder --dir",
    "preelectron-pack": "yarn build"
},
"build": {
    "appId": "com.example.electron-cra",
    "files": [
        "build/**/*",
        "node_modules/**/*"
    ],
    "directories":{
        "buildResources": "assets"
    }
}
```

*main* tells electron where the entry point is. *homepage* has something to do with the build for production.

You should now be able to run the *electron-dev* script.

```
$ yarn run electron-dev
```

