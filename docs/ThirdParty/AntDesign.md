# Ant Design
[Antd's React Page](https://ant.design/docs/react/introduce)

[Setting Up Antd with Webpack](https://medium.com/@GeoffMiller/how-to-customize-ant-design-with-react-webpack-the-missing-guide-c6430f2db10f)

## Install and Importing

You can use the (babel-import-plugin)[https://github.com/ant-design/babel-plugin-import] to have babel transpile so that you don't get the whole library installed and just the components you import, but it confuses me! 

So, instead, I prefer to import manually like this:

```javascript
import DatePicker from 'antd/lib/date-picker';
import Button from 'antd/lib/button';
//Also con import css
import 'antd/dist/antd.css'; //All the css...may be best not sure
//Or import individual styles, but to me it looks like you get it all anyway.. not sure
import 'antd/lib/date-picker/stle/css';
import 'antd/lib/button/stle/css';
```
One thing to note with the css file imports.  They can overwrite existing global styles.  So if you import the antd css after your other base css files, it seems that antd likes to overwrite things like the html *body* settings.

Once option is to just leave off the css and see if it works OR import your css files either in your Sass folder or in the app.js file in the order that you want them to show up.

## Less file for Variable Overrides
Antd uses Less for its CSS preprocessor.  If you want to override some of their default variables, you will need to setup webpack for Less processing.
1. Install the less-vars-to-js package - Takes in the contents of a less file as a string and returns an object containing all the variables it found.
`npm install less-vars-to-js --save-dev`
2. Install the less and less-loader modules
`npm install less less-loader --save-dev`
3. Create an .less file where you will create your override variables.
[Antd Variables](https://github.com/ant-design/ant-design/blob/master/components/style/themes/default.less)
```less
// ant-default-vars.less
// Available theme variables can be found in
// https://github.com/ant-design/ant-design/blob/master/components/style/themes/default.less

@primary-color: #193D71; <-- our first ant theme override!
```
4. Configure webpack loader to read this override variable file in and modify the vars:
[Setting Up Antd with Webpack](https://medium.com/@GeoffMiller/how-to-customize-ant-design-with-react-webpack-the-missing-guide-c6430f2db10f)
```javascript
// webpack*.config.js
const path = require('path');
const fs  = require('fs');
//--Use lessToJs to read in our override file
const lessToJs = require('less-vars-to-js');
const themeVariables = lessToJs(fs.readFileSync(path.join(__dirname, './ant-theme-vars.less'), 'utf8'));
...
// webpack2.config.js
module: {
  rules: [
    ...
    {
      test: /\.less$/,
      use: [
        {loader: "style-loader"},
        {loader: "css-loader"},
        {loader: "less-loader",
          options: {
            modifyVars: themeVariables
          }
        }
      ]
    }
  ]
}
//webpack1.config.js
module: {
  loaders: [
    {
      test: /\.less$/,
      loader: 'style-loader!css-loader!less-loader',
      query: {
        modifyVars: themeVariables
      }
    },
  ...
  ]
}
```