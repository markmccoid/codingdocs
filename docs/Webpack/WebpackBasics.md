## CSS Loaders

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
