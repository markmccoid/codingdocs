# Table Component
[Table Docs](https://ant.design/components/table/#) 

First things first, as with all Ant's components, there is a lot that you can do with the Table.  I am sure I'm only scratching the surface.

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
	render: (text, record, index) => (
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

 **render** : (OPTIONAL) You can use this to do some cool stuff with the data. It takes the function you provide and passes the text from the data object and the full record (row) that is being rendered and lastly the index of the row. This means that you could create a column that has no corresponding data object (as in the Action column above). You could then create something based on all the column data for the current row. Meaning that {record.name} on the first row of the table would be "Mike", the first row of data.  You can also render a component with the render function.  Maybe an editable field component.

Finally, you can display your table. 
```javascript
import { Table } from 'antd';
//OR
import Table from 'antd/lib/table';
import 'antd/lib/table/style/css';

...
		return (
			<div>
				<Table dataSource={dataSource} columns={columns} />
			</div>
		);
	...
```

## Row Selection 
[Table rowSelection Config](https://ant.design/components/table/#rowSelection)

Many times you will want to be able to select a row and do something with it.  To do this, you will need to create a *rowSelection Config* object and pass it as a prop to the Table component.

In the example below, I created a function the returns the config object.  I pass into the function a *onDeleteToggle* function to use when items are selected via the checkbox and the *idsToDelete* array, which is an array of the table keys/data ids that are to be deleted.

```javascript
const getRowConfig = (onDeleteToggle, idsToDelete) => {
  return {
    //checkbox or radio.  If you need multiple, then use checkbox
    type: 'checkbox',
    //an array of rows that you want to be selected.  This is important
    //if there is a page reload.  
    selectedRowKeys: idsToDelete,
    //Called whenever a record is selected
    onSelect: (record, selected, selectedRows) => {
        //create array of ids to delete
        let rowsToDelete = selectedRows.map(row => row.key);
        onDeleteToggle(rowsToDelete);
      },
    //Called whenever the "select all" checkbox is selected
    onSelectAll: (selected, selectedRows, changeRows) => {
      //create array of ids to delete
      let rowsToDelete = selectedRows.map(row => row.key);
      onDeleteToggle(rowsToDelete);
    }
  }
}
```

**type** - checkbox or radio

**selectedRowKeys** - Bottom line whatever array of ids/keys you assign here, will get a checkmark.  I'm storing the ids/keys in the component above the component that has the Table in it and the idsToDelete is held in that components state.  So, if this component gets reloaded, this helps keep things selected.

**onSelect** - This function is called whenever a row is selected.  It gets the following:

  - record - object containing data of record selected or unselected
  - selected - boolean is *true* if selected *false* if unselected
  - selectedRows - Array of objects of all selected rows

I have found that the **selectedRows** argument is very useful.  Most of the time you really only care about what items are currently selected.  You can use this argument to store the selectedRows in a state variable or redux store.

**onSelectAll** - This function is called whenever the "select all" checkbox is checked or unchecked.  It has the following parameters:

- selected - boolean is *true* if checked(select all) and *false* if unchecked.
- selectedRows - When the Select All or Select None is done, these are the rows that are selected.
- changeRows - These are the rows that changed state when the checkbox was checked.  If for example, you had rows 1 and 2 already checked and then you checked the Select All checkbox, the change rows would be all the rows EXCEPT 1 and 2, since their state did not change.  They were already selected.

## Changing Row CSS

First, if you want to change the over-all row CSS, the class to modify is **_.ant-table-row_**.

If you want to dynamically modify based on some state of the row, then you will have to set the **rowClassName** property on the Table component.

The **rowClassName** can either be a string or a function that returns a string.  The most useful is, of course, the function.  
When a function is defined, it will be run on each row in the data. This is what allows you to dynamically select a class name for a row.

*function(record, index)*

- record - an object with the data for that row in the table
- index - the index of the table row (int)

```javascript
//Function to pass to rowClassName property on Table
const getRowClassName = (record, index) => {
    let rowClass = '';
    //if this record is in our idsToDelete array, then give class
    if (props.idsToDelete.filter(id => id === record.key).length > 0) {
      rowClass = 'row-selected';
    }
    //Check if the field isNewWord true and add another class if it is.
    let newWordClass = record.isNewWord ? 'new-word' : '';
    return `${rowClass} ${newWordClass}`;
  }
```

## Table Component Common Properties
These are just the properties that I have found most useful:

```javascript
<Table
  dataSource={tableData}
  columns={tableColumns}
  pagination={false}
  size='small'
  bordered
  rowSelection={rowSelectionConfig}
  rowClassName={getRowClassName}
/>;
```
- **pagination** - turns pagination on and off.  Note that if true, it will use the antd Pagination component, which has it's own set of config properties:    [pagination config](https://ant.design/components/pagination/)  Not sure how to change them.
- **size** - this is the size of the padding between the table rows.
  - options - *default, middle, small*
- **bordered** - Borders between columns (boolean)
- **rowSelection** - config object discussed above
- **rowClassName** - discussed above. 