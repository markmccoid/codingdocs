# Tools 

## Create-React-app

We are going to use create-react-app as the boilerplate for the component library.

Make sure that you have it installed globally 

` npm install -g create-react-app `

Then use it to create the initial application.  If the directory for the new app is not created use the following:

```
create-react-app appName 
cd appName
npm run eject
```

This will eject the app, thus giving you full control over the application.

## Webpack config changes
You will also need to modify the **webpack.config.dev.js** and **webpack.config.prod.js** to allow for script outside of the src directory to be loaded.


1. Search the config files for `new ModuleScopePlugin(paths.appSrc),` and comment it out. 
    - This allows the app to import modules outside of the source directory.  Actually the line we are commenting out **kept** you from import modules outside of the source directory.
1. Search for the `alias:` section and add an alias like `'ps-react': path.resolve(__dirname, '../src/components')`
    - This sets up an alias so we don't have crazy long paths to our components.  Think this will be most useful if publishing to npm.
    - Now when referencing our component in our example files we will use this import statement.
    - ` export { default } from './HelloWorld'; `

## Other tools

- react-docgen - Generate React component metadata
- chokidar - Watches the files
- highlight.js - Syntax highlighting
- npm-run-all - run multiple scripts at once

**npm install script**
```
npm install --save-dev react-docgen@2.14.0 chokidar@1.6.1 npm-run-all@4.0.2

npm install --save highlight.js@9.10.0
```
## Structure of components
First we have the directory structure that must be followed:

![](https://dl.dropboxusercontent.com/s/yykx9odtdbaejan/complibrarystructure.png)

The *src\\components* directory will have a folder for each component.  Inside we will have the js component files needed for the component and to make importing easier we will have an **index.js** file that will have this: ` export { default } from './HelloWorld'; `  This will allow us to reference our component just by referencing our component directory.  This coupled with the alias we set up in the webpack config (*see webpack config section*) will allow us to reference like this: ` import HelloWord from './aliasName/HelloWorld' `

**Examples**
The example directory will have a folder for each component with as many code example as needed.

