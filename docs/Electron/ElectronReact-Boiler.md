# My React Electron Boilerplate app

Check out this [Article link](https://medium.com/@kitze/%EF%B8%8F-from-react-to-an-electron-app-ready-for-production-a0468ecb1da3), it details what we are doing here.

The basics are you first use create-react-app to create your application. Then you add some npm modules to get electron, electron builder and some helper modules.

```
$ npx create-react-app appname
$ yarn add electron-builder concurrently wait-on --dev
$ yarn add electron electron-is-dev
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

## React Router Setup

Now we need to add routing to our boilerplate application.

I'm using React Router 4.

```
$ yarn add react-router-dom
```

I choose to use the MemoryRouter as I believe there would be issues with BrowserRouter and read some things about HashRouter being deprecated.  Also read some stuff about issues with MemoryRouter when trying to route to a page outside of the react app, but will deal with if encountered.

Here is a simple example implementation.

Modify App.js to be:

```jsx
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import { MemoryRouter, Route, Link } from 'react-router-dom';
import { Home, About } from './MyRoutes';

const Header = () => (<header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <h1 className="App-title">Welcome to Electron React</h1>
        <Link to="/about">About</Link>
      </header>
);
class App extends Component {
  render() {
    return (
      <MemoryRouter
        initialEntries={['/', '/about']}
        initialIndex={0}
      >
        <div className="App">
          <Header />
          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
        </div>
      </MemoryRouter>
    );
  }
}

export default App;

```

You can load MemoryRouter with initial routes using the "initialEntries" array.  Think this is only useful to force you to your main path as all routes do not need to be in this initial array.

Next create a **MyPages.js** file to hold our example route destinations.

```jsx
import React from 'react';
import { Redirect } from 'react-router-dom';

export const Home = (props) => {
  return(
    <div>
      <h1>You Are Home</h1>
      <p className="App-intro">
        To get started, edit <code>src/App.js</code> and save to reload.
      </p>
    </div>
  )
};

export const About = (props) => {
  return (
    <div>
      <h1>About</h1>
    </div>
  )
}

export const Contact = (props) => {
  console.log(props);
  let pos = props.history.entries.length - 2;
  return (
    <div>
      <h1>Contacts</h1>
      {props.history.entries[pos].pathname === "/about" ? 
      <Redirect to="/" /> :
      null
      }
    </div>
  )
}
```

In the **Contact** component, I'm playing around with Redirect, just to get a feel for it.  Useless in this context, but good to know how it works.  Believe it can be used in Auth stuff.

Here is an image of the file structure thus far.

![](https://dl.dropbox.com/s/3zofs1inlrorkvr/electron-file-structure.png)

However, I want my main react components in a component directory.  I will refactor as follows.

![](https://dl.dropbox.com/s/kiiqotgtypqmjm3/electron-refactor-structure.png?dl=0)