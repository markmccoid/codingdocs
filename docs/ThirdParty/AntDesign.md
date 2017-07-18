# Ant Design
[Antd's React Page](https://ant.design/docs/react/introduce)

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

