##  Basics of the webpack.config.js File

The Webpack config file is a JavaScript file that exports an object with the configuration information that webpack will use to bundle you code.

[Webpack's Configuration Documentation](https://webpack.js.org/configuration/)

A very basic config, looks like this:

```javascript
const path = require('path');

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.join(__dirname, '/public'),
    filename: 'bundle.js'
  },
  module: {
    rules: [{
      loader: 'babel-loader',
      test: /\.js$/,
      exclude: /node_modules/
    }]
  },
  devtool: 'cheap-module-eval-source-map'
};
```

You will also need a **.babelrc** file that tells the babel loader what presets to use:

```javascript
{
  "presets": [
    "env","react"
  ]
}
```

Note, that to get this to work you will need the following npm packages:

- babel-core
- babel-loader
- babel-preset-env
- babel-preset-react
- webpack (duh)



## CSS Loaders

### Just CSS Loading

If you just want to be able to import a css file and have webpack process it, you will need to add the following loaders:

- style-loader
- css-loader

And then add the following to the **module** object's **rule** array in the webpack config file:

```javascript
 module: {
    rules: [{
      loader: 'babel-loader',
      test: /\.js$/,
      exclude: /node_modules/
    },
    { //HERE IS THE CSS PART
      test: /\.css$/,
      use: [
        'style-loader',
        'css-loader'
      ]
    }]
  },
```

### Setting up SCSS in Webpack

If we also want to use CSS, we will need the sass-loader as well and a new rule or we can repurpose the CSS rule.

First let's look at adding a new rule:

```javascript
 module: {
    rules: [{
      loader: 'babel-loader',
      test: /\.js$/,
      exclude: /node_modules/
    },
    { //HERE IS THE CSS PART
      test: /\.css$/,
      use: [
        'style-loader',
        'css-loader'
      ]
    },
    {
      test: /\.scss$/,
      use: [
        'style-loader',
        'css-loader',
        'sass-loader'
       ]
    }
   ]
  },
```

Or you could just modify your regular expression in your CSS case and add on the sass-loader at the end.  It seems that webpack is smart enough to handle both CSS and SCSS files with one rule.

```javascript
    module: {
      rules: [{
        loader: 'babel-loader',
        test: /\.js$/,
        exclude: /node_modules/
      },
      {
        test: /\.s?css$/,
        use: [
          'style-loader',
          'css-loader',
          'sass-loader'
        ]
      }]
    }, 
```



### CSS Modules

This is based on create-react-app's webpack config, but should work outside of it also.

> **NOTE:** If you enable CSS Modules, you will lose any global styles.  Meaning you need to use CSS Modules everywhere.

The webpack config's *modules* section is an object and one of the keys is **rules:**, this is where you will find the test for CSS.

As an aside, the *rules* section of the *modules* part of the config is an array of objects.  Here is the css loader part:

```javascript
{
  test: /\.css$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        ident: 'postcss', // https://webpack.js.org/guides/migrating/#complex-options
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
  ],
},
```
We only care about the first loader section ` loader: require.resolve('postcss-loader'), ` 

This is the section we need to add two lines to:

```javascript
test: /\.css$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
        modules: true,
        localIdentName: '[name]_[local]_[hash:base64:5]'
      },
    },
```

*modules:* will tell webpack to use modules and *localIdentName:* will tell it how to name, which will be name of component, [local] is the css class name and then the first 5 characters of the hash.

### Extract-Text-Webpack-Plugin for CSS ([Docs](https://github.com/webpack-contrib/extract-text-webpack-plugin))

This plugin allows you to extract some text from Webpack.  The usage explained in this section is so that we can get a separate CSS files from our SASS/SCSS and CSS files and another for our JavaScript.

Here is what needs to be done:

1. import the plugin into our webpack config file

   ```javascript
   const ExtractTextPlugin = require('extract-text-webpack-plugin');
   ```

2. Create new instance of the ExtractTextPlugin. 

   ```javascript
   module.exports = (env) => {
     const isProduction = env === 'production';
     //--This is setting up a new instance(function), telling it what file to create
     //--from the extracted text.
     const CSSExtract = new ExtractTextPlugin('styles.css');
   ```

3. Setup a rule in the module(object)/rules(array).  This rule will replace the CSS/SCSS rule.  Note that we no longer need the style-loader as this loader was used to in-line the styles.

   ```javascript
       module: {
         rules: [{
           loader: 'babel-loader',
           test: /\.js$/,
           exclude: /node_modules/
         },
         {
           test: /\.s?css$/,
           use: CSSExtract.extract({
             use: [
               'css-loader',
               'sass-loader'
             ]
           })
         }]
       },
   ```

4. add it to the plugins array in webpack config file

   ```
       ,
       plugins: [
         CSSExtract
       ],
   ```

When you push all these guys into a single CSS file, you will lose the ability to see which source file they came from when inspecting in chrome.  One way to fix this is to add the source map option onto the css and sass loaders and then change the devtool option.

```javascript
 module: {
      rules: [{
        loader: 'babel-loader',
        test: /\.js$/,
        exclude: /node_modules/
      },
      {
        test: /\.s?css$/,
        use: CSSExtract.extract({
          use: [
            {
              //The use property is now using an array of objects instead
              //of an array of strings.  This allows us to add an options
              //section where we tell it to set "sourceMap" to true.
              loader: 'css-loader',
              options: {
                sourceMap: true
              }
            },
            {
              loader: 'sass-loader',
              options: {
                sourceMap: true
              }
            }
          ]
        })
      }]
    },
    plugins: [
      CSSExtract
    ],
    //To use the source map options we change the devtool for development from 
    //"inline-cheap-source-map" to "inline-source-map"
    devtool: isProduction ? 'source-map' : 'inline-source-map',
```

[Webpack devtool Options](https://webpack.js.org/configuration/devtool/)

Please note that this is only one way to configure the Plugins and other stuff

##Webpack Dev Server

The *webpack-dev-server* is a little Node.js [Express](http://expressjs.com/) server, which uses the *webpack-dev-middleware* to serve a *webpack bundle*. It also has a little runtime which is connected to the server via [Sock.js](http://sockjs.org/).

To install via yarn:

```javascript
> yarn add webpack-dev-server
```

Once installed, you can control the configuration of the dev server through the webpack configuration file.

You will add a new property called *devServer*

```javascript
module.exports = {
  entry: './src/app.js',
  output: {...},
  module: {...},
  devtool: 'cheap-module-eval-source-map',
  devServer: {
      contentBase: path.join(__dirname, '/public')
  }
};
```

The only option needed to run the dev server is the content base, which tells dev server where the assets are.  This most likely will be the same as your output path in the output section of the config file.

[Webpack DevServer Configuration Options](https://webpack.js.org/configuration/dev-server/)

You will then need to create a script in **package.json** to run the dev server:

```json
  "scripts": {
    "build": "webpack",
    "dev-server": "webpack-dev-server"
  },
```

**NOTE:** The webpack dev server doesn't create a physical bundle.js when running.  When you need the physical bundle.js file, you will need to run the webpack command (build script above) to generate the file.

## Babel Options and Presets

Currently in my Javascript and React project (circa 2017), I use the following presets:

- babel-preset-env
- babel-preset-react

And the following babel plugin.

- babel-plugin-transform-class-properties

```javascript
> yarn add babel-plugin-transform-class-properties babel-preset-env babel-preset-react
```

Our **.babelrc** file will now look like this:

```json
{
  "presets": [
    "env","react"
  ],
  "plugins": [
    "transform-class-properties"
  ]
}
```



## Production Build

[Webpack.js.org Production Docs](https://webpack.js.org/guides/production/)



