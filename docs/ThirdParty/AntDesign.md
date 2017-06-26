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

## Table Component
[Table Docs](https://ant.design/components/table/#) 

A couple of ways to accomplish the basic table.

One way would be to pass in two objects, one defining the **columns** and the other defining the **data.** 

Here is the data array format:

```javascript
const dataSource = [{
	key: '1',
	name: 'Mike',
	age: 32,
	address: '10 Downing Street'
}, {
	key: '2',
	name: 'John',
	age: 42,
	address: '10 Downing Street'
}];
```

The **key** property maps to the React Key, so any unique value across your table rows will do.

Next we need to map that data to columns in our table. This is done with an object arrays defining the columns:

```javascript
const columns = [{
	title: 'Name',
	dataIndex: 'name',
	key: 'name',
	render: text => <a href="#">{text}</a>
}, {
	title: 'Age',
	dataIndex: 'age',
	key: 'age',
}, {
	title: 'Address',
	dataIndex: 'address',
	key: 'address',
}, {
	title: 'Action',
	key: 'action',
	dataIndex: 'action',
	render: (text, record) => (
	<div>
	<a href="#">Action - {record.name}</a>
	<span className="ant-divider" />
	<a href="#">New - {record.age}</a>
	</div>
	)
}
];
```

 **title** : Obvious, but the header title for this column

 **dataIndex** : this links us to the data in the data object array. It will match the property you want to pull from each data object.

 **key** : The react key property. Unique within the row of columns. i.e. each column in your row has a unique key.

 **render** : (OPTIONAL) You can use this to do some cool stuff with the data. It takes the function you provide and passes the text from the data object and the full record (row) that is being rendered. This means that you could create a column that has no corresponding data object (as in the Action column above). You could then create something based on all the column data for the current row. Meaning that {record.name} on the first row of the table would be "Mike", the first row of data.

Finally, you can display your table. 
```javascript
import { Table } from 'antd';

...
		return (
			<div>
				<Table dataSource={dataSource} columns={columns} />
			</div>
		);
	...
```
