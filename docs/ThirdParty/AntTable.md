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
